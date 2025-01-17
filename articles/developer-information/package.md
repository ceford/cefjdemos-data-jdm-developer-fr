<!-- Filename: https://docs.joomla.org/Package / Display title: Paquets -->

## Introduction aux Paquets

Parfois, un module (ou plugin ou autre extension) utilise les modèles ou les assistants d'un composant. Dans ces cas, il est préférable de pouvoir installer ou désinstaller un module dépendant lorsque le composant lui-même est installé ou désinstallé. Dans Joomla, cette fonctionnalité est obtenue avec un paquet, bien que l'utilisation des paquets ne soit pas infaillible.

Un paquet est une extension utilisée pour installer plusieurs extensions en une seule fois. Les combiner dans un paquet permet à l'utilisateur d'installer et de désinstaller deux extensions ou plus en une seule opération.

## Création de package

Un package est créé en zippant tous les fichiers d'extension individuels `.zip` avec un fichier manifeste `.xml`. Par exemple, pour un package composé de :

* **composant** helloworld
* **module** helloworld
* **bibliothèque** helloworld
* **plugin système** helloworld
* **modèle** helloworld

Le paquet aurait l'arborescence suivante dans le fichier zip :

```sh
  -- pkg_helloworld.xml
  -- packages <dir>
      |-- com_helloworld.zip
      |-- mod_helloworld.zip
      |-- lib_helloworld.zip
      |-- plg_sys_helloworld.zip
      |-- tpl_helloworld.zip
  -- pkg_script.php
```

Le `pkg_helloworld.xml` pourrait avoir le contenu suivant :

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<extension type="package" method="upgrade">
<name>Hello World Package</name>
<author>Hello World Package Team</author>
<creationDate>May 2022</creationDate>
<packagename>helloworld</packagename>
<version>1.0.0</version>
<url>http://www.yoururl.com/</url>
<packager>Hello World Package Team</packager>
<packagerurl>http://www.yoururl.com/</packagerurl>
<description>Example package to combine multiple extensions</description>
<update>http://www.updateurl.com/update</update>
<scriptfile>pkg_script.php</scriptfile>
<blockChildUninstall>true</blockChildUninstall>
<files folder="packages">
    <file type="component" id="com_helloworld">com_helloworld.zip</file>
    <file type="module" id="helloworld" client="site">mod_helloworld.zip</file>
    <file type="library" id="helloworld">lib_helloworld.zip</file>
    <file type="plugin" id="helloworld" group="system">plg_sys_helloworld.zip</file>
    <file type="template" id="helloworld" client="site">tpl_helloworld.zip</file>
</files>
</extension>
```

Une fois compressé et installé, le paquet sera visible dans la liste des extensions afin qu'un utilisateur puisse désinstaller toutes les extensions contenues dans le paquet.

N'oubliez pas d'utiliser le désinstalleur de package au lieu des désinstalleurs de sous-packages individuels pour éviter des entrées d'extensions orphelines dans le gestionnaire d'extensions. C'est la partie qui n'est pas infaillible !

### Identifiant de fichier

L'élément id dans la balise `<file ..>` n'est **pas** arbitraire ! Le `id=` doit être réglé sur la valeur de la colonne `element` dans la table `#__extensions`. S'ils ne sont pas correctement définis, lors de la désinstallation du package, le `file` enfant ne sera **pas** trouvé et désinstallé.

### Nom du fichier de manifeste et nom du paquet

La nomination du fichier manifeste et la capacité de désinstaller le fichier du paquet sont étroitement liées. Le fichier manifeste doit avoir un préfixe **pkg_**. Le reste du nom du manifeste (sans l'extension **.xml**) doit être utilisé comme `<packagename>`. Ou inversement, un paquet que vous voulez identifier comme **blurpblurp_J3** reçoit cela comme son `<packagename>` et doit se trouver dans un fichier manifeste nommé **pkg_blurpblurp_J3.xml**. Ne pas le faire rendra impossible la désinstallation du paquet lui-même.

Un `<pkg_script.php>` optionnel qui contient une classe `pkg_<packagename>InstallerScript` avec la fonction publique **postflight** peut être utilisé.

### Balise d'extension

La balise `<extension>` doit inclure `method="upgrade"` pour qu'une mise à jour du package réussisse lors des installations ultérieures.

### Désinstallation de la Dépendance de l'Extension

Un module d'extension de package peut déclarer que ses éléments enfants ne peuvent pas être désinstallés indépendamment avec un élément `<blockChildUninstall>true</blockChildUninstall>` dans le manifeste du package. Si votre package nécessite que toutes ses extensions fonctionnent efficacement, définissez cette valeur à true ou 1.

*Traduit par openai.com*

