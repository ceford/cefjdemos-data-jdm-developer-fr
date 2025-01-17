<!-- Filename: J4.x:CLI_example_-_Onoffbydate / Display title: Exemple CLI - Onoffbydate -->

## Introduction

Il y a des occasions où un site doit afficher ou masquer un module en fonction de la date. Un exemple pourrait être un module HTML personnalisé affichant un message pendant l'hiver. Un autre exemple pourrait être l'alternance de modules personnalisés selon le jour de la semaine, par exemple un pour les jours de semaine et un autre pour le week-end. L'exemple présenté ici utilise un plugin, une commande CLI et un cron pour obtenir l'effet souhaité.

Le code est disponible sur [GitHub](https://github.com/ceford/cefjdemos-plg-onoffbydate)

Il y a plus d'articles sur l'utilisation de la CLI dans le [Manuel d'Utilisateur](http://localhost/jdm4/jdocmanual?article=user/command-line-interface/using-the-cli) et dans la Documentation des Programmeurs Joomla! [copie locale](jdocmanual?article=docus/plugins/plugin-examples-console-plugin-sqlfile) ou [source originale](https://manual.joomla.org/docs/building-extensions/plugins/plugin-examples/console-plugin-sqlfile/);

## Normes Joomla

Dans les versions précédentes de Joomla, le système de plugins utilisait une implémentation du modèle Observable/Observer. En conséquence, chaque plugin chargé verrait immédiatement toutes ses méthodes publiques enregistrées comme observateurs. Cela pouvait causer des problèmes.

Joomla 4 et les versions ultérieures utilisent le package Event du Joomla Framework pour gérer les Événements des plugins. Cela offre de meilleures performances et sécurité. En termes pratiques, cela signifie que la structure de fichiers d'un plugin Joomla 4/5/6 est différente de la structure de plugin Héritage des versions antérieures.

## La Structure du Plugin

Le plugin s'appelle **onoffbydate**. Le schéma ci-dessous montre la structure de fichiers utilisée pour le développement local du plugin :

```
cefjdemos-plg-onoffbydate
|-- .vscode (folder for build settings)
|-- cefjdemos-plg-onoffbydate (folder compressed to create an installable zip file)
    |-- language
        |-- en-GB (language folder, kept with the extension code)
            |-- plg_system_onoffbydate.ini
            |-- plg_system_onoffbydate.sys.ini
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Console (folder for extension specific code)
            |-- OnoffbydateCommand.php (the plugin execution code)
        |-- Extension
            |-- Onoffbydate.php (the plugin register code)
    |-- onoffbydate.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (a record of changes in each release)
|-- README.md (brief description and instructions on use)
|-- updates.xml (the update server specification)
```

## Le fichier Manifest onoffbydate.xml

Ceci est le fichier d'installation - un élément standard pour toute extension.

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="plugin" group="console" method="upgrade">
    <name>plg_console_onoffbydate</name>
    <author>Clifford E Ford</author>
    <creationDate>Jamuary 2025</creationDate>
    <copyright>(C) Clifford E Ford</copyright>
    <license>GNU General Public License version 3 or later</license>
    <authorEmail>cliff@xxx.yyy.co.uk</authorEmail>
    <version>0.3.0</version>
    <description>PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Plugin\Console\Onoffbydate</namespace>
    <files>
        <folder>language</folder>
        <folder plugin="onoffbydate">services</folder>
        <folder>src</folder>
    </files>
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-plg-onoffbydate/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Cefjdemosonoffbydate Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-plg-onoffbydate/main/updates.xml</server>
    </updateservers>
    <config>
        <fields>

        </fields>
    </config>
</extension>
```

- La ligne **namespace** indique à Joomla où trouver le code namespace pour ce plugin.
- Le dossier **language** est conservé avec le code du plugin.
- Un **updateserver** est requis pour satisfaire aux exigences du JED.
- Il n'y a pas d'options de **config** pour ce plugin mais cela pourrait être modifié à l'avenir. Par exemple, l'utilisateur pourrait préférer définir les mois d'hiver à l'aide de cases à cocher plutôt que de les avoir codés en dur.

## Inscription : services/fournisseur.php

Ceci est le point d'entrée pour le code du plugin.

```php
<?php

/**
 * @package     Cefjdemos.Plugin
 * @subpackage  Console.Onoffbydate
 *
 * @copyright   Copyright (C) 2025 Clifford E Ford.
 * @license     GNU General Public License version 3 or later.
 */

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\Extension\PluginInterface;
use Joomla\CMS\Plugin\PluginHelper;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;
use Joomla\CMS\Factory;
use Joomla\Event\DispatcherInterface;
use Cefjdemos\Plugin\Console\Onoffbydate\Extension\Onoffbydate;

return new class implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.2.0
     */
    public function register(Container $container)
    {
        $container->set(
            PluginInterface::class,
            function (Container $container) {
                $dispatcher = $container->get(DispatcherInterface::class);
                $plugin     = new Onoffbydate(
                    $dispatcher,
                    (array) PluginHelper::getPlugin('console', 'onoffbydate')
                );
                $plugin->setApplication(Factory::getApplication());

                return $plugin;
            }
        );
    }
};
```

## Abonnement à l'Événement : src/Extension/Onoffbydate.php

C'est l'endroit où l'événement qui déclenche le plugin est enregistré et où est définie l'emplacement du code qui gérera l'événement.

```php
<?php

/**
 * @package     Cefjdemos.Plugin
 * @subpackage  Console.Onoffbydate
 *
 * @copyright   Copyright (C) 2025 Clifford E Ford.
 * @license     GNU General Public License version 3 or later.
 */

namespace Cefjdemos\Plugin\Console\Onoffbydate\Extension;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\Plugin\CMSPlugin;
use Joomla\Event\SubscriberInterface;
use Cefjdemos\Plugin\Console\Onoffbydate\Console\OnoffbydateCommand;

final class Onoffbydate extends CMSPlugin implements SubscriberInterface
{
    /**
     * Returns the event this subscriber will listen to.
     *
     * @return  array
     */
    public static function getSubscribedEvents(): array
    {
        return [
                \Joomla\Application\ApplicationEvents::BEFORE_EXECUTE => 'registerCommands',
        ];
    }

    /**
     * Returns the command class for the Onoffbydate CLI plugin.
     *
     * @return  void
     */
    public function registerCommands(): void
    {
        $myCommand = new OnoffbydateCommand();
        $myCommand->setParams($this->params);
        $this->getApplication()->addCommand($myCommand);
    }
}
```

La classe **OnoffbydateCommand** se trouve dans le dossier *src/Console*, car les commandes intégrées de la Console Joomla sont situées dans un dossier Console. Elle aurait pu être placée dans un dossier *Extension*.

## Le fichier de commande : src/Console/OnoffbydateCommand.php

Ce fichier contient le code qui effectue tout le travail pour cette extension.

```php
<?php

/**
 * @package     Cefjdemos.Plugin
 * @subpackage  Console.Onoffbydate
 *
 * @copyright   Copyright (C) 2025 Clifford E Ford.
 * @license     GNU General Public License version 3 or later.
 */

namespace Cefjdemos\Plugin\Console\Onoffbydate\Console;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\Factory;
use Joomla\CMS\Language\Text;
use Joomla\Console\Command\AbstractCommand;
use Joomla\Database\DatabaseInterface;
use Symfony\Component\Console\Input\InputArgument;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;
use Symfony\Component\Console\Style\SymfonyStyle;
use Joomla\Database\DatabaseAwareTrait;

class OnoffbydateCommand extends AbstractCommand
{
    use DatabaseAwareTrait;

    /**
     * The default command name
     *
     * @var    string
     *
     * @since  4.0.0
     */
    protected static $defaultName = 'onoffbydate:action';

    /**
     * @var InputInterface
     * @since version
     */
    private $cliInput;

    /**
     * SymfonyStyle Object
     * @var SymfonyStyle
     * @since 4.0.0
     */
    private $ioStyle;

    /**
     * The params associated with the plugin, plus getter and setter
     * These are injected into this class by the plugin instance
     */
    protected $params;

    protected function getParams()
    {
        return $this->params;
    }

    public function setParams($params)
    {
        $this->params = $params;
    }

    /**
     * Initialise the command.
     *
     * @return  void
     *
     * @since   4.0.0
     */
    protected function configure(): void
    {
        $lang = Factory::getApplication()->getLanguage();
        $test = $lang->load(
            'plg_console_onoffbydate',
            JPATH_BASE . '/plugins/console/onoffbydate'
        );

        $this->addArgument(
            'season',
            InputArgument::REQUIRED,
            'winter or weekend'
        );
        $this->addArgument(
            'action',
            InputArgument::REQUIRED,
            'publish or unpublish'
        );

        $this->addArgument(
            'module_id',
            InputArgument::REQUIRED,
            'module id'
        );

        $help = Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_1');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_2');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_3');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_4');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_5');

        $this->setDescription(Text::_('PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION'));
        $this->setHelp($help);
    }

    /**
     * Internal function to execute the command.
     *
     * @param   InputInterface   $input   The input to inject into the command.
     * @param   OutputInterface  $output  The output to inject into the command.
     *
     * @return  integer  The command exit code
     *
     * @since   4.0.0
     */
    protected function doExecute(InputInterface $input, OutputInterface $output): int
    {
        $this->configureIO($input, $output);

        $season = $this->cliInput->getArgument('season');
        $action = $this->cliInput->getArgument('action');
        $module_id = $this->cliInput->getArgument('module_id');

        switch ($season) {
            case 'winter':
                $result = $this->winter($module_id, $action);
                break;
            case 'weekend':
                $result = $this->weekend($module_id, $action);
                break;
            default:
                $result = Text::_('PLG_CONSOLE_ONOFFBYDATE_ERROR', $season);
                $this->ioStyle->error($result);
                return 0;
        }

        return 1;
    }

    protected function publish($module_id, $published)
    {
        $db = Factory::getContainer()->get(DatabaseInterface::class);
        //$db    = $this->getDatabase();
        $query = $db->getQuery(true);
        $query->update('#__modules')
            ->set('published = ' . $published)
            ->where('id = ' . $module_id);
        $db->setQuery($query);
        $db->execute();
    }

    protected function weekend($module_id, $action)
    {
        // get the day of the week
        $day = date('w');
        if (in_array($day, array(0,6))) {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_A_WEEKEND');
            $published = $action === 'publish' ? 1 : 0;
        } else {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_A_WEEKEND');
            $published = $action === 'publish' ? 0 : 1;
        }

        $this->publish($module_id, $published);

        $state = empty($published) ? Text::_('JUNPUBLISHED') : Text::_('JPUBLISHED');
        $result = Text::sprintf('PLG_CONSOLE_ONOFFBYDATE_SUCCESS', $msg, $module_id, $state);

        $this->ioStyle->success($result);
    }

    protected function winter($module_id, $action)
    {
        // get the month of the month
        $month = date('n');
        if (in_array($month, array(1,2,11,12))) {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_WINTER');
            $published = $action === 'publish' ? 1 : 0;
        } else {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_WINTER');
            $published = $action === 'publish' ? 0 : 1;
        }

        $this->publish($module_id, $published);

        $state = empty($published) ? Text::_('JUNPUBLISHED') : Text::_('JPUBLISHED');

        $state = empty($published) ? Text::_('JUNPUBLISHED') : Text::_('JPUBLISHED');
        $result = Text::sprintf('PLG_CONSOLE_ONOFFBYDATE_SUCCESS', $msg, $module_id, $state);

        $this->ioStyle->success($result);
    }

    /**
     * Configures the IO
     *
     * @param   InputInterface   $input   Console Input
     * @param   OutputInterface  $output  Console Output
     *
     * @return void
     *
     * @since 4.0.0
     *
     */
    private function configureIO(InputInterface $input, OutputInterface $output)
    {
        $this->cliInput = $input;
        $this->ioStyle = new SymfonyStyle($input, $output);
    }
}
```

- La fonction `configure` établit que trois arguments de ligne de commande sont requis :
    - `season` doit être l'un de `weekend` ou `winter`.
    - `action` doit être l'un de `publish` ou `unpublish`.
    - `module_id` doit être l'ID entier du module à publier ou dépublier.
- La fonction `doExecute` est là où le travail est effectué. Deux options d'action sont reconnues :
    - `winter` appelle une fonction pour publier ou dépublier un module si aujourd'hui est dans les mois d'hiver.
    - `weekend` appelle une fonction pour publier ou dépublier un module si aujourd'hui est un jour de week-end.
- La fonction `publish` effectue un appel à la base de données pour définir le champ `publishes` du module à `0` ou `1` selon le cas.
- Les fonctions `winter` et `weekend` effectuent quelques calculs de dates pour déterminer si aujourd'hui est en hiver (ou non) ou un week-end (ou non) afin de transmettre les paramètres appropriés à la fonction de publication.
- L'appel de la fonction `$this->ioStyle->success()` produit un message texte sur la console avec un texte noir et un fond vert. `$this->ioStyle->error()` produit un message d'ERREUR avec un texte blanc sur un fond marron.

## fichiers linguistiques

Il y a deux fichiers de langue utilisés pendant l'installation et la configuration du plugin. Le fichier en-GB/plg_console_onoffbydate.sys.ini contient les chaînes vues pendant l'installation et la gestion :

```ini
PLG_CONSOLE_ONOFFBYDATE="Console - Onoffbydate"
PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION="Set date-dependent enabled state of a module via cli"
```

Le fichier en-GB/plg_console_onoffbydate.ini contient les chaînes visibles en utilisation :

```ini
PLG_CONSOLE_ONOFFBYDATE="Console - Onoffbydate"
PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION="Set date-dependent enabled state of a module via cli"
PLG_CONSOLE_ONOFFBYDATE_ERROR="Unknown season: %s."
PLG_CONSOLE_ONOFFBYDATE_HELP_1="<info>%command.name%</info> Toggles module Enabled/Disabled state\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_2="Usage: <info>php %command.full_name% season action module_id where\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_3="    season is one of winter or weekend,\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_4="    action is one of publish or unpublish and\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_5="    module_id is the id of the module to publish or unpublish.</info>\n"
PLG_CONSOLE_ONOFFBYDATE_SUCCESS="That seemed to work. %s Module %s has been %s."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_A_WEEKEND="Today is in a weekend."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_WINTER="Today is in winter."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_A_WEEKEND="Today is not in a weekend."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_WINTER="Today is not in winter."
```

Notez que la sortie de la commande est visible uniquement par vous sur votre installation Joomla.

## Le Module

Ce code change simplement l'état de publication d'un module en fonction de certaines fonctions de la date. Vous avez donc besoin de l'identifiant du module comme illustré dans la colonne de droite de la page de liste des Modules (Site).

## La ligne de commande

Installez le plugin dans votre installation de test Joomla et activez-le. Si cela fait planter votre site, utilisez phpMyAdmin pour trouver le plugin nouvellement installé dans la table #__extensions et définissez son paramètre enabled sur 0. Corrigez le problème et réessayez.

Dans une fenêtre de terminal, changez de répertoire vers la racine de votre site et utilisez la commande suivante :

```sh
php cli/joomla.php list
```

Si quelque chose tourne mal à cette étape, vérifiez que la version de PHP invoquée est la version en ligne de commande et non celle utilisée par le serveur web. Vous pouvez supprimer les messages d'obsolescence avec cette version de la ligne de commande.

```sh
php -d error_reporting="E_ALL & ~E_DEPRECATED" cli/joomla.php onoffbydate:action garbage publish 133
```

Si le code fonctionne, vous verrez `onoffbydate` parmi la liste des commandes disponibles et vous pouvez invoquer l'aide pour voir comment il doit être utilisé :

```sh
php cli/joomla.php onoffbydate:action --help
Usage:
  onoffbydate:action <season> <action> <module_id>

Arguments:
  season                       winter or summer or weekday or weekend
  action                       publish or unpublish
  module_id                    module id

Options:
  -h, --help                   Display the help information
  -q, --quiet                  Flag indicating that all output should be silenced
  -V, --version                Displays the application version
      --ansi                   Force ANSI output
      --no-ansi                Disable ANSI output
  -n, --no-interaction         Flag to disable interacting with the user
      --live-site[=LIVE-SITE]  The URL to your site, e.g. https://www.example.com
  -v|vv|vvv, --verbose         Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Help:
  onoffbydate:action Toggles module Enabled/Disabled state
  Usage: php joomla.php onoffbydate:action season action module_id where
      season is one of winter or summer or weekday or weekend,
      action is one of publish or unpublish and
      module_id is the id of the module to publish or unpublish.
```

Et puis essayez-le simplement :

```sh
php cli/joomla.php onoffbydate:action weekend publish 133

     [OK] That seemed to work. Today is not a weekend. Module 133 has been Unpublished
```

### En Utilisation

Cette logique d'utilisation est un peu illogique ! Si vous avez un module qui doit uniquement être publié le week-end, les paramètres à utiliser chaque jour sont `weekend publish module_id`. Cela publie le module les jours du week-end et le dépublie les jours qui ne sont pas dans le week-end. Pour un module qui doit être publié en semaine, les paramètres à utiliser chaque jour sont `weekend unpublish module_id`. Cela dépublie le module les jours du week-end et le publie les jours qui ne sont pas dans le week-end. La même logique s'applique à la version `hiver`.

## Le cron

La commande peut être testée dans une fenêtre de terminal, mais vous voudrez probablement l'utiliser depuis un cron. L'option `winter` pourrait être exécutée le premier jour de chaque mois. L'option `weekend` serait exécutée quotidiennement. Le point important est que vous pouvez avoir autant de crons que nécessaire pour changer l'état publié de autant de modules que vous le souhaitez à des intervalles appropriés. Le même code fonctionne pour tous.

Sur un service d'hébergement, vous devez fournir les chemins complets vers l'exécutable php et la commande joomla cli. Exemple :

```sh
/usr/local/bin/php /home/username/public_html/pathtojoomla/cli/joomla.php onoffbydate:action winter publish 130
```

Selon la manière dont vous avez configuré votre cron et votre système, vous pouvez recevoir un courriel rassurant contenant exactement les mêmes informations que celles visibles dans la ligne de commande.

## Vérifier

Et bien sûr : allez sur votre page d'accueil et vérifiez que le module a bien été publié ou dépublié.

*Traduit par openai.com*

