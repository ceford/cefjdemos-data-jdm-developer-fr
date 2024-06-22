<!-- Filename: J4.x:Improved_Override_Management / Display title: Gestion améliorée des substitutions -->

<span id="main-portal-heading">GSoC 2018  
Documentation  
sur la gestion améliorée des substitutions</span> [<img
src="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/75px-Gsoc2016.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/113px-Gsoc2016.png 1.5x, https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/150px-Gsoc2016.png 2x"
data-file-width="373" data-file-height="373" width="75" height="75"
alt="Gsoc2016.png" />](https://docs.joomla.org/GSOC_2018 "GSOC 2018")
Joomla!  4.x

## Introduction

Ce projet ajoute une fonction de vérification de mise à jour à Joomla de
sorte que si un template est modifié ou mis à jour, il informe
l'utilisateur que l'un des fichiers de base de leur template est modifié
avec la mise à jour, pour éviter les problèmes de sécurité ou de
fonctionnalité et ils peuvent ajuster leur modifications avant que
quelqu'un puisse s'en rendre compte.

**Lien du dépôt du projet :**
<a href="https://github.com/joomla-projects/gsoc18_override_management"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla-projects/gsoc18_override_management</a>

## Obtenir une longueur d'avance grâce à une gestion améliorée de l'override

### Notes

Si vous êtes nouveau dans le développement Joomla, et ne savent pas
grand-chose sur le gestionnaire de modèles et les Overrides, veuillez
lire :

1.  [Comment utiliser le Gestionnaire de
    templates](https://docs.joomla.org/J3.x:How_to_use_the_Template_Manager "Special:MyLanguage/J3.x:How to use the Template Manager")
2.  [Les substitutions de mise en page dans
    Joomla](https://docs.joomla.org/Layout_Overrides_in_Joomla "Special:MyLanguage/Layout Overrides in Joomla")

Maintenant, si vous vous êtes familiarisé avec la façon d'utiliser le
gestionnaire de modèles et les types d'Overrides dans Joomla, alors,
passons en revue les fonctionnalités de ce projet.

- Substitutions supportées
- Affichage des différences
- Override - Quick Icon Notification Plugin (plugin des icônes de
  notification)
- Mise en place - Contourner un plugin
- Mise à jour - Historique d'Override

## Types de substitutions supportées par cette fonctionnalité

Il ne supporte pas les mises en page alternatives (Alternative Layout)
dans laquelle le nom de fichier est renommé en quelque chose d'autre et
les overrides de javascript ou css. Cette fonction prend en charge les
fichiers de remplacement énumérés dans l'onglet "Créer un override".
Exemple :

<a
href="https://docs.joomla.org/index.php?title=Special:Upload&amp;wpDestFile=Selection_035-fr.png"
class="new" title="File:Selection 035-fr.png">800px</a>

## Différence de vue entre les fichiers de base et les fichiers d'override

Cette fonction montre la différence entre le fichier de base et le
fichier modifié. Lorsque vous ouvrez un fichier modifié à éditer, vous
pouvez voir deux boutons ou sélecteurs dans le coin supérieur droit de
la page si le fichier principal de ce fichier est supprimé.

Des boutons comme ceux-ci :

<a
href="https://docs.joomla.org/index.php?title=Special:Upload&amp;wpDestFile=Selection_027-fr.png"
class="new" title="File:Selection 027-fr.png">Buttons</a>

Ici, vous pouvez contrôler l'affichage ou le masquage de la vue des
différences et des fichiers de base. Dans l'image suivante, vous verrez
l'emplacement du fichier de base et la vue diff dans le gestionnaire de
template.

<a
href="https://docs.joomla.org/index.php?title=Special:Upload&amp;wpDestFile=Selection_028-fr.png"
class="new" title="File:Selection 028-fr.png">800px</a>

Vous ne pouvez pas modifier le fichier natif.

### Comment la vue des différence fonctionne

Lorsque vous cliquez sur un fichier d'override pour l'éditer, il appelle
une fonction `getCoreFile` qui reçoit les deux paramètres `path` du
fichier de surcharge dans le template. Exemple :
`/html/html/layouts/joomla/form/field/user.php` et le chemin client du
fichier, qu'il appartienne à `Site` ou `Administrateur`. Ensuite, sur la
base de ces informations, il renvoit le chemin du fichier de base s'il
existe. Pour montrer la différence entre les fichiers core et override,
nous avons utilisé la bibliothèque
<a href="https://github.com/kpdecker/jsdiff" class="external text"
target="_blank" rel="nofollow noreferrer noopener">jsdiff</a>.

Override - Plugin des icônes de notification Un plugin quick icon
notification affiche la notification dans le cpanel et il montre le
total des modifications mises à jour de tous les templates. Lorsque des
overrides sont mis à jour, l'icône affiche quelque chose comme dans
l'exemple ci-dessous :

<a
href="https://docs.joomla.org/index.php?title=Special:Upload&amp;wpDestFile=Selection_020-fr.png"
class="new" title="File:Selection 020-fr.png">800px</a>

Lorsque vous cliquez dessus, il vous redirigera vers la liste de vos
templates avec leur description. Vous verrez un nouvel en-tête de
colonne *Overrides'* montrant le nombre d'override mis à jour qui
appartient au modèle. Si rien n'a été mis à jour dans le template
override, il affichera un badge Mis à Jour.

<a
href="https://docs.joomla.org/index.php?title=Special:Upload&amp;wpDestFile=Selection_022-fr.png"
class="new" title="File:Selection 022-fr.png">800px</a>

Vitesse de fonctionnement de l'icône It makes an AJAX call to the
`TemplateController.php` which returns the information and displays a
notification when such overrides are updated.

**Attention**

Quick icon notification plugin only works or able to fetch data if
`installer/override` plugin is enabled. If `installer/override` is
disabled then you will see this error message in quick icon.

<img
src="https://docs.joomla.org/images/thumb/9/9d/Selection_024-en.png/800px-Selection_024-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/9/9d/Selection_024-en.png/1200px-Selection_024-en.png 1.5x, https://docs.joomla.org/images/9/9d/Selection_024-en.png 2x"
data-file-width="1300" data-file-height="333" width="800" height="205"
alt="Enable" />

If you click on the quick icon you will be redirected to the
`installer/override` plugin settings where you can edit the settings of
the plugin.

## Installer - Override Plugin

This plugin is the main part of this feature. It allows to find the
correct updated overrides during the extension install or update and
Joomla update.

<img
src="https://docs.joomla.org/images/thumb/f/fe/Selection_025-en.png/800px-Selection_025-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/f/fe/Selection_025-en.png/1200px-Selection_025-en.png 1.5x, https://docs.joomla.org/images/thumb/f/fe/Selection_025-en.png/1600px-Selection_025-en.png 2x"
data-file-width="1887" data-file-height="719" width="800" height="305"
alt="Installer Override" />

### How installer override plugin is working

This plugin works on 6 events:

- onExtensionBeforeUpdate
- onExtensionAfterUpdate
- onInstallerBeforeInstaller
- onInstallerAfterInstaller
- onJoomlaBeforeUpdate
- onJoomlaAfterUpdate

Which collect all overrides core file `md5_file()` hash before update
and after update then compairs both values. Then, find the correct
changed or updated file. And, store information in
`#__templates_overrides` table.

## Updated - Override History

You can access this from the Updated Files tab in a template. This is a
list view that shows the list of updated overrides which do belongs to
that template.

<img
src="https://docs.joomla.org/images/thumb/b/b9/Selection_029-en.png/800px-Selection_029-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/b/b9/Selection_029-en.png/1200px-Selection_029-en.png 1.5x, https://docs.joomla.org/images/thumb/b/b9/Selection_029-en.png/1600px-Selection_029-en.png 2x"
data-file-width="1901" data-file-height="737" width="800" height="310"
alt="Selection 029-en.png" />

There are many options available to manage list. Where you can check the
status of override file history whether is checked or not, created date,
date of change and update action like: whether it belongs to (Joomla
Update, Extension Update or Extension Install).

**Note**

These information are only history so if you checked the updated
override changes then, you can delete the history, as not needed
anymore.

**Watch the video tutorial to learn how to use this feature.**
