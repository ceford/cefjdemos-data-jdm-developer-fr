<!-- Filename: J4.x:J4_Plugin_example_-_Table_of_Contents / Display title: Exemple : Table des matières -->

## Introduction

Cet article fournit un exemple de plugin de contenu pour illustrer la programmation à l'aide des dernières conventions de codage de Joomla. Le code complet est disponible sur [GitHub](https://github.com/ceford/cefjdemos-plg-content-toc). Vous pouvez le télécharger et l'installer ou simplement le décompresser pour examiner le code.

Il y a une section sur le [Développement de Plugins](https://manual.joomla.org/docs/building-extensions/plugins/) et plus d'exemples dans la Documentation des Programmeurs Joomla, [copiée sur ce site](jdocmanual?article=docus/plugins/index) pour plus de commodité.

Ce plugin génère une *Table des Matières* à partir des titres de tout article contenant un espace réservé `{cefjdemostoc}`. Le résultat est illustré dans la dernière capture d'écran de cet article.

## Structure du Code Source

Ce qui suit est une illustration schématique de la structure du code source utilisé pour créer l'extension :

```
cefjdemos-plg-toc
|-- .vscode (folder for build settings)
|-- plg_content_cefjdemostoc (folder compressed to create an installable zip file)
    |-- language/en-GB/ (language folder, kept with the extension code)
        |-- plg_content_cefjdemostoc.ini
        |-- plg_content_cefjdemostoc.sys.ini
    |-- media
        |-- css
            |-- cefjdemostoc.css (layout styles)
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Extension (folder for extension specific code)
            |-- Cefjdemostoc.php (the extension class code)
    |-- tmpl (folder for layouts)
        |-- default.php (the table of contents layout)
    |-- cefjdemostoc.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
```

Voici la structure telle qu'elle apparaît dans l'IDE VSCode ou VSCodium :

![Plugin development file structure in vscodium](../../../en/images/plugins/cefjdemostoc-vscodium.png)

## Le Fichier Manifest

Chaque extension doit avoir un fichier manifeste au format xml. Il est utilisé par Joomla pour l'installation, la mise à jour et la suppression. Voici le fichier manifeste pour cette extension :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="plugin" group="content" method="upgrade">
    <name>plg_content_cefjdemostoc</name>
    <author>Clifford E Ford</author>
    <creationDate>2024-12-23</creationDate>
    <copyright>(C) 2024 Clifford E Ford</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <authorEmail>cliff@xxxx.yyyy.co.uk</authorEmail>
    <authorUrl>jdocmanual.org</authorUrl>
    <version>0.2.2</version>
    <description>PLG_CONTENT_CEFJDEMOSTOC_XML_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Plugin\Content\Cefjdemostoc</namespace>
    <files>
        <folder plugin="cefjdemostoc">services</folder>
        <folder>src</folder>
        <folder>language</folder>
    </files>
    <media destination="plg_content_cefjdemostoc" folder="media">
        <folder>css</folder>
    </media>
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-plg-content-toc/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Cefjdemostoc Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-plg-content-toc/main/updates.xml</server>
    </updateservers>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="help"
                    type="list"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DESC"
                    default="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DEFAULT"
                >
                    <option value="">PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DEFAULT</option>
                </field>

                <field
                    name="toc_class"
                    type="text"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_CLASS_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_CLASS_DESC"
                />

                <field
                    name="toc_head_class"
                    type="list"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_HEAD_CLASS_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_HEAD_CLASS_DESC"
                    default="center"
                >
                    <option value="text-center">text-center</option>
                    <option value="text-start">text-start</option>
                </field>
            </fieldset>
        </fields>
    </config>
</extension>
```

## Le fichier services/provider.php

Le but de ce fichier est d'enregistrer un fournisseur de services et de déclarer les outils dont il a besoin. Ce plugin particulier n'a pas besoin de grand-chose. Si votre plugin personnalisé devait effectuer des requêtes à la base de données, vous le déclareriez ici. Jetez un coup d'œil à un plugin d'authentification. Ils nécessitent des outils de base de données et d'utilisateur.

```php
<?php

/**
 * @package     Cefjdemostoc.Plugin
 * @subpackage  Content.cefjdemostoc
 *
 * @copyright   (C) 2024 Clifford E Ford <https://jdocmanual.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\PluginInterface;
use Joomla\CMS\Factory;
use Joomla\CMS\Plugin\PluginHelper;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;
use Joomla\Event\DispatcherInterface;
use Cefjdemos\Plugin\Content\Cefjdemostoc\Extension\Cefjdemostoc;

return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.3.0
     */
    public function register(Container $container)
    {
        $container->set(
            PluginInterface::class,
            function (Container $container) {
                $plugin     = new Cefjdemostoc(
                    $container->get(DispatcherInterface::class),
                    (array) PluginHelper::getPlugin('content', 'cefjdemostoc')
                );
                $plugin->setApplication(Factory::getApplication());

                return $plugin;
            }
        );
    }
};
```

## La classe d'extension

Le but du fichier src/Extension/Cefjdemostoc.php est de préparer les données pour la sortie. Le fichier fait 91 lignes, il n'est donc pas reproduit ici. Cependant, certaines parties méritent une explication supplémentaire.

### Directives Codesniffer

Lignes 16 à 18 ont ceci :

```php
// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects
```

Les lignes commentées évitent les rapports d'erreurs du CodeSniffer. Elles ne servent à aucune autre fin.

### fonction publique onContentPrepare()

C'est l'événement auquel le plugin répond. Il reçoit un *contexte*, les données de l'article et les paramètres de l'article. Une *Table des matières* doit être générée uniquement si la page rendue est un seul article. Sinon, le placeholder `{cefjdemostoc}` doit être supprimé.

Pour générer une table des matières, certains paramètres d'article sont définis et chaque titre de l'article est modifié pour ajouter un numéro de séquence **id**, ainsi `<h2>Introduction</h2>` devient `<h2 id="cefjemostoc0">Introduction</h2>`. Un modèle est ensuite chargé pour ajouter la table des matières rendue.

### Le fichier tmpl/default.php

Le but de ce fichier est de rendre la sortie avec les données fournies. Le fichier peut être remplacé et vous le verrez parmi les remplacements **plugins/contents** pour le modèle Cassiopeia.

Le fichier est reproduit ci-dessous. Notez que certains éléments ont des classes CSS codées en dur et d'autres obtenues à partir des paramètres de l'article. Les classes **fs-** sont des tailles de police Bootstrap. Les titres sont stockés dans le tableau `$headings2`.

```php
<?php

/**
 * @package     Cefjdemostoc.Plugin
 * @subpackage  Content.cefjemostoc
 *
 * @copyright   (C) 2024 Clifford E Ford.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;

/**
 * @var Joomla\CMS\WebAsset\WebAssetManager $wa
 * @var \Joomla\Plugin\Content\Cefjdemostoc\Extension\Cefjdemostoc $this
 */
$wa = $this->getApplication()->getDocument()->getWebAssetManager();
$wa->registerAndUseStyle('plg_content_cefjdemostoc', 'plg_content_cefjdemostoc/cefjdemostoc.css');

?>
<div class="card cefjdemostoc <?php echo $toc_class; ?>">
    <div class="card-header">
        <div class="<?php echo $toc_head_class; ?> fs-2">
            <?php echo Text::_('PLG_CONTENT_CEFJDEMOSTOC_CONTENTS'); ?>
        </div>
    </div>
    <div class="card-body">
        <ul class="list-group">
            <?php foreach ($headings2 as $i => $heading) : ?>
                <li class="list-group-item fs-<?php echo $heading[1] + 2; ?> ps-<?php echo $heading[1] + 2; ?>">
                    <a href="#cefjemostoc<?php echo $i; ?>" class="link-underline-light">
                        <?php echo $heading[2]; ?>
                    </a>
                </li>
            <?php endforeach; ?>
        </ul>
    </div>
</div>
```

#### Le gestionnaire d'actifs Web

Sans style supplémentaire, la table des matières (ToC) occuperait toute la largeur de l'écran à l'emplacement du marqueur `{cefjemostoc}` dans le texte source. C'est parfois le cas dans les publications techniques, mais ce n'est peut-être pas ce que vous souhaitez. Sur les grands écrans, il est souvent préférable de faire flotter la ToC et de permettre au texte de l'article de s'écouler autour. Cependant, cela peut devenir inutilisable sur les écrans étroits. La meilleure solution est de fournir une feuille de style avec des requêtes de média personnalisées. Sur les grands écrans, la ToC est flottée à droite, mais sur les écrans étroits, elle ne l'est pas.

Le code `default.php` ci-dessus montre comment inclure une feuille de style personnalisée pour le plugin. Le CSS se trouve dans le fichier `media/css/cefjdemostoc.css`. Les valeurs figurant dans ce fichier servent à illustrer ce tutoriel.

## Résultat

![The resulting table of contents](../../../en/images/plugins/cefjdemostoc-result.png)

*Traduit par openai.com*

