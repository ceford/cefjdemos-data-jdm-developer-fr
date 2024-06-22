<!-- Filename: J4.x:Creating_a_Plugin_for_Joomla / Display title: Créer un plugin pour Joomla -->

<span id="main-portal-heading">Didacticiel
Comment créer un plugin pour Joomla 4</span> Joomla!  4.x series

La structure des plugins dans Joomla! 1.5, 2.5 et 3.x était très
flexible et puissante. Non seulement les plugins pouvaient être utilisés
pour traiter des événements déclenchés par le noyau de l'application et
les extensions, mais aussi, les plugins pouvaient être utilisés pour
permettre à des extensions tierces d'être extensibles et puissantes.
Avec Joomlaǃ 4.x nous avons réécrit une bonne partie du système
d'acheminement (dispatcher) permettant cela afin d'accroître encore plus
la flexibilité lorsque vous modifiez les paramètres passés en tant
qu'événements tout en augmentant simultanément la performance des
plugins.

Cette procédure devrait vous fournir les éléments de base à connaître
pour créer votre propre plugin. La plupart des plugins sont constitués
d'un seul fichier de code, mais pour installer correctement le code du
plugin, il doit être empaqueté dans un fichier d'installation pour
pouvoir être traité par l'installateur de Joomlaǃ.

### Créer le fichier d'installation

Comme pour toutes les extensions de Joomlaǃ, les plugins sont facilement
installés avec le format de fichier .zip (.tar.gz est aussi supporté) et
un fichier XML correctement formaté doit aussi être inclus.
Voici un exemple de fichier d'installation XML pour le plugin de
recherche de catégories ː

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="plugin" group="search" method="upgrade">
	<name>plg_search_categories</name>
	<author>Joomla! Project</author>
	<creationDate>November 2005</creationDate>
	<copyright>Copyright (C) 2005 - 2018 Open Source Matters. All rights reserved.</copyright>
	<license>GNU General Public License version 2 or later; see LICENSE.txt</license>
	<authorEmail>admin@joomla.org</authorEmail>
	<authorUrl>www.joomla.org</authorUrl>
	<version>3.0.0</version>
	<description>PLG_SEARCH_CATEGORIES_XML_DESCRIPTION</description>
	<files>
		<filename plugin="categories">categories.php</filename>
	</files>
	<languages>
		<language tag="en-GB">en-GB.plg_search_categories.ini</language>
		<language tag="en-GB">en-GB.plg_search_categories.sys.ini</language>
	</languages>
	<config>
		<fields name="params">

			<fieldset name="basic">
				<field
					name="search_limit"
					type="number"
					label="JFIELD_PLG_SEARCH_SEARCHLIMIT_LABEL"
					default="50"
				/>

				<field
					name="search_content"
					type="radio"
					label="JFIELD_PLG_SEARCH_ALL_LABEL"
					layout="joomla.form.field.radio.switcher"
					default="0"
					>
					<option value="1">JYES</option>
					<option value="0">JNO</option>
				</field>

				<field
					name="search_archived"
					type="radio"
					label="JFIELD_PLG_SEARCH_ARCHIVED_LABEL"
					layout="joomla.form.field.radio.switcher"
					default="0"
					>
					<option value="1">JYES</option>
					<option value="0">JNO</option>
				</field>
			</fieldset>

		</fields>
	</config>
</extension>
```

Comme vous pouvez le constater, le système est similaire aux autres
fichiers d'installation XML de Joomlaǃ. Regardez l'entrée `group="xxx"`
de la balise et l'information complémentaire dans la balise . Ces
informations indiquent à Joomla! dans quel dossier copier le fichier et
dans quel groupe le plugin doit être ajouté.

Si vous créez un plugin qui répond aux événements existants du noyau,
l'attribut `groupe="xxx"` devra être modifié pour refléter le nom du
dossier du plugin existant pour le type d'événement que vous souhaitez
augmenter. Par exemple, `group="authentication"` ou `group="user"`.
Consultez les [Événements pour
plugins](https://docs.joomla.org/Plugin/Events "Special:MyLanguage/Plugin/Events")
pour voir une liste complète des catégories d'événements du noyau. Lors
de la création d'un nouveau plugin pour répondre aux événements du
noyau, il est important que le nom de votre plugin soit unique et ne
soit pas en conflit avec d'autres plugins qui pourraient également
répondre à un événement du noyau dont vous souhaitez également vous
servir.

Si vous créez un plugin pour répondre à des évènements système hors
noyau, la balise `groupe="xxx"` doit être différente de toutes les
catégories existantes du noyau.

**Conseil :** *Si vous ajoutez l'attribut `method="upgade"` à la balise
`extension`, ce plugin pourra être installé sans avoir à désinstaller
une version antérieure. Tous les fichiers existants seront remplacés,
mais les anciens fichiers ne seront pas supprimés.*

### Créer le plugin

Dans Joomla 4, vous pouvez également avoir du code espacé par des noms
dans votre plugin - veuillez lire [Conventions d'espace de nom dans
Joomla
4](https://docs.joomla.org/J4.x:Namespace_Conventions_In_Joomla "J4.x:Namespace Conventions In Joomla").
Notez que le fichier d'entrée principal n'est pas associé à un espace de
nom, mais que les champs et tout autre code complémentaire le sont.

La méthode orientée objet de l'écriture de plugins consiste à écrire une
sous-classe de <a
href="https://api.joomla.org/cms-4/classes/Joomla.CMS.Plugin.CMSPlugin.html"
class="external text" target="_blank"
rel="noreferrer noopener">CMSPlugin</a>, une classe de base qui met en
œuvre les propriétés de base des plugins. Dans vos méthodes, les
propriétés suivantes sont disponibles :

- `$this->params`: les
  [paramètres](https://docs.joomla.org/Parameter "Special:MyLanguage/Parameter")
  saisis pour ce plugin par l'administrateur
- `$this->_name`: le nom du plugin
- `$this->_type`: le groupe (type) du plugin
- `$this->db`: l'objet db (base de données)
- `$this->app`: l'objet application

**Conseil :** *Pour utiliser `$this->db` et `$this->app`, `CMSPlugin`
testez si la propriété existe et n'est pas privée. S'il est souhaité
d'utiliser les objets par défaut, créez des variables non instanciées
dans la classe du plugin (i.e. `protected $db; protected $app;` dans la
même zone que `protected $autoloadLanguage = true;`). Les variables
n'existeront pas à moins d'être explicitement créées.*

Dans l'exemple de code suivant, représente le groupe (type) du plugin et
représente son nom. Notez que les noms de classe et de fonction en PHP
sont insensibles à la casse.

Nous implémentons également le <a
href="https://api.joomla.org/framework-2/classes/Joomla.Event.SubscriberInterface.html"
class="external text" target="_blank"
rel="noreferrer noopener">SubscriberInterface</a> ici, ce qui est un
changement majeur depuis Joomlaǃ 1.5-3.x. Plutôt que de détecter la nom
de la fonction automatiquement et qu'elle soit le même que le nom de
l'événement, ceci vous permet d'utiliser des noms de fonction
personnalisés. Ceci nous permet de dire quels plugins implémentent
quelles fonctions et, étant donné que les méthodes publiques d'analyse
syntaxique (parsing) sont lentes dans le code PHP, ceci permet
d'augmenter de manière significative les performances.

Notez que dans toute la série Joomlaǃ 4, il y a une couche
d'obsolescence qui va s'occuper des plugins utilisant l'ancienne
stratégie de nommage des plugins avec le même nom que l'événement tant
que SubscriberInterface n'est pas implémenté.

```php
<?php
// no direct access
defined( '_JEXEC' ) or die;

use Joomla\CMS\Plugin\CMSPlugin;
use Joomla\Event\Event;
use Joomla\Event\SubscriberInterface;

class Plg<PluginGroup><PluginName> extends CMSPlugin implements SubscriberInterface
{
	/**
	 * Load the language file on instantiation
	 *
	 * @var    boolean
	 * @since  3.1
	 */
	protected $autoloadLanguage = true;

	/**
	 * Returns an array of events this subscriber will listen to.
	 *
	 * @return  array
	 */
	public static function getSubscribedEvents(): array
	{
		return [
			'<EventName>' => 'myFunctionName',
		];
	}

	/**
	 * Plugin method is the array value in the getSubscribedEvents method
	 * The plugin then modifies the Event object (if it's not immutable)
	 */
	 public function myFunctionName(Event $event)
	 {
		/*
		 * Plugin code goes here.
		 * You can access parameters via $this->params
		 */
		return true;
	}
}
?>
```

### Utiliser des plugins dans votre code

Si vous créez un plugin pour un nouvel événement non contenu dans le
noyau, n'oubliez pas d'activer votre plugin après son installation.
Précéder toute référence à votre nouveau plugin avec la commande
`JPluginHelper::importPlugin()`.

Maintenant que vous avez créé votre plugin, vous pourriez souhaiter
l'appeler dans votre code. Vous ne devriez pas. Le noyau Joomla! propose
déjà un grand nombre d'événements prédéfinis que vous pourrez lier au
code de votre plugin. Dans ce cas, vous n'avez pas besoin d'effectuer
les opérations suivantes.

#### La nouvelle façon "Joomla 4"

La nouvelle manière de faire ceci avec Joomlaǃ 4 et d'obtenir le
dispatcher et de distribuer le nom de l'événement.

```php
    use Joomla\CMS\Event\AbstractEvent;
    use Joomla\CMS\Factory;

    $dispatcher = Factory::getApplication()->getDispatcher();

    // Here we create an event however as long as you implement EventInterface you can create your own
    // custom classes
    $event = AbstractEvent::create(
        '',
        [
            'name' => $value,
        ]
    );

    $eventResult = $dispatcher->dispatch('', $event);
```

Si vous voulez autoriser l'utilisateur à modifier les valeurs, vous
pouvez ensuite utiliser le résultat de l'événement et retourner
getResults. Vous pouvez regarder

```php
    defined('_JEXEC') or die;

    use BadMethodCallException;
    use Joomla\CMS\Event\AbstractImmutableEvent;
    use Joomla\CMS\Table\TableInterface;

    /**
     * Event class for an event
     */
    class MyCustomEvent extends AbstractImmutableEvent
    {
        /**
         * Constructor.
         *
         * @param   string  $name       The event name.
         * @param   array   $arguments  The event arguments.
         *
         * @throws  BadMethodCallException
         */
        public function __construct($name, array $arguments = array())
        {
            if (!array_key_exists('myProperty', $arguments))
            {
                throw new BadMethodCallException("Argument 'myProperty' is required for event $name");
            }

            parent::__construct($name, $arguments);
        }

        /**
         * Setter for the myProperty argument
         *
         * @param   mixed  $value  The value to set
         *
         * @return  mixed
         *
         * @throws  BadMethodCallException  if the argument is not of the expected type
         */
        protected function setMyProperty($value)
        {
            if (!empty($value) && !is_object($value) && !is_array($value))
            {
                throw new BadMethodCallException("Argument 'src' of event {$this->name} must be empty, object or array");
            }

            return $value;
        }
    }
```

Pourquoi avons nous introduit ce nom de classe au-dessus des
paramétres ? En fait, cela facilite l'ajout de 'setters' et 'getters'
pour les propriétés - actuellement un plugin peut aussi changer une
propriété comme il le souhaite - il n'est pas possible de fixer de
limitations. De plus, cela rend plus facile pour les développeurs
l'ajout et la suppression de paramètres dans un événement sans avoir de
problèmes majeurs de compatibilité (comme vous appelez dorénavant des
méthodes définies et que vous n'êtes pas sujet à une variable qui serait
le second argument de votre fonction).

#### Comment avoir un maximum de compatibilité avec Joomla 3

Si vous voulez déclencher un événement de la même manière que la classe
supprimée J3.x JEventDispatcher, alors vous devez utiliser un code tel
que celui-ci :

```php
    $results = \Joomla\CMS\Factory::getApplication()->triggerEvent( '',  );
```

Il est important de noter que les paramètres doivent être dans un
tableau. La fonction du plugin elle-même va recevoir les paramètres en
tant qu'objet événement si elle implémente SubscriberInterface et comme
valeurs individuelles sinon ; Cependant cette méthode va toujours
retourner le tableau que le plugin retourne.

Notez que si AUCUN plugin dans un groupe n'implémente
SubscriberInterface, alors la propriété du résultat (à la fois le nom du
paramètre et le résultat d'un plugin) est utilisé en tant que propriété
spéciale et ne peut être utilisée.
