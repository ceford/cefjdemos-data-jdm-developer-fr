<!-- Filename: J4.x:Setting_Up_Your_Local_Environment / Display title: Paramétrer votre environnement local -->

<span id="main-portal-heading">Didacticiel  
Comment paramétrer un environnement local pour Joomla 4</span> Joomla! 
4.x

Avec Joomlaǃ 4, nous avons changé le processus de développement. Il
n'est plus possible de cloner le dépôt et d'avoir une installation
Joomla utilisable. Nous suivons ici les meilleures pratiques et mettons
en place un processus de construction pour le CMS.

## Guide d'installation rapide

Les étapes pour configurer votre environnement de développement
dépendent de votre système d'exploitation. Nous ne pouvons pas écrire de
documentation pour tous les systèmes d'exploitation (OS), donc vous
devrez trouver des manuels adéquats sur votre moteur de recherche
préféré.

### Outils nécessaires

1.  PHP - fondamentalement le même que celui dont vous avez besoin pour
    exécuter un site Joomla, mais vous avez besoin de la version PHP CLI
    (interface en ligne de commande) (voir la page [Configuration d'un
    serveur LAMPP pour le développement
    PHP](https://docs.joomla.org/Configuring_a_LAMPP_server_for_PHP_development "Special:MyLanguage/Configuring a LAMPP server for PHP development")).
2.  Composer - pour gérer les dépendances PHP de Joomla - pour obtenir
    de l'aide à l'installation de Composer, lisez la documentation sur
    <a href="https://getcomposer.org/doc/00-intro.md" class="external free"
    target="_blank"
    rel="nofollow noreferrer noopener">https://getcomposer.org/doc/00-intro.md</a>
3.  Node.js - pour compiler les fichiers JavaScript et SASS de Joomla -
    pour obtenir de l'aide pour installer Node.js, veuillez suivre les
    instructions disponibles sur
    <a href="https://nodejs.org/en/" class="external free" target="_blank"
    rel="nofollow noreferrer noopener">https://nodejs.org/en/</a>
4.  Git - pour la gestion des versions

### Etapes pour paramétrer votre environnement local

1.  Cloner le dépôt
2.  Checkout branch *4.1-dev*
3.  Exécutez `composer install` depuis la racine du dépôt git (vous
    pouvez ajouter --ignore-platform-reqs si vous n'avez pas le PHP-LDAP
    installé localement et que vous n'en avez pas besoin).
4.  Exécutez `npm ci` depuis la racine du dépôt git (à noter que vous
    devez avoir une version de npm supérieure ou égale à 5.7 pour
    effectuer cela -\> exécutez `npm install -g npm@lts` pour mettre à
    jour la version de npm dans la version lts).

Les utilisateurs Linux et OSX peuvent créer l'alias de bash suivant en
ajoutant les lignes suivantes dans le fichier ~/.bashrc :

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

Ceci va supprimer tous les fichiers compilés de votre système et
effectuer une installation propre en une seule commande par appel de
`jinstall` dans l'installation de Joomla!.

## Guide d'installation un peu plus détaillé

Joomla n'est pas différent de beaucoup d'autres outils web de nos jours.
Il a une grande partie PHP et il a de plus en plus de code JavaScript.
Alors que le codage PHP n'a pas besoin d'autant de préparation,
JavaScript a besoin de beaucoup d'outils. La raison principale est que
personne n'écrit le code d'une manière que chaque navigateur comprend,
de sorte que le code doit être transposé, par exemple de ES6 à une
version compatible de JavaScript. La même chose est vraie pour le CSS,
pour Joomla nous utilisons SASS et ceci sera converti en CSS natif pour
que n'importe quel navigateur le comprenne. Comme la mise en place d'un
environnement de développement est un peu plus compliquée, mais
l'outillage rend le codage plus pratique. Grâce aux observateurs et à la
recharge automatique de votre navigateur, vous pouvez voir votre
changement en temps réel.

### PHP

Il devrait suffire d'exécuter `composer install` car cela installera les
dépendances PHP indiquées dans le fichier composer.lock. Vous pouvez le
faire autant de fois que vous le souhaitez, il n'installera de nouveaux
paquets que lorsque le fichier composer.lock sera modifié. N'exécutez
pas `composer update` car cela mettra à jour tous les paquets vers des
versions plus récentes et mettra à jour le fichier composer.lock.

Note : Vous pouvez avoir besoin d'exécuter `composer install` avec
l'option `--ignore-platform-reqs` pour ignorer les exigences de la
plate-forme spécifiées dans Composer, c'est-à-dire si vous n'avez pas
l'extension LDAP de PHP installée.

### Scripts Node/npm

Node.js est livré avec un gestionnaire de paquets appelé NPM (en quelque
sorte le même que Composer). NPM a une commande `run` et nous avons
préparé quelques scripts pour vous faciliter la vie. Vous devez exécuter
les commandes à la racine du répertoire.

#### npm run build:css

Il compilera les fichiers SASS en CSS et créera les fichiers minifiés.

#### npm run build:js

Il compilera et transposera les fichiers JavaScript au bon format et
créera des fichiers minifiés.

#### npm run watch:js

C'est la même chose avec la commande `build:js` mais cela surveillera
les changements et construira automatiquement les fichiers mis à jour
dans le répertoire des contenus.

#### npm run lint:js

Ceci exécutera un contrôle de syntaxe sur tous les fichiers JavaScript
ES6 par rapport aux règles de codage javascript. Pour plus
d'informations sur les règles de codage Joomlaǃ 4, lisez le <a
href="https://developer.joomla.org/coding-standards/introduction.html%7C"
class="external text" target="_blank" rel="noreferrer noopener">manuel
des règles de codage</a>.

#### test d'exécution npm

Ceci va s'exécuter au-dessus de la suite de tests JavaScript.

## Problèmes possibles

When running composer install you can run into these errors

    Problem 1
        - Installation request for joomla/ldap 2.0.0-beta -> satisfiable by joomla/ldap[2.0.0-beta].
        - joomla/ldap 2.0.0-beta requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
    Problem 2
        - Installation request for symfony/ldap v5.1.5 -> satisfiable by symfony/ldap[v5.1.5].
        - symfony/ldap v5.1.5 requires ext-ldap * -> the requested PHP extension ldap is missing from your system.

The solution is to run the composer install with the
`--ignore-platform-reqs` option to ignore platform requirements
specified in Composer. That is, if you do not have PHP's LDAP extension
installed.

    composer install --ignore-platform-reqs

Si vous obtenez une erreur de login telle que celle ci-dessous, alors
supprimez le fichier `\library\autoload_psr4.php` comme montré sur la
seconde image.

<img
src="https://docs.joomla.org/images/thumb/b/b3/Install-error.png/400px-Install-error.png"
class="thumbborder" decoding="async"
srcset="https://docs.joomla.org/images/thumb/b/b3/Install-error.png/600px-Install-error.png 1.5x, https://docs.joomla.org/images/thumb/b/b3/Install-error.png/800px-Install-error.png 2x"
data-file-width="1920" data-file-height="1080" width="400" height="225"
alt="Login After Install Error" />
