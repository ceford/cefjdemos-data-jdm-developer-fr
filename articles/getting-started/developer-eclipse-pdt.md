<!-- Filename: J4.x:Developer:_Eclipse_PDT / Display title: Eclipse PDT -->

## Introduction

En tant que développeur PHP, vous aurez besoin de certains outils pour vous aider dans votre travail. Eclipse est un IDE (environnement de développement intégré) qui peut être utilisé pour toutes sortes de projets dans toutes sortes de langages de programmation. Eclipse PDT (PHP Developer Tools) inclut les extensions nécessaires pour le développement PHP. Certaines des fonctionnalités mentionnées sur sa page d'accueil incluent :

- Surlignage de syntaxe
- Validation de syntaxe
- Assistance de contenu
- Navigation dans le code
- Débogage PHP (Zend Debugger / Xdebug)
- Profilage PHP (Zend Debugger / Xdebug)

Ajoutez à cela la possibilité de copier des fichiers là où ils doivent être sur votre site Web de test local et de créer un fichier zip, vous avez de quoi travailler pendant des années. Apprendre à utiliser Eclipse PDT prend du temps, un jour ou deux, mais cela en vaut la peine. Il existe d'autres IDE et éditeurs de code disponibles !

### Alternatives

**Les IDEs** ont toutes les fonctionnalités nécessaires pour créer des projets PHP. Des exemples multiplateformes connus pour être utilisés par certains développeurs Joomla incluent :

- **JetBrains PhpStorm** Commercial, Multi-plateforme.
- **Apache NetBeans** Gratuit, Open Source, Multi-plateforme.
- **Visual Studio Code** Gratuit, Multi-plateforme.

**Éditeurs PHP** sont bons pour éditer du code mais manquent de certaines des fonctionnalités nécessaires pour les grands projets. Les exemples incluent :

- **Notepad++** Gratuit, uniquement pour Windows.

## Installer Eclipse PDT

Allez sur le site [Eclipse PDT](https://www.eclipse.org/pdt/) et téléchargez la version disponible pour votre plateforme (Linux, Mac ou Windows).

Suivez les instructions d'installation pour votre plateforme.

## L'espace de travail Eclipse

Il y a au moins trois endroits différents où le code de votre projet sera situé :

- Les fichiers **source du projet** sont ceux que vous créez et modifiez vous-même. Ils ne doivent pas être dans votre arbre web. Ils devraient être dans votre espace de fichiers personnel. Exemples, où chaque projet sera dans un sous-dossier séparé :
  - /Users/username/git (Mac)
  - /home/username/git (Linux)
  - ... (Windows)
- L'emplacement de **l'arbre web** dépend de l'installation de votre pile logicielle. Vous pouvez utiliser votre propre espace de fichiers. Exemples :
  - /Users/username/Sites/j4test (Mac)
  - /home/username/public_html/j4test (Linux)
  - ... (Windows).
- **Eclipse Workspace** est l'endroit où Eclipse stocke les informations sur les projets individuels. L'emplacement standard est la racine de votre propre espace de fichiers. Exemples :
  - /Users/username/eclipse-workspace (Mac)
  - /home/username/eclipse-workspace (Linux)
  - ... (Windows).

Prêt à commencer Eclipse ?

## Démarrer Eclipse

Après le démarrage, il vous sera demandé de confirmer divers réglages. À un certain moment, vous devrez choisir un espace de travail. Par défaut, c'est /home/nomdutilisateur/eclipse-workspace mais vous souhaitez créer un sous-dossier pour chaque projet, alors sélectionnez le bouton Parcourir puis le bouton Nouveau dossier. Dans l'exemple ci-dessous, j'ai créé un dossier j4tutorials.

![Choose eclipse workspace location](../../../en/images/getting-started/eclipse-pdt-choose-workspace.png)

Sélectionnez le bouton **Lancer**. Si quelque chose ne va pas, vous pouvez sélectionner Fichier / Changer d'espace de travail ultérieurement et le refaire. Vous pouvez également supprimer les espaces de travail inutilisés plus tard.

Au premier démarrage, un écran de bienvenue vous est présenté. Vous pouvez également accéder à cet écran à partir des éléments de menu **Aide / Bienvenue**.

![Eclipse welcome screen](../../../en/images/getting-started/eclipse-pdt-welcome.png)

Commencez par Examiner les Paramètres de Configuration de l'IDE. Vous pouvez Cocher ou Décochez ceux qui semblent appropriés, ou Décocher ou Décrocher ou passer à autre chose si vous n'êtes pas sûr. Vous pouvez également modifier les paramètres à nouveau plus tard.

## Créer un nouveau projet PHP

Le premier projet à ajouter est le code du site web de test. Vous aurez besoin de cela pour vérifier que vos propres fichiers sont copiés aux bons endroits et pour le débogage ultérieur. Sélectionnez l'élément dans l'écran de bienvenue. Si vous êtes passé à l'espace de travail Eclipse, sélectionnez Créer un projet PHP depuis l'Explorateur de projets. Dans le formulaire Nouveau projet PHP :

- Entrez un nom de projet approprié. Ce doit être un mot court qui apparaîtra dans l'Explorateur de projet.
- Sélectionnez le bouton radio **Créer un projet à l'emplacement existant (à partir d'une source existante)**.
- **Parcourez** jusqu'à l'emplacement de votre site web de test Joomla et sélectionnez-le.
- Sélectionnez **Terminer**.

![Eclipse new php project](../../../en/images/getting-started/eclipse-pdt-new-project.png)

Vos fichiers de site web seront ajoutés à l'Explorateur de Projet. Cela peut prendre une minute ou deux pour qu'Eclipse termine ce processus, car il effectue certaines préparations en arrière-plan. Vous pouvez sélectionner le Projet pour développer le premier niveau de dossier.

Deux fichiers seront ajoutés à votre dossier de site : .buildpath et .project. Comme ils commencent par un point (.), vous risquez de ne pas les voir et ne devez pas vous en inquiéter. Ce sont des fichiers XML contenant des informations utilisées par Eclipse.

## La perspective PHP

La collection de panneaux dans l'écran Eclipse est connue sous le nom de perspective. Les deux plus couramment utilisées sont la Perspective PHP et la Perspective Debug. Vous pouvez basculer entre elles en utilisant les icônes en haut à droite de la barre d'outils. Vous pouvez également utiliser le menu : Fenêtre / Perspective / Ouvrir la perspective / PHP ou Debug.

Les illustrations suivantes montrent la perspective PHP avec quelques formulaires d'édition ouverts.

![Eclipse php perspective](../../../en/images/getting-started/eclipse-pdt-php-perspective.png)

Les panneaux Eclipse :

- Le panneau de gauche est l'Explorateur de Projet où vous pouvez naviguer dans la structure de vos fichiers.
- Le panneau central supérieur est l'endroit où apparaissent les éditeurs de texte ouverts.
- Le panneau supérieur droit montre normalement un aperçu du panneau d'édition sélectionné. Il peut afficher d'autres informations.
- Le panneau inférieur droit montre l'une des nombreuses Vues, sélectionnables depuis le menu Fenêtre / Afficher la Vue.

La vue Problèmes indique Erreurs (100 sur 791 éléments) et Avertissements (100 sur 11629). Cela semble beaucoup, mais rien dont il faut s'inquiéter.

## Un nouveau projet PHP Hello Universe

Vous êtes maintenant prêt à créer un nouveau projet PHP pour l'extension que vous envisagez de développer. À titre d'illustration, vous pouvez commencer par un composant simple nommé com_hellouniverse. Pour moi, le code de Hello Universe sera situé dans /Users/username/git/j4xtutorials-com-hellouniverse et j'utiliserai j4xtutorials comme affiliation de nom de domaine (le premier mot dans l'espace de noms du composant).

Dans le menu Eclipse, sélectionnez Fichier / Nouveau / Projet PHP. Le formulaire est celui illustré ci-dessus. Mes données :

- Nom du projet : HelloUniverse
- Contenu : sélectionnez Créer un projet à un emplacement existant (à partir d'une source existante).
- Parcourir : Naviguez vers /Home/nomutilisateur/git et sélectionnez le bouton Nouveau Dossier.
- Dans la boîte de dialogue Nouveau Dossier, entrez j4xtutorials-com-hellouniverse et appuyez sur Créer.
- Avec ce dossier vide ouvert, sélectionnez Ouvrir.
- Le répertoire sélectionné doit indiquer : /Users/nomutilisateur/git/j4xtutorials-com-hellouniverse
- Sélectionnez Terminer.

Vous verrez maintenant le nouveau projet dans l'Explorateur de Projet avec votre projet de site web. Le nouveau projet contient deux éléments, tous deux liés au support PHP. Il y a certains éléments cachés utilisés par Eclipse : .settings(dossier), .buildpath et .project.

## Ajouter le dossier com_hellouniverse

Avec le projet HelloUniverse sélectionné :

- Sélectionnez l'élément de menu Fichier ou cliquez avec le bouton droit sur le nom du projet.
- Sélectionnez les éléments de menu Nouveau / Dossier.
- Dans le formulaire Nouveau Dossier, entrez com_hellouniverse dans le champ Nom du dossier :.
- Sélectionnez Terminer.

Le dossier com_hellouniverse est l'endroit où votre code d'extension ira. Tout ce qui se trouve à l'intérieur de ce dossier se retrouvera dans un fichier zip que vous pourrez utiliser pour l'installer dans Joomla. Tout ce qui ne se trouve pas à l'intérieur de ce dossier est utilisé à d'autres fins. Deux fichiers courants à ce niveau :

- README.md est un fichier texte formaté en Markdown qui décrit le composant. De tels fichiers sont couramment utilisés en conjonction avec le stockage de code à distance, comme sur Github (nous en parlerons ailleurs).
- build.xml est un fichier qui contient des instructions sur ce qu'il faut faire après avoir apporté des modifications au code. Le plus souvent, vous souhaiterez copier les fichiers modifiés aux emplacements corrects dans l'arborescence de votre site web et régénérer le fichier zip. Ensuite, pour la plupart des changements, vous pouvez simplement recharger la page web sur laquelle vous travaillez. Vous n'avez pas besoin de réinstaller le composant. Cela permet de gagner énormément de temps !

## Ajouter des dossiers admin, site et média

- Sélectionnez le dossier com_hellouniverse et utilisez les éléments de menu Fichier / Nouveau / Dossier pour créer un nouveau dossier nommé admin.
- À nouveau, cette fois créez un nouveau dossier nommé media. ATTENTION : assurez-vous que le parent est com_hellouniverse et non HelloUniverse.
- Encore une fois, cette fois créez un nouveau dossier nommé site. S'il est créé au mauvais endroit, il peut être déplacé vers l'endroit correct.

## build.xml

Le code suivant contient un exemple de fichier build.xml à placer à la racine de votre projet HelloUniverse.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="hellouniverse" basedir="." default="main">
    <property file=".project" />

    <!-- Source and Destinations -->

    <property name="package"  value="${phing.project.name}" override="true" />
    <property name="srcdir" value="${project.basedir}/com_hellouniverse" override="true" />
    <property name="sitedir" value="/Users/username/Sites/j4tutorials"  override="true" />
    <property name="zipsdir" value="/Users/username/zips"  override="true" />

    <!-- Filesets -->

    <fileset dir="${srcdir}/admin" id="adminfiles">
        <include name="**" />
    </fileset>
    <fileset dir="${srcdir}/media" id="mediafiles">
        <include name="**" />
    </fileset>
    <fileset dir="${srcdir}/site" id="sitefiles">
        <include name="**" />
    </fileset>
    <fileset dir="${srcdir}" id="allfiles">
        <include name="**" />
    </fileset>

    <!-- Targets -->

    <target name="main" description="main target">
        <copy todir="${sitedir}/administrator/components/com_hellouniverse">
            <fileset refid="adminfiles" />
        </copy>
        <copy todir="${sitedir}/media/com_hellouniverse">
            <fileset refid="mediafiles" />
        </copy>
        <copy todir="${sitedir}/components/com_hellouniverse">
            <fileset refid="sitefiles" />
        </copy>
        <zip destfile="${zipsdir}/com_hellouniverse.zip">
            <fileset refid="allfiles" />
        </zip>
    </target>
</project>
```

Quelques explications :

- **srcdir** est l'emplacement système de fichiers de votre code source, d'où les fichiers sont copiés.
- **sitedir** est l'emplacement système de fichiers de votre site de test, où les fichiers sont copiés.
- **zipsdir** est un emplacement pour le fichier zip - je trouve pratique de conserver les fichiers zip de projets différents ensemble dans un dossier zips.
- **Ensembles de fichiers** sont des groupes de sources à copier vers différentes destinations. signifie tous les dossiers et fichiers.
- **Cibles** sont les destinations et ce qui doit y être copié.

Créez un fichier build.xml contenant le code ci-dessus :

- Sélectionnez Fichier / Nouveau / Fichier XML depuis le menu Eclipse.
- Assurez-vous que HelloUniverse est sélectionné comme parent et entrez le nom build.xml (exactement cela).
- Sélectionnez Terminer
- Dans la Vue Source (choisissez en bas à gauche), copiez et collez le code ci-dessus à la place de la ligne déjà présente.
- Enregistrez.

Pour que cela fonctionne, vous avez besoin d'un outil de construction : Phing.

## L'outil de construction Phing¶

Accédez au site [Phing](https://www.phing.info/) et téléchargez l'archive Phar. Vous pouvez utiliser ce [lien direct](https://www.phing.info/get/phing-latest.phar). Enregistrez le fichier dans un dossier phing dans votre dossier bin personnel /Users/username/bin/phing. Modifiez les permissions du fichier à 755 afin qu'il puisse être exécuté depuis la ligne de commande. Pour tester, ouvrez un terminal, changez de dossier pour votre dossier bin/phing et entrez ./phing-latest.phar -v pour voir le numéro de version. N'oubliez pas le chemin complet vers le fichier :

- /Utilisateurs/nomdutilisateur/bin/phing/phing-dernier.phar

Vous en aurez besoin plus tard.

## Préférences d'Eclipse - Générateurs

C'est ici que vous configurez Phing pour construire votre projet chaque fois que vous apportez des modifications au code.

- Dans l'Explorateur de projets, sélectionnez le projet HelloUniverse.
- Sélectionnez les éléments de menu Fichier / Propriétés.
- Dans la boîte de dialogue Propriétés, sélectionnez Constructeurs puis le bouton Nouveau.
- Dans la boîte de dialogue Type de configuration, sélectionnez Programme puis OK.
- Dans la boîte de dialogue Modifier les propriétés de configuration de lancement, entrez un titre approprié : Compil HelloUniverse.
- Dans le champ Emplacement, entrez le chemin vers le programme phing ou utilisez le bouton Parcourir le système de fichiers pour le rechercher : /Users/ceford/bin/phing/phing-latest.phar
- Pour le champ Répertoire de travail, sélectionnez le bouton Parcourir l'espace de travail puis sélectionnez HelloUniverse.
- Cliquez sur OK, puis Appliquer et Fermer.

Pour tester : sélectionnez les éléments de menu Projet / Construire le projet. La vue Console en bas de l'écran affichera la progression de la construction. À ce stade, il est probable que cela affiche ÉCHEC DE LA CONSTRUCTION en rouge. C'est attendu - les dossiers des composants seront créés lors de l'installation du fichier zip. Ils n'existent pas encore car il n'y a pas de code. Mais cela montre que l'outil de construction Phing fonctionne.

## Comment déboguer

Lorsque quelque chose tourne mal, vous voudrez peut-être parcourir le code ligne par ligne pour voir ce qui se passe réellement. Si vous avez défini le Système de Débogage sur Oui et le Rapport d’Erreurs sur Maximum dans la Configuration Globale de Joomla, vous pourriez avoir une trace de pile pour montrer où une erreur se produit dans le code. Ou vous pourriez remarquer quelque chose d'inhabituel dans le résultat et avoir une bonne idée de l'endroit où la faute pourrait se trouver. Dans les deux cas, vous devez définir des points d'arrêt où l'exécution s'interrompra pour vous permettre de voir les valeurs des variables et de progresser dans le code ligne par ligne.

### Définir un point d'arrêt

Les points d'arrêt doivent être définis dans le projet du site web, J4Tutorials. Par exemple, dans le panneau de l'Explorateur de projet, développez le projet J4Tutorials et trouvez administrator / components / com_users / src / Model et double-cliquez sur UsersModel.php pour l'ouvrir dans une fenêtre d'édition. Allez à la ligne 87 et double-cliquez sur le numéro 87. Un petit marqueur bleu apparaît pour indiquer qu'un point d'arrêt a été défini. L'exécution du code devrait s'arrêter ici lorsque vous affichez la liste des utilisateurs via le menu Administrator / Users / Manage.

### Créer une configuration de débogage

Dans le menu supérieur, sélectionnez Exécuter / Configurations de débogage. Dans le formulaire, entrez un titre reconnaissable, par exemple Déboguer Admin, pour le sélectionner plus tard dans une liste déroulante. Assurez-vous que le fichier sélectionné est /J4Tutorials/administrator/index.php. Dans le formulaire, dans le panneau Commun, sélectionnez Déboguer dans la liste du menu Afficher dans les favoris. Vérifiez que le Débogueur est réglé sur Xdebug. Sélectionnez Appliquer et Fermer.

Vous pouvez maintenant sélectionner Déboguer Administrateur dans la liste déroulante de débogage de la barre d'outils, l'icône symbole de bug.

![Eclipse debug tools](../../../en/images/getting-started/eclipse-pdt-debug-tools.png)

Icônes de débogage, survolez pour les étiquettes :

- **Ignorer tous les points d'arrêt** Un basculement pour ignorer les points d'arrêt sauf la première ligne de index.php.
- **Reprendre** Continuer l'exécution normale jusqu'au prochain point d'arrêt.
- **Terminer** Arrêter le débogage (mais vous devez également retirer le cookie Xdebug).
- **Entrer dans** Sur une ligne contenant un appel de fonction, entrer dans cette fonction.
- **Passer au-dessus** Sur une ligne contenant un appel de fonction, ne pas entrer dans cette fonction.
- **Sortir de** Dans une fonction, ignorer l'arrêt à chaque ligne et revenir à l'appelant.

### Procédure de débogage

Lors du démarrage d'une session de débogage, l'exécution s'arrête à la première ligne de administrator/index.php. C'est la page du tableau de bord d'accueil. Vous devez sélectionner le symbole Reprendre (barre jaune et triangle vert pointant vers la droite). Après un délai d'une seconde environ, il y a plus d'activité et plus de lancements distants en pause dans le panneau de gauche du débogueur admin. C'est la page d'accueil utilisant ajax pour récupérer les informations de mise à jour. Continuez simplement à cliquer sur Reprendre jusqu'à ce que l'activité cesse. Ensuite, dans votre navigateur, sélectionnez l'élément Utilisateurs à partir du tableau de bord d'accueil. Eclipse s'anime et arrête l'exécution au point d'arrêt que vous avez défini.

![Eclipse PHP debug perspective](../../../en/images/getting-started/eclipse-pdt-debug-perspective.png)

Fonctionnalités à noter :

- La ligne de code actuelle est marquée avec un fond vert pâle.
- Le panneau de gauche affiche une trace de la pile - les appels de fonctions précédents qui ont mené à la fonction actuelle. Sélectionnez n'importe quel élément pour voir le code parent.
- Le panneau de droite peut afficher soit les variables actives, soit les points d'arrêt. Développez les objets si nécessaire pour voir les détails.

### Quelques problèmes

- Impossible de s'arrêter aux points d'arrêt : Cela se produit si vous ouvrez un autre programme PHP tel que phpMyAdmin. Pour résoudre ce problème :
  - Allez dans Exécuter / Configurations de débogage et sélectionnez votre élément Admin de débogage.
  - Dans l'onglet Débogueur, sélectionnez Configurer...
  - Dans Mappage de chemin, sélectionnez et supprimez tout chemin non lié à votre site web de test.
  - Sélectionnez Terminer - vous devrez peut-être Terminer et recharger la session de débogage.
- Le débogage reprend après avoir sélectionné le bouton Arrêter. Pour résoudre ce problème :
  - Utilisez les Outils de développement de votre navigateur pour ouvrir une fenêtre d'inspection.
  - Trouvez les cookies que votre navigateur utilise (Stockage dans Firefox).
  - Sélectionnez et supprimez le cookie XDebug.

## Utiliser Git

Git est un système de contrôle de version largement utilisé pour gérer toute sorte de texte, principalement du code, mais cela peut être n'importe quoi. Il fonctionne à partir d'une ligne de commande mais est généralement invoqué par des outils graphiques tels qu'Eclipse. Si vous souhaitez lire à propos de git, consultez le [site web de git](https://git-scm.com/).

Si vous souhaitez utiliser git, vous devez l'installer pour votre plateforme. Ensuite, dans Eclipse, sélectionnez votre projet HelloUniverse, faites un clic droit et sélectionnez Équipe / Partager le projet. Dans la boîte de dialogue Partager le projet, sélectionnez **Utiliser ou créer un dépôt dans le dossier parent du projet** et sélectionnez le projet HelloUniverse dans la liste. Il y a un champ pour indiquer où le dépôt sera créé (/Users/ceford/git/j4xtutorials-com-hellouniverse) - sélectionnez le bouton Créer un dépôt adjacent. Enfin, sélectionnez le bouton Terminer.

![Configure Git Repository](../../../en/images/getting-started/eclipse-pdt-configure-git.png)

Vous remarquerez que de nouvelles décorations sont apparues à côté des éléments dans la vue de l'Explorateur de projets :

- Le marqueur \> indique qu'il y a du contenu qui a changé mais qui n'a pas été validé dans le dépôt.
- Le marqueur ? indique que cet élément n'a pas été ajouté à la liste des éléments à suivre.

![Git Markers](../../../en/images/getting-started/eclipse-pdt-git-markers.png)

Vous êtes maintenant configuré pour le contrôle de version. Faites un clic droit sur le projet et regardez à quoi ressemble le menu Équipe. Beaucoup trop de choses à couvrir ici !

## Enfin

Prêt à commencer à travailler sur votre propre code ?

*Traduit par openai.com*

