<!-- Filename: J4.x:Joomla_Core_APIs / Display title: Joomla - API natives -->

Joomla!  4.0

Cette page répertorie les points de terminaison disponibles dans Joomla
à l'aide d'un exemple de commandes curl.

Chaque URL nécessite une authentification sauf si une URL est désignée
publique. Pour plus de sécurité dans Joomla 4.0.0, nous prévoyons de
faire en sorte que l’application Api par défaut nécessite un compte
Super User (l’API Application étant nouvelle), ce qui risque de
s’atténuer car l’API se stabilise et est bien testée par la communauté.
Si vous utilisez le plug-in d'authentification de base (actuellement le
seul plug-in livré avec Joomla 4 alpha 10), il nécessite l'ajout des
commandes curl ci-dessous à l'aide de --user
nom_utilisateur:mot_de_passe

Chaque URL doit être précédée de l’hôte Joomla avant le chemin (par
exemple, au lieu de `/api/index.php/v1/article` vous avez besoin de
`http://example.com/api/index.php/v1/article`)

Tous les noms de propriétés entre accolades ({}) indiquent que la
propriété est une variable à remplacer.

Sauf indication contraire, ces API ont été introduites dans Joomla 4.
Pour plus d'informations sur la Spécification d'API Joomla (par
opposition à cette liste d'URL et d'options), veuillez consulter la page
[Spécification Joomla
Api](https://docs.joomla.org/Joomla_Api_Specification "Special:MyLanguage/Joomla Api Specification").

Where end points require the value of {app} the variable currently takes
two values, site (front end), or administrator {back end)

## Useful Resources

A number of complimentary resources have been created to introduce
Joomla Web Services and assist you with learning how to implement Web
Services on your project.

- <a href="https://github.com/alexandreelise/j4x-api-collection"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Postman collection of Joomla Web
  Services API calls by Alexandre Elise</a>
- <a href="https://www.youtube.com/watch?v=lT9qodsvfZg"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Youtube: Joomla 4 tutorial: Using the
  Web Services API with Eoin Oliver</a>
- <a
  href="https://magazine.joomla.org/all-issues/august-2020/joomla-web-services-api-101-tokens,-testing-and-a-taste-test"
  class="external text" target="_blank" rel="noreferrer noopener">Joomla
  Community Magazine: Joomla Web Services API 101 by Patrick Jackson</a>

## Bannières

### Bannières

#### Obtenir la liste des bannières

curl -X GET /api/index.php/v1/banners

#### Obtenir une seule bannière

curl -X GET /api/index.php/v1/banners/{banner_id}

#### Supprimer une bannière

curl -X DELETE /api/index.php/v1/banners/{banner_id}

#### Créer une bannière

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners -d

    {
        "catid": 3,
        "clicks": 0,
        "custombannercode": "",
        "description": "Text",
        "metakey": "",
        "name": "Name",
        "params": {
            "alt": "",
            "height": "",
            "imageurl": "",
            "width": ""
        }
    }

#### Mettre à jour une bannière

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/{banner_id} -d

    {
        "alias": "name",
        "catid": 3,
        "description": "New Text",
        "name": "New Name"
    }

### Clients

#### Obtenir la liste des clients

curl -X GET /api/index.php/v1/banners/clients

#### Obtenir un seul client

curl -X GET /api/index.php/v1/banners/clients/{client_id}

#### Supprimer un client

curl -X DELETE /api/index.php/v1/banners/clients/{client_id}

#### Créer un client

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners/clients -d

    {
        "contact": "Name",
        "email": "email@mail.com",
        "extrainfo": "",
        "metakey": "",
        "name": "Clients",
        "state": 1
    }

#### Mettre à jour un client

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/clients/{client_id} -d

    {
        "contact": "new Name",
        "email": "newemail@mail.com",
        "name": "Clients"
    }

### Catégories

#### Obtenir la liste des catégories

curl -X GET /api/index.php/v1/banners/categories

#### Obtenir une seule catégorie

curl -X GET /api/index.php/v1/banners/categories/{category_id}

#### Supprimer une catégorie

curl -X DELETE /api/index.php/v1/banners/categories/{category_id}

#### Créer une catégorie

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners/categories -d

    {
        "access": 1,
        "alias": "cat",
        "extension": "com_banners",
        "language": "*",
        "note": "",
        "parent_id": 1,
        "published": 1,
        "title": "Title"
    }

#### Mettre à jour une catégorie

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/categories/{category_id} -d

    {
        "alias": "cat",
        "note": "Some Text",
        "parent_id": 1,
        "title": "New Title"
    }

### Historique du contenu

#### Obtenir une liste de l'historique du contenu

curl -X GET /api/index.php/v1/banners/contenthistory/{banner_id}

#### Toggle Keep Content History

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/contenthistory/keep/{contenthistory_id}

#### Supprimer l'historique du contenu

curl -X DELETE
/api/index.php/v1/banners/contenthistory/{contenthistory_id}

## Configuration

### Application

#### Get List of Application Configs

curl -X GET /api/index.php/v1/config/application

#### Update Application Config

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/config/application -d

    {
        "debug": true,
        "sitename": "123"
    }

### Composant

#### Obtenir la liste de configuration des composants

curl -X GET /api/index.php/v1/config/{component_name}

Example “component_name” is “com_content”.

#### Update Application Config

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/config/application -d

    {
        "link_titles": 1
    }

## Contact

### Contact

#### Obtenir la liste des contacts

curl -X GET /api/index.php/v1/contact

#### Obtenir un seul contact

curl -X GET /api/index.php/v1/contact/{contact_id}

#### Supprimer un contact

curl -X DELETE /api/index.php/v1/contact/{contact_id}

#### Créer un contact

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/contact -d

    {
        "alias": "contact",
        "catid": 4,
        "language": "*",
        "name": "Contact"
    }

#### Mettre à jour un contact

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/contact/{contact_id} -d

    {
        "alias": "contact",
        "catid": 4,
        "name": "New Contact"
    }

#### Soumettre un formulaire de contact

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/contact/form/{contact_id} -d

    {
        "contact_email": "email@mail.com",
        "contact_message": "some text",
        "contact_name": "name",
        "contact_subject": "subject"
    }

### Catégories

1.  Route Contact Categories is: "v1/contact/categories"
2.  Working with it is similar to [Banners
    Categories](https://docs.joomla.org/#Categories "Special:MyLanguage/").

### Champs des fiches de contact

#### Obtenir la liste des champs de fiches de contact

curl -X GET /api/index.php/v1/fields/contact/contact

#### Obtenir un seul champ de fiches de contact

curl -X GET /api/index.php/v1/fields/contact/contact/{field_id}

#### Supprimer un champ de fiches de contact

curl -X DELETE /api/index.php/v1/fields/contact/contact/{field_id}

#### Créer un champ de fiches de contact

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/fields/contact/contact -d

    {
        "access": 1,
        "context": "com_contact.contact",
        "default_value": "",
        "description": "",
        "group_id": 0,
        "label": "contact field",
        "language": "*",
        "name": "contact-field",
        "note": "",
        "params": {
            "class": "",
            "display": "2",
            "display_readonly": "2",
            "hint": "",
            "label_class": "",
            "label_render_class": "",
            "layout": "",
            "prefix": "",
            "render_class": "",
            "show_on": "",
            "showlabel": "1",
            "suffix": ""
        },
        "required": 0,
        "state": 1,
        "title": "contact field",
        "type": "text"
    }

#### Mettre à jour un champ de fiches de contact

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/fields/contact/contact/{field_id} -d

    {
        "title": "new contact field",
        "name": "contact-field",
        "label": "contact field",
        "default_value": "",
        "type": "text",
        "note": "",
        "description": "Some New Text"
    }

### Fields Contact Mail

1.  Route Fields Contact Mail is: "v1/fields/contact/mail"
2.  Working with it is similar to [Fields
    Contact](https://docs.joomla.org/#Fields_Contact "Special:MyLanguage/").

### Fields Contact Categories

1.  Route Fields Contact Categories is: "v1/fields/contact/categories"
2.  Working with it is similar to [Fields
    Contact](https://docs.joomla.org/#Fields_Contact "Special:MyLanguage/").

### Groups Fields Contact

#### Get List of Groups Fields Contact

curl -X GET /api/index.php/v1/fields/groups/contact/contact

#### Get Single Group Fields Contact

curl -X GET /api/index.php/v1/fields/groups/contact/contact/{group_id}

#### Delete Group Fields Contact

curl -X DELETE
/api/index.php/v1/fields/groups/contact/contact/{group_id}

#### Create Group Fields Contact

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/fields/groups/contact/contact -d

    {
        "access": 1,
        "context": "com_contact.contact",
        "default_value": "",
        "description": "",
        "group_id": 0,
        "label": "contact field",
        "language": "*",
        "name": "contact-field3",
        "note": "",
        "params": {
            "class": "",
            "display": "2",
            "display_readonly": "2",
            "hint": "",
            "label_class": "",
            "label_render_class": "",
            "layout": "",
            "prefix": "",
            "render_class": "",
            "show_on": "",
            "showlabel": "1",
            "suffix": ""
        },
        "required": 0,
        "state": 1,
        "title": "contact field",
        "type": "text"
    }

#### Update Group Fields Contact

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/fields/groups/contact/contact/{group_id} -d

    {
        "title": "new contact group",
        "note": "",
        "description": "new description"
    }

### Group Fields Contact Mail

1.  Route Group Fields Contact Mail is: "v1/fields/groups/contact/mail"
2.  Working with it is similar to [Group Fields
    Contact](https://docs.joomla.org/#Groups_Fields_Contact "Special:MyLanguage/").

### Group Fields Contact Categories

1.  Route Group Fields Contact Categories is:
    "v1/fields/groups/contact/categories"
2.  Working with it is similar to [Group Fields
    Contact](https://docs.joomla.org/#Groups_Fields_Contact "Special:MyLanguage/").

### Historique du contenu

1.  Route Content History is: "v1/contact/contenthistory"
2.  Working with it is similar to [Banners Content
    History](https://docs.joomla.org/#Content_History "Special:MyLanguage/").

## Contenu

### Articles

#### Obtenir la liste des articles

curl -X GET /api/index.php/v1/content/articles

#### Obtenir un seul article

curl -X GET /api/index.php/v1/content/articles/{article_id}

#### Supprimer un article

curl -X DELETE /api/index.php/v1/content/articles/{article_id}

#### Créer un article

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/content/articles -d

    {
        "alias": "my-article",
        "articletext": "My text",
        "catid": 64,
        "language": "*",
        "metadesc": "",
        "metakey": "",
        "title": "Here's an article"
    }

Currently the options mentioned here are required properties. However
the intention is currently to make AT LEAST metakey and metadesc
optional in the API.

#### Mettre à jour un article

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/content/articles/{article_id} -d

    {
        "catid": 64,
        "title": "Updated article"
    }

### Catégories

1.  Route Content Categories is: "v1/content/categories"
2.  Working with it is similar to [Banners
    Categories](https://docs.joomla.org/#Categories "Special:MyLanguage/"),
    note if workflows is enabled then specifying a workflow is required
    (similarly to the UI)

### Fields Articles

1.  Route Fields Articles is: "v1/fields/content/articles"
2.  Working with it is similar to [Fields
    Contact](https://docs.joomla.org/#Fields_Contact "Special:MyLanguage/").

### Groups Fields Articles

1.  Route Groups Fields Articles is: "v1/fields/groups/content/articles"
2.  Working with it is similar to [Groups Fields
    Contact](https://docs.joomla.org/#Groups_Fields_Contact "Special:MyLanguage/").

### Fields Categories

1.  Route Fields Categories is: "v1/fields/groups/content/categories"
2.  Working with it is similar to [Fields
    Contact](https://docs.joomla.org/#Fields_Contact "Special:MyLanguage/").

### Content History

1.  Route Content History is:
    "v1/content/articles/{article_id}/contenthistory"
2.  Working with it is similar to [Banners Content
    History](https://docs.joomla.org/#Content_History "Special:MyLanguage/").

## Langues

### Langues

#### Obtenir la liste des langues

curl -X GET /api/index.php/v1/languages

#### Installation de langues

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages -d

    {
        "package": "pkg_fr-FR"
    }

### Langues de contenu

#### Obtenir la liste des langues de contenu

curl -X GET /api/index.php/v1/languages/content

#### Obtenir une seule langue de contenu

curl -X GET /api/index.php/v1/v1/languages/content/{language_id}

#### Supprimer une langue de contenu

curl -X DELETE /api/index.php/v1/languages/content/{language_id}

#### Créer une langue de contenu

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/content -d

    {
        "access": 1,
        "description": "",
        "image": "fr_FR",
        "lang_code": "fr-FR",
        "metadesc": "",
        "metakey": "",
        "ordering": 1,
        "published": 0,
        "sef": "fk",
        "sitename": "",
        "title": "French (FR)",
        "title_native": "Français (France)"
    }

#### Mettre à jour une langue de contenu

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/languages/content/{language_id} -d

    {
        "description": "",
        "lang_code": "en-GB",
        "metadesc": "",
        "metakey": "",
        "sitename": "",
        "title": "English (en-GB)",
        "title_native": "English (United Kingdom)"
    }

### Substitutions de langue

#### Get List of Overrides Languages Constants

curl -X GET /api/index.php/v1/languages/overrides/{app}/{lang_code}

#### Get Single Override Language Constant

curl -X GET
/api/index.php/v1/languages/overrides/{app}/{lang_code}/{constant_id}

#### Supprimer une langue de contenu

curl -X DELETE
/api/index.php/v1/languages/overrides/{app}/{lang_code}/{constant_id}

#### Create Content Language

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/{app}/{lang_code} -d

    {
        "key":"new_key",
        "override": "text"
    }

#### Mettre à jour une langue de contenu

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/{app}/{lang_code}/{constant_id} -d

    {
        "key":"new_key",
        "override": "new text"
    }

var app - enum {"site", "administrator"}

var lang_code - string Example: “fr-FR“, “en-GB“ you can get lang_code
from v1/languages/content

#### Search Override Constant

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/search -d

    {
        "searchstring": "JLIB_APPLICATION_ERROR_SAVE_FAILED",
        "searchtype": "constant"
    }

var searchtype - enum {“constant”, “value”}. “constant” search by
constant name, “value” - search by constant value

#### Refresh Override Search Cache

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/search/cache/refresh

## Menus

### Menus

#### Obtenir la liste des menus

curl -X GET /api/index.php/v1/menus/{app}

#### Obtenir un seul menu

curl -X GET /api/index.php/v1/menus/{app}/{menu_id}

#### Supprimer un menu

curl -X DELETE /api/index.php/v1/menus/{app}/{menu_id}

#### Créer un menu

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/menus/{app} -d

    {
        "client_id": 0,
        "description": "The menu for the site",
        "menutype": "menu",
        "title": "Menu"
    }

#### Mettre à jour un menu

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/{menu_id} -d

    {
        "menutype": "menu",
        "title": "New Menu"
    }

### Liens de menu

#### Obtenir la liste des types de liens de menu

curl -X GET /api/index.php/v1/menus/{app}/items/types

#### Obtenir la liste des liens de menu

curl -X GET /api/index.php/v1/menus/{app}/items

#### Obtenir un seul lien de menu

curl -X GET /api/index.php/v1/menus/{app}/items/{menu_item_id}

#### Supprimer un lien de menu

curl -X DELETE /api/index.php/v1/menus/{app}/items/{menu_item_id}

#### Créer un lien de menu

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/items -d

    {
        "access": "1",
        "alias": "",
        "associations": {
            "en-GB": "",
            "fr-FR": ""
        },
        "browserNav": "0",
        "component_id": "20",
        "home": "0",
        "language": "*",
        "link": "index.php?option=com_content&view=form&layout=edit",
        "menutype": "mainmenu",
        "note": "",
        "params": {
            "cancel_redirect_menuitem": "",
            "catid": "",
            "custom_cancel_redirect": "0",
            "enable_category": "0",
            "menu-anchor_css": "",
            "menu-anchor_title": "",
            "menu-meta_description": "",
            "menu-meta_keywords": "",
            "menu_image": "",
            "menu_image_css": "",
            "menu_show": "1",
            "menu_text": "1",
            "page_heading": "",
            "page_title": "",
            "pageclass_sfx": "",
            "redirect_menuitem": "",
            "robots": "",
            "show_page_heading": ""
        },
        "parent_id": "1",
        "publish_down": "",
        "publish_up": "",
        "published": "1",
        "template_style_id": "0",
        "title": "title",
        "toggle_modules_assigned": "1",
        "toggle_modules_published": "1",
        "type": "component"
    }

Exemple pour la "Page Créer un article"

#### Mettre à jour un lien de menu

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/items/{menu_item_id} -d

    {
        "component_id": "20",
        "language": "*",
        "link": "index.php?option=com_content&view=form&layout=edit",
        "menutype": "mainmenu",
        "note": "",
        "title": "new title",
        "type": "component"
    }

Exemple pour la "Page Créer un article"

## Messagerie privée

### Messagerie privée

#### Obtenir la liste des messages privés

curl -X GET /api/index.php/v1/messages

#### Obtenir un seul message privé

curl -X GET /api/index.php/v1/messages/{message_id}

#### Supprimer un message privé

curl -X DELETE /api/index.php/v1/messages/{message_id}

#### Créer un message privé

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/messages -d

    {
        "message": "text",
        "state": 0,
        "subject": "text",
        "user_id_from": 773,
        "user_id_to": 772
    }

#### Mettre à jour un message privé

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/messages/{message_id} -d

    {
        "message": "new text",
        "subject": "new text",
        "user_id_from": 773,
        "user_id_to": 772
    }

## Modules

### Modules

#### Obtenir la liste des types de modules

curl -X GET /api/index.php/v1/modules/types/{app}

#### Obtenir la liste des modules

curl -X GET /api/index.php/v1/modules/{app}

#### Obtenir un seul module

curl -X GET /api/index.php/v1/modules/{app}/{module_id}

#### Supprimer un module

curl -X DELETE /api/index.php/v1/modules/{app}/{module_id}

#### Créer un module

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/modules/{app} -d

    {
        "access": "1",
        "assigned": [
            "101",
            "105"
        ],
        "assignment": "0",
        "client_id": "0",
        "language": "*",
        "module": "mod_articles_archive",
        "note": "",
        "ordering": "1",
        "params": {
            "bootstrap_size": "0",
            "cache": "1",
            "cache_time": "900",
            "cachemode": "static",
            "count": "10",
            "header_class": "",
            "header_tag": "h3",
            "layout": "_:default",
            "module_tag": "div",
            "moduleclass_sfx": "",
            "style": "0"
        },
        "position": "",
        "publish_down": "",
        "publish_up": "",
        "published": "1",
        "showtitle": "1",
        "title": "Title"
    }

Example for "Articles - Archived"

#### Mettre à jour un module

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/modules/{app}/{module_id} -d

    {
        "access": "1",
        "client_id": "0",
        "language": "*",
        "module": "mod_articles_archive",
        "note": "",
        "ordering": "1",
        "title": "New Title"
    }

Example for "Articles - Archived"

## Newsfeeds

### Feeds

#### Get List of Feeds

curl -X GET /api/index.php/v1/newsfeeds/feeds

#### Get Single Feed

curl -X GET /api/index.php/v1/newsfeeds/feeds/{feed_id}

#### Delete Feed

curl -X DELETE /api/index.php/v1/newsfeeds/feeds/{feed_id}

#### Create Feed

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/newsfeeds/feeds -d

    {
        "access": 1,
        "alias": "alias",
        "catid": 5,
        "description": "",
        "images": {
            "float_first": "",
            "float_second": "",
            "image_first": "",
            "image_first_alt": "",
            "image_first_caption": "",
            "image_second": "",
            "image_second_alt": "",
            "image_second_caption": ""
        },
        "language": "*",
        "link": "http://samoylov/joomla/gsoc19_webservices/index.php",
        "metadata": {
            "hits": "",
            "rights": "",
            "robots": "",
            "tags": {
                "tags": "",
                "typeAlias": null
            }
        },
        "metadesc": "",
        "metakey": "",
        "name": "Name",
        "ordering": 1,
        "params": {
            "feed_character_count": "",
            "feed_display_order": "",
            "newsfeed_layout": "",
            "show_feed_description": "",
            "show_feed_image": "",
            "show_item_description": ""
        },
        "published": 1
    }

#### Update Feed

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/newsfeeds/feeds/{feed_id} -d

    {
        "access": 1,
        "alias": "test2",
        "catid": 5,
        "description": "",
        "link": "http://samoylov/joomla/gsoc19_webservices/index.php",
        "metadesc": "",
        "metakey": "",
        "name": "Test"
    }

### Catégories

1.  Route Newsfeeds Categories is: "v1/newsfeeds/categories"
2.  Working with it is similar to [Banners Categories](#Categories).

## Confidentialité

### Demande

#### Obtenir la liste des demandes

curl -X GET /api/index.php/v1/privacy/request

#### Obtenir une seule demande

curl -X GET /api/index.php/v1/privacy/request/{request_id}

#### Get Single Request Export Data

curl -X GET /api/index.php/v1/privacy/request/export/{request_id}

#### Créer une demande

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/privacy/request -d

    {
        "email":"somenewemail@com.ua",
        "request_type":"export"
    }

### Consentement

#### Obtenir la liste des consentements

curl -X GET /api/index.php/v1/privacy/consent

#### Obtenir un seul consentement

curl -X GET /api/index.php/v1/privacy/consent/{consent_id}

#### Supprimer un consentement

curl -X DELETE /api/index.php/v1/privacy/consent/{consent_id}

## Redirection

### Redirection

#### Obtenir la liste des redirections

curl -X GET /api/index.php/v1/redirect

#### Obtenir une seule redirection

curl -X GET /api/index.php/v1/redirect/{redirect_id}

#### Supprimer une redirection

curl -X DELETE /api/index.php/v1/redirect/{redirect_id}

#### Créer une redirection

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/redirect -d

    {
        "comment": "",
        "header": 301,
        "hits": 0,
        "new_url": "/content/art/99",
        "old_url": "/content/art/12",
        "published": 1,
        "referer": ""
    }

#### Mettre à jour une redirection

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/redirect/{redirect_id} -d

    {
        "new_url": "/content/art/4",
        "old_url": "/content/art/132"
    }

## Tags

### Tags

#### Obtenir la liste des tags

curl -X GET /api/index.php/v1/tags

#### Obtenir un seul tag

curl -X GET /api/index.php/v1/tags/{tag_id}

#### Supprimer un tag

curl -X DELETE /api/index.php/v1/tags/{tag_id}

#### Créer un tag

curl -X POST -H "Content-Type: application/json" /api/index.php/v1/tags
-d

    {
        "access": 1,
        "access_title": "Public",
        "alias": "test",
        "description": "",
        "language": "*",
        "note": "",
        "parent_id": 1,
        "path": "test",
        "published": 1,
        "title": "test"
    }

#### Mettre à jour un tag

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/tags/{tag_id} -d

    {
        "alias": "test",
        "title": "new title"
    }

## Templates

### Styles de template

#### Obtenir la liste des styles de template

curl -X GET /api/index.php/v1/templates/styles/{app}

#### Obtenir un seul style de template

curl -X GET /api/index.php/v1/templates/styles/{app}/{template_style_id}

#### Supprimer un style de template

curl -X DELETE
/api/index.php/v1/templates/styles/{app}/{template_style_id}

#### Créer un style de template

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/templates/styles/{app} -d

    {
        "home": "0",
        "params": {
            "fluidContainer": "0",
            "logoFile": "",
            "sidebarLeftWidth": "3",
            "sidebarRightWidth": "3"
        },
        "template": "cassiopeia",
        "title": "cassiopeia - Some Text"
    }

#### Mettre à jour un style de template

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/templates/styles/{app}/{template_style_id} -d

    {
        "template": "cassiopeia",
        "title": "new cassiopeia - Default"
    }

## Utilisateurs

### Utilisateurs

#### Obtenir la liste des utilisateurs

curl -X GET /api/index.php/v1/users

#### Obtenir un seul utilisateur

curl -X GET /api/index.php/v1/users/{user_id}

#### Supprimer un utilisateur

curl -X DELETE /api/index.php/v1/users/{user_id}

#### Créer un utilisateur

curl -X POST -H "Content-Type: application/json" /api/index.php/v1/users
-d

    {
        "block": "0",
        "email": "test@mail.com",
        "groups": [
            "2"
        ],
        "id": "0",
        "lastResetTime": "",
        "lastvisitDate": "",
        "name": "nnn",
        "params": {
            "admin_language": "",
            "admin_style": "",
            "editor": "",
            "helpsite": "",
            "language": "",
            "timezone": ""
        },
        "password": "qwertyqwerty123",
        "password2": "qwertyqwerty123",
        "registerDate": "",
        "requireReset": "0",
        "resetCount": "0",
        "sendEmail": "0",
        "username": "ad"
    }

#### Mettre à jour un utilisateur

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/users/{user_id} -d

    {
        "email": "new@mail.com",
        "groups": [
            "2"
        ],
        "name": "name",
        "username": "username"
    }

### Fields Users

1.  Route Fields Users is: "v1/fields/users"
2.  Working with it is similar to [Fields
    Contact](https://docs.joomla.org/J4.x:Joomla_Core_APIs#Fields_Contact "Special:MyLanguage/J4.x:Joomla Core APIs").

### Groups Fields Users

1.  Route Groups Fields Users is: "v1/fields/groups/users"
2.  Working with it is similar to [Groups Fields
    Contact](https://docs.joomla.org/J4.x:Joomla_Core_APIs#Groups_Fields_Contact "Special:MyLanguage/J4.x:Joomla Core APIs").
