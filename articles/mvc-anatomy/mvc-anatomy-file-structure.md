<!-- Filename: J4.x:MVC_Anatomy:_File_Structure / Display title: Anatomie MVC : Structure de Fichiers -->

## Configuration du développeur

Il existe plusieurs façons d'organiser les fichiers à des fins de développement. Une méthode consiste à utiliser un clone d'un dépôt GitHub comme dans la structure schématique suivante :

```
cefjdemos-com-countrybase
|-- .vscode (folder for build settings)
|-- com_countrybase (folder compressed to create an installable zip file)
    |-- admin (the administrator files)
        |-- forms
        |-- help
            |-- countrybase.html (this is a Help screen used in the backend module edit form)
        |-- language
            |-- en-GB (language folder, kept with the extension code)
                |-- mod_downmsg.ini
                |-- mod_downmsg.sys.ini
        |-- services (folder for dependency injection)
            |-- provider.php (the DI code)
        |-- sql
            |-- updates
                |-- mysql
                    |-- index.html (an empty file required to avoid an installation error if there are no sql updates)
        |-- src (folder for namespaced classes)
            |-- more folders: Controller, Extension, Helper, Model, Table and View
        |-- tmpl (folder for layouts)
            |-- more folders: countries, country, currencies and currency
        |-- access.xml (standard list of access permissions)
        |-- config.xml (options parameters)
    |-- media
        |-- css (placeholder for CSS files - contains an empty index.html file)
        |-- js (placeholder for JavaScript files - contains an empty index.html file)
    |-- site (the site files, abbreviated here)
        |-- forms
        |-- language
        |-- src
        |-- tmpl
    |-- countrybase.xml (the manifest file)
    |-- script.php (a script to run on install, update or uninstall)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (record of changes)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
|-- updates.xml (update server specification)
```

Ceci est la structure dans l'IDE VSCodium :

![Vscodium file structure view](../../../en/images/mvc-anatomy/com-countrybase-vscodium.png)

Lors de l'installation, les parties du composant `com_countrybase` sont distribuées à différents emplacements dans la structure de fichiers Joomla :
- Les fichiers administrateur vont dans `root/administrator/components/com_countrybase`.
- Les fichiers du site vont dans `root/components/com_countrybase`.
- Les fichiers média, javascript et css (le cas échéant), vont dans `root/media/com_countrybase`.
- Les fichiers de langue vont dans les structures des composants administrateurs et des sites. Une convention précédente les plaçait dans les dossiers de langue principaux.

C'est le fichier manifeste du composant, dans ce cas countrybase.xml, qui contrôle où vont précisément les éléments.

Notez que le fichier zip contient tout ce qui se trouve dans le dossier com_countrybase. Vous pouvez créer un zip simplement en compressant ce dossier. En dehors de ce dossier se trouvent build.xml, qui est un fichier utilisé pour construire le composant chaque fois qu'une modification est effectuée, et README.md, qui est un fichier Markdown standard au format Github décrivant le composant.

## Notes

- Il y a plus de 40 fichiers dans ce composant simple !
- Lors de l'installation, le fichier countrybase.xml est copié dans administrator/components/com_countrybase où il est nécessaire pour la mise à jour ou la désinstallation.

*Traduit par openai.com*

