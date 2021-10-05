# Annexe

## Configuration des locales serveur (dépréciée)

Permet d’éviter les problèmes d’accents dans le shell

Vérifier la configuration locale

```bash
sudo locale
```

Vérifier la disponibilité des locales

```bash
sudo apt install -y language-pack-fr-base
```

```bash
sudo locale -a
```

```bash
sudo locale-gen fr_FR
```

```bash
sudo locale-gen fr_FR.UTF-8
```

```bash
sudo dpkg-reconfigure locales
```

Préparer la génération des locales fr-FR

```bash
sudo update-locale LANG=fr_FR.utf8 LC_MESSAGES=POSIX 
```

ou

```bash
sudo update-locale LANG="fr_FR.UTF-8" LANGUAGE="fr_FR"
```

Puis

```bash
sudo dpkg-reconfigure locales
```

Générer les locales

```bash
sudo locale-gen
```

Déloguer, reloguer et vérifier les locales 

```bash
sudo locale
```

```bash
sudo locale -a
```

Résoudre l’erreur “Cannot set LC_ALL to default locale” (Optionnel)

```bash
sudo locale-gen "en_US.UTF-8"
```

```bash
sudo dpkg-reconfigure locales
```

> choisir en_US.utf8

Tester avec la commande

```bash
locale
```

## Configuration de la cartographie d'écriture des logs

Décommenter les lignes utiles

```bash
sudo vi /etc/rsyslog.d/50-default.conf
```

```bash
#
# First some standard log files.  Log by facility.
#
auth,authpriv.*                 /var/log/auth.log
*.*;auth,authpriv.none          -/var/log/syslog
cron.*                          /var/log/cron.log
daemon.*                        -/var/log/daemon.log
kern.*                          -/var/log/kern.log
#lpr.*                          -/var/log/lpr.log
mail.*                          -/var/log/mail.log
user.*                          -/var/log/user.log

#
# Logging for the mail system.  Split it up so that
# it is easy to write scripts to parse these files.
#
mail.info                       -/var/log/mail.info
mail.warn                       -/var/log/mail.warn
mail.err                        /var/log/mail.err
```

## Ajout des dépôts des paquets d'installation Apache et PHP pour Ubuntu

```bash
sudo apt-get install software-properties-common && \
sudo add-apt-repository ppa:ondrej/apache2 && \
sudo add-apt-repository ppa:ondrej/php
```

## MySQL

Déclarer les répertoires de stockage dans Apparmor

```bash
sudo vi /etc/apparmor.d/usr.sbin.mysqld
```

```diff
/var/lib/mysql/ r,
/var/lib/mysql/** rwk,
+/srv/{{ (h|r)## }}-datas/var/lib/mysql/ r,
+/srv/{{ (h|r)## }}-datas/var/lib/mysql/** rwk,
/var/log/mysql/ r,
/var/log/mysql/* rw,
```

```bash
sudo systemctl restart apparmor
```
