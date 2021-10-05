# 05 - Installation et configuration des essentiels

## Création des répertoires

```bash
sudo mkdir -p ${SERVER_DATAS_PATH}/etc/apache2/conf-available \
sudo mkdir -p ${SERVER_DATAS_PATH}/etc/apache2/sites-available \
sudo mkdir -p ${SERVER_DATAS_PATH}/etc/ssl/certs \
sudo mkdir -p ${SERVER_DATAS_PATH}/etc/ssl/private \
sudo mkdir -p ${SERVER_DATAS_PATH}/var/lib/mysql \
sudo mkdir -p ${SERVER_DATAS_PATH}/var/log/apache2 \
sudo mkdir -p ${SERVER_DATAS_PATH}/var/www/cgi-bin \
sudo mkdir -p ${SERVER_DATAS_PATH}/var/www/html \
sudo mkdir -p ${SERVER_DATAS_PATH}/var/ftp \
sudo mkdir -p ${SERVER_DATAS_PATH}/var/scripts 
```

## Modification des droits des répertoires

```bash
sudo chown -R root.root ${SERVER_DATAS_PATH}
```

```bash
sudo chown -R root.root ${SERVER_BACKUP_PATH}
```

## Installation des utilitaires

### net-tools

**Net-tools** fourni des utilitaires qui permettent entre autre de récupérer des informations sur le réseaux du serveur.

```bash
sudo apt install net-tools
```

Tester l'installation :

```bash
sudo ifconfig
```

### Htop

**Htop** fourni une interface terminal de visualisation des ressources du serveur.

```bash
sudo apt install htop
```

Tester l'intallation :

```bash
htop
```

### Vim

**Vim** est un éditeur de texte en ligne de commande. Il s'agit d'une version amélioré de **vi**.

```bash
sudo apt install vim
```

### Midnight commander

**Midnight commander** est un explorateur de fichier en ligne de commande.

```bash
sudo apt install mc
```

### Cron

**Cron** est un gestionnaire de tâches planifiée.

```bash
sudo apt install cron
```

### Archives zip

**Zip** et **Unzip** sont des utilitaires de compression et de décompression des archives au format .zip.

```bash
sudo apt install zip unzip
```

## Installation et configuration de SSH

### Installation

```bash
sudo apt install ssh
```

### Configuration du démon SSH

Le démon SSH permet d'accéder au serveur via le protocole SSH.

```bash
sudo vi /etc/ssh/sshd_config
```

1. Changer le port (recommandé)

```diff
#       $OpenBSD: sshd_config,v 1.103 2018/04/09 20:41:22 tj Exp $

# This is the sshd server system-wide configuration file.  See
# sshd_config(5) for more information.

# This sshd was compiled with PATH=/usr/bin:/bin:/usr/sbin:/sbin

# The strategy used for options in the default sshd_config shipped with
# OpenSSH is to specify options with their default value where
# possible, but leave them commented.  Uncommented options override the
# default value.

-#Port 22
+Port {{ new_SSH_port }}
```

2. Autoriser uniquement la connexion avec une clé publique SSH (Bonne pratique)

```bash
PasswordAuthentication no
PubkeyAuthentication yes
```

### Limiter l'accès via SSH à une liste blanche d'IP connue

```bash
sudo vi /etc/hosts.allow
```

```bash
sshd : localhost : allow
# IP autorisée 1
sshd : {{ Allowed_IP_1 }} : allow
# IP autorisée 2
sshd : {{ Allowed_IP_2 }} : allow
# IP autorisée 3 
sshd : {{ Allowed_IP_3 }} : allow
# Autres
sshd : ALL : deny
```

### Redémarrage du service

```bash
sudo service sshd restart
```

## Installation et configuration du parefeu

### Installer IPtables

```bash
sudo apt install iptables
```

### Création du fichier des règles de parefeu

```bash
sudo vi ${SERVER_DATAS_PATH}/var/scripts/iptables.sh
```

*Adapter aux services que vous comptez utiliser.*

```bash
#!/bin/bash 
## On flush iptables. 
iptables -F

## On supprime toutes les chaînes utilisateurs. 
iptables -X 

## On drop tout le trafic entrant. 
iptables -P INPUT DROP

## On drop tout le trafic sortant. 
iptables -P OUTPUT DROP 

## On drop le forward. 
iptables -P FORWARD DROP 

## On drop les scans XMAS et NULL. 
iptables -A INPUT -p tcp --tcp-flags FIN,URG,PSH FIN,URG,PSH -j DROP 
iptables -A INPUT -p tcp --tcp-flags ALL ALL -j DROP 
iptables -A INPUT -p tcp --tcp-flags ALL NONE -j DROP
iptables -A INPUT -p tcp --tcp-flags SYN,RST SYN,RST -j DROP

## Dropper silencieusement tous les paquets broadcastés. 
iptables -A INPUT -m pkttype --pkt-type broadcast -j DROP 

## Permettre à une connexion ouverte de recevoir du trafic en entrée. 
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT 

## Permettre à une connexion ouverte de recevoir du trafic en sortie. 
iptables -A OUTPUT -m state ! --state INVALID -j ACCEPT

## On accepte la boucle locale en entrée. 
iptables -I INPUT -i lo -j ACCEPT 

## On log les paquets en entrée. 
#iptables -A INPUT -j LOG 

## On log les paquets forward. 
#iptables -A FORWARD -j LOG

# ICMP (Ping)
iptables -t filter -A INPUT -p icmp -j ACCEPT
iptables -t filter -A OUTPUT -p icmp -j ACCEPT

# SSH
iptables -t filter -A INPUT -p tcp --dport {{ new_SSH_port }} -j ACCEPT
iptables -t filter -A OUTPUT -p tcp --dport {{ new_SSH_port }} -j ACCEPT

# DNS
iptables -t filter -A OUTPUT -p tcp --dport 53 -j ACCEPT
iptables -t filter -A OUTPUT -p udp --dport 53 -j ACCEPT
iptables -t filter -A INPUT -p tcp --dport 53 -j ACCEPT
iptables -t filter -A INPUT -p udp --dport 53 -j ACCEPT

# HTTP
iptables -t filter -A OUTPUT -p tcp --dport 80 -j ACCEPT
iptables -t filter -A INPUT -p tcp --dport 80 -j ACCEPT

# HTTPS
iptables -t filter -A OUTPUT -p tcp --dport 443 -j ACCEPT
iptables -t filter -A INPUT -p tcp --dport 443 -j ACCEPT

# FTP + SFTP
iptables -t filter -A OUTPUT -p tcp --dport 20 -j ACCEPT
iptables -t filter -A INPUT -p tcp --dport 20 -j ACCEPT
iptables -t filter -A INPUT -p tcp --dport 29799:29899 -j ACCEPT
iptables -t filter -A OUTPUT -p tcp --dport 29799:29899 -j ACCEPT
# FTP
iptables -t filter -A OUTPUT -p tcp --dport 21 -j ACCEPT
iptables -t filter -A INPUT -p tcp --dport 21 -j ACCEPT
# SFTP (Optionnel >> Décommenter et Commenter #FTP)
# iptables -t filter -A INPUT -p tcp --dport 22 -j ACCEPT
# iptables -t filter -A OUTPUT -p tcp --dport 22 -j ACCEPT

# Mail SMTP
iptables -t filter -A OUTPUT -p tcp --dport 25 -j ACCEPT

# Mail POP3
iptables -t filter -A OUTPUT -p tcp --dport 110 -j ACCEPT

# Mail IMAP
iptables -t filter -A OUTPUT -p tcp --dport 143 -j ACCEPT

# Mail SERVICES (Mandrill, Sendgrid …)
iptables -t filter -A OUTPUT -p tcp --dport 587 -j ACCEPT

# NTP (horloge du serveur)
iptables -t filter -A OUTPUT -p udp --dport 123 -j ACCEPT

# MYSQL
#iptables -A INPUT -p tcp -s {{ Allowed_external_IP_access }} --dport 3306 -j ACCEPT

# SVN
#iptables -t filter -A OUTPUT -p tcp --dport 3690 -j ACCEPT
#iptables -t filter -A INPUT -p tcp --dport 3690 -j ACCEPT

# SAMBA (Optionnel)
#iptables -t filter -A INPUT -p tcp -s 192.168.1.0/24 --dport 139 -j ACCEPT
#iptables -t filter -A OUTPUT -p tcp --dport 139 -j ACCEPT
#iptables -t filter -A INPUT -p tcp -s 192.168.1.0/24 --dport 445 -j ACCEPT
#iptables -t filter -A OUTPUT -p tcp --dport 445 -j ACCEPT
#iptables -t filter -A INPUT -p udp -s 192.168.1.0/24 --dport 137 -j ACCEPT
#iptables -t filter -A OUTPUT -p udp --dport 137 -j ACCEPT
#iptables -t filter -A INPUT -p udp -s 192.168.1.0/24 --dport 138 -j ACCEPT
#iptables -t filter -A OUTPUT -p udp --dport 138 -j ACCEPT
#iptables -t filter -A INPUT -p udp -s 192.168.1.0/24 --dport 445 -j ACCEPT
#iptables -t filter -A OUTPUT -p udp --dport 445 -j ACCEPT
```

### Lancer la configuration des règles du parefeu

> Une mauvaise configuration du parefeu pourrait vous interdire l'accès au serveur en SSH et par la même vous empêcher de réctifier.
> Dans ce cas, utilisez la console d'urgence de Gandi prévue à cet effet.

```bash
sudo /bin/bash ${SERVER_DATAS_PATH}/var/scripts/iptables.sh
```

Sauvegarde des règles de parefeu

```bash
sudo -s iptables-save -c
```

### Installer un service de maintien des régles de parefeu

Par défaut, à chaque redémarrage les régles de parefeu sont supprimées et il faudrait relancer le script de configuration.
Le service **iptables-persistent** permet de conserver les régles de parefeu à chaque redémarrage.

**Installation**

```bash
sudo apt install iptables-persistent
```

**Activer la persistance**

```bash
sudo service netfilter-persistent save
```

### Aide mémoire des commandes

**Lister les règles de parefeu :**

```bash
sudo iptables -S
```

ou

```bash
sudo iptables -L
```

**Garantir la persistance des règles de parefeu :**

```bash
sudo service iptables-persistent save
```

**Déstruction (flush) des règles de parefeu :**

```bash
sudo service iptables-persistent flush
```

**Afficher les règles de parefeu par ligne :**

```bash
sudo iptables -vnL --line-numbers
```

**Supprimer une règle de parefeu selon sa ligne :**

```bash
sudo iptables -D INPUT {{ line_number }}
```
