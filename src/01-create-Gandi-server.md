# Création d'un serveur chez Gandi

## Création du serveur

1. Connectez-vous à l'interface d'administration Gandi

[Interface de connection Gandi](https://id.gandi.net)

![Gandi login page](../resources/screenshot/01-login.png)

2. Depuis l'interface d'administration rendez-vous dans l'interface CLOUD et cliquez sur le bouton **Créer un serveur**.

![Gandi create button](../resources/screenshot/01-create-button.png)

3. Remplissez le formulaire avec les informations suivante :

- Modèle de serveur : ```Small```
- Centre de données : ```FR SD5``` | ```FR SD6```
- Système d'exploitation : ```Debian 10```
- Configuration de serveur :
    - Nom du serveur : ```hostingserver{{ ## }}``` | ```stagingserver{{ ## }}```
    - Nom du super-utilisateur : ```{{ username }}```
    - Mot de passe : ```{{ password }}``` (au moins 12 caractère, au moins 1 maj, au moins 1 caractère spécial)
    - Ajout d'un clé SSH (recommandé) : ```{{ ~/.ssh/id_rsa.pub }}```

Il sera possible d'ajouter une clé SSH à postériori.

![Gandi create form](../resources/screenshot/01-create-form.png)

4. Le serveur est ```cours de creation```. Patientez jusqu'à ce que le statut passe à ```actif```.

![Gandi server status in progress](../resources/screenshot/01-status-in_progress.png)

![Gandi server status active](../resources/screenshot/01-status-active.png)

## Configuration du serveur

Depuis la liste des serveurs, cliquez sur le serveur que vous avez créé.

![Gandi configuration](../resources/screenshot/01-configuration.png)

### Coeurs et mémoire

Cliquez sur le lien **modifier** de la section *Coeurs et RAM*.
Adaptez selon le type de serveur et le besoin en ressources. (2x && 2048Mo || 4x && 4096Mo).

![Gandi config core + RAM](../resources/screenshot/01-config-core_RAM.png)

Attendez le redémarrage du serveur avant de poursuivre.

### Disques

> **Important** La scalabilité (redimensionnement) des disques chez Gandi, permet, une fois créé d'augmenter sa taille mais pas de la réduire.
>
> Prenez en compte cette contrainte lors de la création des disques de données et de sauvegarde.

Le renommage du disque système nécessite l'arrêt du serveur.

![Gandi config server shutdown button](../resources/screenshot/01-config-shutdown_button.png)

![Gandi config server shutdown in progress](../resources/screenshot/01-config-shutdown_progress.png)

![Gandi config server halted](../resources/screenshot/01-config-halted.png)

Accèder à l'interface **Gérer les volumes**.

![Gandi config manage volumes](../resources/screenshot/01-config-manage_volumes.png)

#### Modifiez le nom du disque système

1. Cliquez sur le lien de modification du disque système.

![Gandi config volume system edit](../resources/screenshot/01-config-volume_sys_edit.png)

2. Cliquez sur le bouton **Modifier**

![Gandi config volume system modify](../resources/screenshot/01-config-volume_sys_modify.png)

3. Adaptez le nom du disque

![Gandi config volume system rename](../resources/screenshot/01-config-volume_sys_rename.png)

### Créez le disque de données

1. Cliquez sur la liste **Ajouter** et choisissez l'option *Créer un nouveau volume*.

![Gandi config volume datas add](../resources/screenshot/01-config-volume_datas_add.png)

2. Renseignez les attributs du disque.

Nom: ```{{ (h|r)## }}-datas```
Attacher au serveur: ```{{ (hosting|staging)## }}```
Localisation: FR SD5| FR SD6 (Doit être dans le même centre de données que votre serveur)
Taille: ```{{ [20-50] }}```Go

![Gandi config volume datas create](../resources/screenshot/01-config-volume_datas_create.png)

Patientez jusqu'à la création du disque.

![Gandi config volume datas create](../resources/screenshot/01-config-volume_datas_created.png)

### Créez le disque de sauvegarde

Suivre la même procédure que pour la création du disque de données.

![Gandi config volume datas create](../resources/screenshot/01-config-volume_backup_created.png)

## Configuration des DNS chez Gandi

1. Récupérez l'IPv4 et l'IPv6 du serveur depuis l'interface d'administration du serveur

![Gandi config DNS get IP](../resources/screenshot/01-config-DNS_get_IP.png)

2. Rendez-vous dans l'interface d'administration des domaines de Gandi

[Liste des domaines Gandi](https://admin.gandi.net/domain/)

![Gandi config DNS list](../resources/screenshot/01-config-DNS_list.png)

3. Sélectionnez le nom de domaine a éditer

Cliquez sur le domaine a éditer et rendez-vous dans l'onglet **Enregistrement DNS**.

4. Configurer ...

@TODO