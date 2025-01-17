<!-- Filename: Working_with_git_and_github / Display title: Travailler avec git et github -->

## Introduction

Ce document fournira des informations sur la contribution au CMS Joomla! avec l'aide de Git et GitHub. Si vous souhaitez effectuer un changement simple (un seul fichier), il est plus facile d'utiliser cette documentation : [Utilisation de l'interface utilisateur de Github pour créer des Pull Requests](https://docs.joomla.org/Using_the_Github_UI_to_Make_Pull_Requests). Si vous souhaitez ajouter des modifications plus complexes ou si cela vous intéresse simplement, continuez à lire !

## Qu'est-ce que Git et GitHub ?

Git est un système de contrôle de version distribué. C'est un système qui enregistre les modifications dans les fichiers et conserve ces modifications dans un fichier d'historique. Vous pouvez toujours revenir à une version antérieure de votre code et restaurer les modifications si vous le souhaitez. Grâce à l'archive historique, Git est très utile lorsque vous travaillez avec de nombreuses personnes sur le même projet.

Voici comment GitHub peut être utilisé. [GitHub](https://www.github.com) est un site web qui aide à gérer des projets Git de manière visuelle. En tant que propriétaire d'un projet, vous pouvez modifier le code et comparer différentes versions. En tant qu'utilisateur du projet, vous pouvez contribuer en faisant des demandes de fusion (Pull Requests). Une demande de fusion est une requête adressée au propriétaire pour intégrer du code dans le projet. Vous proposez un extrait de code (peut-être une solution pour un bug) et demandez si le propriétaire du projet souhaite l'utiliser. Si le propriétaire l'apprécie, il peut le fusionner (l'ajouter) à son projet.

Joomla! utilise GitHub et Git pour maintenir son code. Tout le monde peut contribuer au logiciel Joomla! via son [dépôt GitHub](https://github.com/joomla/joomla-cms).

## Inscrivez-vous sur GitHub et installez Git

Tout d'abord, vous devrez [vous inscrire sur Github](http://www.github.com). C'est gratuit et facile à faire. Il suffit de suivre les étapes fournies.

Une fois inscrit, vous devez installer Git sur l'ordinateur que vous utilisez pour le développement (station de travail ou ordinateur portable). Suivez les instructions d'installation sur le site [git](http://git-scm.com). Git est un programme CLI (Interface en Ligne de Commande). Au début, cela peut être déroutant et un peu effrayant, mais ce document vous guidera à travers le processus.

Si vous n'êtes pas un utilisateur avancé, il vous suffit de lancer l'installateur et d'appuyer sur les boutons "suivant" jusqu'à ce que le programme soit installé. Git ne nuira pas à votre système. Plus tard, vous pourrez le supprimer comme n'importe quel autre programme.

Avec Git installé, ouvrez une application Terminal. Commencez par définir votre nom et adresse e-mail Git. Git utilisera ces informations lorsque vous contribuerez à un projet :

```sh
    git config --global user.name "Your name"
    git config --global user.email youremail@mail.com
```

Dans les commandes ci-dessus, et toutes les autres commandes données dans cette documentation, chaque ligne est une nouvelle commande. Vous tapez donc la première ligne, appuyez sur entrer, puis tapez la seconde ligne et appuyez sur entrer.

Vous êtes maintenant prêt à utiliser Git et à aller plus loin avec la configuration de votre installation de test.

## Configuration d'une installation de test

Pour votre installation de test, vous avez besoin d'une pile logicielle afin de pouvoir installer et exécuter Joomla! sur votre ordinateur. Les piles telles que MAMP, XAMP ou WAMP sont couvertes ailleurs dans cette documentation.

Après l'installation de votre pile logicielle, vous devez installer la dernière version de Joomla!. Ce n'est pas la dernière version stable. La dernière version de Joomla! est la branche de développement sur GitHub.

## Forker et cloner Joomla !

Sur GitHub, vous pouvez trouver des projets dans ce qu'on appelle des Dépôts. À l'intérieur d'un projet, vous pouvez trouver plusieurs versions. Une telle version est appelée une Branche. Joomla! a les branches suivantes :

- **4.2-dev** Fichiers pour le développement de la version actuelle.
- **4.3-dev** Fichiers pour le développement de la prochaine version mineure.
- **4.4-dev** Fichiers pour le développement de l'avant-dernière version mineure.
- **5.0-dev** Fichiers pour le développement de la prochaine version majeure.

Sur votre ordinateur de test, vous allez utiliser la branche **4.2-dev**. Cependant, vous ne pouvez pas modifier cette branche car vous n'en êtes pas le propriétaire. Vous devez en faire une copie. Sur GitHub, cela s'appelle un Fork. Vous êtes le propriétaire de cette copie, donc vous pouvez la modifier. Après avoir modifié votre fork, vous pouvez faire une Pull Request pour les modifications que vous avez apportées. Plus d'informations à ce sujet plus tard. Vous pouvez fork une branche en appuyant sur le bouton Fork dans le [dépôt GitHub de Joomla! CMS](https://github.com/joomla/joomla-cms). Ce bouton est situé en haut à droite de la page.

![Fork joomla in github](../../../en/images/getting-started/core-git-fork-joomla.png)

Après le fork, vous devez installer Joomla! sur votre ordinateur local. Allez dans le dossier où vous pouvez placer les fichiers utilisés par votre serveur Web. De nombreux programmes utilisent un dossier appelé `htdocs`. Certains utilisent `www` et d'autres utilisent entièrement d'autres dossiers. Tout dépend de si vous utilisez Windows, Mac ou Linux. Finalement, votre racine Web contiendra différents dossiers pour différents sites Web. Une fois que vous êtes dans votre dossier racine web, soit vous utilisez la commande cd dans une fenêtre de terminal ouverte pour changer le répertoire courant vers la racine web, soit, dans votre explorateur de fichiers GUI, trouvez le dossier racine web, appuyez sur le bouton droit de la souris et cliquez sur : "Git Bash Here" ou "Ouvrir le Terminal" ou quelque chose de similaire.

Dans le Terminal, avec le dossier racine du site défini comme le dossier actuel, tapez la commande suivante pour télécharger les fichiers de votre Fork du CMS Joomla! sur votre ordinateur. Veuillez remplacer *username* par le nom d'utilisateur que vous utilisez sur GitHub.

```sh
    git clone https://github.com/username/joomla-cms.git
```

Le processus de clonage peut prendre un certain temps. Une fois terminé, votre répertoire racine web contiendra un dossier nommé joomla-cms. Vous devez faire de ce dossier le dossier courant pour exécuter les commandes git pour ce dossier :

```sh
    cd joomla-cms
```

Veuillez vous rappeler que pour d'autres commandes dans cette documentation.

## Installer Joomla !

Votre clone téléchargé de votre dépôt forké nécessite une action supplémentaire avant d'être prêt à l'utilisation. L'un des fichiers téléchargés s'appelle README.md. Ouvrez-le avec un éditeur de texte et suivez les instructions dans **Comment obtenir une installation fonctionnelle à partir du code source**.

Lorsque vous êtes prêt, ouvrez votre navigateur et accédez à l'installation sur votre localhost. En général, l'URL est quelque chose comme :
`http://localhost/joomla-cms`. Vous verrez maintenant le processus d'installation par défaut de Joomla !

## Apporter des modifications au code

Toutes les modifications que vous apportez au code Joomla sur votre site local seront enregistrées et surveillées par Git. À tout moment, vous pouvez taper la commande `git status` pour voir quels fichiers sont modifiés ou non suivis. Non suivi signifie que le fichier à cet emplacement est nouveau pour Git.

À ce stade, il est probablement préférable d'utiliser un Environnement de Développement Intégré (IDE) pour travailler sur le code Joomla. Visual Studio Code (VSCode) est fortement recommandé. Il est gratuit et fonctionne sur toutes les plateformes. En utilisant VSCode, vous pouvez apporter des modifications, les **committer** à votre clone local de Git et les **pusher** vers votre fork distant sur GitHub.

## Pousser les modifications vers le fork

Le téléchargement des modifications s'appelle `push` dans le jargon de Git. Avant de pouvoir le faire, il est nécessaire de réaliser une opération très importante. Vous devez créer une nouvelle branche pour vos modifications. (Une branche est une version de votre projet, vous vous souvenez ?) Si vous ne le faites pas et effectuez votre changement directement dans la branche actuelle, la première fois, cela ne posera pas de problème. Mais si vous apportez des modifications une seconde fois, et que les modifications que vous avez faites la première fois ne sont pas encore fusionnées, toutes ces modifications seront également enregistrées comme de nouvelles modifications.

Vous pouvez configurer VSCode pour exécuter toutes les commandes suivantes en quelques clics. Mais si vous souhaitez utiliser les commandes git depuis la ligne de commande du terminal :

Ainsi, la première commande que vous allez exécuter créera une nouvelle branche. Remplacez name-new-branch par le nom de la nouvelle branche. Ce nom doit être court, et ne peut contenir que des lettres minuscules et des chiffres. Ne **PAS** utiliser d'espaces. Au lieu des espaces, utilisez - (moins).

```sh
    git checkout -b name-new-branch
```

La commande suivante indique à git que toutes les modifications sont prêtes à être validées.

```sh
    git add --all
```

La commande suivante ajoute votre modification à la branche. Veuillez remplacer le message par une brève description de vos modifications. Cette description sera le titre de la demande de tirage que vous allez faire.

```sh
    git commit -m "description"
```

La commande finale poussera (téléversera) les changements vers votre fork. Veuillez remplacer nom-nouvelle-branche par le nom de la branche que vous avez créée quelques étapes plus haut.

```sh
    git push origin name-new-branch
```

## Comparer les forks et faire une demande de tirage

Après avoir poussé votre modification sur GitHub, rendez-vous sur votre fork de Joomla! CMS sur le site GitHub. Sélectionnez votre branche et faites une demande de tirage.

Une fois terminé, dans votre clone local, revenez à la branche d'origine :

```sh
git checkout 4.2-dev
```

Vous pouvez maintenant apporter de nouvelles modifications sans affecter les modifications de votre branche précédente.

## Rester à jour

Étant donné que la version actuelle de Joomla! peut changer chaque jour, il est important de maintenir votre fork à jour. Vous pouvez le faire en ajoutant le dépôt original de Joomla à votre projet forké :

```sh
    git remote add upstream https://github.com/joomla/joomla-cms.git
```

Ensuite, mettez à jour votre clone local avec la commande suivante :

```sh
    git pull upstream 4.2-dev
```

Et mettez à jour votre fork distant :

```sh
    git push
```

*Traduit par openai.com*

