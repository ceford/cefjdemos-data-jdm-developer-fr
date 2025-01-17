<!-- Filename: API_Guides / Display title: Guides API -->

Cette page contient un index du jeu de guides API Joomla. Ces guides visent à vous aider à comprendre comment utiliser ces fonctions Joomla dans vos propres extensions Joomla.

Chaque guide de l'API inclut un exemple de code que vous pouvez copier et installer dans votre propre environnement de développement. En général, ces exemples de code sont conçus pour être inclus et installés comme un module Joomla, donc si vous n'êtes pas familier avec le développement de modules Joomla, vous pourriez trouver utile de parcourir la courte série [Créer un Module Simple](https://docs.joomla.org/Creating_a_simple_module "Créer un module simple").

Autour de Joomla 3.8, l'équipe de développement de Joomla a commencé à modifier la convention de nommage des classes Joomla pour utiliser les espaces de noms, de sorte que par exemple JFactory est devenu Factory dans l'espace de noms Joomla\CMS. En lisant le code et la documentation existants de Joomla, vous pouvez trouver des classes suivant soit le nouveau soit l'ancien standard de nommage. Vous pouvez trouver la correspondance entre les deux conventions de nommage dans le fichier `libraries/classmap.php` de votre instance Joomla.

- Les classes de base de l'Application, leur hiérarchie et leurs objectifs sont décrits dans [Comprendre les classes de l'Application](https://docs.joomla.org/J3.x:Understanding_the_Application_classes "J3.x:Understanding the Application classes")
- La gestion d'Ajax au sein des composants Joomla est décrite dans [Réponses JSON avec JResponseJson](https://docs.joomla.org/JSON_Responses_with_JResponseJson "JSON Responses with JResponseJson"). Ajax peut également être utilisé dans les Modules et Plugins Joomla comme décrit dans [Utiliser l'interface Ajax de Joomla](https://docs.joomla.org/Using_Joomla_Ajax_Interface "Using Joomla Ajax Interface").
- [Cache](https://docs.joomla.org/Cache_Basic_API_Guide "Cache Basic API Guide") - comment utiliser le cache de rappel dans votre code.
- [Catégories](https://docs.joomla.org/Categories_and_CategoryNodes_API_Guide "Categories and CategoryNodes API Guide") Utilisation des classes `Categories` et `CategoryNode` pour accéder aux données relatives aux catégories Joomla.
- Le CSS peut être ajouté comme décrit dans [Ajouter du JavaScript et du CSS à la page](https://docs.joomla.org/Adding_JavaScript_and_CSS_to_the_page)
- Base de données / JDatabase. Il y a deux guides API, couvrant [Sélectionner des données en utilisant JDatabase](https://docs.joomla.org/Selecting_data_using_JDatabase "Selecting data using JDatabase")
  et [Insérer, mettre à jour et supprimer des données en utilisant JDatabase](https://docs.joomla.org/Inserting,_Updating_and_Removing_data_using_JDatabase "Inserting, Updating and Removing data using JDatabase")
- [Date/JDate](https://docs.joomla.org/How_to_use_JDate "How to use JDate") est la classe de Date de Joomla.
- Fichiers et Dossiers. Voir [Comment utiliser le package de fichiers](https://docs.joomla.org/How_to_use_the_filesystem_package "How to use the filesystem package").
- Form / JForm. Il y a un [Guide de base](https://docs.joomla.org/Basic_form_guide "Basic form guide") pour utiliser l'API Formulaire de Joomla et intégrer des formulaires dans un composant Joomla, ainsi qu'un [Guide avancé sur les formulaires](https://docs.joomla.org/Advanced_form_guide "Advanced form guide") couvrant des aspects plus avancés des API.
- FormField / JFormField. Cette classe et les classes connexes comme JFormFieldList qui en héritent sont principalement utiles pour définir des champs de formulaire personnalisés, comme décrit dans [Créer un type de champ de formulaire personnalisé](https://docs.joomla.org/Creating_a_custom_form_field_type "Creating a custom form field type").
- [Input / JInput](https://docs.joomla.org/Retrieving_request_data_using_JInput "Retrieving request data using JInput") Utiliser la classe `Input` pour obtenir les valeurs des paramètres dans les requêtes HTTP GET et POST.
- Le JavaScript peut être ajouté comme décrit dans [Ajouter du JavaScript et du CSS à la page](https://docs.joomla.org/Adding_JavaScript_and_CSS_to_the_page)
- L'utilisation des gabarits Joomla est décrite dans [Partager les gabarits entre les vues ou extensions avec JLayout](https://docs.joomla.org/J3.x:Sharing_layouts_across_views_or_extensions_with_JLayout "J3.x:Sharing layouts across views or extensions with JLayout"). La flexibilité a été augmentée dans Joomla 3.2, comme décrit dans [Améliorations de JLayout pour Joomla!](https://docs.joomla.org/J3.x:JLayout_Improvements_for_Joomla! "J3.x:JLayout Improvements for Joomla!").
- [Log / JLog](https://docs.joomla.org/Using_JLog "Using JLog") Enregistrez les messages (par exemple messages d'erreur, messages de débogage) dans un fichier de log, et éventuellement dans la console de débogage.
- [Menu et Éléments de menu](https://docs.joomla.org/Menu_and_Menuitems_API_Guide "Menu and Menuitems API Guide")
- [Ensembles imbriqués](https://docs.joomla.org/Using_nested_sets "Using nested sets"), qui permettent d'implémenter une hiérarchie d'arbre dans la table de base de données, sont utilisés par les menus Joomla, les articles, les catégories, etc.
- [Registry/JRegistry](https://github.com/joomla-framework/registry) est une classe utilitaire très utile pour gérer les tableaux PHP, convertir en JSON, etc.
- [JResponseJson](https://docs.joomla.org/JSON_Responses_with_JResponseJson "JSON Responses with JResponseJson") prend en charge les réponses au format JSON aux requêtes Ajax.
- Route / JRoute voir [URLs dans Joomla](https://docs.joomla.org/URLs_in_Joomla "URLs in Joomla")
- La Table / JTable fournit des fonctionnalités pour effectuer des opérations CRUD (et plus) sur les tables de base de données. Le guide est divisé en un [Guide API de base](https://docs.joomla.org/Table_Basic_API_Guide "Table Basic API Guide")
  et un [Guide API avancé](https://docs.joomla.org/Table_Advanced_API_Guide "Table Advanced API Guide").
- Les [Contrôleurs](https://docs.joomla.org/Controllers "Controllers") (BaseController, AdminController, FormController, ApiController) sont responsables de l'analyse de la demande de l'utilisateur, de la vérification que l'utilisateur est autorisé à effectuer cette action et de la détermination de la manière de satisfaire la demande.
- Les [Modèles](https://docs.joomla.org/Models "Models") (BaseModel, BaseDatabaseModel, ItemModel, ListModel, FormModel, AdminModel) encapsulent les données utilisées par le composant. Ils sont également responsables de la mise à jour de la base de données si nécessaire.
- Les [Vues](https://docs.joomla.org/Views "Views") (AbstractView, CategoriesView, CategoryFeedView, CategoryView, FormView, HtmlView, JsonApiView, JsonView, ListView) spécifient ce qui doit apparaître sur la page Web, et assemblent toutes les données nécessaires pour générer la réponse HTTP.
- [Tags](https://docs.joomla.org/Tags_API_Guide "Tags API Guide").
- Uri / JUri voir [URLs dans Joomla](https://docs.joomla.org/URLs_in_Joomla "URLs in Joomla")
- [Utilisateur / JUser](https://docs.joomla.org/Accessing_the_current_user_object "Accessing the current user object") API liée à l'utilisateur Joomla.

*Traduit par openai.com*

