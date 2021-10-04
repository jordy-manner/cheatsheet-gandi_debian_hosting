# Installation du serveur LAMP

LAMP est un accronyme utilisé pour signifier l'ensemble des technologies couplées (Linux Apache MySQL PHP) et utilisées pour rendre opérant l'hébergement de sites internet.

## Ajout des dépôts des paquets d'installation Apache et PHP

Les paquets PHP7.4, PHP8.0 et PHP8.1 ne sont pas disponibles sur les serveurs miroirs officiels de Debian 10.
Il faut donc ajouter **sury** à la liste des dépôts des paquets d'installation déclarés.

```bash
sudo apt install -y lsb-release apt-transport-https ca-certificates wget && \
sudo wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg && \
sudo echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" |sudo tee /etc/apt/sources.list.d/php.list
```
