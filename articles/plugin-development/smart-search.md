<!-- Filename: Creating_a_Smart_Search_plug-in / Display title: Exemple : Recherche intelligente -->

## Introduction

Les plugins de recherche intelligente sont situés dans le groupe `finder` (dans plugins/finder/xxxx). Le codage des plugins finder a changé de manière significative dans Joomla 4 et 5. Pour créer un nouveau plugin finder, il est probablement préférable de copier un plugin core existant et de modifier son contenu pour l'adapter à son nouvel objectif.

Cet article décrit un plugin de recherche conçu pour prendre en charge l'extension *Jdocmanual*. Le contenu à indexer est situé dans la table `#__jdm_articles`. Le code peut être trouvé sur [GitHub](https://github.com/ceford/cefjdemos-plg-finder-jdocmanual).

## Configuration de l'EDI - VSCode ou VSCodium

Dans mon environnement de développement local, le code source de cette extension se trouve dans ~/git/cefjdemos-plg-jdm-finder. Ce nom de dossier n'a aucune signification ; c'est juste ma façon de garder mes propres extensions organisées. La structure schématique du dossier est la suivante :

```
cefjdemos-plg-finder-jdocmanual
|-- .vscode (folder for build settings)
|-- plg_finder_jdocmanual (folder compressed to create an installable zip file)
    |-- language/en-GB/ (language folder, kept with the extension code)
        |-- plg_finder_jdocmanual.ini
        |-- plg_finder_jdocmanual.sys.ini
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Extension (folder for extension specific code)
            |-- Jdocmanual.php (the extension class code)
    |-- jdocmanual.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
```

Dans VSCodium, cela ressemble à ceci :

![Plugin development file structure in vscodium](../../../en/images/plugins/jdocmanual-vscodium.png)

## Personnaliser le code

Ayant commencé avec un plugin de recherche principal, la première tâche consiste à changer toutes les références au plugin original par des valeurs appropriées pour le nouveau plugin. Dans ce cas, il y a 63 occurrences du mot `jdocmanual` dans 7 fichiers. Il y a une bonne probabilité que certaines soient manquées lors du premier passage et que le plugin ne fonctionne pas.

## Construire l'extension

Il y a deux parties dans la construction. J'ai un fichier build.xml qui contient des instructions pour [`phing`](https://www.phing.info/), un outil de construction pour des projets PHP. Il peut être appelé depuis VSCode/VSCodium ou Eclipse ou tout autre IDE.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="jdocmanual" basedir="." default="main">

    <property name="joomladir" value="/Users/ceford/public_html/jdm3"  override="true" />
    <property name="sitecomdir" value="${joomladir}/components" override="true" />
    <property name="admincomdir" value="${joomladir}/administrator/components" override="true" />
    <property name="adminmediadir" value="${joomladir}/media/com_jdocmanual" override="true" />
    <property name="plgdir" value="${joomladir}/plugins/finder/jdocmanual" override="true" />
    <property name="srcdir" value="${project.basedir}" override="true" />

    <property name="zipsdir" value="/Users/ceford/git/zips/cefjdemos"  override="true" />

    <!-- Fileset for plugin files -->
    <fileset dir="./plg_finder_jdocmanual" id="plgfiles">
        <include name="**" />
    </fileset>

    <!-- fileset for zip -->
    <fileset dir="./plg_finder_jdocmanual" id="plgfiles">
        <include name="**" />
    </fileset>

    <!-- ============================================  -->
    <!-- (DEFAULT) Target: main                        -->
    <!-- ============================================  -->
    <target name="main" description="main target">

        <copy todir="${plgdir}">
            <fileset refid="plgfiles" />
        </copy>

        <zip destfile="${zipsdir}/plg_finder_jdocmanual.zip">
            <fileset refid="plgfiles" />
        </zip>

    </target>
</project>
```

La deuxième partie est un fichier `tasks.json` dans le dossier .vscode. Il indique à VSCode où trouver le fichier phing.

```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
      {
        "label": "Build plg_finder_jdocmanual",
        "type": "shell",
        "command": "php ~/bin/phing-latest.phar",
        "windows": {
          "command": "php ~/bin/phing-latest.phar"
        },
        "group": "build",
        "presentation": {
          "reveal": "always",
          "panel": "shared"
        }
      }
    ]
}
```

Dans VSCode/VScodium, je sélectionne **Terminal / Exécuter la tâche de build...** dans la barre de menu et je choisis la tâche appropriée.

## Installer l'extension

Après la construction, le fichier zip de l'extension doit être installé de la même manière que toute autre extension. Il doit être activé et toutes les options doivent être définies. Jdocmanual propose trois options *Taxonomies à indexer* :

- **Manuel** un ou tous les Manuels disponibles (Utilisateur, Développeur, ...)
- **Langue** les recherches sont limitées aux articles dans la même langue que la langue de la page.
- **Type** un ou tous les types de contenu (Jdocmanuel, Contenu, ...)

Dans le cas du site Jdocmanual, les autres plugins de recherche sont désactivés car il n'y a pas d'autres contenus à indexer.

Après la première installation, il est normalement suffisant d'exécuter à nouveau la tâche de construction après la correction du code. L'installation du fichier zip n'est requise que s'il y a des modifications dans le fichier manifest.xml.

## Indexer le contenu

Jdocmanual est configuré pour indexer ses articles lorsqu'un Super Utilisateur en fait la demande. Cela diffère du plugin de Contenu normal qui indexe le contenu chaque fois qu'un article est enregistré. Une première exécution prend quelques minutes pour indexer les ~5000 articles de Jdocmanual en 8 langues.

## Créer un module de recherche intelligente

Dans le **Contenu / Modules du site**, sélectionnez **Nouveau** et installez un nouveau module **Smart Search**. Réglez les paramètres pour obtenir une apparence appropriée.

## Test

Finalement, votre plugin de recherche intelligente personnalisé devrait fonctionner. Voici un exemple de page de résultats pour Jdocmanual qui recherche un terme dans cette page. La page de résultats omet le formulaire de recherche de la barre de titre car il est présent dans le corps de la page.

![Smart search result](../../../en/images/plugins/jdocmanual-search-result.png)

Une parenthèse : le plugin System - Joomla Accessibility Checker indique qu'il y a 3 erreurs liées au formulaire de saisie de données *Termes de recherche*. Cela nécessite une correction ou un remplacement au niveau du cœur.

*Traduit par openai.com*

