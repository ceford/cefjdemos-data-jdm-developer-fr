<!-- Filename: J4.x:Developer:_Required_Software / Display title: Logiciel requis -->

## Introduction

Ce tutoriel s'adresse aux nouveaux venus dans le développement de code Joomla qui souhaitent préparer des extensions sur un ordinateur local. Peu importe que vous utilisiez Windows, Mac ou Linux. Tous les logiciels requis sont disponibles pour toutes ces plateformes. Pour commencer sur votre ordinateur portable ou de bureau, qui a généralement le nom de domaine *localhost*, vous devrez installer un ensemble standard d'éléments logiciels distincts.

- **Apache**, un serveur web. D'autres serveurs web sont disponibles mais les nouveaux venus devraient s'en tenir à Apache.
- **MySQL** ou **MariaDB**, serveurs de bases de données. PostgreSQL est également pris en charge mais n'est pas destiné aux débutants.
- **PHP**, la dernière version recommandée par Joomla ou la version minimale pour votre plateforme.
- **phpMyAdmin**, un outil utilisé pour gérer les bases de données MySQL et MariaDB.
- **xDebug**, une extension de PHP utilisée pour le débogage.
- Un IDE tel que **VSCode**. D'autres sont disponibles et couverts dans un article séparé.

## Empilements de logiciels

Les quatre premiers éléments de cette liste sont souvent appelés une pile et peuvent être nommés LAMP, MAMP ou WAMP, où les lettres de l'acronyme signifient les éléments suivants :

- **L, M ou W** plateforme. L pour Linux, M pour Mac et W pour Windows.
- **A : Apache** serveur web.
- **M : MySQL ou MariaDB** base de données. Les deux sont interchangeables.
- **P : PHP** langage de script. Un langage de script largement utilisé. Il n'y a pas d'alternative car Joomla est codé en PHP.

## Piles Emballées

Une bonne façon de commencer est d'utiliser un package qui combine le logiciel essentiel :

- **WAMP** pour Windows est gratuit sur le site [Wampserver](https://www.wampserver.com/en/).
- **Bearsampp** pour Windows est gratuit sur le site [Bearsampp](https://bearsampp.com/). Il contient plus d'outils.
- **XAMPP** Pour Windows, Mac et Linux est gratuit sur le site [Apache Friends](https://www.apachefriends.org/). Il y a un tutoriel local pour [XAMPP](jdocmanual?article=user/hosting/local-hosting-with-xampp).
- **MAMP** pour Mac et Windows est disponible en versions gratuite et commerciale sur le site [MAMP](https://www.mamp.info/en/mac/).

## Pas de Piles

Si vous avez un ordinateur Linux ou Macintosh, vous constaterez que vous pouvez installer chacun des logiciels requis indépendamment depuis les dépôts distants qui prennent en charge votre système d'exploitation. Il est possible qu'ils soient déjà installés et prêts à l'emploi. Pour vérifier, ouvrez votre navigateur web préféré et entrez **localhost** dans la barre d’url. Vous verrez une page de remplissage ou une page d'erreur de connexion.

## Répertoire racine des documents du serveur web

Lors de l'installation, votre serveur Apache aura défini un répertoire racine par défaut pour les documents. L'emplacement varie d'une plateforme à l'autre et vous devez soit savoir où il se trouve, soit créer un hôte virtuel pour le placer où vous le souhaitez. Exemples d'emplacements par défaut :

- Mac OS : "/Library/WebServer/Documents"
- Linux : /var/www/html
- Windows : ...

Pour éviter des problèmes ultérieurs avec les permissions de fichiers, il est souvent pratique de créer un hôte virtuel pour pointer vers le répertoire public_html de votre propre espace de fichiers. Cela pourrait être /home/nomdutilisateur/public_html sur Linux ou /Users/nomdutilisateur/Sites sur Mac.

Ceci est un exemple d'entrée de machine virtuelle Mac dans le fichier /etc/apache2/vhosts/localhost.conf :

```bash
<VirtualHost *:80>
        DocumentRoot "/Users/username/Sites"
        ServerName localhost
        ErrorLog "/private/var/log/apache2/localhost-error_log"
        CustomLog "/private/var/log/apache2/localhost-access_log" common
        <Directory "/Users/username/Sites">
            AllowOverride All
            Require all granted
        </Directory>
</VirtualHost>
```

Alternativement, vous pouvez créer un lien symbolique du répertoire racine par défaut vers le dossier public_html de votre espace de fichiers personnel.

*Traduit par openai.com*

