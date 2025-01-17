<!-- Filename: J4.x:MVC_Anatomy:_Site_Files / Display title: Anatomie MVC : Fichiers du site -->

## Structure des fichiers

Il y a moins de fichiers dans la partie Site du composant que dans la partie Administrateur, donc cela semble être un bon point de départ. Seules les parties de chaque fichier nécessitant des explications seront abordées. Il est préférable d'ouvrir chaque fichier mentionné, de jeter un œil au contenu global, puis de trouver les parties expliquées. L'ordre alphabétique de la structure des fichiers est le suivant :

```bash
    site
    |- forms
        |- filter_countries.xml
    |- language
        |- en-GB
           |- com_countrybase.ini
    |- src
        |- Controller
           |- DisplayController
        |- Model
           |- CountriesModel.php
        |- Service
           |- Router.php
        |- View
           |- Countries
              |- HtmlView.php
    |- tmpl
        |- countries
           |- default.php
           |- default.xml
```

## DisplayContoller.php

```php
<?php

/**
 * @package     Countrybase.Site
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2025 Clifford E Ford
 * @license     GNU General Public License version 3 or later; see LICENSE.txt
 */

namespace Cefjdemos\Component\Countrybase\Site\Controller;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\MVC\Controller\BaseController;
use Joomla\CMS\Router\Route;
use Joomla\CMS\Session\Session;

/**
 * Countrybase Component Controller
 *
 * @since  1.0.0
 */
class DisplayController extends BaseController
{
    /**
     * The default view.
     *
     * @var    string
     */
    protected $default_view = 'countries';
}
```

Les parties de ce fichier sont expliquées comme suit :

### Avis de droit d'auteur

Chaque fichier php devrait commencer par un avis de droit d'auteur comme le suivant :

```php
<?php

/**
 * @package     Countrybase.Site
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2025 Clifford E Ford
 * @license     GNU General Public License version 3 or later; see LICENSE.txt
 */
```

Si vous créez souvent de nouveaux fichiers et copiez/collez cette section, n'oubliez pas de mettre à jour le nom du composant et l'avis de droits d'auteur. De plus, l'outil d'analyse de code nécessite une ligne vide avant un bloc de documentation.

### Espace de noms et vérification définie

Après l'avis de droit d'auteur, chaque fichier php doit comporter une ligne contenant defined('_JEXEC') ou die; sauf que les fichiers avec espace de noms doivent déclarer l'espace de noms avant tout autre code php, donc avant la vérification définie. Les fichiers php avec espace de noms sont ceux contenant des classes php de composants dans le dossier src ou ses sous-dossiers.

```php
namespace Cefjemos\Component\Countrybase\Site\Controller;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects
```

Le contrôle `defined` empêche un fichier php d'être exécuté en l'appelant directement via son URL. La constante `_JEXEC` est définie lorsque l'application démarre via le fichier index.php de la racine ou de l'administrateur. C'est une aide importante pour la sécurité.

Le contrôle `defined` est enveloppé dans `phpcs:disable` et `phpcs:enable` pour permettre une violation de la norme de codage PSR12.

Combiné avec l'espace de noms déclaré dans le fichier manifeste countrybase.xml, Joomla recherchera toute classe déclarée dans le fichier actuel dans root/components/com_countrybase/src/Controller - dans ce cas, ajoutant le nom de ce fichier, DisplayController.php.

### instructions use

Les déclarations use suivent généralement la vérification définie et sont souvent listées par ordre alphabétique. Les déclarations use définissent les emplacements des classes utilisées par ce fichier PHP. Parfois, des déclarations use sont présentes par erreur, déclarées mais non utilisées. Cela ne fait pas de mal mais devrait être corrigé. Il y a deux déclarations non utilisées ici :

```php
    use Joomla\CMS\Router\Route;
    use Joomla\CMS\Session\Session;
```

### Classe de Contrôleur

Le contrôleur d'affichage n'a presque rien à faire puisque tout le travail est effectué dans la classe parente. La seule chose importante qu'il fait est de définir la vue par défaut, dans ce cas **countries**. Cela amènera la vue du composant par défaut à utiliser les fichiers Countries/HtmlView.php et tmpl/countries/default.php pour afficher les données des pays.

```php
/**
 * Countrybase Component Controller
 *
 * @since  1.0.0 Created for first release.
 */
class DisplayController extends BaseController
{
    /**
     * The default view.
     *
     * @var    string
     */
    protected $default_view = 'countries';
}
```

Le bloc de documentation (DocBlock) précédant l'instruction `class` est utilisé pour la documentation automatique avec des outils tels que *Doxygen*. L'instruction `@since` est une *étiquette* qui indique le numéro de version de l'extension où cet élément a été introduit. Elle peut être suivie d'une explication en texte clair. Tout élément peut avoir un DocBlock avec un nombre quelconque d'étiquettes standard. Il est bon de garder votre propre code bien documenté !

N'oubliez pas de définir \$default_view pour ce contrôleur. Sans cela, le DisplayController utilisera la vue par défaut définie dans le fichier de configuration du composant ou, si elle n'existe pas, le nom du composant.

## src/View/Countries/HtmlView.php

Le contrôleur a défini la vue par défaut sur **pays**, donc l'étape suivante consiste à charger le code correspondant dans HtmlView.php. Certaines parties de ce fichier nécessitent une explication.

### Variables de classe

```php
class HtmlView extends BaseHtmlView
{
    /**
     * The model state
     *
     * @var  \Joomla\CMS\Object\CMSObject
     */
    protected $state = null;
    ...
    protected $items = null;
    ...
    protected $pagination = null;
    ...
    public $filterForm;
    ...
    public $activeFilters;
```

Les variables de classe sont utilisées pour stocker des informations sur la page à afficher :

- `$state` - l'état du modèle, souvent initialisé par des entrées de formulaire ou de chaîne de requête.
- `$items` - la liste des données des pays récupérées de la base de données.
- `$pagination` - un objet utilisé pour afficher un mécanisme de navigation de page s'il y a plus de pages que la limite de la liste, normalement 20 éléments.
- `$filterForm` - généralement non présent dans les pages du site mais utilisé dans com_countrybase pour filtrer par nom de pays ou état publié.
- `$activeFilters` - utilisé pour suivre quels filtres sont en cours d'utilisation.

### fonction d'affichage

C'est assez standard pour une vue de site, à part les parties filterForm et activeFilters :

```php
    public function display($tpl = null)
    {
        $this->state      = $this->get('State');
        $this->items      = $this->get('Items');
        $this->pagination = $this->get('Pagination');
        $this->filterForm    = $this->get('FilterForm');
        $this->activeFilters = $this->get('ActiveFilters');

        // Flag indicates to not add limitstart=0 to URL
        $this->pagination->hideEmptyLimitstart = true;

        // Check for errors.
        if (count($errors = $this->get('Errors')))
        {
            throw new GenericDataException(implode("\n", $errors), 500);
        }

        parent::display($tpl);
    }
```

Les déclarations de la forme `$this->get('Xxxx')` incitent Joomla à rechercher dans `CountriesModel.php` une fonction nommée `getXxxx()` et à renvoyer toutes les données produites par ce code pour le stockage et l'utilisation dans la vue. Souvent, la fonction se trouve dans le parent de CountriesModel. Par exemple, la fonction getItems n'est pas présente dans CountriesModel.php mais est présente dans le ListModel qu'elle étend.

En résumé, la classe de vue récupère les données du modèle puis appelle sa classe parente pour afficher les données.

## Modèle/PaysModèle.php

Il y a un petit nombre de fonctions dans le modèle que vous devez généralement compléter vous-même. D'autres sont héritées du modèle parent ListModel.

### __construct

Il est courant d'inclure dans le constructeur tous les champs de filtrage que vous pourriez souhaiter utiliser. Sans eux, les filtres peuvent sembler n'avoir aucun effet. Ils sont souvent oubliés lorsque vous souhaitez ajouter un filtre un peu plus tard. Chaque champ est donné par son nom de champ avec un alias de nom de table, généralement a, b, c, et ainsi de suite, mais cela peut être n'importe quoi tant que c'est cohérent avec le code ailleurs.

```php
       public function __construct($config = array())
        {
            if (empty($config['filter_fields']))
            {
                $config['filter_fields'] = array(
                        'id', 'a.id',
                        'title', 'a.title',
                        'iso_2', 'a.iso_2',
                        'iso_3', 'a.iso_3',
                        'country_code', 'a.country_code',
                        'region_code', 'a.region_code',
                        'state', 'a.state',
                        'subregion_code', 'a.subregion_code',
                        'phone_prefix', 'a.phone_prefix',
                        'currency_code', 'a.currency_code',
                );
            }

            parent::__construct($config);
        }
```

### remplirÉtat

Cette fonction prend des paramètres d'entrée et les prépare pour une utilisation dans une requête de base de données. Certains paramètres sont gérés dans le parent, il n'est donc pas nécessaire de faire quoi que ce soit ici. Par exemple, si le champ de recherche est **titre**, il est géré par le parent, de même que les champs **état** et de pagination **début** et **limite**.

```php
       protected function populateState($ordering = 'title', $direction = 'ASC')
        {
            // List state information.
            parent::populateState($ordering, $direction);
        }
```

### getStoreId

Cette fonction crée un hachage pour stocker une requête à utiliser ailleurs.

```php
       protected function getStoreId($id = '')
        {
            // Compile the store id.
            $id .= ':' . $this->getState('filter.search');
            $id .= ':' . $this->getState('filter.published');

            return parent::getStoreId($id);
        }
```

### getListQuery

C'est là que la requête est créée pour extraire des données de la base de données. Cela nécessite une certaine compréhension de la manière dont les requêtes sont construites.

```php
    protected function getListQuery()
    {
        // Create a new query object.
        $db = $this->getDatabase();
        $query = $db->getQuery(true);

        // Select the required fields from the table.
        $query->select(
            $this->getState(
                'list.select',
                [
                    $db->quoteName('a.title'),
                    $db->quoteName('a.iso_2'),
                    $db->quoteName('a.iso_3'),
                    $db->quoteName('a.country_code'),
                    $db->quoteName('a.region_code'),
                    $db->quoteName('a.subregion_code'),
                    $db->quoteName('a.phone_prefix'),
                    $db->quoteName('a.currency_code'),
                    $db->quoteName('a.state'),
                    $db->quoteName('b.title') . ' AS currency_title',
                    $db->quoteName('b.symbol'),
                    $db->quoteName('b.dollar_exchange_rate'),
                ]
            )
        )
            ->from($db->quoteName('#__countrybase_countries', 'a'))
            ->leftjoin($db->quoteName('#__countrybase_currencies', 'b') . 'ON a.currency_code = b.currency_code');

        // Filter by search in title.
        $search = $this->getState('filter.search');

        if (!empty($search)) {
            $search = '%' . str_replace(' ', '%', trim($search) . '%');
            $query->where($db->quoteName('a.title') . ' LIKE :search')
            ->bind(':search', $search, ParameterType::STRING);
        }

        // Filter by published state
        $published = (string) $this->getState('filter.published');

        if ($published !== '*') {
            if (is_numeric($published)) {
                $state = (int) $published;
                $query->where($db->quoteName('a.state') . ' = :state')
                    ->bind(':state', $state, ParameterType::INTEGER);
            }
        }

        // Add the list ordering clause.
        $orderCol = $this->state->get('list.ordering', 'a.title');
        $orderDirn = $this->state->get('list.direction', 'ASC');

        $query->order($db->escape($orderCol) . ' ' . $db->escape($orderDirn));
        return $query;
    }
```

Explication

- **getQuery(true)** obtient un nouvel objet de requête vide.
- **\$query-\>select()** ajoute une instruction SELECT. Il peut y avoir plusieurs instructions select - Joomla les concatène.
- **table aliases a and b** remarquez que certaines colonnes proviennent de tables différentes.
- **-\>from()** définit quelle table est la table a. Cela peut être dans une instruction séparée : \$query-\>from();
- **-\>leftjoin()** définit la table b et comment elle doit être jointe à la table a.
- **\$query-\>where()** utilise tous les filtres définis, un pour la **recherche** et un autre pour l'**état**.
- **return \$query** il n'y a pas d'appel de parent, tout doit être configuré ici dans la requête.

## tmpl/pays/default.php

C'est la partie du code où le contenu HTML est créé. Pour la liste des pays, une table est nécessaire avec une ligne d'en-tête et une ligne pour les données de chaque pays. Comme il y a 250 pays, un mécanisme de pagination est nécessaire pour afficher un sous-ensemble de pays quelques-uns à la fois. Cela nécessite un formulaire. Et dans ce cas, une barre d'outils de recherche Joomla **searchtools** standard est utile. Voici :

```php
<?php

/**
 * @package     Countrybase.Site
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2025 Clifford E Ford
 * @license     GNU General Public License version 3 or later; see LICENSE.txt
 */

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;
use Joomla\CMS\Layout\LayoutHelper;
use Joomla\CMS\Router\Route;

$listOrder = $this->escape($this->state->get('list.ordering'));
$listDirn  = $this->escape($this->state->get('list.direction'));

?>
<h1><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES'); ?></h1>

<form action="<?php echo Route::_('index.php?option=com_countrybase'); ?>"
    method="post" name="adminForm" id="adminForm">

<?php echo LayoutHelper::render('joomla.searchtools.default', array('view' => $this)); ?>

<div class="table-responsive">
    <table class="table table-striped">
    <caption><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_TABLE_CAPTION'); ?></caption>
    <thead>
    <tr>
        <th scope="col">
            <?php echo HTMLHelper::_(
                'searchtools.sort',
                'COM_COUNTRYBASE_COUNTRIES_COUNTRY',
                'a.title',
                $listDirn,
                $listOrder
            ); ?>
        </th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_ISO_2'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_ISO_3'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_CURRENCY_TITLE'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_CURRENCY_SYMBOL'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_CURRENCY_CODE'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_XRATE'); ?></th>
    </tr>
    </thead>
    <tbody>
    <?php foreach ($this->items as $id => $item) : ?>
    <tr>
        <td><?php echo $item->title; ?></td>
        <td><?php echo $item->iso_2; ?></td>
        <td><?php echo $item->iso_3; ?></td>
        <td><?php echo $item->currency_title; ?></td>
        <td><?php echo $item->symbol; ?></td>
        <td><?php echo $item->currency_code; ?></td>
        <td><?php echo $item->dollar_exchange_rate; ?></td>
    </tr>
    <?php endforeach; ?>
    </tbody>
    </table>
</div>

<?php echo $this->pagination->getListFooter(); ?>

<input type="hidden" name="task" value="">
<input type="hidden" name="boxchecked" value="0">
<?php echo HTMLHelper::_('form.token'); ?>

</form>
```

Points à noter :

- **\$listOrder** et **\$listDirection** sont utilisés pour trier par en-tête de colonne. Seul le titre a été configuré pour cela.
- **form action** est généralement configuré pour se référer à lui-même.
- **LayoutHelper::render('joomla.searchtools.default',...)** crée une barre de recherche du type vue dans les pages de liste de l'administrateur. **Il a besoin d'un formulaire de filtre !**
- **\$this-\>pagination-\>getListFooter()** récupère le code html pour le widget de pagination.
- **task** ce champ caché est rempli par javascript lorsque le formulaire est soumis.
- **boxchecked** ce champ caché est utilisé lorsqu'une ou plusieurs cases à cocher de ligne sont sélectionnées pour une opération par lot. Pas vraiment nécessaire ici !
- **HTMLHelper::\_('form.token');** obtient le code pour un jeton de formulaire utilisé comme dispositif de sécurité pour les soumissions de formulaire impliquant une soumission de données.

## tmpl/countries/default.xml

Ce fichier est utilisé pour créer un élément de menu. Il porte le même nom que le fichier php, donc default.xml dans ce cas.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<metadata>
    <layout title="COM_COUNTRYBASE_VIEW_DEFAULT_MENU_LABEL"
        option="COM_COUNTRYBASE_VIEW_DEFAULT_OPTION">
        <help
            url="components/com_countrybase/help/en-GB/countrybase.html"
        />
        <message>
            <![CDATA[COM_COUNTRYBASE_VIEW_DEFAULT_MENU_DESC]]>
        </message>
    </layout>

    <!-- Add fields to the parameters object for the layout. -->
    <fields name="params">

        <!-- Options -->
        <fieldset name="options">
        </fieldset>

    </fields>
</metadata>
```

Notes :

- **l'url d'aide** pointe vers un fichier d'aide dans le dossier de l'administrateur. Il vous permet de créer vos propres fichiers d'aide locaux, invoqués à partir du bouton Aide du formulaire de modification du menu après avoir sélectionné le type de menu Vue par défaut Countrybase.
- **paramètres** vous permet d'utiliser des paramètres, par exemple s'il faut ou non afficher une colonne particulière dans la liste des pays. Aucun paramètre n'a encore été spécifié.
- Les traductions de **clé** doivent être dans le fichier administrator/language/en-GB/countrybase.sys.ini. La clé *COM_COUNTRYBASE_VIEW_DEFAULT_MENU_DESC*, traduite en *Afficher une page de pays.*, apparaît comme une description dans le formulaire de sélection du type d'élément de menu.

## forms/filter_countries.xml

Ce fichier est nécessaire pour la barre de recherche. Sans lui, Joomla renverra une erreur fatale. Le nom du fichier doit être exactement comme indiqué : le nom de la vue précédé de filter\_. Le contenu est simple, juste des définitions pour le champ de recherche et tout autre filtre que vous souhaitez utiliser.

```xml
<?xml version="1.0" encoding="utf-8"?>
<form>
    <fields name="filter">
        <field
            name="search"
            type="text"
            label="COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_LABEL"
            description="COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_DESC"
            hint="JSEARCH_FILTER"
        />

        <field
            name="published"
            type="status"
            label="JOPTION_SELECT_PUBLISHED"
            onchange="this.form.submit();"
            >
            <option value="">JOPTION_SELECT_PUBLISHED</option>
        </field>

    </fields>

    <fields name="list">
        <field
            name="fullordering"
            type="list"
            label="JGLOBAL_SORT_BY"
            default="a.title ASC"
            onchange="this.form.submit();"
            >
            <option value="">JGLOBAL_SORT_BY</option>
            <option value="a.title ASC">COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_ASC</option>
            <option value="a.title DESC">COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_DESC</option>
            <option value="a.iso_2 ASC">ISO2 ASC</option>
            <option value="a.iso_2 DESC">ISO2 DESC</option>
            <option value="a.iso_3 ASC">ISO3 ASC</option>
            <option value="a.iso_3 DESC">ISO3 DESC</option>
            <option value="a.currency_code ASC">COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_ASC</option>
            <option value="a.currency_code DESC">COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_DESC</option>
            <option value="a.id ASC">JGRID_HEADING_ID_ASC</option>
            <option value="a.id DESC">JGRID_HEADING_ID_DESC</option>
        </field>

        <field
            name="limit"
            type="limitbox"
            label="JGLOBAL_LIST_LIMIT"
            default="25"
            onchange="this.form.submit();"
        />
    </fields>
</form>
```

Notez que toutes les clés de chaîne commençant par J sont définies par Joomla et vous ne devriez pas les inclure dans vos fichiers de langue.

## language/fr-FR/com_countrybase.ini

Joomla charge toujours les traductions des clés de langue anglaise avant toute autre langue. Cela garantit que les clés n'apparaissent pas dans le résultat si une langue est partiellement traduite. Étant donné que les clés de langue sont de longs mots en pseudo-anglais, il est considéré préférable d'avoir un mélange d'anglais et d'une autre langue plutôt qu'un mélange de clés et d'une autre langue. Si une autre langue est utilisée, Joomla remplace les chaînes anglaises par celles de l'autre langue.

Notez qu'il est courant de lister les chaînes par ordre alphabétique des clés :

```ini
    ; Joomla! Project
    ; (C) 2005 Open Source Matters, Inc. https://www.joomla.org
    ; License GNU General Public License version 2 or later; see LICENSE.txt
    ; Note : All ini files need to be saved as UTF-8

    COM_COUNTRYBASE_COUNTRIES_COUNTRY="Country"
    COM_COUNTRYBASE_COUNTRIES_CURRENCY_CODE="Code"
    COM_COUNTRYBASE_COUNTRIES_CURRENCY_SYMBOL="Symbol"
    COM_COUNTRYBASE_COUNTRIES_CURRENCY_TITLE="Currency"
    COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_ASC="Country ASC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_DESC="Country DESC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_ASC="Currency code ASC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_DESC="Currency code DESC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_DESC="Search in Country Name"
    COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_LABEL="Search"
    COM_COUNTRYBASE_COUNTRIES_ISO_2="ISO2"
    COM_COUNTRYBASE_COUNTRIES_ISO_3="ISO3"
    COM_COUNTRYBASE_COUNTRIES_TABLE_CAPTION="Table of Country Currencies"
    COM_COUNTRYBASE_COUNTRIES_XRATE="Exchange Rate"
    COM_COUNTRYBASE_COUNTRIES="Countries"
```

## src/Service/Router.php

Le routeur est nécessaire pour les URL SEO. Sans lui, un lien de menu peut apparaître comme option=com_countrybase&view=countries. Avec lui, un lien apparaîtra comme country-base.html ou tout autre nom que vous choisissez pour l'alias du titre du lien.

```php
    public function __construct(
        SiteApplication $app,
        AbstractMenu $menu
    ) {

        $countries = new RouterViewConfiguration('countries');
        $countries->setKey('id');
        $this->registerView($countries);

        parent::__construct($app, $menu);

        $this->attachRule(new MenuRules($this));
        $this->attachRule(new StandardRules($this));
        $this->attachRule(new NomenuRules($this));
    }
```

S'il y a plus de vues, par exemple un tableau de devises, vous devez définir chaque vue ici avant l'instruction parent::\_\_construct().

*Traduit par openai.com*

