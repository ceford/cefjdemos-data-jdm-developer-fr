<!-- Filename: J4.x:MVC_Anatomy:_Administrator_Edit_Files / Display title: MVC Anatomie : Fichiers d'édition de l'administrateur -->

## Fichiers de données du pays

Les fichiers d'administration comprennent quatre vues : deux vues de liste pour les données des pays et les données des devises, et deux vues de modification pour gérer la saisie des données dans chacune des deux tables impliquées. Les vues de liste sont presque identiques à la vue de liste des pays décrite pour l'interface du site, elles ne seront donc pas abordées à nouveau ici. De même, les vues de modification sont presque identiques, donc une seule sera abordée, la vue de saisie des données pour un seul pays.

## src/Controller/CountryController.php

C'est extrêmement simple ! À part la déclaration de classe, il est dépourvu de contenu. Tout est pris en charge par la classe parente.

```php
    class CountryController extends FormController
    {
    }
```

## src/View/Country/HtmlView.php

C'est ici que les données sont récupérées du modèle pour être utilisées dans le formulaire de saisie de données. Il y a une différence significative par rapport à la vue de liste décrite précédemment : il est courant d'utiliser une barre d'outils avec des boutons pour Enregistrer, Annuler, Aide, etc.

Lorsque vous invoquez le formulaire d'édition en sélectionnant le titre d'un élément de la liste, l'URL contient un identifiant, par exemple option=com_countrybase&view=country&layout=edit&id=1. L'identifiant est le numéro d'enregistrement de la table. Pour un nouvel enregistrement invoqué via le bouton Nouveau de la liste, l'identifiant est absent. S'il est présent, le modèle remplit le formulaire de saisie de données avec les données de l'enregistrement existant.

```php
    public function display($tpl = null)
    {
        $this->form  = $this->get('Form');
        $this->item  = $this->get('Item');
        $this->state = $this->get('State');

        if (count($errors = $this->get('Errors')))
        {
            throw new GenericDataException(implode("\n", $errors), 500);
        }

        $this->addToolbar();

        return parent::display($tpl);
    }
```

### La barre d'outils

La fonction addToolbar() est généralement utilisée pour définir le titre de la page et ajouter les boutons appropriés pour les autorisations d'accès de la personne utilisant le formulaire. Remarquez l'utilisation de la variable \$isNew basée sur l'identifiant de l'enregistrement pour modifier légèrement la sortie.

```php
    protected function addToolbar()
    {
        Factory::getApplication()->input->set('hidemainmenu', true);
        $isNew      = ($this->item->id == 0);

        $canDo = ContentHelper::getActions('com_countrybase');

        $toolbar = Toolbar::getInstance();

        ToolbarHelper::title(
            Text::_('COM_COUNTRYBASE_COUNTRY_PAGE_TITLE_' . ($isNew ? 'ADD' : 'EDIT'))
        );

        if ($canDo->get('core.create')) {
            if ($isNew) {
                $toolbar->apply('country.save');
            } else {
                $toolbar->apply('country.apply');
            }
            $toolbar->save('country.save');
        }
        $toolbar->cancel('country.cancel', 'JTOOLBAR_CLOSE');

        ToolbarHelper::inlinehelp();
    }
```

**Remarques :**

- **hidemainmenu** est défini sur true pour tous les formulaires de modification afin d'éviter de quitter le formulaire via le menu Administrateur sans sauvegarder.
- **ToolbarHelper::title()** définit le titre dans la barre de titre de la page, dans ce cas à Modifier le pays ou Ajouter un pays.
- Les boutons **toolbar->action**, où action peut être sauvegarder, appliquer, annuler ou l'un des nombreux autres, prennent (controller.function) comme arguments. Donc, dans ce cas, lorsqu'un bouton est sélectionné, une action est transmise à une fonction de sauvegarde ou d'application dans la classe CountryController.
- **cancel** prend un argument supplémentaire, la clé de chaîne utilisée pour étiqueter le bouton.
- **inlinehelp()** affiche un bouton qui active ou désactive les descriptions des champs sous chaque champ de saisie de données.

## forms/pays.xml

Ce fichier contient la spécification des champs du formulaire, généralement dans l'ordre présenté dans le formulaire de modification. La plupart des champs sont explicites, donc seuls deux sont présentés ci-dessous. Les valeurs des noms sont les noms de colonnes utilisés dans les tables de la base de données.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<form>
        <field
            name="title"
            type="text"
            label="COM_COUNTRYBASE_COUNTRY_TITLE"
            required="true"
            maxlength="128"
        />

        <field
            name="iso_2"
            type="text"
            label="COM_COUNTRYBASE_COUNTRY_ISO_2"
            description="COM_COUNTRYBASE_COUNTRY_ISO_2_DESC"
            required="true"
            maxlength="3"
        />
```

## src/Model/CountryModel.php

Les fonctions dans ce fichier sont assez standard. Cependant, la fonction getTable() nécessite quelques explications.

```php
    public function getTable($name = '', $prefix = '', $options = array())
    {
        $name = 'Countries';
        $prefix = 'Table';

        if ($table = $this->_createTable($name, $prefix, $options)) {
            return $table;
        }

        throw new \Exception(Text::sprintf('JLIB_APPLICATION_ERROR_TABLE_NAME_NOT_SUPPORTED', $name), 0);
    }
```

Cette fonction ne fait pas référence au tableau lui-même ! Elle fait référence au constructeur de src/Table/CountriesTable.php. Cela peut être un peu déroutant car elle est appelée depuis la classe CountryModel.

## src/Table/CountriesTable.php

Ainsi, c'est ici que le nom de la table pour la liste des pays est défini.

```php
    class CountriesTable extends Table
    {
        /**
         * Constructor
         *
         * @param   DatabaseDriver  $db  Database connector object
         *
         * @since   1.6
         */
        public function __construct(DatabaseDriver $db)
        {
            parent::__construct('#__countrybase_countries', 'id', $db);

            $this->setColumnAlias('published', 'state');
        }
    }
```

Remarquez la déclaration de l'alias de colonne. Cela est utilisé car le formulaire utilise un champ de saisie de données nommé **published** mais le champ utilisé pour le stockage est nommé **status**.

## tmpl/pays/editer.php

Ceci est le fichier qui crée la sortie HTML. Il commence par deux assistants : HTMLHelper::_('behavior.formvalidator'); charge le JavaScript nécessaire pour la validation de formulaire côté client. Bien que les navigateurs modernes appliquent une validation de formulaire, cela n'empêche pas la soumission du formulaire. HTMLHelper::_('behavior.keepalive'); charge le JavaScript pour pinger le serveur afin de prévenir l'expiration de session lors de la saisie de formulaires complexes.

**Notes :**

- LayoutHelper::render('joomla.edit.title_alias', \$this); affiche un champ de titre Joomla standard.
- \$this-\>form-\>renderField('iso_2'); affiche le champ spécifié, dans ce cas iso_2.
- LayoutHelper::render('joomla.edit.global', \$this); affiche les champs sur le côté droit de l'onglet Détails.

```php
<?php

/**
 * @package     Countrybase.Administrator
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

HTMLHelper::_('behavior.formvalidator');
HTMLHelper::_('behavior.keepalive');

?>

<form action="<?php echo Route::_('index.php?option=com_countrybase&view=country&layout=edit&id=' .
        (int) $this->item->id); ?>"
    method="post" name="adminForm" id="country-form" class="form-validate">

    <?php echo LayoutHelper::render('joomla.edit.title_alias', $this); ?>

    <div>
        <?php echo HTMLHelper::_('uitab.startTabSet', 'myTab', array('active' => 'details')); ?>

        <?php echo HTMLHelper::_('uitab.addTab', 'myTab', 'details', Text::_('COM_COUNTRYBASE_COUNTRY_TAB_DETAILS')); ?>
        <div class="row">
            <div class="col-md-9">
                <div class="row">
                    <div class="col-md-6">
                        <?php echo $this->form->renderField('iso_2'); ?>
                        <?php echo $this->form->renderField('iso_3'); ?>
                        <?php echo $this->form->renderField('country_code'); ?>
                        <?php echo $this->form->renderField('region_code'); ?>
                        <?php echo $this->form->renderField('subregion_code'); ?>
                        <?php echo $this->form->renderField('phone_prefix'); ?>
                        <?php echo $this->form->renderField('currency_code'); ?>
                        <?php echo $this->form->renderField('id'); ?>
                    </div>
                </div>
            </div>
            <div class="col-md-3">
                <div class="card card-light">
                    <div class="card-body">
                        <?php echo LayoutHelper::render('joomla.edit.global', $this); ?>
                    </div>
                </div>
            </div>
        </div>
        <?php echo HTMLHelper::_('uitab.endTab'); ?>

        <?php echo HTMLHelper::_('uitab.endTabSet'); ?>
    </div>
    <input type="hidden" name="task" value="">
    <?php echo HTMLHelper::_('form.token'); ?>
</form>
```

Vous pouvez parcourir les ensembles de champs de formulaire et les champs à l'intérieur de chaque ensemble de champs. Cela peut simplifier la sortie de formulaires complexes avec de nombreux onglets.

![country edit form](../../../en/images/mvc-anatomy/com-countrybase-edit-country.png)

*Traduit par openai.com*

