# 00 - Prérequis

- OS : Debian
- Disque système : 20go
- Disque de données : 10go
- Disque de backup : 20go

## Configuration Serveur dédié

- Coeur : x2
- RAM : 2048Mo

## Configuration Serveur mutualisé

- Coeur : x4
- RAM : 4096Mo

## Nomenclature

### Variables

**Définition de variables :** ```{{ var }}```

**Numéro de serveur :** ```{{ ## }}```

**Type de server :**

- h : serveur de production
- r : serveur de recette

Exemple d'usage : ```{{ (h|r)## }}```

### Nommage serveurs

- Production : hosting```{{ ## }}```
- Recette : staging```{{ ## }}```

### Nommage des disques

- Système : ```{{ (h|r)## }}```-sys
- Données : ```{{ (h|r)## }}```-datas
- Sauvegarde : ```{{ (h|r)## }}```-backup

---
[>> Étape suivante : Création du serveur chez Gandi](01-create-Gandi-server.md)