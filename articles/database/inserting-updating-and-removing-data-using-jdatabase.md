<!-- Filename: J4.x:Inserting_Updating_and_Removing_data_using_JDatabase / Display title: Insertion, mise à Jour et suppression de données à l'aide de JDatabase -->

Joomla!  4.x Joomla!  3.x Joomla!  2.5 Note de version

Notez que de nombreux exemples fournis en ligne n'utilisent pas les
méthodes de déclarations préparées qui ont été introduites avec la
version Joomla 4.x ; n'utilisez pas, s'il vous plaît, ces anciennes APIs
pour les nouveaux projets, car ils vont introduire des problèmes de
sécurité conséquents si les saisies des utilisateurs ne sont pas
correctement échapées.

Ce didacticiel est divisé en deux parties indépendantes :

- L'insertion, la mise à jour et la suppression de données de la base de
  données.
- La sélection des données à partir d'une ou plusieurs tables et la
  récupération sous différentes formes.

Cette partie de la documentation va s'intéressée à l'insertion, la mise
à jour et la suppression de données à partir d'une table de base de
données. Pour comprendre la seconde partie, veuillez consulter :
[Sélectionner des données via
JDatabase](https://docs.joomla.org/Selecting_data_using_JDatabase "Special:MyLanguage/Selecting data using JDatabase")

## Introduction

Joomla! propose un système sophistiqué de couche d'abstraction de base
de données pour en simplifier l'utilisation par les développeurs
d'extensions tierces. Les nouvelles versions de la Plateforme d'API
Joomla! proposent des fonctionnalités supplémentaires qui permettent
d'étendre encore plus la couche de base de données et inclues des
fonctionnalités telles que des connecteurs pour une plus grande variété
de serveurs de base de données ou encore le chaînage de requêtes pour
améliorer la lisibilité des codes de connexion et pour simplifier le
codage SQL.

Joomla permet l'utilisation de différents types de systèmes de base de
données d'exécution SQL et peut être utilisé sur une grande variété
d'environnements différents avec différents préfixes de table. En plus
de ces fonctionnalités, la classe crée automatiquement la connexion à la
base de données. Il suffit de deux lignes de code pour l'instanciation
de l'objet dont vous avez et obtenir un résultat à partir de la base de
données dans différents formats. Grace à la couche de base de données,
Joomla assure un maximum de compatibilité et de flexibilité pour vos
extensions.

## La requête

L'interrogation de bases de données pour Joomla! a changé depuis
l'introduction dans le Framework Joomla! du "chaînage de requêtes" qui
est maintenant la méthode recommandée pour la construction de requêtes
de base de données (bien que les requêtes de chaînes soient toujours
prises en charge).

Le chaînage de requêtes fait référence à une méthode permettant la
connexion à un certain nombre de méthodes, les unes après les autres où
chaque méthode retournant un objet peut prendre en charge la nouvelle
méthode, améliorant ainsi la lisibilité et la simplification du code.

Pour obtenir une nouvelle instance de la classe `JDatabaseQuery`, nous
utilisons la méthode `JDatabaseDriver getQuery` :

    $db = JFactory::getDbo();

    $query = $db->getQuery(true);

`JDatabaseDriver::getQuery` peut prendre un argument optionnel, `$new`,
ce qui peut être *true* (vrai) ou *false* (faux) (la valeur par défaut
est *false*).

Pour interroger notre source de données, nous pouvons faire appel à un
certain nombre de méthodes `JDatabaseQuery`. Ces méthodes vont
encapsuler la requête des données sources (SQL) en cachant la syntaxe
spécifique de la requête du développeur et augmenter ainsi la
portabilité du code source du développeur.

Les méthodes les plus fréquemment utilisées comprennent :
`select, from, join, where et order`. Il existe également d'autres
méthodes telles que `insert, update et delete` pour modifier des
enregistrements dans la base de données. En chaînant ces méthodes
d'appel à d'autres, vous pouvez créer presque n'importe quelle requête à
votre base de données sans compromettre la portabilité de votre code.

## Insertion d'un enregistrement

### Utilisation de SQL

La classe `JDatabaseQuery` fournit un certain nombre de méthodes pour la
construction de requêtes de type `insert`, les plus courantes étant : <a
href="http://api.joomla.org/11.4/Joomla-Platform/Database/JDatabaseQuery.html#insert"
class="external text" target="_blank"
rel="noreferrer noopener">insert</a>, <a
href="http://api.joomla.org/11.4/Joomla-Platform/Database/JDatabaseQuery.html#columns"
class="external text" target="_blank"
rel="noreferrer noopener">columns</a> et <a
href="http://api.joomla.org/11.4/Joomla-Platform/Database/JDatabaseQuery.html#values"
class="external text" target="_blank"
rel="noreferrer noopener">values</a>.

    // Get a db connection.
    $db = JFactory::getDbo();

    // Create a new query object.
    $query = $db->getQuery(true);

    // Insert columns.
    $columns = array('user_id', 'profile_key', 'profile_value', 'ordering');

    // Prepare the insert query.
    $query
        ->insert($db->quoteName('#__user_profiles'))
        ->columns($db->quoteName($columns))
        ->values(':user_id, :profile_key, :profile_value, :ordering');

    // Bind values
    $query
        ->bind(':user_id', 1001, Joomla\Database\ParameterType::INTEGER)
        ->bind(':profile_key', 'custom.message')
        ->bind(':profile_value', 'Inserting a record using insert()')
        ->bind(':ordering', 1, Joomla\Database\ParameterType::INTEGER);

    // Set the query using our newly populated query object and execute it.
    $db->setQuery($query);

    $db->execute();

### Utilisation d'un objet

La classe `JDatabaseDriver` fournit également une méthode pratique pour
la sauvegarde d'un objet directement dans la base de données, nous
autorisant à ajouter un enregistrement à une table sans avoir à écrire
une seule ligne de SQL.

    // Create and populate an object.
    $profile = new stdClass();
    $profile->user_id = 1001;
    $profile->profile_key='custom.message';
    $profile->profile_value='Inserting a record using insertObject()';
    $profile->ordering=1;

    // Insert the object into the user profile table.
    $result = JFactory::getDbo()->insertObject('#__user_profiles', $profile);

Remarquez ici que nous n'avons pas besoin d'échapper le nom de la table,
la méthode `insertObject` fait cela pour nous.

La méthode `insertObject` va générer une erreur si un problème se
présente lors de l'insertion de l'enregistrement dans la table de base
de données.

Si vous donnez vous-même une valeur à la clé primaire - valeur unique
(comme dans l'exemple ci-dessus), il est recommandé que vous vérifiez
que cette valeur n'est pas déjà utilisée en exécutant un select dans
cette table avec cette valeur de clé primaire avant de de tenter un
ajout.

Si vous ajoutez simplement une ligne de plus dans votre table (i.e. que
la base génère elle-même la clé primaire), vous pouvez indiquer le nom
correspondant à la colonne de la clé primaire dans l'objet en tant que
troisième paramètre de la méthode insertObject() ; la méthode va alors
mettre à jour l'objet qui vient d'être créé avec la valeur de la clé
primaire.

Par exemple, en utilisant la ligne suivante :

    $result = $dbconnect->insertObject('#__my_table', $object, 'primary_key');

après exécution, \$object-\>primary_key va être mis à jour avec la
valeur de la clé primaire de la ligne qui vient d'être insérée.

**ASTUCE :** Mettre \$object-\>primary_key à null ou 0 (zéro) avant de
réaliser l'insertion pour pouvoir détecter si la valeur a été mise à
jour.

## Mise à jour d'un enregistrement

### Utilisation de SQL

La classe `JDatabaseQuery` fournit également des méthodes pour
construire des requêtes de mise à jour, en particulier <a
href="http://api.joomla.org/11.4/Joomla-Platform/Database/JDatabaseQuery.html#update"
class="external text" target="_blank"
rel="noreferrer noopener">update</a> et <a
href="http://api.joomla.org/11.4/Joomla-Platform/Database/JDatabaseQuery.html#set"
class="external text" target="_blank" rel="noreferrer noopener">set</a>.
Nous réutilisons également une autre méthode que nous avons utilisée
lors de la création d'instructions `select` à savoir : la méthode
`where`.

    $db = JFactory::getDbo();

    $query = $db->getQuery(true);

    // Fields to update.
    $fields = array(
        $db->quoteName('profile_value') . ' = :profile_value',
        $db->quoteName('ordering') . ' = :ordering'
    );

    // Conditions for which records should be updated.
    $conditions = array(
        $db->quoteName('user_id') . ' = :user_id', 
        $db->quoteName('profile_key') . ' = :profile_key'
    );

    $query->update($db->quoteName('#__user_profiles'))->set($fields)->where($conditions);

    $query
        ->bind(':profile_value', 'Updating custom message for user 1001.')
        ->bind(':ordering', 2, Joomla\Database\ParameterType::INTEGER)
        ->bind(':user_id', 42, Joomla\Database\ParameterType::INTEGER)   
        ->bind(':profile_key', 'custom.message');

    $db->setQuery($query);

    $result = $db->execute();

### Utilisation d'un objet

Comme pour `insertObject`, la classe `JDatabaseDriver` fournit une
méthode pratique pour la mise à jour d'un objet.

Ci-dessous, nous allons mettre à jour notre table personnalisée avec de
nouvelles valeurs à l'aide d'une ID de clé primaire existante :

    // Create an object for the record we are going to update.
    $object = new stdClass();

    // Must be a valid primary key value.
    $object->id = 1;
    $object->title = 'My Custom Record';
    $object->description = 'A custom record being updated in the database.';

    // Update their details in the users table using id as the primary key.
    $result = JFactory::getDbo()->updateObject('#__custom_table', $object, 'id');

Tout comme `insertObject`, `updateObject` va s'occuper d'échapper les
noms de table à notre place.

La méthode `updateObject` va générer une erreur si un problème se
présente lors de la mise à jour de l'enregistrement dans la table de
base de données.

Nous devons nous assurer que l'enregistrement existe déjà avant de
tenter de le mettre à jour, de sorte que nous souhaiterions probablement
ajouter certaines vérifications de données avant de procéder à
l'exécution de la méthode `updateObject`.

## Suppression d'un enregistrement

Enfin, il existe également la méthode de suppression `delete` pour
supprimer des enregistrements de la base de données.

    $db = JFactory::getDbo();

    $query = $db->getQuery(true);

    // delete all custom keys for user 1001.
    $conditions = array(
        $db->quoteName('user_id') . ' = :user_id', 
        $db->quoteName('profile_key') . ' = :profile_key'
    );

    $query->delete($db->quoteName('#__user_profiles'));
    $query->where($conditions);

    $query
        ->bind(':user_id', 1001, Joomla\Database\ParameterType::INTEGER)
        ->bind(':profile_key', 'custom.%');

    $db->setQuery($query);

    $result = $db->execute();

### See also

- [Selecting data using
  JDatabase](https://docs.joomla.org/J4.x:Selecting_data_using_JDatabase "Special:MyLanguage/J4.x:Selecting data using JDatabase")
- [Moving Joomla To Prepared
  Statements](https://docs.joomla.org/J4.x:Moving_Joomla_To_Prepared_Statements "Special:MyLanguage/J4.x:Moving Joomla To Prepared Statements")
- [Using the union methods in database
  queries](https://docs.joomla.org/Using_the_union_methods_in_database_queries "Special:MyLanguage/Using the union methods in database queries")
  (Joomla! 3.3+)
- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html"
  class="external text" target="_blank" rel="noreferrer noopener">Joomla
  Framework API</a>
