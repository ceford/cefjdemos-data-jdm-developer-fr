<!-- Filename: Manifest_files / Display title: Fichiers manifest -->

Joomla!  4.x Joomla!  3.x Joomla!  2.5

Avec Joomla, toutes les extensions proposent des fichiers manifestes.
Ces fichiers recouvrent les informations générales concernant
l'installation, ainsi que des paramètres pour la configuration de
l'[extension](https://docs.joomla.org/extension "Special:MyLanguage/extension")
elle-même. Depuis Joomla! 2.5, il existe très peu de différences entre
les différents formats de fichiers manifestes en fonction des différents
[types
d'extensions](https://docs.joomla.org/Extension_types_(technical_definitions) "Special:MyLanguage/Extension types (technical definitions)")
permettant ainsi à chaque type de bénéficier de la puissance de
l'installateur Joomla.

## Conventions de nommage

Le fichier doit être nommé `manifest.xml` (pour les versions de Joomlaǃ
2.5 et 3) ou `.xml` (pour les versions de Joomlaǃ 2.5, 3 et 4) et situé
dans le répertoire racine du package d'installation.

Joomla 4.x : la correspondance automatique du namespace va échouer si un
nom de fichier tel que `manifest.xml` est utilisé. Voir
<a href="https://github.com/joomla/joomla-cms/issues/37750"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms/issues/37750</a>

## Syntaxe

### Élément à la racine

La balise principale du fichier d'installation est :

La balise ouvrante et fermante est la même pour toutes les extensions.
Les attributs suivants sont autorisés à l'intérieur de la balise :

<table class="wikitable">

<tbody>
<tr class="header">
<th>Attribut</th>
<th>Valeurs</th>
<th>Applicable à</th>
<th>Description</th>
</tr>
&#10;<tr class="odd">
<td>type</td>
<td><code>component</code><br />
<code>file</code><br />
<code>language</code><br />
<code>library</code><br />
<code>module</code><br />
<code>package</code><br />
<code>plugin</code><br />
<code>template</code><br />
<code>element</code></td>
<td>Toutes les extensions</td>
<td>Cet attribut décrit à l'installateur le type de l'extension. En
fonction de ce type, d'autres d'exigences supplémentaires s'appliqueront
aux sous-balises.</td>
</tr>
<tr class="even">
<td>version</td>
<td><code>2.5</code><br />
<code>3.0</code></td>
<td>Toutes les extensions</td>
<td>Chaîne de caractères qui identifie la version de Joomla! pour
laquelle cette extension est développée. Ceci n'est pas utilisé dans le
version <img
src="https://docs.joomla.org/images/4/4d/Compat_icon_3_x.png"
decoding="async" data-file-width="40" data-file-height="17" width="40"
height="17" alt="Joomla 3.x" /> et a été retiré de la version <img
src="https://docs.joomla.org/images/f/f4/Compat_icon_4_0.png"
decoding="async" data-file-width="40" data-file-height="17" width="40"
height="17" alt="Joomla 4.0" /> et des versions ultérieures.</td>
</tr>
<tr class="odd">
<td>method</td>
<td><code>install</code><br />
<code>upgrade</code></td>
<td>Toutes les extensions</td>
<td>La valeur par défaut <code>install</code> sera également utilisée si
l'attribut <code>method</code> n'est pas utilisé. La valeur
<code>install</code> signifie que l'installateur va s'arrêter s'il
trouve un fichier/dossier de la nouvelle extension.</td>
</tr>
<tr class="even">
<td>client</td>
<td><code>site</code><br />
<code>administrator</code></td>
<td>Modules</td>
<td>L'attribut <code>client</code> permet de préciser pour quel type de
client d'application le nouveau module est disponible.</td>
</tr>
<tr class="odd">
<td>group</td>
<td><em>string</em></td>
<td>Plugins</td>
<td>Le nom du groupe indique pour quels groupes de plugins le nouveau
plugin est disponible. Les groupes existants sont les noms de dossier du
répertoire <code>/plugins</code>. Le programme d'installation va créer
de nouveaux noms de dossier pour les noms de groupe qui n'existent pas
encore.</td>
</tr>
</tbody>
</table>

### Métadonnées

Les éléments suivants peuvent être utilisés pour insérer des
métadonnées. Aucun de ces éléments n'est obligatoire et s'ils sont
présents, ils doivent être un enfant de l'élément racine.

     – raw component name (e.g. com_banners).
     – author's name (e.g. Joomla! Project)
     – date of creation or release (e.g. April 2006)
     – a copyright statement (e.g. (C) 2005 - 2011 Open Source Matters. All rights reserved.)
     – a license statement (e.g. NU General Public License version 2 or later; see LICENSE.txt)
     – author's email address (e.g. admin@joomla.org)
     – URL to the author's website (e.g. www.joomla.org)
     – the version number of the extension (e.g. 1.6.0)
     – the description of the component. This is a translatable field. (e.g. COM_BANNERS_XML_DESCRIPTION)
     – the internal name of the component. If omitted, name will be cleaned and used

Remarque : les balises et sont des champs traduisibles de sorte que le
nom et la description de l'extension pourront être présents dans la
langue maternelle de l'utilisateur.

### Fichiers de frontend

```xml
	<files folder="from-folder">
		<filename>example.php</filename>
		<folder>examples</folder>
	</files>
```

Les fichiers à copier dans le répertoire de frontend doivent être placés
dans les balises . Vous pouvez utiliser l'attribut `folder` en option
pour spécifier un répertoire **du package ZIP**. Chaque fichier à copier
doit être nommé par l'élément . Si vous souhaitez copier un dossier
entier à la fois, vous pouvez le définir comme un .

Pour les **plugins**, le nom brut du plugin doit être placé dans
l'attribut `plugin` de l'élément qui pointe vers le fichier contenant la
classe du plugin. Par exemple, dans le cas d'un système de plugin appelé
"exemple" (nom complet : `plg_system_exemple`), il faudra utiliser
`exemple.php`.

### Fichiers multimédias

```xml
	<media folder="media" destination="com_example">
		<filename>com_example_logo.png</filename>
		<folder>css</folder>
		<folder>js</folder>
	</media>
```

Cet exemple permet de copier le fichier (`/media/com_example_logo.png`)
et les dossiers ( `/media/css/` et `/media/js/` ) listés vers
`/media/com_example/`, ainsi que la création du dossier `com_example` si
nécessaire. Vous pouvez utiliser en option l'attribut `folder` pour
spécifier un répertoire **dans le package ZIP** à partir duquel réaliser
la (dans ce cas, `media`).

Les extensions doivent stocker les éléments, dont elles ont besoin pour
être accessible sur le web (JS, CSS, images, etc), dans `media`. Cette
fonctionnalité a été ajoutée dans le cadre du projet de développement
d'une interface multi-site et du placement éventuel de fichiers de code
(PHP) en dehors des espaces web serveurs accessibles.
Remarque: la section médias n'est pas analysé pour les extensions de
type 'package'.

Ref:

- <a
  href="https://groups.google.com/forum/#!msg/joomla-dev-cms/4CAASJqFY-k/PvPj14gP29EJ"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Google Groups - joomla-dev-cms
  thread</a>
- <a
  href="https://groups.google.com/forum/#!msg/joomla-dev-cms/uNmhX98sKbE/p8p68Jke680J"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Google Groups - joomla-dev-cms
  thread</a>

### Section de l'administration

```xml
	<administration>
		<!-- various elements -->
	</administration>
```

La section d'administration est définie par l'élément . Puisque seuls
les
[composants](https://docs.joomla.org/Component "Special:MyLanguage/Component")
concernent à la fois l'espace
[administration](https://docs.joomla.org/Site_(Application) "Special:MyLanguagehttps://docs.joomla.org/Administrator_(Application)"),
**seuls les manifests pour composants doivent intégrer ces éléments**.

#### Fichiers de backend

Les fichiers à copier dans le répertoire de backend doivent être placés
dans l'élément sous . Vous pouvez utiliser l'attribut optionnel `folder`
pour spécifier un répertoire à copier depuis le **pack ZIP**. Voir les
*fichiers frontend* pour plus d'informations.

#### Liens de menu et sous-menus

**Note de version :** avant Joomla! 3.4, le fait de ne pas avoir de
balise dans votre fichier XML avait pour conséquence l création d'un élément de
menu. Cette anomalie a été corrigée dans Joomla! 3.4 et ainsi, si aucune
balise n'est présente dans votre fichier manifest, aucun menu d'administration
ne sera créé pour le composant.

```xml
	<menu>COM_EXAMPLE</menu>
	<submenu>
		<!--
			Note that all & must be escaped to &amp; for the file to be valid
			XML and be parsed by the installer
		-->
		<menu link="anoption=avalue&amp;anoption1=avalue1">COM_EXAMPLE_SUBMENU_ANOPTION</menu>
		<menu view="viewname">COM_EXAMPLE_SUBMENU_VIEWNAME</menu>
	</submenu>
```

Le texte pour l'élément du menu principal pour le composant est défini
dans la balise `<menu>`, enfant de de la balise `<administration>`. Une
`<submenu>` balise peut être présente (également comme enfant de
`<administration>`), qui pourra contenir d'autres éléments de menu définis
par `<menu>`.

En outre, chaque élément `<menu>` permet de définir les attributs suivants :

<table class="wikitable">

<tbody>
<tr class="header">
<th>Attribut</th>
<th>Description</th>
</tr>
&#10;<tr class="odd">
<td>link</td>
<td>Un lien vers lequel renvoyer l'utilisateur lorsque l'élément de menu
est cliqué. Vous pouvez utiliser "view" à la place.</td>
</tr>
<tr class="even">
<td>img</td>
<td>Le chemin (relatif) à une image (16x16 pixels) qui apparait à côté
de l'élément de menu.
<p><u>Doit être url compatible (par exemple sans espaces).</u></p></td>
</tr>
<tr class="odd">
<td>alt</td>
<td></td>
</tr>
<tr class="even">
<td>view</td>
<td>Un paramètre URL à ajouter au lien. Par exemple,
COM_EXAMPLE
dans le manifest XML com_example générerait l'URL de l'élément de menu
<code>index.php?option=com_example&amp;view=cpanel</code>. Vous pouvez
utiliser "link" à la place.</td>
</tr>
</tbody>
</table>

La valeur à l'intérieur de la balise est le nom du menu. À La différence
de ce qui se faisait pour Joomla! 1.5, vous ne pouvez pas utiliser une
chaîne de caractère dans votre langue naturelle. Par exemple, si vous
renseignez "Composant d'exemple" au lieu de COM_EXAMPLE, votre nom de
composant qui s'afficherait en titre serait "composant-d-exemple" et
vous seriez dans l'impossibilité de le traduire. Pour réaliser une
traduction, vous devez créer un fichier nommé
`fr-FR.com_example.sys.ini` **(NDT : avant de pouvoir créer le fichier
fr-FR, il convient de créer le fichier d'origine en-GB)** dans
`administrator/languages/fr-FR` (vous pouvez utiliser la balise manifest
pour qu'il soit copié lors de l'installation) ou dans
`administrator/components/com_example/language/fr-FR`. Dans ce dernier
cas, vous ne devez pas inclure le fichier de traduction dans la balise
balise. Tant que vous aurez placé le répertoire de langue dans vos
balises , il sera copié lors de l'installation du composant.

Le contenu de ce fichier devrait être :

```ini
    COM_EXAMPLE="Example Component"
    COM_EXAMPLE_SUBMENU_ANOPTION="Another Option"
    COM_EXAMPLE_SUBMENU_VIEWNAME="Another View"
```

Notez que les chaînes de caractères de langues doivent être contenues à
l'intérieur de guillemets doubles, comme toutes les chaînes de
traductions habituelles de Joomla!.

Joomla! 1.6 and later sorts the Component menu items based on the actual
translation of the key you supply in your XML manifest. This means that
the sorting order is correct no matter what you call your translation
key and no matter which language the site is being displayed in.
Essentially, Joomla! 1.6 fixed the wrong sorting of the Components menu
experienced under Joomla! 1.5 for the majority (non-English speaking!)
of Joomla! users.

### Tableaux de bord

Specifies the details for displaying a dashboard for the component in
the Administrator area for the site.

- It will make a dashboard icon appear next to the administrator menu
  item for the component
- The dashboard icon will click through to display modules assigned to
  the cpanel-example administrator module position
- The title and icon defined in the XML file will be used as the header
  and icon at the top of the component's dashboard page.

```xml
  <dashboards>
		<dashboard title="COM_EXAMPLE_DASHBOARD_TITLE" icon="icon-lock">example</dashboard>
	</dashboards>
```

### Configuration

<img
src="https://docs.joomla.org/images/thumb/4/41/Stop_hand_nuvola.svg.png/25px-Stop_hand_nuvola.svg.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/4/41/Stop_hand_nuvola.svg.png/38px-Stop_hand_nuvola.svg.png 1.5x, https://docs.joomla.org/images/4/41/Stop_hand_nuvola.svg.png 2x"
data-file-width="40" data-file-height="40" width="25" height="25"
alt="Stop hand nuvola.svg.png" />Warning!

La balise , enfant de la racine, décrit les options de configuration de
l'extension. En cas de besoin, les options seront affichées par le
gestionnaire concerné (gestionnaire de plugins, gestionnaire de modules
ou gestionnaire de templates). **Les options de configuration peuvent
également être définies dans un fichier séparé nommé `config.xml`. Son
élément racine doit être .**

Chaque groupe de champs

peut contenir un ou plusieurs éléments (champ), chacun représentant un
seul [champ de
formulaire](https://docs.joomla.org/form_field "Special:MyLanguage/form field")
avec une étiquette. Consultez les [types de champs pour formulaires
standards](https://docs.joomla.org/Standard_form_field_types "Special:MyLanguage/Standard form field types")
pour une liste des types de champs autorisés et un exemple de
définitions de champs de formulaire XML.

### Namespace

Specify the namespace to be used for autoloading the plugin. The given
namespace will autoload to the root directory of your extension by
default however you can optionally add a "path" attribute to the
namespace element to specify a subpath within your extensions root
directory.

### SQL

```sql
    <install>
        <sql>
            <file driver="mysql" charset="utf8">sql/example.install.sql</file>
        </sql>
    </install>
    <uninstall>
        <sql>
            <file driver="mysql" charset="utf8">sql/example.uninstall.sql</file>
        </sql>
    </uninstall>
```

Dans l'exemple ci-dessus, nous avons placé les fichiers SQL dans le
répertoire `admin/sql` du paquet d'installation. Vous devez inclure le
dossier `sql` dans les fichiers d'administration (tel que décrit dans
"les fichiers backend").

Vous pouvez exécuter SQL lors de l'installation et/ou la désinstallation
à l'aide des éléments et . Une balise doit apparaître comme un enfant de
ces éléments. peut contenir n'importe quel nombre d'éléments , chacun
devant définir un seul fichier SQL à exécuter. Leurs types de pilote de
base de données sont décrits par l'attribut `driver`, leurs jeux de
caractères par l'attribut `charset`.

#### Mise à jour du schéma SQL

Depuis la version 1.6, une balise est disponible et vous permet de
fournir une série de fichiers SQL pour mettre à jour le schéma actuel.

```sql
	<update>
		<schemas>
			<schemapath type="mysql">sql/updates/mysql</schemapath>
			<schemapath type="sqlsrv">sql/updates/sqlsrv</schemapath>
		</schemas>
	</update>
```

Par exemple, pour passer de la version `1.0.0` à la version `1.0.1` dans
une base de données **MySQL**, un fichier `1.0.1.sql` doit être créé
dans le dossier `sql/updates/mysql` et la balise du manifest doit être
mis à jour :

```sql
<version>1.0.1</version>
```

La structure finale du dossier sql ressemblera à ceci (dans l'hypothèse
d'une base de données **MySQL**)

    sql
     |-->example.install.sql
     |-->example.uninstall.sql
     |-->updates
         |-->mysql
            |-->1.0.1.sql

Des fichiers similaires doivent être créés pour les versions suivantes.

### Fichiers langue

Dans Joomla! 1.5, les développeurs d'extensions devaient placer les
fichiers de langue dans le dossier général des langues de Joomla! à
l'aide des balises ... comme indiqué ci-dessous. **Cette méthode peut
toujours être utilisée avec la version**.

```xml
<!-- Joomla! language tag -->
<languages folder="langfiles">
	<language tag="en-GB">en-GB.com_example.ini</language>
</languages>
```

Depuis Joomla! 1.6, il vous est conseillé de placer les fichiers langue
de votre extension dans le dossier même de votre extension. Joomla!
chargera alors automatiquement les fichiers de langue depuis votre
extension.

En plaçant les fichiers de langue dans le dossier de l'extension, vous
permettez ainsi de les isoler et donc de les protéger. Par exemple, dans
le cas ou un administrateur supprimerait une langue de son installation
Joomlaǃ. Dans ce cas, les fichiers de langue de l'extension ne seront
pas supprimés. Ils resteront alors en place et ne seront accessibles que
si la langue est installée à nouveau.

La structure du dossier de langue pour le frontend et le backend est
identique. Il convient de les placer dans la balise (par exemple
**en-GB**) pour chaque langue et dans le dossier comme suit :
**language/en-GB/**. Vous devez aussi spécifier ces dossiers pour le
frontend et le backend.

Dans votre manifest, il suffit simplement d'inclure votre dossier dans
la section fichiers. Les sous-répertoires pour chaque langue seront
alors automatiquement copiés. À l'intérieur des groupes , il vous suffit
d'ajouter une balise à côté des éléments dans le groupe , comme indiqué
dans cet exemple :

```xml
<files>
	<filename plugin="alpha">alpha.php</filename>
	<folder>sql</folder>
	<folder>language</folder>
</files>
```

Notez que les deux techniques peuvent fonctionner ensemble. Voici un
exemple du noyau :

```xml
<files>
	<filename plugin="languagecode">languagecode.php</filename>
	<filename>index.html</filename>
	<folder>language</folder>
</files>
<languages>
	<language tag="en-GB">language/en-GB/en-GB.plg_system_languagecode.ini</language>
	<language tag="en-GB">language/en-GB/en-GB.plg_system_languagecode.sys.ini</language>
</languages>
```

Les avantages pour cette solution sont les suivants :

Tous les fichiers *.ini* présents dans le dossier du noyau ont la
priorité sur les fichiers dans les dossiers langue des extensions. Par
exemple, s'il existe, un fichier `sys.ini` d'un dossier du noyau sera
toujours chargé dans le backend, sauf lors de l'installation d'une
extension qui contient son propre fichier `sys.ini` dans un dossier de
langue. Dans ce cas et uniquement ce cas, le fichier `sys.ini` dans le
dossier de l'extension affichera son contenu traduit au moment de
l'installation. C'est très pratique car un développeur peut avoir deux
fichiers `sys.ini` avec un contenu différent.

Il est également beaucoup plus facile pour un utilisateur ayant besoin
de trouver le fichier `.ini` d'une extension qui n'est pas disponible
dans sa langue de l'ajouter dans le dossier principal. Il n'y a aucun
risque qu'il ne soit supprimé en cas de désinstallation par erreur de
l'extension.

Veuillez consulter également :

- [Créer un pack langue hors du
  noyau](https://docs.joomla.org/J2.5:Making_non-core_language_packs "Special:MyLanguage/J2.5:Making non-core language packs")
- [Créer un pack langue pour une extension Joomla!
  2.5](https://docs.joomla.org/Creating_language_packs_for_extensions_in_Joomla_2.5 "Special:MyLanguage/Creating language packs for extensions in Joomla 2.5")

Au cours du développement, vous pouvez activer le système de débogage de
langue dans la configuration globale de Joomlaǃ. Vous pourrez découvrir
si un problème est levé. Depuis
<img src="https://docs.joomla.org/images/3/3f/Compat_icon_3_2.png"
decoding="async" data-file-width="40" data-file-height="17" width="40"
height="17" alt="Joomla 3.2" />, cela est nécessaire afin d'aider à
déboguer le en-GB qui est **toujours** chargé en premier lorsque l'on
est pas en mode de débogage et ainsi empêcher l'affichage des
constantes.

### Fichier de script

```xml
    <scriptfile>example.script.php</scriptfile>
```

Un **fichier de script** (code PHP qui est exécuté avant, pendant et/ou
après l'installation, la désinstallation ou la mise à jour) peut, en
option, être défini à l'aide d'une balise.

Ce fichier doit contenir une
classe nommée '`InstallerScript` *où est le nom de votre extension (par
exemple: com_nomducomposant, mod_nomdumodule, etc.). Pour les plugins,
il convient de nommer également le groupe (par exemple :
plgsystemnomduplugin). Les packages de bibliothèque ne prennent pas en
charge les fichiers scripts. La structure de la classe sera comme
suit:*


In <img src="https://docs.joomla.org/images/f/f4/Compat_icon_4_0.png"
decoding="async" data-file-width="40" data-file-height="17" width="40"
height="17" alt="Joomla 4.0" /> and later the structure of the class is
as follows:

```php
<?php

use Joomla\CMS\Installer\InstallerAdapter;

class com_componentnameInstallerScript
{
	/**
	 * Constructor
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 */
	public function __construct(InstallerAdapter $adapter)
	{
	}

	/**
	 * Called before any type of action
	 *
	 * @param   string  $route  Which action is happening (install|uninstall|discover_install|update)
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function preflight($route, InstallerAdapter $adapter)
	{
		return true;
	}

	/**
	 * Called after any type of action
	 *
	 * @param   string  $route  Which action is happening (install|uninstall|discover_install|update)
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function postflight($route, $adapter)
	{
		return true;
	}

	/**
	 * Called on installation
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function install(InstallerAdapter $adapter)
	{
		return true;
	}

	/**
	 * Called on update
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function update(InstallerAdapter $adapter)
	{
		return true;
	}

	/**
	 * Called on uninstallation
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 */
	public function uninstall(InstallerAdapter $adapter)
	{
		return true;
	}
}

?>
```

Note that since Joomla 3.6 Joomla has shipped a basic script that you
can use instead of shipping your own from scratch **JInstallerScript**
which contains various helper methods commonly used through the
community.

### Library Manifests

A simple library manifest might look like this:

A simple library manifest might look like this:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="library" method="upgrade" version="4.0">
    <name>My Test library.</name>
    <libraryname>mytest</libraryname>
    <files>
        <folder>Classes</folder>
        <folder>language</folder>
        <filename>mytest.php</filename>
    </files>
</extension>
```

This will install the library into the `JPATH_SITE/libraries/mytest`
folder.

What if your company has several libraries and you want to group them
together under folder `JPATH_SITE/libraries/mycompany`?

Simple - include your company name in the `libraryname` property of each
library like this:

```xml
    <libraryname>mycompany/mylibrary1</libraryname>
```

```xml
    <libraryname>mycompany/mylibrary2</libraryname>
```

These libraries will then be installed in the
`JPATH_SITE/libraries/mycompany/mylibrary1` and
`JPATH_SITE/libraries/mycompany/mylibrary2` folders.

Uninstalling `mylibrary1` will still leave `mylibrary2` installed on
your site.

When using `script files` with such company libraries the installer
class name should look like this:

```php
    class mycompanymylibrary1InstallerScript
```

```php
    class mycompanymylibrary2InstallerScript
```
### Serveurs de mise à jour

```xml
    <updateservers>
        <server type="extension" priority="1" name="Extension Update Site">http://example.com/extension.xml</server>
        <server type="collection" priority="2" name="Collection Update Site">http://example.com/collection.xml</server>
    </updateservers>
```

Les serveurs de mise à jour peuvent être définis dans les balises comme
enfant de la racine. Cet élément peut contenir une ou plusieurs balises
chacune avec la description depuis laquelle chercher les mises à jour.
Chaque rubrique permet de définir les attributs suivants :

<table class="wikitable">

<tbody>
<tr class="header">
<th>Attribut</th>
<th>Valeurs</th>
<th>Description</th>
</tr>
&#10;<tr class="odd">
<td>type</td>
<td><code>extension</code><br />
<code>collection</code></td>
<td>Le type de serveur de mise à jour.</td>
</tr>
<tr class="even">
<td>priority</td>
<td><em>integer</em></td>
<td>La priorité du serveur de mise à jour.</td>
</tr>
<tr class="odd">
<td>name</td>
<td><em>string</em></td>
<td>Le nom du serveur de mise à jour.</td>
</tr>
</tbody>
</table>

Pour plus d'informations :

- [Création d'une extension Joomla! - Ajout d'un serveur de mise à
  jour](https://docs.joomla.org/J2.5:Developing_a_MVC_Component/Adding_an_update_server "Special:MyLanguage/J2.5:Developing a MVC Component/Adding an update server")
- [Gestion des mises à jour de composants pour Joomla!
  2.5](https://docs.joomla.org/J2.5:Managing_Component_Updates "Special:MyLanguage/J2.5:Managing Component Updates")

### Supporting Download Keys

As of Joomla 4.0.0 users can enter their download keys in the Update
Sites list. This gives them a single place to manage the download keys.
When a user is going to update an extension, Joomla will check if there
is a download key. If there is a download key, Joomla will add the
download key to the update URL.

To support download keys you must include the `dlid` tag in the manifest
file. The `dlid` tag takes 2 arguments:

- prefix
- suffix

The `dlid` tag will look like this in your manifest file:

```xml
<dlid prefix="dlid=" suffix="&amp;dummy=my.zip"/>
```

The prefix will be added before the download key and the suffix after
the download key. Using the example above the full query added to the
download link will be:

    dlid=KEY&dummy=my.zip

The key is added before the `onInstallerBeforePackageDownload` event is
triggered, so the full URL will be passed to the event.

### Exemples

## Exemples

Pour un exemple concret, veuillez consulter <a
href="https://github.com/joomla/joomla-cms/blob/3.6.4/administrator/components/com_banners/banners.xml"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">le manifest pour le composant
bannière de la version Joomla! 3.6.4</a>.

D'autres exemples peuvent être trouvés dans le répertoire autonome des
liens web :

- <a
  href="https://github.com/joomla-extensions/weblinks/blob/master/src/administrator/components/com_weblinks/weblinks.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">manifest com_weblinks</a>
- <a
  href="https://github.com/joomla-extensions/weblinks/blob/master/src/modules/mod_weblinks/mod_weblinks.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">manifest mod_weblinks</a>
- <a
  href="https://github.com/joomla-extensions/weblinks/blob/master/src/plugins/search/weblinks/weblinks.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">manifest plg_search_weblinks</a>
- <a
  href="https://github.com/joomla/joomla-cms/blob/3.6.4/templates/protostar/templateDetails.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">manifest tpl_protostar (3.6.4)</a>
- <a
  href="https://github.com/joomla/joomla-cms/blob/3.6.4/administrator/language/en-GB/en-GB.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">manifest en-GB (3.6.4)</a>
