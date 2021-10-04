# Mises à jour du serveur

## Se connecter en tant que super-utilisateur

```bash
su -
```

*Indiquez le mot de passe de l'administrateur système renseigné au moment de la création du votre serveur Gandi*.

> IMPORTANT : Le ```-``` vous permet de passer des commandes de n'importe quel chemin.

## Mettre à jour les dépendances

```bash
apt update
```

## Mettre à jour les dépôts installés vers les dernières versions stables

```bash
apt upgrade
```

## Installer sudo

**Sudo** est un utilitaire système qui accorde une élevation de privilèges lorsqu'on lance d'une commande.
Dans la plupart des cas, il va vous permettre de lancer une commande avec les droits de super-utilisateur grâce une commande préfixée par l'instruction ```sudo```.

### Lancez l'installation de sudo

```bash
sudo apt install sudo
```

### Habilitez votre utilisateur système

```bash
/usr/sbin/adduser {{ username }} sudo
```

### Déconnectez votre utilisateur système et tester la commande sudo

1. Rebasculez vers votre utilisateur système.

```bash
exit
```

2. Déconnectez-vous du serveur.

```bash
exit
```

3. Connectez-vous de nouveau via SSH

```bash
ssh {{ username }}@{{ server_IPv4 }} -p 22
```

4. Tester la mise à jour des dépôts du serveur depuis votre utilisateur sytème avec la commande sudo

```bash
sudo apt update
```

## Permettre les commandes depuis n'importe quel chemin avec $PATH

1. Editez le fichier des variables d'environnement.

```bash
sudo vi /etc/environment
```

2. Ajoutez les chemins vers les dossiers des commandes. 

```bash
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"
```

3. Tester

```bash 
sudo adduser --help
```

---
[>> Étape suivante : Groupes et Habilitations](03-groups-and-capabilities.md)