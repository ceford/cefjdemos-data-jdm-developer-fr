<!-- Filename: J4.x:MVC_Anatomy:_Getting_Started / Display title: Anatomie MVC : Commencer -->

## Introduction

Les composants Joomla suivent l'approche Modèle, Vue, Contrôleur, ou MVC en abrégé. Le Modèle est censé gérer le chargement et le stockage des données. La Vue est censée gérer l'affichage des données. Et le Contrôleur est censé gérer le flux du programme, l'interaction entre le code du Modèle et de la Vue du composant.

Si vous souhaitez créer votre propre composant, il existe deux approches d'apprentissage que vous pourriez adopter :

- **Construisez-le étape par étape :** en suivant un tutoriel simple qui décrit chaque étape.
- **Disséquez-le fichier par fichier :** en démontant un composant fonctionnel pour apprendre comment chaque partie fonctionne.

Ce tutoriel utilise la dernière approche. Le composant utilisé pour le tutoriel s'appelle com_countrybase. Il est utilisé pour gérer et afficher certaines données de base sur les pays du monde. Il est en fait utile en soi et pourrait servir de base pour construire quelque chose de plus sophistiqué.

## Objectifs du plan

Quel que soit votre projet, vous devriez commencer par un aperçu de vos objectifs et peut-être vous demander si ces objectifs peuvent être atteints avec les composants de base de Joomla ou une extension disponible. Pour com_countrybase, une table de données de pays est nécessaire, avec des pages d'entrée et de sortie. Et peut-être un module pour afficher un taux de change monétaire quotidien. Et même un plugin appelé par une commande cli pour mettre à jour les données de devises à intervalles réguliers. Ce tutoriel couvre uniquement le composant.

Le composant nécessite une table de base de données pour stocker les données des pays et une table pour stocker les données des devises. Chacune nécessitera une vue liste et une vue édition. Ce n'est pas quelque chose qui peut être réalisé avec les composants de base de Joomla, c'est donc clairement un cas pour un composant personnalisé.

## Tables de départ

Il existe une multitude de données sur les pays disponibles à partir de diverses sources et dans divers formats. Comme il y a au moins 250 pays dans le monde, saisir des données pour chaque pays un par un à l'aide d'un formulaire de saisie de données serait assez fastidieux. J'ai donc préparé des tables de démarrage en collectant des données de plusieurs sources, en les combinant dans une feuille de calcul puis en exportant des instructions SQL pour créer les tables.

Les données sont basées sur les codes pays ISO 3166, mais certains pays très connus ne sont pas inclus, par exemple l'Angleterre, l'Écosse et le Pays de Galles. Vous n'avez pas besoin de vous en préoccuper. Les tableaux sont fournis avec le composant com_countrybase.

Les fichiers pour le composant com_countrybase sont disponibles sur GitHub. Vous pouvez télécharger un fichier [ZIP](https://github.com/ceford/j4xdemos-com-countrybase/archive/refs/heads/master.zip) et l'installer pour le voir fonctionner dans le menu Administrateur. Créez un élément de menu si vous souhaitez le voir fonctionner dans le modèle de votre site. Décompressez également le fichier zip dans l'espace de fichiers de votre projet, et non dans l'arborescence de votre site web de test, pour examiner la structure des fichiers du composant et le contenu des fichiers avec votre IDE préféré ou un outil d'édition de texte.

## Capture d'écran

Cette liste d'administrateurs de pays comprend cinq éléments pour minimiser la taille de l'image. Joomla affiche normalement 20 éléments.

![List of countries](../../../en/images/mvc-anatomy/com-countrybase-countries.png)

## Composant Standard

Pour vous aider à démarrer avec votre propre composant, il y a un [composant modèle](https://github.com/ceford/j4xdemos-com-bpsrc/archive/refs/heads/master.zip) disponible sur Github. Téléchargez et décompressez ceci dans votre espace de fichiers projet, et non dans l'arborescence de votre site web de test. Après le téléchargement, effectuez toutes les modifications indiquées dans le README et vous êtes prêt à partir.

Il existe également un certain nombre de générateurs d'extension gratuits et commerciaux que vous pourriez essayer pour créer un composant de base pour vos propres besoins. [Joomla! Component Builder](https://www.joomlacomponentbuilder.com/) est gratuit et semble complet.

*Traduit par openai.com*

