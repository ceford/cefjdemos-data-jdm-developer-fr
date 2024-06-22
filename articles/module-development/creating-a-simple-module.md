<!-- Filename: J4.x:Creating_a_Simple_Module / Display title: Création d'un module simple -->

Joomla!  4.x \>Didacticiel Créer un module simple pour Joomlaǃ 4

Il s'agit d'un didacticiel sur la façon de créer un module simple pour
Joomla Version 4x.

## Introduction

Joomlaǃ 4 propose cinq types de extensions ː

- **[Composants](https://docs.joomla.org/Component "Special:MyLanguage/Component")**
  Un composant est la partie principale du site. Un composant gère la
  manipulation des données ainsi que la saisie et le stockage dans la
  base de données. Un composant sur la plupart des sites est l'objet
  principal de la page.
- **[Modules](https://docs.joomla.org/Module "Special:MyLanguage/Module")**
  Un module est un ajout sur le site qui étend la fonctionnalité.
  Habituellement, un module occupe un espace secondaire de la page web
  et n'est pas considéré comme l'élément principal de la page. Il peut
  être affiché à différentes positions et vous pouvez choisir sur quels
  éléments de menu actifs il sera affiché. Les modules sont de petites
  extensions flexibles. Ils sont utilisés pour des petites parties de la
  page qui sont en général moins complexes et qu'il est possible de voir
  à travers différents composants.
- **[Plugins](https://docs.joomla.org/Plugin "Special:MyLanguage/Plugin")**
  Un plug-in manipule des données qui sont déjà générées par le système.
  Typiquement, il ne fonctionne pas de manière séparée du site. Il prend
  des données depuis d'autres sources (i.e. le contenu) et les manipule
  avant de les afficher. Un plug-in fonctionne typiquement de manière
  masquée.
- **[Langues](https://docs.joomla.org/Languages "Special:MyLanguage/Languages")**
  Les extensions les plus simples sont certainement les langues. Par
  essence, les fichiers du paquet de langue consistent en des paires
  clé/valeur, qui fournissent la traduction des chaînes de texte
  statiques, assignées au sein du code source de Joomlaǃ.
- **[Templates](https://docs.joomla.org/Templates "Special:MyLanguage/Templates")**
  Un template génère le design de votre site Web généré par Joomlaǃ.

Joomla! 4 est construit en utilisant cinq applications différentes :

- Installation (utilisée pour installer Joomlaǃ et supprimée apr̠ès
  installation) ;
- Administration (backend - utilisée pour gérer le contenu) ;
- Publique ou site (frontend - utilisée pour afficher le contenu) ;
- CLI (utilisée pour accéder à Joomlaǃ en ligne de commande pour les
  tâches en arrière plan) ;
- API (services web - utilisée pour créer des interfaces API pour
  accéder au contenu de manière programmatique) ;

L'application d'installation est seulement utilisée une fois. Les
interfaces d'administration et publique sont utilisées au travers des
concepts de *composants* avec de *modules*. Chaque module a un seul
point d'entrée situé dans les *modules* et conformément au répertoire
*administrator/modules*. Ce point d'entrée est appelé
*`mod_nommodule/mod_nommodule.php`* (le préfixe *mod\_* est là pour des
raisons historiques). Le point d'entrée pour le module de connexion est
par exemple */mod_login/mod_login.php*.

### Pré-requis

Vous avez besoin de Joomla! 4.x pour ce didacticiel l
(<a href="https://github.com/joomla/joomla-cms/releases"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">Téléchargez la dernière version
ici</a>)

Vous pouvez télécharger Joomla! 4 sur
<a href="https://github.com/joomla/joomla-cms/releases"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">GitHub</a>, sur le
<a href="https://developer.joomla.org/nightly-builds.html"
class="external text" target="_blank" rel="noreferrer noopener">site des
développeurs</a> ou vous pouvez créer un site gratuit sur
<a href="https://launch.joomla.org" class="external free"
target="_blank" rel="noreferrer noopener">https://launch.joomla.org</a>.

## Créer un module simple / développer un module de base - Partie 1

Vous pouvez voir de nombreux exemples de modules dans l'installation
native de Joomla. Par exemple :

- Menus
- Dernières informations
- Formulaire de connexion
- et bien d'autres.

Ce didacticiel vous explique comment créer un module basique. Grâce à
lui, vous apprendrez la structure de fichiers de base d'un module. Cette
structure de base peut ensuite être étendue afin de créer des modules
plus élaborés.

### Structure de fichier

Il existe plusieurs fichiers de base qui sont utilisés dans le modèle
standard de développement de modules :

- `mod_foo.php` - Ce fichier est le point d'entrée principal pour le
  module. Il va effectuer toute routine d'initialisation nécessaire,
  appeler les routines helper pour collecter les données nécessaires et
  inclure le template à afficher pour le rendu du module.
- `mod_foo.xml` - Ce fichier contient les informations sur le module. Il
  définit les fichiers à installer par l'installateur Joomla! et
  spécifie les paramètres de configuration du module.
- `tmpl/default.php` - C'est le template du module. Ce fichier va
  prendre les données recueillies par mod_foo.php et générer le code
  HTML à afficher sur la page.
- `language/en-GB/mod_foo.ini` et `language/en-GB/mod_foo.sys.ini`- Ce
  sont les fichiers qui fournissent le texte en anglais.

### Création de mod_foo.php

Le fichier `mod_foo.php` exécute les tâches suivantes :

- Importer la classe `ModuleHelper` dans le contexte actuel. Nous en
  aurons besoin plus tard pour afficher la sortie.
- Inclure le template pour afficher la sortie.

La classe helper est importée dans notre périmètre courant au début du
fichier.

```php
    use Joomla\CMS\Helper\ModuleHelper;
```

Enfin, nous incluons le template pour afficher la sortie via

```php
    require ModuleHelper::getLayoutPath('mod_foo', $params->get('layout', 'default'));
```

#### Fichier mod_foo.php complet

Le fichier `mod_foo.php` complet se présente ainsi :

```php
<?php
/**
 * @package    [PACKAGE_NAME]
 *
 * @author     [AUTHOR] <[AUTHOR_EMAIL]>
 * @copyright  [COPYRIGHT]
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 * @link       [AUTHOR_URL]
 */

// No direct access to this file
defined('_JEXEC') or die;

use Joomla\CMS\Helper\ModuleHelper;

require ModuleHelper::getLayoutPath('mod_foo', $params->get('layout', 'default'));
```

*A noter* ː Avec Joomlaǃ 3x, vous utilisiez habituellement une ligne
telle que
`$moduleclass_sfx = htmlspecialchars($params->get('moduleclass_sfx'));`.
Vous n'aurez plus besoin de cela. Voir cette PR ː
<a href="https://github.com/joomla/joomla-cms/pull/17447"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms/pull/17447</a>.

La seule ligne que nous n'ayons pas encore expliquée est la première
ligne `defined('_JEXEC') or die;`. Cette ligne vérifie que ce fichier
est inclus dans une application Joomlaǃ. Cela est nécessaire pour éviter
des injections frauduleuses de variables et d'autres problèmes de
sécurité.

### Création du fichier tmpl/default.php

Le fichier `default.php` est le template qui affiche le rendu du module.

#### Fichier tmpl/default.php complet

Le code du fichier tmpl/default.php se présente ainsi :

```php
<?php
/**
 * @package    [PACKAGE_NAME]
 *
 * @author     [AUTHOR] <[AUTHOR_EMAIL]>
 * @copyright  [COPYRIGHT]
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 * @link       [AUTHOR_URL]
 */

// No direct access to this file
defined('_JEXEC') or die;

echo '[PROJECT_NAME]';
```

Il est important de noter que le fichier de template a la même portée
que le fichier `mod_foo.php`. Cela signifie qu'une variable peut être
définie dans le fichier `mod_foo.php` puis être utilisée dans le fichier
de template sans déclarations supplémentaires ou appels de fonction.

### Création du fichier mod_foo.xml

Le fichier `mod_foo.xml` est le fichier d'installation. La plupart des
entrées sont explicites.

#### Fichier mod_foo.xml complet

Le code du fichier `mod_foo.xml` se présente ainsi :

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="module" version="4.0" client="site" method="upgrade">
    <name>MOD_FOO</name>
    <creationDate>[DATE]</creationDate>
    <author>[AUTHOR]</author>
    <authorEmail>[AUTHOR_EMAIL]</authorEmail>
    <authorUrl>[AUTHOR_URL]</authorUrl>
    <copyright>[COPYRIGHT]</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <version>1.0</version>
    <description>MOD_FOO_XML_DESCRIPTION</description>
    <files>
        <filename module="mod_foo">mod_foo.php</filename>
        <folder>tmpl</folder>
        <folder>language</folder>
        <filename>mod_foo.xml</filename>
    </files>
</extension>
```

### Création des fichiers de langue

Les fichiers `language/en-GB/mod_foo.ini`
et`language/en-GB/mod_foo.sys.ini` sont utilisés pour traduire le texte
dans le frontend et dans le backend. Notez que la structure des fichiers
de langue a été mise à jour dans Joomla 4, et les préfixes de langue sur
les fichiers individuels dans un dossier de langue ne sont plus
nécessaires.

Le code pour `language/en-GB/en-GB.mod_foo.sys.ini` se présente ainsi :

```ini
    MOD_FOO="[PROJECT_NAME]"
    MOD_FOO_XML_DESCRIPTION="Foo Module"
```

Le code pour `language/en-GB/en-GB.mod_foo.ini` se présente ainsi :

```ini
    MOD_FOO="[PROJECT_NAME]"
    MOD_FOO_XML_DESCRIPTION="Foo Module"
```

Le fichier `.sys.ini` est utilisé pour traduire la description de
l'extension lors de l'installation, alors que le fichier `.ini` est
utilisé pour traduire le autres chaînes de caractères et la description
lors de l'utilisation de l'extension.

Consultez cette
<a href="https://docs.joomla.org/Specification_of_language_files"
class="new"
title="Special:MyLanguage/Specification of language files (page does not exist)">page</a>
pour plus d'informations sur les fichiers de langue.

## Tester votre module

Maintenant, vous pouvez zipper tous les fichiers et les installer via le
gestionnaire d'extensions de Joomla.
Après cela, vous pouvez choisir votre module dans le gestionnaire de
modules, lorsque vous créez un nouveau module de site.

<img src="https://docs.joomla.org/images/9/99/Moduletutorial1-fr.png"
decoding="async" data-file-width="700" data-file-height="348"
width="700" height="348" alt="Moduletutorial1-fr.png" />
<img src="https://docs.joomla.org/images/e/ef/Moduletutorial2-fr.png"
decoding="async" data-file-width="700" data-file-height="233"
width="700" height="233" alt="Moduletutorial2-fr.png" />

Dans le site public, vous allez voir votre module comme affiché dans
l'image suivante.

<img src="https://docs.joomla.org/images/e/ea/Moduletutorial3-fr.png"
decoding="async" data-file-width="700" data-file-height="396"
width="700" height="396" alt="Moduletutorial3-fr.png" />

## Conclusion

Le développement de modules pour Joomla est un processus assez simple.
En utilisant les techniques de ce didacticiel, une variété infinie de
module peut être développée sans peine.

Vous pouvez trouver des modèles standards ici ː

- <a
  href="https://github.com/joomla-extensions/boilerplate/tree/master/module"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla-extensions/boilerplate/tree/master/module</a>

Retrouvez les fichiers d'exemple de ce didacticiel à cette adresse :

- <a
  href="https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules</a>

## Ajouter une classe helper utilisant les espaces de noms - Partie 2

### Pré-requis

Vous avez besoin de Joomla! 4.x pour ce didacticiel (au moment de
l'écriture de cet article, Joomla! 4.0.0-alpha6-dev).

## Espaces de noms

Avec PHP 5.3, les espaces de noms apparaissent en tant que tel. Depuis
longtemps utilisées dans d'autres langages de programmation, ces petites
structures nous aident également maintenant pour la clarté du code.

Les espaces de noms sont des zones séparées dans lesquelles certains
éléments logiques (dans notre cas, des classes, des interfaces, des
fonctions, et des constantes) peuvent exister. Ces zones fournissent de
l'encapsulation de code et évitent des conflits de noms.

Regardons maintenant comment les utiliser dans Joomlaǃ 4. Nous allons
utiliser le fichier helper pour cela.

### Structure de fichier

Nous allons créer / modifier les fichiers suivants :

- `Helper/FooHelper.php` - C'est le fichier que nous allons utiliser en
  tant qu'exemple. Nous devons le créer.
- `mod_foo.php` - Dans ce fichier, nous allons principalement charger
  l'espace de noms.
- `tmpl/default.php` - Dans ce fichier, nous allons démontrer comment
  les données du helper peuvent être affichées.
- `mod_foo.xml` - Nous allons créer un nouveau répertoire avec un
  nouveau fichier. Ceci doit être installé durant l'installation. Pour
  cela, nous devons les spécifier dans ce fichier.

### Création du fichier Helper/FooHelper.php

Notre nouvelle classe helper doit appartenir à un espace de noms. Nous
réalisons cela avec le code suivant. Il est important que cette ligne
soit au début du fichier.

```php
    namespace Joomla\Module\Foo\Site\Helper;
```

Ensuite, nous créons une classe simple avec une méthode simple. Bien
sûr, vous pouvez faire beaucoup plus que cela. Regardez les méthodes du
cœur de Joomlaǃ. Vous y trouverez de nombreux exemples. Ils vous donnent
de première main quelles possibilités s'offrent à vous.

#### Fichier Helper/FooHelper.php complet

Le fichier `FooHelper.php` complet se présente ainsi :

```php
<?php
/**
 * @package     Joomla.Site
 * @subpackage  mod_foo
 *
 * @copyright   Copyright (C) 2005 - 2019 Open Source Matters, Inc. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace Joomla\Module\Foo\Site\Helper;

// No direct access to this file
defined('_JEXEC') or die;

/**
 * Helper for mod_foo
 *
 * @since  4.0
 */
class FooHelper
{
	/**
	 * Retrieve foo test
	 *
	 * @param   Registry        $params  The module parameters
	 * @param   CMSApplication  $app     The application
	 *
	 * @return  array
	 */
	public static function getText()
	{
		return 'FooHelpertest';
	}
}
```

### Editing mod_foo.php

Our new helper class is imported to our current scope at the beginning of the file.

```php
    use Joomla\Module\Foo\Site\Helper\FooHelper;
```

Last use the helper file for testing if it is loaded correctlyː

```php
    $test  = FooHelper::getText($params, $app);
```

#### Fichier mod_foo.php complet

Le fichier `mod_foo.php` complet se présente ainsi :

```php
<?php
/**
 * @package    [PACKAGE_NAME]
 *
 * @author     [AUTHOR] <[AUTHOR_EMAIL]>
 * @copyright  [COPYRIGHT]
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 * @link       [AUTHOR_URL]
 */

// No direct access to this file
defined('_JEXEC') or die;

use Joomla\CMS\Helper\ModuleHelper;
use Joomla\Module\Foo\Site\Helper\FooHelper;

$test  = FooHelper::getText();

require ModuleHelper::getLayoutPath('mod_foo', $params->get('layout', 'default'));
```

### Modification du fichier tmpl/default.php

Dans ce fichier vous réalisez seulement un petit changement pour
démontrer, sur le site public, que votre fichier de helper fonctionne
correctement. Vous ajoutez uniquement la valeur de la variable à la fin
de la sortie actuelle.

```php
    echo '[PROJECT_NAME]' . $test;
```

#### Fichier tmpl/default.php complet

Le fichier `tmpl/default.php` complet se présente ainsi :

```php
<?php
/**
 * @package    [PACKAGE_NAME]
 *
 * @author     [AUTHOR] <[AUTHOR_EMAIL]>
 * @copyright  [COPYRIGHT]
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 * @link       [AUTHOR_URL]
 */

// No direct access to this file
defined('_JEXEC') or die;

echo '[PROJECT_NAME]' . $test;
```

### Modification du fichier mod_foo.xml

Tout d'abord, vous devez ajouter une ligne dans le fichier, afin que
l'espace de noms soit automatiquement connu de Joomlaǃ. Après ce
changement, vous devez réinstaller votre module. Un simple changement
dans un module déjà installé n'est pas suffisant. Si vous réalisez un
développement en local, vous pouvez aussi supprimer les fichiers
libraries/autoload_psr4.php et ce dernier sera automatiquement recréé.
Après l'insertion et l'installation, l'espace de noms est connu par le
chargeur `JPATH_LIBRARIES . '/autoload_psr4.php'`.

```xml
<namespace>Joomla\Module\Foo</namespace>
```

Joomla! devrait copier votre fichier helper lors de l'installation du
module. Cela signifie que Joomla! doit connaître votre fichier helper.
Pour réaliser cela, vous devez ajouter la ligne suivante dans votre
fichier XML.

```xml
<folder>Helper</folder>
```

#### Fichier mod_foo.xml complet

Le fichier `mod_foo.xml` complet se présente ainsi :

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="module" version="4.0" client="site" method="upgrade">
    <name>MOD_FOO</name>
    <creationDate>[DATE]</creationDate>
    <author>[AUTHOR]</author>
    <authorEmail>[AUTHOR_EMAIL]</authorEmail>
    <authorUrl>[AUTHOR_URL]</authorUrl>
    <copyright>[COPYRIGHT]</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <version>1.0</version>
    <description>MOD_FOO_XML_DESCRIPTION</description>
    <namespace>Joomla\Module\Foo</namespace>
    <files>
        <filename module="mod_foo">mod_foo.php</filename>
        <folder>tmpl</folder>
        <folder>Helper</folder>
        <folder>language</folder>
        <filename>mod_foo.xml</filename>
    </files>
</extension>
```

## Tester votre module

Maintenant, vous pouvez zipper tous les fichiers et les installer via le
gestionnaire d'extensions de Joomla. Après cela, vous pouvez choisir
votre module dans le gestionnaire de modules, lorsque vous créez un
nouveau module de site.

Dans le site public, vous verrez le module tel qu'affiché sur l'image
suivante. Le texte fournit par votre fichier helper est affiché.

<img src="https://docs.joomla.org/images/5/5e/Moduletutorial23-fr.png"
decoding="async" data-file-width="700" data-file-height="383"
width="700" height="383" alt="Moduletutorial23-fr.png" />

## Conclusion

Comme vous pouvez le voir, lorsque vous avez un peu touché aux espaces
de noms, ils ne sont plus du tout aussi compliqués que cela. Dans des
grands projets, ils peuvent apporter de nombreux bénéfices. Cependant,
nous devons planifier les espaces de noms, car sinon cela consommera
rapidement beaucoup de temps supplémentaire.

Vous pouvez trouver des modèles standards ici ː

- <a
  href="https://github.com/joomla-extensions/boilerplate/tree/master/module"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla-extensions/boilerplate/tree/master/module</a>

Retrouvez les fichiers d'exemple de ce didacticiel à cette adresse :

- <a
  href="https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules</a>

## Personnalisation ː Ajout de paramètres au travers des champs de formulaires - Partie 3

### Pré-requis

Vous avez besoin de Joomla! 4.x pour ce didacticiel (au moment de
l'écriture de cet article, Joomla! 4.0.0-alpha6-dev).

## Personnalisation avec les champs de formulaire et les paramètres

Dans Joomlaǃ, les champs de formulaire mettent en œuvre de nombreuses
possibilités de personnalisation et, pour les modules, c'est la seule
façon de permettre à l'utilisateur de configurer le module selon les
besoins de son site.

Pour montrer ce cas, nous allons étendre notre exemple précédent en
utilisant un champ url pour insérer un nom de domaine que nous pourrons
utiliser en tant que lien dans notre site public. Pour permettre cela,
nous allons utiliser le [Type de champ de formulaire pour
URL](https://docs.joomla.org/URL_form_field_type "Special:MyLanguage/URL form field type").

Ensuite, nous insérons des paramètres qui permettent d'utiliser les
fonctionnalités standards de Joomlaǃ.

Jetons un oeil sur la façon de les utiliser dans Joomlaǃ 4.

### Structure de fichier

Nous allons modifier les fichiers suivants :

- `mod_foo.xml` - C'est dans ce fichier que sont ajoutés les champs.
- `tmpl/default.php` - Dans ce fichier, nous montrons comment les
  données du champ peuvent être utilisées.
- `language/en-GB/en-GB.mod_foo.ini` - Dans ce fichier, nous utilisons
  une chaîne de langue afin que le champ puisse avoir un libellé traduit
  dans les différentes langues. \[NDLT\] Vous pourrez également
  compléter le fichier `language/fr-FR/fr-FR.mod_foo.ini`.

### Modification du fichier mod_foo.xml

Tout d'abord, nous définissons un paramètre personnalisé.

```xml
<field
    name="domain"
    type="url"
    label="MOD_FOO_FIELD_URL_LABEL"
    filter="url"
/>
```

Ensuite, nous insérons les champs par défaut de Joomlaǃ, afin de pouvoir
utiliser le cache, les suffixes de classe de modules et les layouts.

```xml
<field
    name="layout"
    type="modulelayout"
    label="JFIELD_ALT_LAYOUT_LABEL"
    class="custom-select"
/>

<field
    name="moduleclass_sfx"
    type="textarea"
    label="COM_MODULES_FIELD_MODULECLASS_SFX_LABEL"
    rows="3"
/>

<field
    name="cache"
    type="list"
    label="COM_MODULES_FIELD_CACHING_LABEL"
    default="0"
>
    <option value="1">JGLOBAL_USE_GLOBAL</option>
    <option value="0">COM_MODULES_FIELD_VALUE_NOCACHING</option>
</field>

<field
    name="cache_time"
    type="number"
    label="COM_MODULES_FIELD_CACHE_TIME_LABEL"
    default="0"
/>

<field
    name="cachemode"
    type="hidden"
    default="itemid"
>
    <option value="itemid"></option>
</field>
```

#### Fichier mod_foo.xml complet

Le fichier `mod_foo.xml` complet se présente ainsi :

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="module" version="4.0" client="site" method="upgrade">
    <name>MOD_FOO</name>
    <creationDate>[DATE]</creationDate>
    <author>[AUTHOR]</author>
    <authorEmail>[AUTHOR_EMAIL]</authorEmail>
    <authorUrl>[AUTHOR_URL]</authorUrl>
    <copyright>[COPYRIGHT]</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <version>1.0</version>
    <description>MOD_FOO_XML_DESCRIPTION</description>
    <namespace>Joomla\Module\Foo</namespace>
    <files>
        <filename module="mod_foo">mod_foo.php</filename>
        <folder>tmpl</folder>
        <folder>Helper</folder>
        <folder>language</folder>
        <filename>mod_foo.xml</filename>
    </files>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="domain"
                    type="url"
                    label="MOD_FOO_FIELD_URL_LABEL"
                    filter="url"
                />
            </fieldset>
            <fieldset name="advanced">
                <field
                    name="layout"
                    type="modulelayout"
                    label="JFIELD_ALT_LAYOUT_LABEL"
                    class="custom-select"
                />
                <field
                    name="moduleclass_sfx"
                    type="textarea"
                    label="COM_MODULES_FIELD_MODULECLASS_SFX_LABEL"
                    rows="3"
                />
                <field
                    name="cache"
                    type="list"
                    label="COM_MODULES_FIELD_CACHING_LABEL"
                    default="0"
                >
                    <option value="1">JGLOBAL_USE_GLOBAL</option>
                    <option value="0">COM_MODULES_FIELD_VALUE_NOCACHING</option>
                </field>
                <field
                    name="cache_time"
                    type="number"
                    label="COM_MODULES_FIELD_CACHE_TIME_LABEL"
                    default="0"
                />
                <field
                    name="cachemode"
                    type="hidden"
                    default="itemid"
                >
                    <option value="itemid"></option>
                </field>
            </fieldset>
        </fields>
    </config>
</extension>
```

### Modification du fichier tmpl/default.php

Nous pouvons utiliser la valeur du paramètre pour créer un lien
hypertexte dans le site public. Nous pouvons accéder à la valeur via la
variable \$params.

```php
    $domain = $params->get('domain', 'https://www.joomla.org');
```

Ensuite nous utilisons cette variable pour créer le lien.

```php
<a href="<?php echo $domain; ?>">
	<?php echo '[PROJECT_NAME]' . $test; ?>
</a>
```

#### Fichier tmpl/default.php complet

Le fichier `tmpl/default.php` complet se présente ainsi :

```php
<?php
/**
 * @package    [PACKAGE_NAME]
 *
 * @author     [AUTHOR] <[AUTHOR_EMAIL]>
 * @copyright  [COPYRIGHT]
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 * @link       [AUTHOR_URL]
 */

// No direct access to this file
defined('_JEXEC') or die;

$domain = $params->get('domain', 'https://www.joomla.org');
?>

<a href="<?php echo $domain; ?>">
	<?php echo '[PROJECT_NAME]' . $test; ?>
</a>
```

### Modification du fichier language/en-GB/en-GB.mod_foo.ini

Dans ce fichier, nous pouvons spécifier le texte pour la version
anglaise (English) du libellé du champ.

```ini
    MOD_FOO_FIELD_URL_LABEL="Url"
```

#### Fichier language/en-GB/en-GB.mod_foo.ini complet

Le fichier `language/en-GB/en-GB.mod_foo.ini` complet se présente
ainsi :

```ini
    MOD_FOO="[PROJECT_NAME]"
    MOD_FOO_XML_DESCRIPTION="Foo Module"
    MOD_FOO_FIELD_URL_LABEL="Url"
```

## Tester votre module

Maintenant, vous pouvez zipper tous les fichiers et les installer via le
gestionnaire d'extensions de Joomlaǃ. Après cela, vous pouvez choisir
votre module dans le gestionnaire de modules, lorsque vous créez un
nouveau module de site.

Dans l'interface d'administration, vous allez voir votre module comme
affiché sur l'image suivante.

Dans l'onglet Module, vous verrez le champ personnalisé qui vous permet
de saisir l'url de domaine. Le texte pour le libellé est extrait du
fichier de langue.

<img src="https://docs.joomla.org/images/4/49/Moduletutorial31-fr.png"
decoding="async" data-file-width="700" data-file-height="320"
width="700" height="320" alt="Moduletutorial31-fr.png" />

Dans l'onglet Paramètres avancés, vous verrez tous les paramètres par
défaut sauf le mode de mise en cache qui est un paramètre masqué.

<img src="https://docs.joomla.org/images/b/b6/Moduletutorial32-fr.png"
decoding="async" data-file-width="700" data-file-height="318"
width="700" height="318" alt="Moduletutorial32-fr.png" />

Dans le site public, vous verrez le module tel qu'affiché sur l'image
suivante. Le texte fournit par votre fichier helper est affiché et vous
pouvez voir un lien hypertext.

<img src="https://docs.joomla.org/images/4/4f/Moduletutorial33-fr.png"
decoding="async" data-file-width="700" data-file-height="216"
width="700" height="216" alt="Moduletutorial33-fr.png" />

## Conclusion

Les champs de formulaire donnent à l'utilisateur une manière aisée de
personnaliser le module suivant les configurations de leur site. Ceci
permet d'accroître le périmètre des modules.

Vous pouvez trouver des modèles standards ici ː

- <a
  href="https://github.com/joomla-extensions/boilerplate/tree/master/module"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla-extensions/boilerplate/tree/master/module</a>

Retrouvez les fichiers d'exemple de ce didacticiel à cette adresse :

- <a
  href="https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules</a>

Voir cette demande pour d'éventuelles évolutions futures ː

- <a href="https://github.com/joomla/joomla-cms/pull/23553"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms/pull/23553</a>

## Utilisez les scripts d'installation, de mise à jour et de désinstallation - Partie 4

### Pré-requis

Vous avez besoin de Joomla! 4.x pour ce didacticiel (au moment de
l'écriture de cet article, Joomla! 4.0.0-alpha6-dev).

### Scripts

L'installation, la mise à jour et la désinstallation d'un module peuvent
nécessiter des opérations qui ne peuvent être réalisées par les
opérations de base décrites dans le fichier XML principal. Joomla
propose une nouvelle approche pour résoudre ce problème. Elle consiste
en l'utilisation d'un fichier de script PHP contenant une classe
utilisant cinq méthodes :

- preflight, est exécuté avant *install* et *update*
- install
- update
- uninstall
- postflight, exécuté après *install* et *update*

Regardons maintenant comment les utiliser dans Joomlaǃ 4. Nous allons
utiliser le fichier helper pour cela.

### Structure de fichier

Nous allons créer / modifier les fichiers suivants :

- `script.php` - Nous utilisons ici ce fichier en tant qu'exemple. Nous
  devons le créer.
- `language/en-GB/en-GB.mod_foo.sys.ini` - Nous allons ajouter du texte
  dans ce fichier. \[NDLT\] Vous pouvez aussi compléter le fichier
  `language/fr-FR/fr-FR.mod_foo.sys.ini` avec la traduction des chaînes
  de caractères.
- `mod_foo.xml` - Nous créons un nouveau fichier qui devra être installé
  lors de l'installation. Pour réaliser cela, c'est dans ce fichier que
  nous allons le spécifier.

### Création du fichier script.php

Écrire un script d'extension consiste à déclarer une classe dont le nom
est `mod_ModuleNameInstallerScript` avec ces 5 méthodes. Voir les
commentaires dans le fichier pour plus d'information. Dans ces
commentaires, j'explique à quel moment cette méthode est appelée.

Dans cet exemple, j'utilise seulement du texte pour montrer quand telle
ou telle méthode va être exécutée. En plus, je vous montre comment
vérifier les besoins minimaux. Bien sûr, vous pouvez faire bien plus que
cela. Par exemple, vous pouvez supprimer des fichiers qui ne sont plus
utilisés dans une nouvelle version de votre module. Ceci peut être vu
dans le fichier. Une autre idée pour ce fichier est de créer des
contenus d'exemples, d'afficher un message de succès indiquant le numéro
de la version actuellement installée ou bien vous pouvez faire une
redirection après une installation réussie vers la page de la
configuration du module.

#### Fichier script.php complet

Le fichier `script.php` complet se présente ainsi :

```php
<?php
/**
 * @package    [PACKAGE_NAME]
 *
 * @author     [AUTHOR] <[AUTHOR_EMAIL]>
 * @copyright  [COPYRIGHT]
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 * @link       [AUTHOR_URL]
 */

// No direct access to this file
defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;
use Joomla\CMS\Log\Log;

/**
 * Script file of Foo module
 */
class mod_fooInstallerScript {

    /**
     * Extension script constructor.
     *
     * @return  void
     */
    public function __construct() {
        $this->minimumJoomla = '4.0';
        $this->minimumPhp = JOOMLA_MINIMUM_PHP;
    }

    /**
     * Method to install the extension
     *
     * @param   InstallerAdapter  $parent  The class calling this method
     *
     * @return  boolean  True on success
     */
    function install($parent) {
        echo Text::_('MOD_FOO_INSTALLERSCRIPT_INSTALL');

        return true;
    }

    /**
     * Method to uninstall the extension
     *
     * @param   InstallerAdapter  $parent  The class calling this method
     *
     * @return  boolean  True on success
     */
    function uninstall($parent) {
        echo Text::_('MOD_FOO_INSTALLERSCRIPT_UNINSTALL');

        return true;
    }

    /**
     * Method to update the extension
     *
     * @param   InstallerAdapter  $parent  The class calling this method
     *
     * @return  boolean  True on success
     */
    function update($parent) {
        echo Text::_('MOD_FOO_INSTALLERSCRIPT_UPDATE');

        return true;
    }

    /**
     * Function called before extension installation/update/removal procedure commences
     *
     * @param   string            $type    The type of change (install, update or discover_install, not uninstall)
     * @param   InstallerAdapter  $parent  The class calling this method
     *
     * @return  boolean  True on success
     */
    function preflight($type, $parent) {
        // Check for the minimum PHP version before continuing
        if (!empty($this->minimumPhp) && version_compare(PHP_VERSION, $this->minimumPhp, '<')) {
            Log::add(Text::sprintf('JLIB_INSTALLER_MINIMUM_PHP', $this->minimumPhp), Log::WARNING, 'jerror');

            return false;
        }

        // Check for the minimum Joomla version before continuing
        if (!empty($this->minimumJoomla) && version_compare(JVERSION, $this->minimumJoomla, '<')) {
            Log::add(Text::sprintf('JLIB_INSTALLER_MINIMUM_JOOMLA', $this->minimumJoomla), Log::WARNING, 'jerror');

            return false;
        }

        echo Text::_('MOD_FOO_INSTALLERSCRIPT_PREFLIGHT');
        echo $this->minimumJoomla . ' ' . $this->minimumPhp;

        return true;
    }

    /**
     * Function called after extension installation/update/removal procedure commences
     *
     * @param   string            $type    The type of change (install, update or discover_install, not uninstall)
     * @param   InstallerAdapter  $parent  The class calling this method
     *
     * @return  boolean  True on success
     */
    function postflight($type, $parent) {
        echo Text::_('MOD_FOO_INSTALLERSCRIPT_POSTFLIGHT');

        return true;
    }
}
```

### Modification du fichier language/en-GB/en-GB.mod_foo.sys.ini

Il n'y a pas grand-chose à expliquer ici. Écrivez le texte de la
traduction dans ce fichier. \[NDLT\] Faites le également dans le fichier
de la traduction `language/fr-FR/fr-FR.mod_foo.sys.ini`.

#### Fichier language/en-GB/en-GB.mod_foo.sys.ini complet

Le fichier `language/en-GB/en-GB.mod_foo.sys.ini` complet se présente
ainsi :

```ini
    MOD_FOO="[PROJECT_NAME]"
    MOD_FOO_XML_DESCRIPTION="Foo Module"
    MOD_FOO_INSTALLERSCRIPT_PREFLIGHT="Anything here happens before the installation/update/uninstallation of the module"
    MOD_FOO_INSTALLERSCRIPT_UPDATE="The module has been updated"
    MOD_FOO_INSTALLERSCRIPT_UNINSTALL="The module has been uninstalled"
    MOD_FOO_INSTALLERSCRIPT_INSTALL="The module has been installed"
    MOD_FOO_INSTALLERSCRIPT_POSTFLIGHT="Anything here happens after the installation/update/uninstallation of the module"
```

### Modification du fichier mod_foo.xml

Vous devez ajouter une ligne, de sorte que le script soit appelé
automatiquement dans Joomla.

```xml
<scriptfile>script.php</scriptfile>
```

#### Fichier mod_foo.xml complet

Le fichier `mod_foo.xml` complet se présente ainsi :

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="module" version="4.0" client="site" method="upgrade">
    <name>MOD_FOO</name>
    <creationDate>[DATE]</creationDate>
    <author>[AUTHOR]</author>
    <authorEmail>[AUTHOR_EMAIL]</authorEmail>
    <authorUrl>[AUTHOR_URL]</authorUrl>
    <copyright>[COPYRIGHT]</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <version>1.0</version>
    <description>MOD_FOO_XML_DESCRIPTION</description>
    <namespace>Joomla\Module\Foo</namespace>
    <scriptfile>script.php</scriptfile>
    <files>
        <filename module="mod_foo">mod_foo.php</filename>
        <folder>tmpl</folder>
        <folder>Helper</folder>
        <folder>language</folder>
        <filename>mod_foo.xml</filename>
    </files>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="domain"
                    type="url"
                    label="MOD_FOO_FIELD_URL_LABEL"
                    filter="url"
                />
            </fieldset>
            <fieldset name="advanced">
                <field
                    name="layout"
                    type="modulelayout"
                    label="JFIELD_ALT_LAYOUT_LABEL"
                    class="custom-select"
                />
                <field
                    name="moduleclass_sfx"
                    type="textarea"
                    label="COM_MODULES_FIELD_MODULECLASS_SFX_LABEL"
                    rows="3"
                />
                <field
                    name="cache"
                    type="list"
                    label="COM_MODULES_FIELD_CACHING_LABEL"
                    default="0"
                >
                    <option value="1">JGLOBAL_USE_GLOBAL</option>
                    <option value="0">COM_MODULES_FIELD_VALUE_NOCACHING</option>
                </field>
                <field
                    name="cache_time"
                    type="number"
                    label="COM_MODULES_FIELD_CACHE_TIME_LABEL"
                    default="0"
                />
                <field
                    name="cachemode"
                    type="hidden"
                    default="itemid"
                >
                    <option value="itemid"></option>
                </field>
            </fieldset>
        </fields>
    </config>
</extension>
```

## Tester votre module

Maintenant, vous pouvez zipper tous les fichiers et les installer via le
gestionnaire d'extensions de Joomlaǃ. Aussitôt après l'installation,
vous verrez l'information suivante. Ceci a été saisi dans le script.

<img src="https://docs.joomla.org/images/8/8d/Moduletutorial14-fr.png"
decoding="async" data-file-width="700" data-file-height="455"
width="700" height="455" alt="Moduletutorial14-fr.png" />

## Conclusion

Comme vous pouvez le voir, Joomla vous offre beaucoup pour rendre votre
extension facile à utiliser - dès le début.

Vous pouvez trouver des modèles standards ici ː

- <a
  href="https://github.com/joomla-extensions/boilerplate/tree/master/module"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla-extensions/boilerplate/tree/master/module</a>

Retrouvez les fichiers d'exemple de ce didacticiel à cette adresse :

- <a
  href="https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules</a>

## Utilisation du composant de mise à jour de Joomla : Ajout de la mise à jour automatique - Partie 5

### Pré-requis

Vous avez besoin de Joomla! 4.x pour ce didacticiel (au moment de
l'écriture de cet article, Joomla! 4.0.0-alpha6-dev).

### Composant de mise à jour de Joomla

Tout d'abord, veuillez lire le didacticiel sur la [gestion des mises à
niveau des composants avec Joomla! 2.5 - Partie
1](https://docs.joomla.org/J2.5:Managing_Component_Updates "Special:MyLanguage/J2.5:Managing Component Updates")
pour vous donner une idée du fonctionnement du processus de mise à jour
dans Joomlaǃ. Même si ce didacticiel a été écrit pour la version 2.5, le
processus n'a pas changé. Consultez également [Le déploiement d'un
serveur de mise à
jour](https://docs.joomla.org/Deploying_an_Update_Server "Special:MyLanguage/Deploying an Update Server")
(ce que nous allons mettre en oeuvre dans ce didacticiel).

### Structure de fichier

Nous allons créer / modifier les fichiers suivants :

- `mod_foo.xml` - Le seul moyen de mettre à jour notre module est de
  l'ajouter dans un serveur de mise à jour - par exemple
  `http://www.example.com/foo_update.xml` - dans le fichier xml.
- `foo_update.xml` - Ensuite nous devons créer un fichier XML pour le
  serveur de mise à jour à `http://www.example.com/foo_update.xml` pour
  que Joomlaǃ sache qu'une mise à jour est disponible.

#### Modification du fichier mod_foo.xml

Pour ajouter notre serveur de mise à jour, nous devons insérer les
lignes suivantes dans votre fichier XML ː

```xml
<updateservers>
    <server type="extension" priority="1" name="[PROJECT_NAME]">https://www.example.com/mod_foo.xml</server>
</updateservers>
```

#### Fichier mod_foo.xml complet

Le fichier `mod_foo.xml` complet se présente ainsi :

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="module" version="4.0" client="site" method="upgrade">
    <name>MOD_FOO</name>
    <creationDate>[DATE]</creationDate>
    <author>[AUTHOR]</author>
    <authorEmail>[AUTHOR_EMAIL]</authorEmail>
    <authorUrl>[AUTHOR_URL]</authorUrl>
    <copyright>[COPYRIGHT]</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <version>1.0</version>
    <description>MOD_FOO_XML_DESCRIPTION</description>
    <namespace>Joomla\Module\Foo</namespace>
    <files>
        <filename module="mod_foo">mod_foo.php</filename>
        <folder>tmpl</folder>
        <folder>Helper</folder>
        <folder>language</folder>
        <filename>mod_foo.xml</filename>
    </files>
    <updateservers>
        <server type="extension" priority="1" name="[PROJECT_NAME]">https://www.example.com/mod_foo.xml</server>
    </updateservers>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="domain"
                    type="url"
                    label="MOD_FOO_FIELD_URL_LABEL"
                    filter="url"
                />
            </fieldset>
            <fieldset name="advanced">
                <field
                    name="layout"
                    type="modulelayout"
                    label="JFIELD_ALT_LAYOUT_LABEL"
                    class="custom-select"
                />
                <field
                    name="moduleclass_sfx"
                    type="textarea"
                    label="COM_MODULES_FIELD_MODULECLASS_SFX_LABEL"
                    rows="3"
                />
                <field
                    name="cache"
                    type="list"
                    label="COM_MODULES_FIELD_CACHING_LABEL"
                    default="0"
                >
                    <option value="1">JGLOBAL_USE_GLOBAL</option>
                    <option value="0">COM_MODULES_FIELD_VALUE_NOCACHING</option>
                </field>
                <field
                    name="cache_time"
                    type="number"
                    label="COM_MODULES_FIELD_CACHE_TIME_LABEL"
                    default="0"
                />
                <field
                    name="cachemode"
                    type="hidden"
                    default="itemid"
                >
                    <option value="itemid"></option>
                </field>
            </fieldset>
        </fields>
    </config>
</extension>
```

Donc, maintenant que Joomlaǃ recherche les mises à jour de votre
extension - créons en une pour vérifier notre procédure. Mais tout
d'abord, nous devons créer le fichier `foo_update.xml`. Jusqu'ici nous
avons seulement décrit le serveur de mise à jour. Nous ne savons pas
encore si il y a une mise à jour. Dans le fichier `foo_update.xml`, nous
indiquons quand une nouvelle version est publiée et où elle peut être
téléchargée.

### Création du fichier foo_update.xml

Nous devons maintenant créer le fichier XML sur
`http://www.example.com/foo_update.xml` pour faire savoir à Joomlaǃ
qu'une mise à jour est disponible.

#### Fichier foo_update.xml complet

Le fichier `foo_update.xml` complet se présente ainsi :

```xml
<?xml version="1.0" ?>
<updates>
    <update>
        <name>Foo</name>
        <description>This is mod_foo 1.0.1</description>
        <element>mod_foo</element>
        <type>module</type>
        <version>1.0.1</version>
        <downloads>
            <downloadurl type="full" format="zip">http://www.example.com/mod_foo_101.zip</downloadurl>
        </downloads>
        <maintainer>Joomla</maintainer>
        <maintainerurl>http://www.example.com</maintainerurl>
        <targetplatform name="joomla" version="4.0"/>
        <client>site</client>
    </update>
</updates>
```

Après le dépôt de ce fichier à l'adresse
`https://www.example.com/mod_foo.xml` la mise à jour sera affichée dans
l'interface d'administration du site Joomlaǃ.

### Tester la mise à jour de votre module

Créez une copie du module dans son état actuel. Puis, modifiez le numéro
de version à 1.0.1. Et créez une archive zip avec tous les fichiers.
Ensuite, déposez votre fichier à l'adresse
`http://www.example.com/mod_foo_101.zip`. Dorénavant, vous pouvez mettre
à jour votre module via le serveur de mise à jour Joomlaǃ.

### Modules commerciaux

Notez que dans nos fichiers nous avons des liens vers les fichiers
d'extensions xml. Et à partir de ce fichier xml, nous avons une
localisation pour le fichier zip du module. Cela signifie que si
quelqu'un suivait cela en sens inverse, il pourrait trouver les sources
physiques du fichier zip de vos modules. Si vous ne le souhaitez pas,
vous pouvez trouver une solution dans ce PR ː
<a href="https://github.com/joomla/joomla-cms/pull/15185"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms/pull/15185</a>.

### Conclusion

Grâce au composant de mise à jour de Joomlaǃ, vous pouvez facilement
atteindre tous les utilisateurs et les informer de la mise à disposition
d'une nouvelle version. Même la mise à jour en elle-même est simple.

Chaque extension doit utiliser un serveur de mise à jour. Surtout pour
des raisons de sécurité.

Vous pouvez trouver des modèles standards ici ː

- <a
  href="https://github.com/joomla-extensions/boilerplate/tree/master/module"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla-extensions/boilerplate/tree/master/module</a>

Retrouvez les fichiers d'exemple de ce didacticiel à cette adresse :

- <a
  href="https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules</a>
