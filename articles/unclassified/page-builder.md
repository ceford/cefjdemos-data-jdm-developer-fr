<!-- Filename: J4.x:Page_Builder / Display title: Constructeur de page -->

<span id="main-portal-heading">GSoC 2019
Constructeur de page de Joomlaǃ 4
Documentation</span> [<img
src="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/75px-Gsoc2016.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/113px-Gsoc2016.png 1.5x, https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/150px-Gsoc2016.png 2x"
data-file-width="373" data-file-height="373" width="75" height="75"
alt="Gsoc2016.png" />](https://docs.joomla.org/GSoC_2019 "GSoC 2019")
Joomla!  4.x

## Introduction

Imaginez que quelqu'un veuille créer un modèle de page mais sans écrire
en HTML. Les CSS devraient uniquement être utilisés pour définir les
styles et pas pour définir la mise en page. C'est pourquoi le
Constructeur de page et le modèle de page du site "Apodis" sont des
outils utiles. Le principal atout tient dans un éditeur, qui rend
possible de créer des positions personnalisées avec l'aide d'interfaces
acceptant le glisser/déposer. Les utilisateurs peuvent utiliser des
éléments prédéfinis - conteneurs, grilles, colonnes, position de module
ainsi que des éléments personnalisés. Les développeurs tiers peuvent
ajouter leurs propres éléments au travers de plug-ins.

## Comment démarrer

Pour utiliser le Constructeur de pages, le template actif du site public
doit avoir un champ *pagebuilder* dans le fichier `templateDetails.xml`
tel que dans Apodis :

```xml
<fieldset name="pagebuilder" label="TPL_APODIS_PAGEBUILDER">
   <field
		name="grid"
		type="pagebuilder"
		hidden="true"
		label="TPL_APODIS_PAGEBUILDER"
   />
</fieldset>
```

Le template lui-même doit charger le RenderHelper, qui affiche le
paramètre, complété par l'éditeur. Ceci est réalisé dans le fichier
`index.php` du template.

```php
    use Joomla\Component\Templates\Administrator\Helper\RenderHelper;

    $grid = $this->params->get('grid');
```

Ensuite, la sortie est placée dans le corps HTML :

```php
<?php echo RenderHelper::renderElements($grid); ?>
```

## Editeur

Le Constructeur de pages est disponible dans le gestionnaire de styles
et dans l'onglet approprié. Il prend en charge l'ajout de nouveaux
éléments, le glisser-déposer pour les réarranger et les positionner
selon vos préférences et la possibilité de redimensionner les colonnes.
L'imbrication d'éléments est également disponible et peut être modifiée
en modifiant les paramètres du plugin. Cela crée des possibilités
presque illimitées.

Editeur du constructeur de page :

<img src="https://docs.joomla.org/images/7/76/Pagebuilder-editor-en.jpg"
decoding="async" data-file-width="800" data-file-height="318"
width="800" height="318" alt="Pagebuilder-editor-en.jpg" />

Les utilisateurs peuvent ajouter différents éléments au modèle en
utilisant le bouton "Ajouter un élément" en bas des lignes. Une fenêtre
modale apparaît alors, dans laquelle ils peuvent sélectionner les
éléments disponibles et définir directement d'autres options.

<img
src="https://docs.joomla.org/images/9/9d/Pagebuilder-add-element-en.jpg"
decoding="async" data-file-width="800" data-file-height="480"
width="800" height="480" alt="Add elements to the layout" />

It is possible to add custom CSS classes using the settings icon,
present on every element and here users can also add offsets to columns
and select name and module chrome for each module position. Plugins are
able to offer their own options for the sidebar.

Les utilisateurs peuvent éditer des éléments dans la barre latérale des
paramètres :

<img
src="https://docs.joomla.org/images/b/bc/Pagebuilder-element-settings-en.jpg"
decoding="async" data-file-width="800" data-file-height="492"
width="800" height="492" alt="Pagebuilder-element-settings-en.jpg" />

La sortie est un objet JSON stocké dans le champ de paramètres avec le
type, les options, la taille et les enfants de chaque élément. L'éditeur
charge automatiquement la mise en page sauvegardée à partir du paramètre
afin que l'utilisateur puisse directement poursuivre l'édition.

## Éléments

Il y a quatre éléments par défaut fournis avec le Constructeur de
pages :

1.  Container
2.  Grid
3.  Colonnes
4.  Position de module

La configuration par défaut est disponible ci-dessous, celles-ci peuvent
être modifiées via des plugins.

|                       |              |                         |        |                               |
|-----------------------|--------------|-------------------------|--------|-------------------------------|
| Config                | Container    | Grid                    | Column | Module Position               |
| Parents allowed       | Root, Column | Root, Column, Container | Grid   | Root, Grid, Column, Container |
| Contain children      | True         | True                    | True   | False                         |
| Can contain component | True         | True                    | True   | False                         |
| Can contain message   | True         | True                    | True   | False                         |

## Composant et Message

Les positions des modules ne sont pas les seuls éléments importants dans
le Constructeur de pages. La position du composant et les messages
affichés peuvent être définis par glisser-déposer sur les éléments qui
l'acceptent. Il est ainsi extrêmement facile de sélectionner les
positions auxquelles les utilisateurs sont habitués à afficher le
contenu de la page. Un seul des deux peut être placé sur un seul
élément. Un clic sur le 'x' supprime la position et elle peut être
remplacée à nouveau.

Les utilisateurs peuvent définir la position du composant ou du message
sur les éléments du Constructeur de pages :

<img
src="https://docs.joomla.org/images/3/37/Pagebuilder-set-component-and-message-en.jpg"
decoding="async" data-file-width="800" data-file-height="402"
width="800" height="402"
alt="Users can set the component or message position on Page builder elements" />

## Apodis

Le Constructeur de pages est intégré avec un nouveau modèle de frontend
nommé Apodis, qui permet aux utilisateurs de sélectionner,
glisser-déposer leurs propres styles de modèles à l'aide de l'éditeur.
Le nouveau modèle de conception récupère les éléments du constructeur de
pages dans le fichier index.php et affiche directement le rendu. Ceci
est utile pour les tests indépendants, où aucune feuille de style ne
peut interrompre le comportement par défaut. This is how it works: A
com_templates helper function gets called in index.php and recursively
iterates through the JSON param obtained from the \#\_template_styles
table, which includes type, position and all the information of saved
Page-builder elements. Depending on the type and options, the rendering
changes or adds HTML. In the future, the renderer will observe
page-builder plugins and their desired rendering too.

## Intégrer l'état et les méthodes grâce à Vuex

Vuex est utilisé pour permettre à plusieurs composants d'obtenir leur
état à partir d'un "store" Vuex centralisé et peut être mis à jour de
manière réactive et efficace lorsque l'état du "store" change.
L'utilisation de Vuex a rendu la programmation et l'architecture du
constructeur de pages beaucoup plus modulaires, efficaces et extensibles
pour de futures extensions. Il nous permet également de suivre les
événements qui changent d'état et nous aide à déboguer en utilisant Vue
Devtools Extension.

## Embarquer Bootstrap 4

Nous avons inclus nos propres fichiers BS4 avec l'éditeur de
constructeur de page pour tenir compte de la situation où les fichiers
Bootstrap ne seraient pas inclus dans le modèle backend.
