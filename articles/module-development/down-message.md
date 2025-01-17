<!-- Filename: J4.x:J4_Module_example_-_Mydownmsg / Display title: Exemple : Message de Site Hors Ligne -->

## Introduction

Cet article présente un module de site Joomla complet pour que les nouveaux développeurs puissent le démonter afin de voir sa structure et son fonctionnement. Il remplace un module précédent qui utilisait des conventions de codage plus anciennes. Celui-ci est conçu pour Joomla 5 et ultérieur.

Il existe des articles d'introduction sur les *Concepts Généraux* et les *Modules* dans la Documentation des Programmeurs Joomla et reproduits dans le Jdocmanual pour votre commodité. Cet article est similaire au [Module Basique JPD](jdocmanual?article=docus/modules/basic-module) mais possède quelques fonctionnalités supplémentaires.

## Objectif

Le module est conçu pour afficher un message temporaire dans l'une des plusieurs langues pendant une courte période avant que le site ne ferme pour maintenance du système. Cela donne aux utilisateurs l'opportunité de finir ce qu'ils font avant que la fermeture n'ait lieu.

Le nom du module est `mod_downmsg` et le message qu'il affiche en anglais est similaire à *Ce site fermera pour une courte période à 12:00 GMT*. L'heure et le message peuvent être choisis pour convenir à la fermeture imminente du site.

Le module peut être téléchargé depuis GitHub pour être installé ou examiné selon les besoins.

## Structure du code source

Le texte suivant est une illustration schématique de la structure du code source utilisé pour créer l'extension :

```
cefjdemos-plg-toc
|-- .vscode (folder for build settings)
|-- mod_downmsg (folder compressed to create an installable zip file)
    |-- help
        |-- en-GB
            |-- downmsg.html (this is Help screen used in the backend module edit form)
    |-- language
        |-- de-DE
            |-- mod_downmsg.ini (only frontend strings are included)
        |-- en-GB (language folder, kept with the extension code)
            |-- mod_downmsg.ini
            |-- mod_downmsg.sys.ini
        |-- fr-FR
            |-- mod_downmsg.ini (only frontend strings are included)
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Dispatcher (folder for extension specific code)
            |-- Dispatcher.php (the extension class code)
    |-- tmpl (folder for layouts)
        |-- default.php (the table of contents layout)
    |-- mod_downmsg.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (record of changes)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
|-- updates.xml (update server specification)
```

Voici la structure telle qu'elle apparaît dans l'IDE VSCode ou VSCodium :

![Plugin development file structure in vscodium](../../../en/images/modules/downmsg-module-vscodium.png)

## Le fichier manifeste

Le fichier manifeste contrôle les processus d'installation, de mise à jour et de désinstallation de l'extension :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="module" method="upgrade" client="site">
    <name>mod_downmsg</name>
    <creationDate>December 2024</creationDate>
    <author>Clifford E Ford</author>
    <authorEmail>cliff@xxx.yyy.co.uk</authorEmail>
    <authorUrl>http://jdocmanual.org/</authorUrl>
    <copyright>Copyright (C) 2024 Clifford E Ford, All rights reserved.</copyright>
    <license>GNU/GPLv3 http://www.gnu.org/licenses/gpl-3.0.html</license>
    <!--  The version string is recorded in the components table -->
    <version>0.1.0</version>
    <!-- The description is optional and defaults to the name -->
    <description>MOD_DOWNMSG_XML_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Module\Downmsg</namespace>
    <files>
        <folder>help</folder>
        <folder>language</folder>
        <folder module="mod_downmsg">services</folder>
        <folder>src</folder>
        <folder>tmpl</folder>
    </files>
    <help url="MOD_DOWNMSG_HELP_URL" />
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-mod-downmsg/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Mod Downmsg Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-mod-downmsg/main/updates.xml</server>
    </updateservers>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="msg_id"
                    type="list"
                    label="MOD_DOWNMSG_PARAMS_MSG_ID_LABEL"
                    description="MOD_DOWNMSG_PARAMS_MSG_ID_DESC"
                    default="v1"
                    required="true"
                >
                    <option value="v1">MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V1</option>
                    <option value="v2">MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V2</option>
                </field>
                <field
                    name="hour"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_HOUR_LABEL"
                    description="MOD_DOWNMSG_PARAMS_HOUR_DESC"
                    default="12"
                    min="0"
                    max="23"
                    required="true"
                />
                <field
                    name="minute"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_MINUTE_LABEL"
                    description="MOD_DOWNMSG_PARAMS_MINUTE_DESC"
                    default="00"
                    min="00"
                    max="59"
                    required="true"
                />
                <field
                    name="tz"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_TZ_LABEL"
                    description="MOD_DOWNMSG_PARAMS_TZ_DESC"
                    default="0"
                    min="-11"
                    max="11"
                    required="true"
                />
            </fieldset>
        </fields>
    </config>
</extension>
```

### Notes sur l'élément

- Les attributs de **l'extension** :
  - **type="module"** indique le type d'extension.
  - **method="upgrade"** signifie que cette extension peut être installée et mise à jour si de nouvelles versions sont disponibles.
  - **client="site"** indique à Joomla qu'il s'agit d'un module *Site* plutôt que d'un module *Administrateur*.
- La déclaration de **version** est utilisée pour contrôler la mise à niveau. Si un serveur de mise à jour est disponible, il est comparé avec la version enregistrée là-bas. Cela empêche également une version plus ancienne de remplacer une version plus récente.
- L'attribut de chemin **namespace** indique à Joomla où chercher les classes de l'espace de noms.
- La section **files** liste les dossiers et fichiers à installer. Si un élément n'est pas présent ici, il ne sera pas installé même s'il est présent dans le fichier d'installation zippé.
- L'attribut url de **l'aide** permet d'utiliser un fichier d'aide personnalisé dans le formulaire de modification du module. Le chemin vers le fichier d'aide est une valeur pour la clé donnée dans le fichier en-GB/mod_downmsg.ini.
- La section **config** montre quels paramètres ce module nécessite. Ils devraient tous avoir des valeurs par défaut car ils sont définis lors de l'installation.

## Les fichiers de langue

Ce module contient très peu de chaînes. Celles dans les fichiers **sys.ini** sont utilisées lors de l'installation et pour la liste parmi d'autres modules. Les fichiers **.ini** sont utilisés dans le formulaire Administrateur et dans le rendu du Site du module. Notez les conventions pour identifier les chaînes :

MOD\_\[nom\]\_\[objectif\]\_\[utilisation\]

Les fichiers ci-dessous montrent que les traductions allemande et française n'incluent que les chaînes visibles par les visiteurs du site, car le formulaire des paramètres sera toujours administré en anglais. Au cas où vous ne le sauriez pas : les chaînes en-GB sont toujours chargées en premier par défaut pour s'assurer que les chaînes non traduites par accident n'apparaissent pas comme des clés de chaîne. Les chaînes de la langue requise sont ensuite chargées et remplacent les chaînes anglaises équivalentes.

Les versions françaises et allemandes des chaînes ici ont été obtenues à l'aide des outils de traduction de Google. Cela peut mieux fonctionner pour les phrases que pour les mots seuls !

### en-GB/mod_downmsg.ini

```ini
MOD_DOWNMSG="System Down Message"
MOD_DOWNMSG_XML_DESCRIPTION="Show a message about imminent system down time."

MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"

MOD_DOWNMSG_MSG_V1="This site will close for a short period at %s"
MOD_DOWNMSG_MSG_V2="Please log out. This site will close for one hour or more at %s"

MOD_DOWNMSG_PARAMS_MSG_ID_LABEL="Select Message version"
MOD_DOWNMSG_PARAMS_MSG_ID_DESC="The short downtime or long downtime message vesion."

MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V1="Short < 1 hour"
MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V2="Long > 1 hour"

MOD_DOWNMSG_PARAMS_HOUR_LABEL="Hour"
MOD_DOWNMSG_PARAMS_HOUR_DESC="Set the hour when the site will close"

MOD_DOWNMSG_PARAMS_MINUTE_LABEL="Minute"
MOD_DOWNMSG_PARAMS_MINUTE_DESC="Set the minutes when the site will close"

MOD_DOWNMSG_PARAMS_TZ_LABEL="Time Zone"
MOD_DOWNMSG_PARAMS_TZ_DESC="Set your time zone offset from GMT"
```

### en-GB/mod_downmsg.sys.ini

Ce fichier est utilisé pour la gestion du système.

```ini
MOD_DOWNMSG="System Down Message"
MOD_DOWNMSG_XML_DESCRIPTION="Show a message about imminent system down time."
```

### de-DE/mod_downmsg.ini

```ini
MOD_DOWNMSG_MSG_V1="Diese Seite wird für kurze Zeit um %s geschlossen"
MOD_DOWNMSG_MSG_V2="Bitte melden Sie sich ab. Diese Seite wird für eine Stunde oder länger um %s geschlossen"
MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"
```

### fr-FR/mod_downmsg.ini

```ini
MOD_DOWNMSG_MSG_V1="Ce site sera fermé pour une courte période à %s"
MOD_DOWNMSG_MSG_V2="Veuillez vous déconnecter. Ce site sera fermé pendant une heure ou plus à %s"
MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"
```

## Le code du module

Le code du module est incroyablement simple. Il y a trois fichiers PHP :

- services/provider.php (le code d'injection de dépendance).
- src/Dispatcher/Dispatcher.php (assemble les données à afficher).
- tmpl/default.php (le modèle pour afficher les données, qui peut être remplacé).

### services/provider.php

```php
<?php

/**
 * @package     Cefjdemos.Site
 * @subpackage  mod_downmsg
 *
 * @copyright   (C) 2023 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\Service\Provider\Module;
use Joomla\CMS\Extension\Service\Provider\ModuleDispatcherFactory;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;

/**
 * The module Downmsg service provider.
 *
 * @since  4.4.0
 */
return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.4.0
     */
    public function register(Container $container): void
    {
        $container->registerServiceProvider(new ModuleDispatcherFactory('\\Cefjdemos\\Module\\Downmsg'));
        $container->registerServiceProvider(new Module());
    }
};
```

### src/Dispatcher/Dispatcher.php

```php
<?php

/**
 * @package     Cefjdemos.Site
 * @subpackage  mod_downmsg
 *
 * @copyright   (C) 2023 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace Cefjdemos\Module\Downmsg\Site\Dispatcher;

use Joomla\CMS\Dispatcher\AbstractModuleDispatcher;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

/**
 * Dispatcher class for mod_downmsg
 *
 * @since  4.4.0
 */
class Dispatcher extends AbstractModuleDispatcher
{
    /**
     * Returns the layout data.
     *
     * @return  array
     *
     * @since   4.4.0
     */
    protected function getLayoutData()
    {
        $data = parent::getLayoutData();

        return $data;
    }
}
```

### tmpl/default.php

```php
<?php

/**
 * @package     Cefjdemos.Module
 * @subpackage  mod_downmsg
 *
 * @copyright   Copyright (C) 2019 Clifford E Ford. All rights reserved.
 * @license     GNU/GPLv3 http://www.gnu.org/licenses/gpl-3.0.html
 */

defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;

// the $msg string contains a %s placeholder to be replaced in a sprintf statement
$msg = Text::_('MOD_DOWNMSG_MSG_' . strtoupper($params->get('msg_id')));

$tod = $params->get('hour') . ':' . $params->get('minute');
$tz = $params->get('tz');

if ($tz > 0) {
    $tz = '(+' . $tz . ')';
} elseif ($tz < 0) {
    $tz = '(-' . $tz . ')';
} else {
    $tz = '';
}

$tod .= ' GMT ' . $tz;

?>

<div class="alert alert-warning text-center" role="alert">
    <?php echo sprintf($msg, $tod); ?>
</div>
```

## Installation

Depuis le menu Administrateur, allez à la page Système / Installer / Extensions et utilisez l'onglet Télécharger le fichier du paquet pour télécharger le fichier zip. Ensuite, allez à Contenu / Modules du site et modifiez le module nouvellement installé.

Dans le formulaire de modification du module :

1. Entrez un titre. N'oubliez pas qu'un module peut avoir plus d'une instance, donc ils peuvent apparaître à des endroits différents ou avoir des paramètres différents.
2. Définissez l'heure à laquelle le site sera hors ligne.
3. Définissez le fuseau horaire (il y a probablement une meilleure façon que d'utiliser GMT ± n heures).

Dans la liste des paramètres communs à droite

1. Réglez le **Titre** sur **caché**.
2. Sélectionnez une **Position** de modèle - dans Cassiopeia **topbar** place le message au-dessus du contenu de la page, parfait.
3. Réglez le **Statut** sur **Publié**.
4. Dans l’onglet **Affectation de menu**, sélectionnez **Sur toutes les pages**.
5. **Enregistrez** et vous êtes prêt à vérifier l'apparence du site.

![the module edit form](../../../en/images/modules/downmsg-module-edit-form.png)

## Test

Voici comment le message apparaît en anglais :

![site down message in english](../../../en/images/modules/downmsg-module-result-en.png)

En allemand :

![site down message in english](../../../en/images/modules/downmsg-module-result-de.png)

Et français :

![site down message in english](../../../en/images/modules/downmsg-module-result-fr.png)

## Mettre à jour le site et journal des modifications

Ces éléments sont inclus dans la source mais ne sont pas traités ici.

*Traduit par openai.com*

