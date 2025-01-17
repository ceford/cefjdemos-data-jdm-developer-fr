<!-- Filename: J4.x:Developer:_Required_Software / Display title: Configuration de la base de données -->

## À propos de MySQL et MariaDB

Les nouveaux venus peuvent avoir l'impression que MySQL et MariaDB sont des bases de données et peuvent être désorientés lorsque les instructions indiquent *créer une nouvelle base de données*. En fait, ce sont tous deux des Systèmes de Gestion de Base de Données Relationnelle (SGBDR) capables de gérer de nombreuses bases de données individuelles. Votre site de test local peut avoir de nombreuses installations individuelles de Joomla, chacune avec sa propre base de données. Par exemple, vous pouvez souhaiter tester votre extension dans la version actuelle de Joomla et séparément dans une version candidate à venir.

La capture d'écran suivante montre une partie d'une liste de plus de 30 bases de données créées pour tester divers projets d'installations et d'extensions Joomla.

![Phypadmin screenshot of list of databases](../../../en/images/getting-started/phpmyadmin-databases.png)

Une note annexe : les classements sont principalement utf8mb4_0900_ai_ci :

- utf8mb4 signifie que chaque caractère est stocké avec un maximum de 4 octets dans le schéma d'encodage UTF-8.
- 0900 se réfère à la version de l'algorithme de collation Unicode.
- ai signifie insensibilité aux accents, il n'y a pas de différence entre e, è, é, ê et ë lors du tri.
- ci signifie insensibilité à la casse, il n'y a pas de différence entre p et P lors du tri.

Les tables et colonnes individuelles peuvent avoir un classement différent. Les tables Joomla ont généralement un classement utf8mb4_unicode_ci.

## Configuration de la base de données avec phpMyAdmin

Avant d'installer Joomla, vous devez disposer d'une base de données vide prête à être remplie. Vous avez également besoin d'un utilisateur de base de données.

### Créer une base de données

- **Démarrer phpMyAdmin** Entrez localhost/phpmyadmin dans la barre d'adresse de votre navigateur.
- **Se connecter** Selon la manière dont il a été installé, le nom d'utilisateur sera root ou admin et il peut ou non y avoir un mot de passe.
- Sélectionnez **Bases de données** dans le menu du haut de la page d'accueil de phpMyAdmin.
- Dans **Créer une base de données** saisissez un nom court à la place de l'indication **Nom de la base de données**, par exemple, jtest.
- Sélectionnez **utf8mb4_0900_ai_ci** dans la liste déroulante Collation.
- Sélectionnez **Créer** pour créer la base de données.

Cela vous mènera à une base de données vide. En haut, il y a un message : *Aucune table trouvée dans la base de données.*

### Créer un utilisateur

- Sélectionnez l'icône **Accueil** en haut à gauche de phpMyAdmin pour aller à la page d'accueil.
- Sélectionnez **Comptes utilisateurs** dans le menu supérieur de la page d'accueil.
- Sélectionnez **Ajouter un compte utilisateur** dans le panneau Nouveau en dessous de la liste des utilisateurs actuels (le cas échéant).
- Entrez un **Nom d'utilisateur** qui peut être un nom court dont vous aurez besoin plus tard lors de l'installation de Joomla. Exemple : jtest. Vous pouvez utiliser ce même utilisateur pour d'autres bases de données créées plus tard !
- Sélectionnez **Local** dans la liste de champs Nom d'hôte. Cela définira localhost dans le champ de valeur adjacent.
- Utilisez le bouton **Générer** pour générer un mot de passe. Vous devez copier la valeur générée et la coller dans un éditeur de texte pour une utilisation ultérieure. Vous n'avez pas besoin de vous en souvenir à long terme car elle sera stockée en texte clair dans le fichier configuration.php de Joomla. Exemple : 8t8mGQq.gw\[\]8lp(
- **Enregistrer** ignorez les sections Base de données pour utilisateur et Privilèges globaux du formulaire. Le bouton **Exécuter** (Enregistrer) se trouve en bas.

### Attribuer des Permissions de Base de Données à l'Utilisateur

- Dans la page **Comptes Utilisateur**, sélectionnez l'utilisateur (jtest), si nécessaire via Accueil / Comptes utilisateur / Nom d'utilisateur.
- Sélectionnez **Base de données** près du haut de la page. Cela affichera une liste des bases de données pour lesquelles cet utilisateur a reçu des autorisations d'accès et, en bas, une liste des bases de données pour lesquelles l'utilisateur n'a pas reçu d'accès.
- **Sélectionnez** la base de données à laquelle l'accès doit être accordé et sélectionnez le bouton **Exécuter**.
- **Privilèges spécifiques à la base de données** Sélectionnez **Tout cocher** puis **Exécuter** pour sauvegarder.

C'est tout ! Vous pouvez maintenant vous déconnecter de phpMyAdmin. Avez-vous oublié de noter le mot de passe de l'utilisateur de la base de données ? Retournez en arrière et modifiez le compte utilisateur que vous avez créé pour le changer. Si vous utilisez le même utilisateur de base de données pour plusieurs bases de données Joomla, vous trouverez le mot de passe dans un fichier configuration.php de Joomla.

*Traduit par openai.com*

