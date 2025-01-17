<!-- Filename: Setting_up_Eclipse_PDT_2020_and_Git_for_Pulls / Display title: Configuration d'Eclipse PDT 2020 et Git pour les Pulls -->

## Introduction

J'ai commencé à utiliser Eclipse il y a de nombreuses années pour un projet Joomla privé et un dépôt git privé distant qui n'était pas sur Github. J'ai lu les tutoriels disponibles et je me suis débrouillé joyeusement jusqu'à cette année où j'ai décidé de m'impliquer dans les tests de base et la correction de bugs pour Joomla! 4. Et ensuite quelques pull requests. C'est alors que j'ai réalisé que je n'avais pas exactement compris ce que je faisais. Finalement, j'ai décidé de recommencer et de documenter le processus pour d'autres développeurs semi-expérimentés qui sont nouveaux sur Joomla, en particulier les parties que je n'avais pas complètement comprises la première fois. Premier sur la liste :

## Structure de fichiers

Il n'est pas évident avant de télécharger et d'installer Eclipse qu'il y a au moins deux endroits où les données sont stockées, en plus de l'emplacement où se trouve le code Eclipse.

### L'espace de travail

C'est ici qu'Eclipse stocke ses propres données pour chaque projet. Cela ne devrait pas être dans l'arborescence du web ! Comme je travaille principalement sur un MacBook, l'emplacement par défaut pour moi est **/Users/username/eclipse-workspace**. Il y a un sous-dossier pour chaque projet. Donc, cela pourrait être **/Users/username/eclipse-workspace/joomla-cms**. Il contient un dossier : .metadata. Les utilisateurs de Linux sauront probablement remplacer Users par home.

### Le dépôt local git et le site web de test

C'est ici que le code du projet est stocké. Il pourrait être dans l'arborescence web locale si vous souhaitez l'utiliser pour des tests. Pour moi, c'est **/Users/username/Sites/joomla-cms-4**. Les utilisateurs de Linux sauront probablement remplacer Sites par public_html. Faites attention aux étapes supplémentaires nécessaires pour que le dépôt local fonctionne comme un site Joomla.

### Un site web de test séparé

Ceci est facultatif, mais nécessite que le dépôt git dispose d'un fichier build.xml personnalisé, par exemple build-local.xml, couvert en annexe.

## Logiciel requis

Pour configurer un site web de développement et de test fonctionnel, vous devrez d'abord installer le logiciel suivant :

- Apache
- MySQL ou Maridb
- PHP
- phpMyAdmin - [PhpMyAdmin Page d'accueil](https://www.phpmyadmin.net/)
- Composer - [Composer Page d'accueil](https://getcomposer.org/)
- Node.js - [Node.js Page d'accueil](https://nodejs.org/en/)
- Eclipse PDT - [Eclipse PHP Development Tools](https://www.eclipse.org/pdt/)
- git (optionnel pour les utilisateurs de git en ligne de commande) - [git Page d'accueil](https://git-scm.com/)
- phing (optionnel pour des constructions personnalisées) - [phing Page d'accueil](https://www.phing.info/)

Les trois ou quatre premiers sont souvent disponibles sous forme de pack pour votre plateforme, connu sous le nom de pile LAMP, WAMP ou XAMP. Utilisez simplement ce que votre plateforme offre ou consultez Apache Friends [XAMPP](https://www.apachefriends.org/)

Supposons que vous ayez installé tout sauf Eclipse.

## Forker joomla-cms sur Github

Il y a un flux de travail décrit dans [Ma première demande de tirage sur Joomla! sur Github](https://docs.joomla.org/My_first_pull_request_to_Joomla!_on_Github "Special:MyLanguage/My first pull request to Joomla! on Github") que je ne peux trop louer. Il montre exactement ce que vous devez faire :

![Github work flow](../../../en/images/getting-started/core-work-flow-joomla.png)

Étapes

- Allez sur [Github](https://github.com/) et obtenez un compte, gratuit et rapide.
- Dans votre compte Github, allez dans le dépôt joomla/joomla-cms : tapez joomla/joo... dans la boîte de recherche en haut à gauche et sélectionnez lorsque les options apparaissent.
- Dans le dépôt joomla/joomla-cms, cliquez sur le bouton fork en haut à droite. Cela vous donne une copie forkée de tout le code pour Joomla 4, Joomla 5, ..., tout, dans votre propre compte Github.

Retour à votre poste de travail.

## Installer Eclipse

J'ai déjà deux versions d'Eclipse installées, Oxygen d'il y a quelques années et Cocoa de 2020. À partir de maintenant, j'installe une deuxième instance de Cocoa. Voyons ce qui se passe :

- Allez sur le [site Eclipse](https://www.eclipse.org/pdt/) et téléchargez la version pour votre plateforme.
- Suivez la procédure d'installation et finalement démarrez l'application Eclipse, pour moi **Eclipse 2.app**.
- Avertissement : *Eclipse 2.app* est une application téléchargée depuis internet. Êtes-vous sûr de vouloir l'ouvrir? **Ouvrir**
- Sélectionnez un répertoire comme espace de travail - Par défaut, c'est /Users/nomdutilisateur/eclipse-workspace. **Parcourir** et créez un sous-dossier joomla-cms-4.
- Lancement : Eclipse est installé et affiche la page de bienvenue.
- Vérifiez les paramètres de configuration de l'IDE. Réglez les éléments 1, 2, 5 et 6.
- Sélectionnez l'icône Workbench en haut à droite.

Au lieu d'installer une autre version d'Eclipse, j'aurais pu ouvrir un nouvel espace de travail vide.

Jusqu'ici tout va bien !

## Importer un fork dans Eclipse

Dans votre nouvelle installation locale d'Eclipse :

- Ouvrez Préférences et définissez Équipe / Git / Dossier de dépôt par défaut sur /Users/username/git (vous pouvez ou non utiliser cet emplacement)
- Ouvrez Fichier / Importer / Git / Projets depuis Git (avec importation intelligente). Suivant ...
- Cloner URI. Suivant ...
- Copiez la barre d'URL de **votre fork Github** et collez-la dans le champ URL. Vous n'avez pas besoin d'ajouter de justificatifs d'authentification. Suivant ...
- Branches à importer ... euh ... Probablement mieux d'importer tout. Suivant ...
- Répertoire. C'est ici que vous pourriez choisir un emplacement différent pour conserver le code. J'ai parcouru jusqu'à /Users/username/public_html/joomla-cms-4, en le créant si nécessaire.
- Branche initiale - si vous avez importé toutes les branches. Je travaille sur Joomla 4 donc j'ai sélectionné 4.0-dev. Et j'ai noté que mon clone sera **origin**. Suivant ...
- Gorgée de café. Le clonage prendra quelques minutes.
- Importer la source. La source doit être l'emplacement du code. Terminer.
- Eclipse montre maintenant la racine de l'arborescence du code dans le panneau Explorateur de projets. Cliquez pour voir plus de l'arbre. Remarquez que ce code ne fonctionnera pas encore comme un site Joomla. Entre autres, il n'y a pas de dossier média.

## Ajouter le dépôt joomla-cms original à Eclipse

- Affichez la fenêtre des dépôts Git : sélectionnez Fenêtre / Afficher la vue / Autre
- Sélectionnez Git / Dépôts Git - la fenêtre apparaît en bas de l'écran.
- Développez joomla-cms / remotes / origin - si vous apportez des modifications à votre code et poussez vers origin, c'est ici que ça va.
- Faites un clic droit sur Remotes et sélectionnez Créer un remote...
- Définissez le nom du remote sur **joomla-origin** et sélectionnez **Configurer fetch**.
- Dans Configurer Fetch, sélectionnez **Changer**.
- Dans Sélectionner un URI, collez l'URL du dépôt original joomla-cms : `https://github.com/joomla/`
- Laissez les identifiants vides. Terminer.
- dans Configurer Fetch : **Enregistrer et récupérer**.

## Créer un site fonctionnel

Votre copie du code joomla-cms nécessite plus d'étapes pour être utilisable comme site web.

- Ouvrez un terminal et passez au dossier contenant votre code cloné.
- Exécutez composer install :
  - Les utilisateurs de Linux et OSX peuvent configurer l'alias bash suivant en plaçant ce qui suit à l'intérieur du fichier `~/.bash_profile` ou `~/.zsh` (`\$ source ~/.bash_profile` pour prendre effet immédiatement) :
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
    alias jinstall="jclean; composer install --ignore-platform-reqs; npm ci"
```

- composer n'est pas dans mon chemin, donc j'ai substitué php ~/composer/composer.phar
- jinstall
- qui récupère toutes les dépendances PHP de Joomla, les dépendances Javascript, compile tout le Javascript ES6 et place les fichiers dans leurs emplacements appropriés.
- sirotent un café pendant que les dépendances sont téléchargées et que les fichiers médias sont créés.
- Dans Eclipse, cliquez avec le bouton droit sur la racine du projet et sélectionnez Refresh. Vous verrez que votre code a maintenant un dossier media.
- Si vous êtes intéressé, utilisez un gestionnaire de fichiers pour voir que la racine contient également un fichier nommé .gitignore et le dossier media y est mentionné. Cela signifie qu'il ne sera pas commis à vos dépôts git locaux ou distants forkés.

Vous êtes maintenant prêt pour une installation de Joomla :

- Créez une base de données en utilisant phpMyAdmin (ou la ligne de commande mysql si vous préférez).
- J'ai créé une base de données nommée joomla-cms-4 avec le classement utf8mb4-unicode-ci.
- Créez un nouvel utilisateur : le mien est jcms4 avec un mot de passe aléatoire généré (GAOC26r77bBLkkdA). Vous devez noter le mot de passe pour l'utiliser pendant l'installation. Il se retrouve en texte clair dans le fichier de configuration.
- Accordez tous les privilèges sur la base de données à cet utilisateur - par défaut.
- Dans votre navigateur préféré, rendez-vous à la racine du nouveau site : localhost/joomla-cms-4/

### Sur mon Mac exécutant macOS Catalina

- Configuration de l'environnement incomplète : Plus de détails. Bah - corrigez simplement le problème et vérifiez que la barre d'adresse contient l'URL que vous avez tapée - la mienne avait étrangement ajouté des caractères supplémentaires.
- Sinon, cela mène à un site fonctionnel - ne supprimez pas le dossier d'installation.

### Sur mon poste de travail Linux exécutant Linux Mint 20

Oups ! Quelque chose s'est mal passé !

Le problème est que j'ai mon code Joomla dans mon espace de fichiers personnel pour un accès en lecture/écriture par Eclipse. Le serveur web a également besoin d'un accès en écriture pour écrire des fichiers de configuration, de cache et de journal, mais il fonctionne en tant qu'utilisateur et groupe avec des privilèges limités. Comme je suis sur un réseau domestique privé, j'ai modifié /etc/apache2/apache2.conf pour commenter User \${APACHE_RUN_USER} et Group \${APACHE_RUN_GROUP} et ajouter User monnomutilisateur et Group mongroupenom. Redémarrez apache, puis...

- Il mène à un site fonctionnel - ne supprimez pas le dossier d'installation.

## Révision de Code

En bref :

- Récupérer de joomla-origin pour s'assurer que mon clone local est à jour.
- Équipe / Fusionner / Sélectionner la branche à fusionner - attention - J'ai sélectionné joomla-origin/4.0-dev.
- Pousser vers origin pour s'assurer que mon fork distant est à jour.
- Créer une branche pour quelques modifications de code que je souhaite effectuer.
- Effectuer les modifications de code
- Si les modifications concernent des fichiers source de css ou js (ceux en sass ou es6), aller à une fenêtre de terminal et exécuter à nouveau jinstall.
- TESTEZ votre installation locale pour voir s'il y a des problèmes.
- Valider les modifications de code
- Pousser vers origin pour mettre à jour mon fork distant avec mes modifications.
- Aller sur votre compte personnel sur Github et sélectionner la branche que vous avez créée avec le code mis à jour. Quelque chose d'effrayant : il est dit **Cette branche est 11084 commits en avance, 134 commits en retard par rapport à joomla:staging.** Ai-je fait quelque chose de mal ? Apparemment non !
- Sélectionner le bouton Pull Request. Assurez-vous de sélectionner la bonne branche joomla dans laquelle fusionner. Pour moi, c'est 4.0-dev. Et assurez-vous d'avoir sélectionné votre branche avec le code modifié. Allez-y !
- La demande de tirage doit être testée et approuvée et peut prendre des jours, des semaines ou des mois. Et la modification peut être rejetée !

## Pendant ce temps

De retour à votre poste de travail :

- Revenez à votre branche d'origine, pour moi : Équipe / Basculer vers / 4.0-dev
- Reconstruire : pour moi Projet / Construire le projet
- Voir les fichiers précédemment modifiés être à nouveau copiés sur votre site de travail.
- TEST : votre site de travail est revenu à l'état dans lequel il était avant vos modifications de code.

Vous êtes maintenant prêt à créer une autre branche pour un autre ensemble de modifications de code.

## En Cas de Catastrophe

À un moment donné, mon clone local s'est corrompu d'une manière ou d'une autre et je ne savais pas comment le réparer. J'ai donc supprimé mon clone local et tous ses fichiers associés, vidé la base de données, puis je suis revenu à l'étape **Importer le fork dans Eclipse** ci-dessus. Cela a synchronisé mon clone local avec mon fork distant, y compris toutes les branches que j'avais créées pour les demandes de tirage. La nouvelle installation a fonctionné sans accroc et j'étais content !

## Autres ressources

- [Configurer Eclipse pour le développement Joomla](https://docs.joomla.org/Configuring_Eclipse_for_joomla_development "Special:MyLanguage/Configuring Eclipse for joomla development") (2012-2013) Mais obtenez la dernière version d'Eclipse PDT !
- [Ma première requête de tirage à Joomla! sur Github](https://docs.joomla.org/My_first_pull_request_to_Joomla!_on_Github "Special:MyLanguage/My first pull request to Joomla! on Github") (2011-2014) Bon aperçu, même s'il est légèrement dépassé.
- [Travailler avec git et github](https://docs.joomla.org/Working_with_git_and_github "Special:MyLanguage/Working with git and github") (2011-2015)
- [Configurer votre environnement local](https://docs.joomla.org/J4.x:Setting_Up_Your_Local_Environment "Special:MyLanguage/J4.x:Setting Up Your Local Environment") (2018-2020)
- [Configurer Eclipse et Xdebug](https://docs.joomla.org/Configuring_Eclipse_and_Xdebug "Special:MyLanguage/Configuring Eclipse and Xdebug") (2013) Tout sur le débogage.
- [Travailler avec Git et Eclipse](https://docs.joomla.org/Working_with_Git_and_Eclipse "Special:MyLanguage/Working with Git and Eclipse") (2014) Méthode pour les anciennes éditions d'Eclipse.
- [Exécuter des tests automatisés depuis Eclipse](https://docs.joomla.org/Running_Automated_Tests_from_Eclipse "Special:MyLanguage/Running Automated Tests from Eclipse") (2020) Pour les utilisateurs avancés ?
- [Configurer Xdebug pour le développement PHP/Linux](https://docs.joomla.org/Configuring_Xdebug_for_PHP_development/Linux "Special:MyLanguage/Configuring Xdebug for PHP development/Linux") (2016) Installer et configurer.
- [Configurer Eclipse IDE pour le développement PHP/Linux](https://docs.joomla.org/Configuring_Eclipse_IDE_for_PHP_development/Linux "Special:MyLanguage/Configuring Eclipse IDE for PHP development/Linux") (2019) Installer et configurer pour Linux.
- [Configurer Xdebug pour le développement PHP](https://docs.joomla.org/Configuring_Xdebug_for_PHP_development "Special:MyLanguage/Configuring Xdebug for PHP development") (2014) Étapes pour Linux, Windows et Mac OS X.
- [Webinaire : Utiliser Eclipse pour le développement Joomla!](https://docs.joomla.org/Webinar:_Using_Eclipse_for_Joomla!_Development "Special:MyLanguage/Webinar: Using Eclipse for Joomla! Development") (2014) Ce webinaire vidéo de 45 minutes, enregistré le 30 avril 2009, fournit un aperçu des fonctionnalités d'Eclipse pour le développement Joomla!
- [Configurer votre poste de travail pour le développement Joomla](https://docs.joomla.org/Setting_up_your_workstation_for_Joomla_development "Special:MyLanguage/Setting up your workstation for Joomla development") (2020) Bref aperçu des logiciels requis et des IDE alternatifs.

## Annexe

À un moment donné, j'avais mon dépôt git local en dehors de la racine de mon site web et j'avais besoin de copier toutes les modifications que je faisais vers un site local installé séparément. Cela nécessitait un fichier de construction personnalisé, montré ici à titre de référence :

### Créer un fichier build-local.xml

L'Eclipse clone contient un fichier build.xml, mais il est utilisé pour tester et créer un fichier zip téléchargeable pour une nouvelle installation. Ce que je veux faire, c'est copier toutes les modifications que j'apporte à mon code cloné sur mon site de test sur mon ordinateur portable. Notez que je veux seulement apporter des modifications au code PHP et non au Javascript ou au CSS. Pour ce faire, j'ai créé un fichier séparé nommé build-local.xml à la racine du projet :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="joomla-cms" basedir="." default="main">
    <property file=".project" />

    <property name="joomladir" value="/Users/username/public_html/joomla-cms"  override="true" />

    <property name="srcdir" value="${project.basedir}" override="true" />

    <!-- Fileset for all files -->
    <fileset dir="${srcdir}" id="allfiles">
        <include name="administrator/**" />
        <include name="api/**" />
        <include name="cli/**" />
        <include name="components/**" />
        <include name="images/**" />
        <include name="includes/**" />
        <include name="language/**" />
        <include name="layouts/**" />
        <include name="libraries/**" />
        <include name="modules/**" />
        <include name="plugins/**" />
        <include name="templates/**" />
        <include name="index.php" />

        <exclude name="**/.*" />
    </fileset>

    <!-- ============================================  -->
    <!-- (DEFAULT) Target: main                        -->
    <!-- ============================================  -->
    <target name="main" description="main target">
        <copy todir="${joomladir}">
            <fileset refid="allfiles" />
        </copy>
    </target>
</project>
```

### Ajouter un outil de construction

- Cliquez avec le bouton droit sur la racine du projet et sélectionnez Propriétés.
- Sélectionnez Builders / New / Program / OK.
- Nom : Build local avec phing
- Emplacement : où que phing soit installé. Pour moi, c'est /usr/local/php5/bin/phing même si j'utilise PHP 7.4.
- Répertoire de travail : Parcourir l'espace de travail et sélectionner joomla-cms - s'affiche comme \${workspace_loc:/joomla-cms}
- Arguments : -f build-local.xml
- Appliquer / OK
- Dans Builders : Appliquer et Fermer
- Projet / Construire le projet
- Voir ce qui se passe :

```sh
    Buildfile: /Users/username/git/joomla-cms/build-local.xml
     [property] Loading /Users/username/git/joomla-cms/.project

    joomla-cms > main:

    BUILD FINISHED

    Total time: 0.2121 seconds
```

*Traduit par openai.com*

