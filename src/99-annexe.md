# Annexe

## Récupération des dépôts originaux (optionnel)

> A réaliser en cas de pb uniquement !!

### Méthode recommandée (Gandi)

```bash
sudo vi /etc/apt/sources.list.d/multistrap-ubuntu-xenial.list
````

Puis ajouter "universe" à la fin de la ligne qui s'y trouve, comme ceci

```bash
deb [arch=amd64] https://mirrors.gandi.net/ubuntu/ xenial main universe
```

### Méthode alternative (depréciée)

```bash
vi /etc/apt/sources.list
```

```bash
----------------------------------------------------------------------------------------------------------------
+# See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to newer versions of the distribution.
+deb http://de.archive.ubuntu.com/ubuntu/ xenial main restricted
+# deb-src http://de.archive.ubuntu.com/ubuntu/ xenial main restricted
+## Major bug fix updates produced after the final release of the distribution.
+deb http://de.archive.ubuntu.com/ubuntu/ xenial-updates main restricted
+# deb-src http://de.archive.ubuntu.com/ubuntu/ xenial-updates main restricted
+## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu team, and may not be under a free licence. Please satisfy yourself as to your rights to use the software. Also, please note that software in universe WILL NOT receive any review or updates from the Ubuntu security team.
+deb http://de.archive.ubuntu.com/ubuntu/ xenial universe
+# deb-src http://de.archive.ubuntu.com/ubuntu/ xenial universe
+deb http://de.archive.ubuntu.com/ubuntu/ xenial-updates universe
+# deb-src http://de.archive.ubuntu.com/ubuntu/ xenial-updates universe
+## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu team, and may not be under a free licence. Please satisfy yourself as to your rights to use the software. Also, please note that software in multiverse WILL NOT receive any review or updates from the Ubuntu security team.
+deb http://de.archive.ubuntu.com/ubuntu/ xenial multiverse
+# deb-src http://de.archive.ubuntu.com/ubuntu/ xenial multiverse
+deb http://de.archive.ubuntu.com/ubuntu/ xenial-updates multiverse
+# deb-src http://de.archive.ubuntu.com/ubuntu/ xenial-updates multiverse
+## N.B. software from this repository may not have been tested as extensively as that contained in the main release, although it includes newer versions of some applications which may provide useful features.Also, please note that software in backports WILL NOT receive any review or updates from the Ubuntu security team.
+deb http://de.archive.ubuntu.com/ubuntu/ xenial-backports main restricted universe multiverse
+# deb-src http://de.archive.ubuntu.com/ubuntu/ xenial-backports main restricted universe multiverse
+## Uncomment the following two lines to add software from Canonical's 'partner' repository. This software is not part of Ubuntu, but is offered by Canonical and the respective vendors as a service to Ubuntu users.
+# deb http://archive.canonical.com/ubuntu xenial partner
+# deb-src http://archive.canonical.com/ubuntu xenial partner
+deb http://security.ubuntu.com/ubuntu xenial-security main restricted
+# deb-src http://security.ubuntu.com/ubuntu xenial-security main restricted
+deb http://security.ubuntu.com/ubuntu xenial-security universe
+# deb-src http://security.ubuntu.com/ubuntu xenial-security universe
+deb http://security.ubuntu.com/ubuntu xenial-security multiverse
+# deb-src http://security.ubuntu.com/ubuntu xenial-security multiverse
```

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
