# 06f - Installation de libraries annexe

## ImageMagick

```bash
sudo apt install imagemagick
```

Editer les habilitations ImageMagick

```bash
sudo vi /etc/ImageMagick-6/policy.xml
```

```diff
- <policy domain="coder" rights="none" pattern="PDF" />
+ <!-- <policy domain="coder" rights="none" pattern="PDF" /> -->
```

## Composer

### Lancer l'installation

```bash
cd /tmp && \
curl -sS https://getcomposer.org/installer | php && \
sudo mv composer.phar /usr/local/bin/composer.phar
```

Ou suivre la [documentation officielle](https://getcomposer.org/download/)

### Créer un alias pour l'utilisateur système courant

Modifier les paramètres de l'utilisateur système courant :

```bash
vi ~/.bashrc
```

```diff
+# COMPOSER
+alias composer="/usr/local/bin/composer.phar"
```

Valider la modification :

```bash
. ~/.bashrc
```

Tester :

```bash
composer -v
```

## WP-CLI

### Installation

Récupération la version en ligne :

```bash
cd /tmp && \
curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
```

Vérification de version :

```bash
php wp-cli.phar --info
```

Installer :

```bash
chmod +x wp-cli.phar && \
sudo mv wp-cli.phar /usr/local/bin/wp
```

Contrôle d'installation :

```bash
wp --info
```

Ou suivre la [documentation officiel](https://wp-cli.org/#installing)

### Mettre à jour

```bash
sudo wp cli update
```

### Installer l'auto-complétion (Optionnelle)

Récupérer et préparer la source

```bash
cd /tmp && \
curl -O https://raw.githubusercontent.com/wp-cli/wp-cli/v2.4.0/utils/wp-completion.bash && \
chmod +rx wp-completion.bash && \
sudo mv wp-completion.bash /usr/local/bin/wp-completion.bash
```

Créer le fichier .bash_profile (optionnel)

```bash
touch ~/.bash_profile
```

Installation

```bash
source /usr/local/bin/wp-completion.bash && \
source ~/.bash_profile
```

## NodeJS

### Avec Nvm (Recommandé)

Nvm est un gestionnaire de version.

Vérifier le numéro de version courant depuis le [dépôt](https://github.com/nvm-sh/nvm) et lancer l’installation :

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
```

Une fois installé, créer un fichier .bashrc si nécessaire :

```bash
source ~/.bashrc
```

Lister les versions de NodeJS disponible :

```bash
nvm list-remote
```

```bash
    v13.0.1
    v13.1.0
    v13.2.0
    v13.3.0
    v13.4.0
    v13.5.0
```

Installer la version souhaitée :

```bash
nvm install v16.8.0
```

#### Installer une version LTS (optionnel)

Lister :

```bash
nvm list
```

```bash
default -> v16.8.0
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v16.8.0) (default)
stable -> 16.8 (-> v16.8.0) (default)
lts/* -> lts/fermium (-> v14.17.6)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.24.1 (-> N/A)
lts/erbium -> v12.22.6 (-> N/A)
lts/fermium -> v14.17.6 (-> N/A)
```

Exemple avec la version fermium :

```bash
nvm install lts/fermium
```

#### Basculer vers une version spécifique par défaut

> Par défaut, c’est la version lts qui est utilisée.

Changer de version :

```bash
nvm use v16.8.0
```

#### Commandes utiles

Version des versions installés :

```bash
echo nvm npm node npx yarn && nvm --version && npm -v && node -v && npx -v && yarn -v
```

Installé la dernière version stable de NodeJS

```bash
nvm install stable
```

### Installation standard

```bash
sudo apt install gpg-agent
```

Récupérer le numéro de la dernière version stable sur le [site officiel](https://nodejs.org/en/) :

```bash
curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
```

```bash
sudo apt update && sudo apt install -y nodejs
```

Vérifier l'installation :

```bash
node -v
```

## NPM

> NodeJs est requis.

```bash
npm install -g npm@latest
```

## NPX

> NodeJs est requis.

```bash
npm install -g npx
```

## Yarn

> NodeJs est requis.

Installer :

```bash
npm install -g yarn
```

Mettre à jour :

```bash
yarn set version latest
```

## Webpack

> NodeJs est requis.

```bash
sudo npm install -g webpack@latest
```

## Webpack-cli

> NodeJs est requis.

```bash
sudo npm install -g webpack-cli@latest
```