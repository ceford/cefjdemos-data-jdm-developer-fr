<!-- Filename: J4.x:Setting_Up_Your_Local_Environment / Display title: Configuration d'un environnement local -->

## Guide de Démarrage Rapide

Pour configurer un test, vous aurez besoin d'une pile logicielle comprenant Apache, MySQL et PHP. Les étapes requises sont couvertes ailleurs dans ce manuel. En cas de doute, utilisez une pile logicielle adaptée à votre plateforme.

## Outils supplémentaires

Pour tester les demandes de tirage de Joomla et contribuer au code principal de Joomla, vous aurez besoin d'outils supplémentaires, tous facilement installables, souvent avec une commande en une ligne :

1. **Composer** - pour gérer les dépendances PHP de Joomla. Lisez la [documentation de Composer](https://getcomposer.org/doc/00-intro.md) pour obtenir de l'aide avec l'installation.
2. **Node.js** - pour compiler les fichiers JavaScript et SASS de Joomla. Lisez la [documentation de Node.js](https://nodejs.org/en/) pour obtenir de l'aide avec l'installation.
3. **Git** - pour la gestion des versions. Lisez la [documentation de Git](https://git-scm.com/) pour obtenir de l'aide avec l'installation.

## Étapes pour configurer un site de test local

1. Allez sur le [référentiel Joomla! sur GitHub](https://github.com/joomla/joomla-cms).
2. Sélectionnez la branche Joomla sur laquelle travailler. Cela sélectionne les instructions README appropriées.
3. Faites défiler la page jusqu'à la section **Étapes pour configurer l'environnement local :** du texte README.
4. Suivez les étapes énumérées là-bas. Vous pouvez changer le nom du dossier joomla-cms cloné pour faire autant de clones que vous le souhaitez pour différents usages.
5. Créez une base de données et installez Joomla, mais ne supprimez pas le dossier d'installation à la fin du processus d'installation.

## Mise à jour de Joomla

De temps en temps, vous pourriez avoir besoin de mettre à jour un clone de Joomla. C'est une commande en une ligne qui est généralement rapide. Cependant, il est souvent nécessaire de reconstruire les fichiers CSS et JavaScript, ce qui est un processus plus complexe et plus long.

Les utilisateurs de Linux et OSX peuvent configurer l'alias bash suivant en plaçant ce qui suit dans le *fichier ~/.bashrc*:

```sh
    alias jclean="rm -rf administrator/templates/atum/css; \
    rm -rf templates/cassiopeia/css; \
    rm -rf administrator/templates/system/css; \
    rm -rf templates/system/css; \
    rm -rf media/; \
    rm -rf node_modules/; \
    rm -rf libraries/vendor/; \
    rm -f administrator/cache/autoload_psr4.php; \
    rm -rf installation/template/css"
    alias jinstall="jclean; composer install; npm ci"
```

Cela supprimera tous les fichiers compilés et exécutera une nouvelle installation en une seule commande en appelant `jinstall` à l'intérieur de votre installation Joomla. Vous pouvez également utiliser la commande `jclean` pour revenir à une autre branche de Joomla.

**Remarque :** Vous devrez peut-être exécuter `composer install` avec l'option `--ignore-platform-reqs` pour ignorer les exigences de plateforme spécifiées dans Composer. C'est-à-dire, si vous n'avez pas l'extension LDAP de PHP installée.

### Modifications de la base de données

Si une mise à jour de Joomla inclut des modifications de la base de données, vous devrez peut-être trouver les modifications individuelles et les exécuter manuellement ou recommencer avec un nouveau clone.

## Scripts Node/npm

L'installation de Joomla à partir d'un clone de dépôt utilise deux commandes :

- **composer install** Installez tous les paquets composer nécessaires.
- **npm ci** Installez tous les paquets npm nécessaires.

Node.js est fourni avec un gestionnaire de paquets appelé NPM qui possède une commande `run`. Certains scripts sont disponibles pour accélérer le processus de construction si seuls les fichiers CSS ou JavaScript ont été modifiés.

### npm run build:css

Cette commande compile les fichiers SASS en CSS et crée également les fichiers minifiés.

### npm run build:js

Cette commande compile et transpile les fichiers JavaScript au format correct et crée des fichiers minimisés.

### npm exécuter watch

Cette commande est identique à la commande `build:js` mais elle surveillera les modifications et construira automatiquement les fichiers mis à jour dans le répertoire media. Les fichiers SASS ne sont pas encore inclus.

### npm exécuter lint:js

Cette commande effectue une vérification de la syntaxe sur tous les fichiers JavaScript ES6 selon la norme de code JavaScript. Pour plus d'informations, consultez le [manuel des normes de codage Joomla](https://developer.joomla.org/coding-standards/introduction.html).

### npm exécute le test

Cette commande exécutera une suite de tests JavaScript.

## Problèmes Possibles

Lors de l'exécution de composer install, vous pouvez rencontrer ces erreurs.

```sh
    Problem 1
        - Installation request for joomla/ldap 2.0.0-beta -> satisfiable by joomla/ldap[2.0.0-beta].
        - joomla/ldap 2.0.0-beta requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
    Problem 2
        - Installation request for symfony/ldap v5.1.5 -> satisfiable by symfony/ldap[v5.1.5].
        - symfony/ldap v5.1.5 requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
```

La solution consiste à exécuter la commande `composer install` avec l'option `--ignore-platform-reqs` pour ignorer les exigences de plateforme spécifiées dans Composer. C'est-à-dire, si vous n'avez pas l'extension LDAP de PHP installée.

```sh
    composer install --ignore-platform-reqs
```

Si vous recevez une erreur de connexion, supprimez le fichier `administrator/cache/autoload_psr4.php`.

*Traduit par openai.com*

