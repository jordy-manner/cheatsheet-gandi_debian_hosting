# Prérequis

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

**Définition de variables :** {{ var }}

**Prefixes de variables :**

- h : serveur de production
- r : serveur de recette

**Numéro de serveur :** ##

### Nommage serveurs

- Production : hostingserver{{ ## }}
- Recette : stagingserver{{ ## }}

### Nommage des disques

- Système : {{ (h|r)## }}-sys
- Données : {{ (h|r)## }}-datas
- Sauvegarde : {{ (h|r)## }}-backup