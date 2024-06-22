<!-- Filename: Working_with_git_and_github / Display title: Travailler avec Git et GitHub -->

# Introduction

Cette documentation a vocation de proposer des informations sur la
manière de contribuer au CMS Joomla! via Git et GitHub. Si vous
souhaitez faire une simple modification (un seul fichier), il est
préférable de consulter cet article : [Utiliser l'interface GitHub pour
créer des Pull
Requests](https://docs.joomla.org/Using_the_Github_UI_to_Make_Pull_Requests "Special:MyLanguage/Using the Github UI to Make Pull Requests").
Si vous souhaitez réaliser des modifications plus complexes ou si vous
êtes juste curieux, continuez ici votre lecture !

## Git et GitHub c'est quoi ?

Git est un système de contrôle et de distribution de version. C'est un
système qui enregistre les modifications dans les fichiers et conserve
ces changements dans un fichier historique. Vous pouvez toujours
consulter en arrière dans une version antérieure de votre code et
restaurer les modifications si vous le souhaitez. En raison de
l'historique des archives, Git est très utile lorsque vous travaillez à
plusieurs, ensemble sur le même projet.

Voici comment utiliser GitHub.
<a href="https://www.github.com" class="external text" target="_blank"
rel="nofollow noreferrer noopener">GitHub</a> est un site Web qui permet
de gérer les projets Git de manière visuelle. En tant que propriétaire
d'un projet, vous pouvez modifier le code et comparer différentes
versions. En tant qu'utilisateur du projet, vous pouvez contribuer en
faisant une demande d'extraction. Une demande d'extraction est une
demande adressée au propriétaire pour extraire du code dans le projet.
Vous proposez un morceau de code (peut-être une solution à un bogue) et
demandez si le propriétaire du projet souhaite l'utiliser. Si le
propriétaire l’aime, il peut le fusionner (ajouter) à son projet.

Joomla! utilise GitHub et Git pour maintenir son code. Tout le monde
peut contribuer au logiciel Joomla!. L'URL du projet Joomla! CMS sur
GitHub est:
<a href="https://github.com/joomla/joomla-cms" class="external free"
target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms</a>

# Bien débuter

## Inscrivez-vous à GitHub et installer Git

Tout d'abord, vous devrez vous enregistrer sur
<a href="http://www.github.com" class="external text" target="_blank"
rel="nofollow noreferrer noopener">GitHub</a>. C'est gratuit et facile à
faire. Suivez simplement les étapes fournies.

Une fois que nous sommes inscrits, nous devons installer Git. La
dernière version de Git est disponible sur
<a href="http://git-scm.com" class="external free" target="_blank"
rel="nofollow noreferrer noopener">http://git-scm.com</a>. Téléchargez
et ouvrez le programme d'installation. Git est un programme CLI (Command
Line Interface ou Ligne de Commande). Au début, cela peut être déroutant
et un peu effrayant, mais ce document vous guidera tout au long du
processus.

Si vous n'êtes pas un utilisateur expérimenté, lancez simplement le
programme d'installation et appuyez sur les boutons "Suivant" jusqu'à ce
que le programme soit installé. Git n'endommagera pas votre système.
Plus tard, vous pourrez le supprimer comme n'importe quel autre
programme si vous le souhaitez.

Une fois que nous avons installé Git, nous ouvrons le programme appelé
"Git Bash". Une ligne de commande sera ouverte. Nous allons dire à Git
notre nom et notre adresse email. Git utilisera ces informations lorsque
nous contribuerons à un projet. Avec les commandes suivantes, nous
donnons cette information à Git:

    git config --global user.name "Your name"
    git config --global user.email youremail@mail.com

Dans les commandes ci-dessus et dans toutes les autres commandes données
dans cette documentation, chaque ligne est une nouvelle commande. Donc,
vous tapez la première ligne, appuyez sur Entrée, puis tapez la deuxième
ligne et appuyez sur Entrée.

Nous sommes maintenant prêts à utiliser Git et à aller plus loin dans la
configuration de notre installation de test.

# Paramétrage d'une installation de test

Pour notre installation de test, nous avons besoin d’un programme de
serveur Web afin d’installer et d’exécuter Joomla! sur notre ordinateur.
Beaucoup de programmes peuvent le faire, tels que
<a href="https://www.mamp.info/" class="external text" target="_blank"
rel="nofollow noreferrer noopener">MAMP</a> et
<a href="https://www.apachefriends.org/" class="external text"
target="_blank" rel="nofollow noreferrer noopener">XAMPP</a>.
Télécharger une distribution de l'un d'eux.

Après l'installation d'un tel programme (j'utilise MAMP dans cette
documentation), nous allons installer la dernière version de Joomla!.
Dans notre cas, la dernière version de Joomla! n'est pas la dernière
version stable. La dernière version de Joomla! est la branche de mise en
scène sur GitHub. Tout d’abord, laissez-moi vous en expliquer un peu
plus sur GitHub.

## Fork et Cloner Joomla!

Sur GitHub, vous pouvez trouver des projets, appelés Repositories ou
dépôts. Dans un projet, vous pouvez trouver plusieurs versions. Une
telle version s'appelle une branche. Joomla! a les branches suivantes:

- **Staging:** Cette branche contient les dernières corrections de bugs
  et les nouvelles fonctionnalités de Joomla!
- **Master:** Cette branche est la version stable actuelle de Joomla!
- **3.5-dev** Cette branche contient les fichiers pour Joomla! 3.5, qui
  n'est pas stable au moment d'écrire ces lignes.

Sur notre site de test, nous allons utiliser la branche **Staging** mais
si nous devons utiliser cette branche directement, nous avons un
problème. Nous ne pouvons pas modifier cette branche car nous n'en
sommes pas les propriétaires. Nous allons en faire une copie. Sur
GitHub, cela s'appelle Fork ou une fourche. Nous sommes les
propriétaires de cette copie afin de pouvoir la modifier. Après
modification, nous comparons notre fourche avec le projet original.
Ensuite, nous pouvons faire une requête d'extraction pour les
modifications que nous avons apportées. Nous en verrons plus à ce sujet
plus tard. Vous pouvez créer une branche en cliquant sur le bouton Fork
du bouton
<a href="https://github.com/joomla/joomla-cms" class="external text"
target="_blank" rel="nofollow noreferrer noopener">Répertoire Joomla!
Github CMS</a>. Ce bouton est situé en haut à droite de la page.

<img src="https://docs.joomla.org/images/6/6c/Github-fork-button.png"
decoding="async" data-file-width="1002" data-file-height="222"
width="1002" height="222" alt="Github-fork-button.png" />

Après avoir bifurqué, nous allons installer Joomla! sur notre serveur
Web local. Accédez au dossier dans lequel vous pouvez exécuter des
fichiers sur votre serveur Web. La plupart des programmes utilisent un
dossier appelé `htdocs`. Une fois que nous sommes dans ce dossier,
appuyez sur le bouton droit de la souris et cliquez sur: "Git Bash
Here". La ligne de commande s'ouvrira pour cet emplacement.

Tapez la commande suivante pour télécharger les fichiers de votre Fork,
ou fourche, du logiciel Joomla! CMS sur votre ordinateur. Veuillez
remplacer *nom d'utilisateur* par le nom d'utilisateur que vous utilisez
sur GitHub.

    git clone https://github.com/username/joomla-cms.git

Pour toutes les commandes données dans cette documentation, vous devez
ouvrir Git via les étapes décrites ci-dessus. Gardez cela à l'esprit
pour les autres commandes de cette documentation.

Une fois que Git est prêt, ouvrez votre navigateur et accédez à
l'installation sur votre hôte local. En général, l'URL ressemble à
quelque chose comme:
<a href="http://localhost/joomla-cms" class="external free"
target="_blank"
rel="nofollow noreferrer noopener"><code>http://localhost/joomla-cms</code></a>.
Vous verrez maintenant le fichier Joomla! processus d'installation.

## Installer Joomla!

L'installation de Joomla! pour notre test local est presque identique à
une installation normale. Il y a deux petites différences.

Pour les paramètres de la base de données, le mot de passe et le nom
d'utilisateur sont par défaut. Le plus souvent, le nom d'utilisateur est
`root` et le mot de passe est également `root` ou il n'y a pas de mot de
passe. Si vous ne parvenez toujours pas à vous connecter à la base de
données, recherchez dans le manuel de votre serveur Web local le nom
d'utilisateur et le mot de passe.

La dernière différence est la dernière étape de l'installation.
Normalement, nous devons supprimer le dossier d'installation pour aller
plus loin dans l'administration ou le site de Joomla!. Pour une
installation test, nous pouvons ignorer cette partie et aller
directement à l'administration ou au site. Ne supprimez pas le dossier
d'installation. Cela peut être très utile lorsque nous devons
réinstaller Joomla.

# Faites vos modifications

Il est maintenant temps de modifier notre fichier dans Joomla!. Toutes
les modifications que nous apportons seront enregistrées et surveillées
par Git. A tout moment, vous pouvez taper la commande `git status` pour
voir quels fichiers sont modifiés ou non suivis. Untracked signifie que
le fichier à cet emplacement est nouveau pour Git.

Si vous avez commis une erreur ou si vous souhaitez restaurer un
fichier, utilisez cette commande:

    git checkout path/to/file

Si vous souhaitez supprimer toutes les modifications que vous avez
apportées, utilisez les commandes suivantes:

    git checkout .
    git clean -f -d

La première commande réinitialise tous les fichiers. La deuxième
commande supprime les fichiers et les dossiers non suivis.

## Publier nos modifications sur GitHub

# Poussez le changement dans la Fourche

Après avoir effectué nos modifications, nous devons les télécharger dans
notre répertoire sur GitHub. Après cela, nous pouvons faire une demande
de tirage avec nos modifications.

Le téléchargement des modifications s'appelle `push` ou une poussée en
termes Git. Avant de pouvoir le faire, nous devons faire quelque chose
de très important. Nous devons créer une nouvelle branche pour nos
changements. (Une branche est une version de notre projet, vous en
souvenez-vous?) Si nous ne le faisons pas et que nous apportons notre
modification directement à la branche intermédiaire, la première fois,
il n'y aura pas de problème. Mais si nous apportons des modifications
pour la deuxième fois et que les modifications apportées la première
fois ne sont pas encore fusionnées, toutes ces modifications seront
également enregistrées en tant que nouvelles modifications.

Ainsi, la première commande que nous allons exécuter créera une nouvelle
branche. Cela évitera le problème décrit ci-dessus. Remplacez
name-new-branch par le nom de la nouvelle branche. Ce nom doit être
court et ne peut contenir que des lettres minuscules et des chiffres. Ne
**PAS** utiliser des espaces. Au lieu d'espaces, utilisez le - (moins).

    git checkout -b name-new-branch

La commande suivante indique à git que toutes les modifications
apportées sont correctes et prêtes à être validées.

    git add --all

La commande suivante ajoute notre modification à la branche. Veuillez
remplacer le message par une brève description de vos modifications.
Cette description sera le titre de la demande de tirage que nous allons
faire.

    git commit -m "description"

La dernière commande va pousser (uploader) les modifications sur notre
fourche. Veuillez remplacer name-new-branch par le nom de la branche
dans laquelle vous avez effectué les étapes ci-dessus.

    git push origin name-new-branch

## Comparer les Fourches et faire une Demande de Tirage

Après avoir poussé notre changement vers GitHub, allez à la fourche de
votre Joomla! CMS.

# Informations complémentaires

Parce que la version mise en scène de Joomla! peut changer à tout
moment, il est très utile d’avoir la possibilité de garder notre fourche
à jour. Nous pouvons le faire en ajoutant une commande à distance à
notre bifurcation du projet:

    git remote add upstream https://github.com/joomla/joomla-cms.git

Nous avons maintenant ajouté un appel à distance "upstream". Avec la
commande suivante, Git recherchera une nouvelle contribution (commits)
dans la branche de préparation que nous n'avons pas dans notre fourche.
S'il en a trouvé, il les ajoutera à notre fourche:

    git pull upstream staging

Les modifications ne sont maintenant effectuées que sur notre fourche
locale. Pour les télécharger sur GitHub, utilisez la commande suivante:

    git push
