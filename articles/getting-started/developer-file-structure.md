<!-- Filename: J4.x:Developer:_File_Structure / Display title: Exemple de Structure de Fichier -->

## Introduction

En tant qu'individu débutant dans le développement d'extensions Joomla sur un ordinateur personnel, portable ou de bureau, vous devez mettre en place une structure de fichiers appropriée pour le code de votre extension. L'emplacement de votre site Joomla est prédéfini par votre système d'exploitation et l'installation d'Apache. L'emplacement du code que vous créez dépend de vous.

## Emplacement du site Web de test

Dans cet exemple, les sites de test Joomla sont situés dans des sous-dossiers de la racine du document. Cela permet de créer de nombreux sites web pour toutes sortes de projets différents sans avoir besoin de créer des hôtes virtuels. Certains sites de test peuvent ne pas être liés à Joomla du tout. La racine du site pour différentes plateformes :

- Mac : /Users/username/Sites
- Linux : /home/username/public_html
- Windows : ...

Voici une capture d'écran d'une partie de la liste du dossier Sites montrant une sélection de nombreux sites de test :

![multiple sites on mac](../../../en/images/getting-started/developer-file-structure-mac-sites.png)

Chacun est accessible par son nom de sous-dossier. Exemples :

- `http://localhost/j4test`
- `http://localhost/j520`

Il existe des circonstances dans lesquelles vous pourriez préférer créer des sites virtuels séparés. Cela n'est pas couvert ici.

## Tester la structure des fichiers du site Web

Si vous ne l'avez pas déjà fait, vous devrez vous familiariser avec la structure d'un site web Joomla. L'illustration suivante montre un arbre typique de fichiers et dossiers Joomla, avec le dossier Administrator développé pour montrer son contenu.

![joomla file structure with administrator expanded](../../../en/images/getting-started/developer-file-structure-mac-joomla.png)

C'est ici que le code fonctionnel sera installé. Le code source est ailleurs.

## Emplacement du code de l'extension du développeur

La localisation de votre code d'extension est un choix personnel. J'aime conserver mon code d'extension dans un arborescence de fichiers adaptée à la création d'un fichier zip installable. La base de mon arborescence est /Users/username/git parce que je sais écrire git et que j'utilise git pour le contrôle de version. Vous n'êtes pas obligé de faire cela - git sera abordé dans un tutoriel séparé. Mon dossier parent git contient de nombreux sous-dossiers qui peuvent chacun utiliser des dossiers git distincts pour le contrôle de version. Voici une capture d'écran montrant une liste partielle de projets :

![joomla file structure project folders](../../../en/images/getting-started/developer-file-structure-mac-project-folders.png)

Remarquez que certains des noms de dossiers commencent par `j4xdemos`, que j'ai adopté pour utiliser comme première partie de l'espace de noms utilisé pour mes projets créés à des fins de tutoriels Joomla 4. Ce n'est pas nécessaire en tant que partie du nom de dossier, mais c'est quelque chose à considérer : la première partie de votre espace de noms doit être quelque chose d'unique pour vous ou votre organisation. J'ai depuis adopté `cefjdemos` comme préfixe de mon espace de noms, car il est plus personnel et non spécifique à une version de Joomla.

### Structure du Dossier du Projet

Prenons j4xdemos-com-mywalks comme exemple, tout le code qui sera intégré à l'extension se trouve dans le dossier com_mywalks. Lorsque celui-ci est compressé, le fichier com_mywalks.zip créé est utilisé pour l'installation dans Joomla. Aucun des autres fichiers à la racine n'est inclus dans le fichier zip, mais ils peuvent être inclus dans le dépôt GitHub. Par exemple, README.md est un fichier texte au format Markdown contenant une brève description de l'extension. Le nom du dossier j4xdemos-com-mywalks n'est pas significatif. Il n'est utilisé nulle part ailleurs, sauf pour trier les dossiers par ordre alphabétique.

Dans l'illustration suivante, le dossier j4xdemos-com-mywalks a été ouvert dans VSCodium pour montrer la structure du code du projet. Le fichier mywalks.xml est un fichier manifeste qui indique à Joomla quoi installer et où. Les dossiers admin et site contiennent le code qui ira dans administrator/components/com_mywalks et components/com_mywalks.

![Project folder open in vscodium](../../../en/images/getting-started/developer-file-structure-mac-vscodium.png)

Il devrait être évident que même un petit composant nécessite un grand nombre de dossiers et de fichiers. Il existe des outils de création standards pour les extensions qui permettent de créer rapidement un composant squelette. Ils sont traités ailleurs. À faire

Prêt à créer du code ?

*Traduit par openai.com*

