<!-- Filename: J4.x:Joomla_Core_APIs / Display title: APIs de base de Joomla -->

Cette page répertorie les points de terminaison disponibles dans Joomla à l'aide d'exemples de commandes curl. Elle a été préparée pour Joomla 4 et nécessite une vérification de conformité avec les versions actuelles de Joomla.

Chaque URL nécessite une authentification, sauf si elle est désignée comme une URL publique. Pour des raisons de sécurité dans Joomla 4.0.0, nous prévoyons de faire en sorte que l'application API par défaut nécessite un compte Super Utilisateur (car l'application API est toute nouvelle), cela pourrait être assoupli à mesure que l'API se stabilise et est bien testée dans la communauté. Si vous utilisez le plugin d'authentification de base (actuellement le seul plugin fourni depuis Joomla 4 alpha 10), il nécessite l'ajout aux commandes curl ci-dessous en utilisant --user nom_utilisateur:mot_de_passe.

Chaque URL doit être précédée de l'hôte Joomla avant le chemin (par exemple, au lieu de `/api/index.php/v1/article`, vous avez besoin de `http://example.com/api/index.php/v1/article`).

Les noms de propriétés entre accolades ({}) indiquent que la propriété est une variable qui doit être substituée.

Sauf indication contraire, ces API ont été introduites dans Joomla 4. Pour plus d'informations sur la spécification de l'API Joomla (par opposition à cette liste d'URL et d'options), veuillez visiter la [spécification de l'API Joomla](https://docs.joomla.org/Joomla_Api_Specification).

Où les points finaux nécessitent la valeur de {app}, la variable prend actuellement deux valeurs : site (front end), ou administrateur {back end}.

## Ressources Utiles

Un certain nombre de ressources complémentaires ont été créées pour présenter les services web Joomla et vous aider à apprendre comment implémenter les services web sur votre projet.

- Collection Postman des appels à l'[API des services Web Joomla](https://github.com/alexandreelise/j4x-api-collection) par Alexandre Elise.
- Tutoriel Youtube Joomla 4 : [Utilisation de l'API des services Web](https://www.youtube.com/watch?v=lT9qodsvfZg) avec Eoin Oliver.
- Magazine de la communauté Joomla : [API des services Web Joomla 101](https://magazine.joomla.org/all-issues/august-2020/joomla-web-services-api-101-tokens,-testing-and-a-taste-test) par Patrick Jackson.

## Composant Bannières

### Bannières

#### Obtenir la liste des bannières

curl -X GET /api/index.php/v1/bannières

#### Obtenir une seule bannière

curl -X GET /api/index.php/v1/banners/{banner_id}

#### Supprimer la bannière

curl -X DELETE /api/index.php/v1/bannières/{banner_id}

#### Créer une bannière

```markdown
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners -d
```

```json
{
    "catid": 3,
    "clicks": 0,
    "custombannercode": "",
    "description": "Texte",
    "metakey": "",
    "name": "Nom",
    "params": {
        "alt": "",
        "height": "",
        "imageurl": "",
        "width": ""
    }
}
```

#### Mettre à jour la bannière

```markdown
curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/banners/{banner_id} -d
```

```json
{
    "alias": "nom",
    "catid": 3,
    "description": "Nouveau texte",
    "name": "Nouveau nom"
}
```

### Clients

#### Obtenir la liste des clients

curl -X GET /api/index.php/v1/bannières/clients

#### Obtenir un seul client

curl -X GET /api/index.php/v1/banners/clients/{client_id}

#### Supprimer le client

curl -X DELETE /api/index.php/v1/banners/clients/{client_id}

#### Créer un client

```
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/banners/clients -d
```

```json
{
    "contact": "Nom",
    "email": "email@mail.com",
    "extrainfo": "",
    "metakey": "",
    "name": "Clients",
    "state": 1
}
```

#### Mettre à jour le client

```markdown
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/clients/{client_id} -d
```

```json
{
    "contact": "nouveau Nom",
    "email": "nouvelemail@mail.com",
    "name": "Clients"
}
```

### Catégories de Bannières

#### Obtenir la liste des catégories

curl -X GET /api/index.php/v1/bannières/catégories

#### Obtenir une seule catégorie

curl -X GET /api/index.php/v1/banners/categories/{category_id}

#### Supprimer la catégorie

curl -X DELETE /api/index.php/v1/banners/categories/{id_catégorie}

#### Créer une catégorie

```
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/banners/categories -d
```

```json
{
    "access": 1,
    "alias": "cat",
    "extension": "com_banners",
    "language": "*",
    "note": "",
    "parent_id": 1,
    "published": 1,
    "title": "Titre"
}
```

#### Mettre à jour la catégorie

```markdown
curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/banners/categories/{category_id} -d
```

```json
{
    "alias": "chat",
    "note": "Du texte",
    "parent_id": 1,
    "title": "Nouveau titre"
}
```

### Historique du contenu des bannières

#### Obtenir la liste des historiques de contenu

curl -X GET /api/index.php/v1/banners/contenthistory/{id_bannière}

#### Basculer l'historique du contenu conservé

```markdown
curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/banners/contenthistory/garder/{contenthistory_id}
```

#### Supprimer l'historique du contenu

curl -X DELETE
/api/index.php/v1/bannières/historiquecontenu/{historiquecontenu_id}

## Composant de Configuration

### Application

#### Obtenir la liste des configurations d'application

curl -X GET /api/index.php/v1/config/application

#### Mettre à jour la configuration de l'application

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/config/application -d

```json
{
    "debug": true,
    "sitename": "123"
}
```

### Composant

#### Obtenir la liste des configurations de composants

curl -X GET /api/index.php/v1/config/{nom_composant}

Exemple “component_name” est “com_content”.

#### Mettre à jour la configuration de l'application de composant

`curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/config/application -d`

{
    "link_titles": 1
}

## Composant de Contact

### Contactez

#### Obtenir la liste de contacts

curl -X GET /api/index.php/v1/contact

#### Obtenir un seul contact

curl -X GET /api/index.php/v1/contact/{contact_id}

#### Supprimer le contact

curl -X DELETE /api/index.php/v1/contact/{id_contact}

#### Créer un contact

```
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/contact -d
```

```json
{
    "alias": "contact",
    "catid": 4,
    "language": "*",
    "name": "Contact"
}
```

#### Mettre à jour le contact

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/contact/{contact_id} -d
```

```json
{
    "alias": "contact",
    "catid": 4,
    "name": "Nouveau contact"
}
```

#### Envoyer le formulaire de contact

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/contact/formulaire/{contact_id} -d
```

```markdown
{
    "contact_email": "email@mail.com",
    "contact_message": "quelque texte",
    "contact_name": "nom",
    "contact_subject": "sujet"
}
```

### Catégories de Contact

1. La route des catégories de contact est : "v1/contact/categories"
2. Travailler avec ça est similaire aux catégories de bannières.

### Contacter les champs

#### Obtenir la liste des champs de contact

curl -X GET /api/index.php/v1/fields/contact/contact

#### Obtenir un contact à champ unique

curl -X GET /api/index.php/v1/fields/contact/contact/{id_champ}

#### Supprimer le champ de contact

`curl -X DELETE /api/index.php/v1/fields/contact/contact/{field_id}`

#### Créer un contact terrain

```
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/champs/contact/contact -d
```

```json
{
    "access": 1,
    "context": "com_contact.contact",
    "default_value": "",
    "description": "",
    "group_id": 0,
    "label": "champ de contact",
    "language": "*",
    "name": "champ-contact",
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
    "title": "champ de contact",
    "type": "text"
}
```

#### Mettre à jour le champ Contact

```
curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/fields/contact/contact/{field_id} -d
```

```json
{
    "title": "nouveau champ de contact",
    "name": "champ-de-contact",
    "label": "champ de contact",
    "default_value": "",
    "type": "texte",
    "note": "",
    "description": "Du Nouveau Texte"
}
```

### Champs Adresse E-mail

1. La route pour les champs de contact par courriel est : "v1/fields/contact/mail"
2. Travailler avec cela est similaire aux champs de contact.

### Catégories de contact des champs

1. Les catégories de contact de Route Fields sont : "v1/fields/contact/categories"
2. Travailler avec cela est similaire à Fields Contact.

### Champs de groupes Contact

#### Obtenir la liste des champs de contact de groupe

```
curl -X GET /api/index.php/v1/fields/groups/contact/contact
```

#### Obtenir les champs de contact d'un groupe individuel

curl -X GET /api/index.php/v1/champs/groupes/contact/contact/{group_id}

#### Supprimer les champs de contact du groupe

curl -X DELETE  
/api/index.php/v1/champs/groupes/contact/contact/{group_id}

#### Créer des champs de groupe de contact

curl -X POST -H "Content-Type: application/json" /api/index.php/v1/fields/groups/contact/contact -d

```json
{
    "access": 1,
    "context": "com_contact.contact",
    "default_value": "",
    "description": "",
    "group_id": 0,
    "label": "champ de contact",
    "language": "*",
    "name": "champ-contact3",
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
    "title": "champ de contact",
    "type": "text"
}
```

#### Mettre à jour les champs du contact de groupe

```markdown
curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/fields/groups/contact/contact/{group_id} -d
```

{
    "title": "nouveau groupe de contact",
    "note": "",
    "description": "nouvelle description"
}

### Contact du Groupe de Champs Mail

1. Le champ de groupe de route Contact Mail est : "v1/fields/groups/contact/mail"
2. Travailler avec lui est similaire au champ de groupe Contact.

### Catégories de contacts de groupe

1. Les champs de groupe de route Catégories de contact sont : "v1/fields/groups/contact/categories"
2. Travailler avec est similaire à Group Fields Contact.

### Historique du contenu des contacts

1. L'historique du contenu de la route est : "v1/contact/contenthistory"
2. Travailler avec cela est similaire à l'historique du contenu des bannières.

## Composant de Contenu

### Articles

#### Obtenir la liste des articles

curl -X GET /api/index.php/v1/contenu/articles

#### Obtenir un seul article

curl -X GET /api/index.php/v1/content/articles/{id_article}

#### Supprimer l'article

curl -X DELETE /api/index.php/v1/contenu/articles/{article_id}

#### Créer un article

```markdown
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/content/articles -d
```

```json
{
    "alias": "mon-article",
    "articletext": "Mon texte",
    "catid": 64,
    "language": "*",
    "metadesc": "",
    "metakey": "",
    "title": "Voici un article"
}
```

Actuellement, les options mentionnées ici sont des propriétés requises. Cependant, l'intention est actuellement de rendre AU MOINS metakey et metadesc optionnels dans l'API.

#### Mettre à jour l'article

```markdown
curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/content/articles/{article_id} -d
```

```json
{
    "catid": 64,
    "title": "Article mis à jour"
}
```

### Catégories de contenu

1. Les catégories de contenu de la route sont : "v1/content/categories"
2. Travailler avec cela est similaire à travailler avec les catégories de bannières. Remarque : si les flux de travail sont activés, alors spécifier un flux de travail est requis (de même que dans l'interface utilisateur)

### Articles de champs

1. Les articles des champs de route sont : "v1/fields/content/articles"
2. Travailler avec, c'est similaire à Champs Contact.

### Articles des champs de groupes

1. Route Groupes Champs Articles est : "v1/fields/groups/content/articles"
2. Travailler avec cela est similaire à Groupes Champs Contact.

### Catégories de Champs

1. Les catégories de champs de route sont : "v1/fields/groups/content/categories"
2. Travailler avec ceci est similaire à Champs Contact.

### Historique du Contenu du Composant de Contenu

1. L'historique du contenu de la route est :  
   "v1/content/articles/{article_id}/contenthistory"
2. Travailler avec est similaire à l'historique du contenu des bannières.

## Composant de Langues

### Langues

#### Obtenir la liste des langues

curl -X GET /api/index.php/v1/langues

#### Installer la langue

curl -X POST -H "Content-Type: application/json" /api/index.php/v1/langues -d

```json
{
    "package": "pkg_fr-FR"
}
```

### Langues de contenu

#### Obtenir la liste des langues de contenu

curl -X GET /api/index.php/v1/langues/contenu

#### Obtenir une langue de contenu unique

curl -X GET /api/index.php/v1/v1/langues/contenu/{language_id}

#### Supprimer la langue du contenu

curl -X DELETE /api/index.php/v1/languages/content/{id_langue}

#### Créer la langue du contenu

```
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/languages/content -d
```

```json
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
    "title": "Français (FR)",
    "title_native": "Français (France)"
}
```

#### Mettre à jour la langue du contenu

curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/languages/content/{language_id} -d

```json
{
    "description": "",
    "lang_code": "fr-FR",
    "metadesc": "",
    "metakey": "",
    "sitename": "",
    "title": "Français (fr-FR)",
    "title_native": "Français (France)"
}
```

### Remplacements de langues

#### Obtenir la liste des constantes de langues personnalisées

curl -X GET /api/index.php/v1/langues/remplacements/{app}/{lang_code}

#### Obtenir une constante de langue de substitution unique

curl -X GET
/api/index.php/v1/langues/surcharges/{app}/{code_langue}/{id_constante}

#### Supprimer les remplacements de langue du contenu

```
curl -X DELETE
/api/index.php/v1/languages/overrides/{app}/{lang_code}/{constant_id}
```

#### Créer des substituts de langue pour le contenu

curl -X POST -H "Content-Type: application/json" /api/index.php/v1/languages/overrides/{app}/{lang_code} -d

```markdown
{
    "clé":"nouvelle_clé",
    "remplacer": "texte"
}
```

#### Mettre à jour les remplacements de langue de contenu

```
curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/languages/overrides/{app}/{lang_code}/{constant_id} -d 
```

```json
{
    "key": "nouvelle_clé",
    "override": "nouveau texte"
}
```

var app - enum {"site", "administrateur"}

var lang_code - chaîne Exemple : “fr-FR“, “en-GB“ vous pouvez obtenir lang_code
à partir de v1/languages/content

#### Constante de remplacement de recherche

```markdown
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/search -d
```

```markdown
{
    "searchstring": "JLIB_APPLICATION_ERROR_SAVE_FAILED",
    "searchtype": "constant"
}
```

var searchtype - enum {« constant », « value »}. « constant » recherche par nom constant, « value » - recherche par valeur constante

#### Actualiser le cache de recherche de remplacement

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/search/cache/refresh
```

## Composant Menus

### Menus

#### Obtenir la liste des menus

curl -X GET /api/index.php/v1/menus/{app}

#### Obtenir un menu unique

curl -X GET /api/index.php/v1/menus/{app}/{menu_id}

#### Supprimer le menu

curl -X DELETE /api/index.php/v1/menus/{app}/{menu_id}

#### Créer un menu

```
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/menus/{app} -d
```

{
    "client_id": 0,
    "description": "Le menu pour le site",
    "menutype": "menu",
    "title": "Menu"
}

#### Mettre à jour le menu

```markdown
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/{menu_id} -d
```

```json
{
    "menutype": "menu",
    "title": "Nouveau Menu"
}
```

### Éléments de menu

#### Obtenir la liste des types d'éléments de menus

curl -X GET /api/index.php/v1/menus/{app}/articles/types

#### Obtenir la liste des éléments de menu

curl -X GET /api/index.php/v1/menus/{app}/items

#### Obtenir un seul élément de menu

curl -X GET /api/index.php/v1/menus/{app}/items/{menu_item_id}

#### Supprimer l'élément du menu

curl -X DELETE /api/index.php/v1/menus/{app}/items/{menu_item_id}

#### Créer un élément de menu

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/items -d

```markdown
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
    "title": "titre",
    "toggle_modules_assigned": "1",
    "toggle_modules_published": "1",
    "type": "component"
}
```

Exemple pour "Créer une page d'article"

#### Mettre à jour l'élément de menu

```
curl -X PATCH -H "Content-Type: application/json" 
/api/index.php/v1/menus/{app}/items/{menu_item_id} -d
```

{
    "component_id": "20",
    "language": "*",
    "link": "index.php?option=com_content&view=form&layout=edit",
    "menutype": "mainmenu",
    "note": "",
    "title": "nouveau titre",
    "type": "composant"
}

Exemple pour "Créer une page d'article"

## Composant Messages

### Messages

#### Obtenir la liste des messages

curl -X GET /api/index.php/v1/messages

#### Obtenir un seul message

curl -X GET /api/index.php/v1/messages/{message_id}

#### Supprimer le message

`curl -X DELETE /api/index.php/v1/messages/{message_id}`

#### Créer un message

curl -X POST -H "Content-Type: application/json" /api/index.php/v1/messages -d

```json
{
    "message": "texte",
    "state": 0,
    "subject": "texte",
    "user_id_from": 773,
    "user_id_to": 772
}
```

#### Mettre à jour le message

```
curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/messages/{message_id} -d
```

{
    "message": "nouveau texte",
    "subject": "nouveau texte",
    "user_id_from": 773,
    "user_id_to": 772
}

## Composant Modules

### Modules

#### Obtenir la liste des types de modules

curl -X GET /api/index.php/v1/modules/types/{app}

#### Obtenir la liste des modules

curl -X GET /api/index.php/v1/modules/{app}

#### Obtenir un seul module

curl -X GET /api/index.php/v1/modules/{app}/{id_module}

#### Supprimer le module

curl -X DELETE /api/index.php/v1/modules/{app}/{module_id}

#### Créer un module

```
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/modules/{app} -d
```

```json
{
    "accès": "1",
    "assigné": [
        "101",
        "105"
    ],
    "attribution": "0",
    "client_id": "0",
    "langue": "*",
    "module": "mod_articles_archive",
    "note": "",
    "ordre": "1",
    "params": {
        "taille_bootstrap": "0",
        "cache": "1",
        "temps_cache": "900",
        "mode_cache": "statique",
        "compte": "10",
        "classe_entête": "",
        "balise_entête": "h3",
        "mise_en_page": "_:default",
        "balise_module": "div",
        "suffixe_classe_module": "",
        "style": "0"
    },
    "position": "",
    "publier_descendre": "",
    "publier_monter": "",
    "publié": "1",
    "afficher_titre": "1",
    "titre": "Titre"
}
```

Exemple pour "Articles - Archivés"

#### Mettre à jour le module

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/modules/{app}/{module_id} -d

```json
{
    "access": "1",
    "client_id": "0",
    "language": "*",
    "module": "mod_articles_archive",
    "note": "",
    "ordering": "1",
    "title": "Nouveau Titre"
}
```

Exemple pour "Articles - Archivés"

## Composant des flux d'actualités

### Flux

#### Obtenir la liste des flux

curl -X GET /api/index.php/v1/newsfeeds/feeds

#### Obtenir un flux unique

curl -X GET /api/index.php/v1/newsfeeds/feeds/{feed_id}

#### Supprimer le flux

curl -X DELETE /api/index.php/v1/newsfeeds/feeds/{feed_id}

#### Créer un flux

```
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/newsfeeds/feeds -d
```

```json
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
    "name": "Nom",
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
```

#### Mettre à jour le fil d'actualités

```
curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/newsfeeds/feeds/{feed_id} -d
```

```json
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
```

### Catégories de flux d'actualités

1. La route des catégories de flux d'actualités est : "v1/newsfeeds/categories"
2. Travailler avec cela est similaire aux catégories de bannières.

## Composant de Confidentialité

### Demande

#### Obtenir la liste des demandes

curl -X GET /api/index.php/v1/privacy/request

#### Obtenir une demande unique

curl -X GET /api/index.php/v1/confidentialite/demande/{request_id}

#### Obtenir les données d'exportation d'une seule demande

curl -X GET /api/index.php/v1/privacy/request/exportation/{request_id}

#### Créer une demande

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/privacy/request -d

{
    "email":"somenewemail@com.ua",
    "request_type":"exportation"
}

### Consentement

#### Obtenir la liste des consentements

curl -X GET /api/index.php/v1/confidentialité/consentement

#### Obtenir un consentement unique

curl -X GET /api/index.php/v1/privacy/consentement/{consent_id}

#### Supprimer le consentement

curl -X DELETE /api/index.php/v1/privacy/consentement/{consent_id}

## Composant de Redirection

### Redirection

#### Obtenir la liste des redirections

```
curl -X GET /api/index.php/v1/redirect
```

#### Obtenir une redirection unique

curl -X GET /api/index.php/v1/redirect/{redirect_id}

#### Supprimer la redirection

curl -X DELETE /api/index.php/v1/redirect/{identifiant_redirection}

#### Créer une redirection

curl -X POST -H "Content-Type: application/json" /api/index.php/v1/rediriger -d

{
        "commentaire": "",
        "en-tête": 301,
        "coups": 0,
        "nouvelle_url": "/contenu/art/99",
        "ancienne_url": "/contenu/art/12",
        "publié": 1,
        "référent": ""
    }

#### Mettre à jour la redirection

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/redirect/{redirect_id} -d

{
    "new_url": "/contenu/art/4",
    "old_url": "/contenu/art/132"
}

## Composant de balayage

### Étiquettes

#### Obtenir la liste des tags

curl -X GET /api/index.php/v1/tags

#### Obtenir un seul tag

curl -X GET /api/index.php/v1/tags/{tag_id}

#### Supprimer le tag

curl -X DELETE /api/index.php/v1/tags/{tag_id}

#### Créer une étiquette

```markdown
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/tags
-d
```

```markdown
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
```

#### Mettre à jour l'étiquette

```markdown
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/tags/{tag_id} -d
```

{
        "alias": "test",
        "title": "nouveau titre"
}

## Modèles

### Styles de Modèles

#### Obtenir la liste des styles de modèles

curl -X GET /api/index.php/v1/templates/styles/{app}

#### Obtenir le style de modèle unique

curl -X GET /api/index.php/v1/gabarits/styles/{app}/{template_style_id}

#### Supprimer le style du modèle

curl -X DELETE
/api/index.php/v1/templates/styles/{app}/{identifiant_style_template}

#### Créer un style de modèle

```
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/templates/styles/{app} -d
```

```json
{
    "home": "0",
    "params": {
        "fluidContainer": "0",
        "logoFile": "",
        "sidebarLeftWidth": "3",
        "sidebarRightWidth": "3"
    },
    "template": "cassiopeia",
    "title": "cassiopeia - Quelques Texte"
}
```

#### Mettre à Jour le Style du Modèle

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/templates/styles/{app}/{template_style_id} -d

```json
{
    "template": "cassiopeia",
    "title": "nouvelle cassiopeia - Par défaut"
}
```

## Composant Utilisateurs

### Utilisateurs

#### Obtenir la liste des utilisateurs

curl -X GET /api/index.php/v1/utilisateurs

#### Obtenir un seul utilisateur

curl -X GET /api/index.php/v1/utilisateurs/{utilisateur_id}

#### Supprimer l'utilisateur

curl -X DELETE /api/index.php/v1/utilisateurs/{user_id}

#### Créer un utilisateur

curl -X POST -H "Content-Type: application/json" /api/index.php/v1/utilisateurs -d

```json
{
    "bloquer": "0",
    "email": "test@mail.com",
    "groupes": [
        "2"
    ],
    "id": "0",
    "dateDerniereReinitialisation": "",
    "dateDerniereVisite": "",
    "nom": "nnn",
    "parametres": {
        "langue_admin": "",
        "style_admin": "",
        "editeur": "",
        "siteAide": "",
        "langue": "",
        "fuseau_horaire": ""
    },
    "motDePasse": "qwertyqwerty123",
    "motDePasse2": "qwertyqwerty123",
    "dateEnregistrement": "",
    "reinitialisationRequise": "0",
    "compteReinitialisation": "0",
    "envoyerEmail": "0",
    "nom_utilisateur": "ad"
}
```

#### Mettre à jour l'utilisateur

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/utilisateurs/{user_id} -d
```

{
        "email": "nouveau@courriel.com",
        "groups": [
            "2"
        ],
        "name": "nom",
        "username": "nomd'utilisateur"
    }

### Champs Utilisateurs

1.  La route Fields Users est : "v1/fields/users"
2.  Travailler avec cela est similaire à travailler avec Fields Contact.

### Utilisateurs des champs des groupes

1. Le champ Route Groups Users est : "v1/fields/groups/users"
2. Travailler avec cela est similaire à Groups Fields Contact.

*Traduit par openai.com*

