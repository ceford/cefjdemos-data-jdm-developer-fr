<!-- Filename: J4.x:MVC_Anatomy:_Manifest_File / Display title: MVC Anatomie : Fichier Manifest -->

## Métadonnées

Le fichier manifeste du composant doit être nommé manifest.xml ou componentname.xml, dans ce cas countrybase.xml. Notez que la partie com_ n'est pas incluse. La première partie du fichier contient des métadonnées :

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="component" method="upgrade">
    <name>com_countrybase</name>
    <author>Clifford E Ford</author>
    <authorEmail>john.doe@example.com</authorEmail>
    <authorUrl>example.com</authorUrl>
    <creationDate>2025-01-02</creationDate>
    <copyright>(C) 2025 Clifford E Ford</copyright>
    <license>GNU General Public License version 3 or later; see LICENSE.txt</license>
    <version>0.0.1</version>
    <description>COM_COUNTRYBASE_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Component\Countrybase</namespace>
```

La plupart des métadonnées sont évidentes. Points à noter :

- Le type d'extension dans ce cas est **composant**. Il y a d'autres types d'extensions, comme module ou plugin.
- La méthode peut être install ou upgrade. Cette dernière permet la réinstallation après mise à jour du code.
- Le numéro de version est important ! Il doit rendre impossible la réinstallation d'une version plus ancienne. Et il est utilisé pour contrôler si des mises à jour de la base de données sont nécessaires.
- Le chemin d'espace de noms src comporte trois parties :
  - La première partie est un préfixe défini par le fournisseur, donc Joomla pour le code fourni par Joomla, Acme pour le code fourni par le fournisseur tiers d'extension Acme, et dans ce cas Cefjdemos, un nom choisi pour les démonstrations de code Joomla.
  - La deuxième partie indique le type d'extension. Cela pourrait être Composant ou Module ou Plugin ou ...
  - La troisième partie est le nom spécifique de l'extension, dans ce cas Countrybase.

## Base de données

Le fichier manifeste définit les emplacements de tous les fichiers sql d'installation, de mise à jour ou de suppression. Ils se retrouvent dans un dossier sql dans le dossier administrateur.

```xml
    <install>
        <sql>
            <file driver="mysql" charset="utf8">sql/install.mysql.sql</file>
        </sql>
    </install>
    <uninstall>
        <sql>
            <file driver="mysql" charset="utf8">sql/uninstall.mysql.sql</file>
        </sql>
    </uninstall>
    <update>
        <schemas>
            <schemapath type="mysql">sql/updates/mysql</schemapath>
        </schemas>
    </update>
```

## Fichier de script

Un fichier script peut être utilisé à des fins de pré-vol ou de post-vol. Par exemple, il pourrait être utilisé pour vérifier les exigences système minimales avant l'installation ou pour définir des paramètres après l'installation.

## Fichiers multimédias

Le composant com_countrybase n'a pas besoin de CSS ou de JavaScript spécifique au composant, mais le code pour les installer est inclus par précaution. Les dossiers css et js ne contiennent que des fichiers index.html vides. Sans ces fichiers, les dossiers ne sont pas créés.

```xml
    <scriptfile>script.php</scriptfile>

    <media destination="com_countrybase" folder="media">
        <file>joomla.asset.json</file>
        <folder>css</folder>
        <folder>js</folder>
    </media>
```

Notez que le fichier joomla.asset.json est utilisé par le gestionnaire de ressources web, généralement appelé à partir d'un fichier modèle.

## Fichiers du site

Ce sont les fichiers qui sont copiés dans root/components/com_countrybase. Notez que le dossier de langue est copié dans le dossier du composant et non dans le dossier root/language :

```xml
    <files folder="site">
        <folder>language</folder>
        <folder>forms</folder>
        <folder>src</folder>
        <folder>tmpl</folder>
    </files>
```

## Fichiers administrateur

Il y a plus de fichiers dans la partie administration du fichier manifeste car il y a plus à faire :

```xml
    <administration>
        <files folder="admin">
            <filename>access.xml</filename>
            <filename>config.xml</filename>
            <folder>forms</folder>
            <folder>help</folder>
            <folder>language</folder>
            <folder>layouts</folder>
            <folder>services</folder>
            <folder>sql</folder>
            <folder>src</folder>
            <folder>tmpl</folder>
        </files>
        <menu img="class:default">countrybase</menu>
        <submenu>
            <!--
                Note that all & must be escaped to &amp; for the file to be valid
                XML and be parsed by the installer
            -->
            <menu
                link="option=com_countrybase&amp;view=countries"
                img="default"
            >
                COM_COUNTRYBASE_COUNTRIES
            </menu>
            <menu
                link="option=com_countrybase&amp;view=currencies"
                img="default"
            >
                COM_COUNTRYBASE_CURRENCIES
            </menu>
        </submenu>
    </administration>
```

Notez la méthode pour créer les éléments de menu de l'administrateur Joomla. Il y a un élément de menu qui n'a pas de lien. Et deux éléments de menu qui renvoient aux deux vues d'administration disponibles : la liste des pays et la liste des devises. De plus, les traductions des clés de chaînes doivent être dans le fichier com_countrybase.sys.ini.

## Journal des modifications

Le journal des modifications est utilisé pour conserver un enregistrement des changements avec chaque version de l'extension. Consultez l'article [Changelogs](jdocmanual?article=docus/install-update/installation-change-log) pour des informations sur le contenu du journal des modifications.

```xml
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-com-countrybase/master/changelog.xml</changelogurl>
```

## Mettre à jour le serveur

Le code de serveur de mise à jour indique à Joomla où trouver les informations de mise à jour. Il s'exécute à intervalles réguliers pour vérifier si une mise à jour est disponible. Omettez cette section si vous n'utilisez pas un serveur de mise à jour pour votre propre composant.

```xml
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" name="Countrybase Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-com-countrybase/master/updates.xml</server>
    </updateservers>
```

*Traduit par openai.com*

