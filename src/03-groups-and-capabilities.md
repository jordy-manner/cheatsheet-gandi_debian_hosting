# Groupe et Habilitations

## Groupe et habilitations de votre utilisateur système

1. Créez un groupe correspondant à votre nom d'utilisateur.

```bash
sudo groupadd {{ username }}
```

2. Affectez ce groupe en tant que groupe principal.

```bash
sudo usermod -g {{ username }} {{ username }}
```

3. Créez un groupe spécifique pour les administrateurs.

```bash
sudo groupadd admin
```

4. Affectez votre utilisateur système au groupe **admin**.

```bash
sudo usermod -a -G admin {{ username }}
```

5. Affectez votre utilisateur système au groupe **users**.

```bash
sudo usermod -a -G users {{ username }}
```

6. Vérifiez les groupes d'affectation de votre utilisateur système.

```bash
sudo groups {{ username }}
```

*Procédez à l'identique pour tous vos administrateurs ou compte de service système*.

## Commandes de base

[@see Utilisateurs et groupes](https://wiki.archlinux.fr/Utilisateurs_et_Groupes)

### Lister les groupes

```bash
sudo less /etc/group
```

### Lister les utilisateurs

```bash
sudo less /etc/passwd
```

### Créer un groupe

```bash
sudo groupadd {{ new_group }}
```

### Affecter un utilisateur à un groupe

```bash
sudo usermod -a -G {{ new_group }} {{ username }}
```

### Changer le groupe principal d'affectation d'un utilisateur

```bash
sudo usermod -g {{ new_main_group }} {{ username }}
```

### Contrôle des groupes d’affectation d'un utilisateur

```bash
sudo groups {{ username }}
```

### Modification du répertoire home d'un utilisateur

```
sudo usermod --home /srv/{{ (h|r)## }}-datas/{{ new_home_directory }} {{ username }}
```

Sans création automatique du répertoire **home**.

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

L'option -r permet de supprimer le répertoire **home** de l'utilisateur ainsi que son fichier de mails en attente.

### Supprimer un groupe

```bash
sudo groupdel {{ groupname }}
```

---
[>> Étape suivante : Montage des disques](04-disk-mounting.md)