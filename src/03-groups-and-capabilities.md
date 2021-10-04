# Groupe et Habilitations

## Créer un groupe spécifique pour votre utilisateur système

1. Créer un groupe correspondant à votre nom d'utilisateur

```bash
groupadd {{ username }}
```

2. Affecter ce groupe en tant que groupe principal

```bash
usermod -g {{ username }} {{ username }}
```



## Commandes de base

[@see Utilisateurs et groupes](https://wiki.archlinux.fr/Utilisateurs_et_Groupes)

### Créer un groupe

```bash
groupadd {{ new_group }}
```

### Affecter un utilisateur à un groupe

```bash
usermod -a -G {{ new_group }} {{ username }}
```

### Changer le groupe principal d'affectation d'un utilisateur

```bash
usermod -g {{ new_main_group }} {{ username }}
```

### Contrôle des groupes d’affectation d'un utilisateur

```bash
groups {{ username }}
```

### Modification du répertoire de home d'un utilisateur

```
sudo usermod --home /srv/{{ (h|r)## }}-datas/{{ new_home_directory }} {{ username }}
```

ou sans création du répertoire

```
sudo usermod --home /srv/{{ (h|r)## }}-datas/{{ new_home_directory }} --no-create-home {{ username }}
```

### Changer le nom d'un utilisateur

```bash
sudo usermod -l {{ new_username }} {{ old_username }}
```

### Changer le nom d'un groupe

```bash
sudo groupmod --new-name {{ new_groupname }} {{ old_groupname }}
```

### Supprimer un utilisateur

```bash
sudo userdel -r {{ username }}
```

L'option -r permet de supprimer le répertoire de l'utilisateur ainsi que son fichier de mails en attente.

### Supprimer un groupe

```bash
sudo groupdel {{ groupname }}
```