<!-- Filename: Adding_changelog_to_your_manifest_file / Display title: Ajout d'un journal des modifications -->

Depuis Joomla 4.0, les développeurs d'extensions peuvent exploiter la capacité de Joomla à lire un fichier de journal des modifications et à fournir une représentation visuelle de celui-ci. Si une version donnée n'est pas trouvée dans le journal des modifications, le bouton du journal ne sera pas affiché.

Les modifications d'une version seront présentées de la manière suivante :

![changelog modal view](../../../en/images/developer-information/adding-changelog-example-1.png)

Le journal des modifications est utilisé à deux endroits différents.

## Mettre à jour la vue

L'installateur affichera le journal des modifications de la version qui peut être installée si disponible.

![changelog installer view](../../../en/images/developer-information/adding-changelog-update-view.png)

En cliquant sur le bouton Journal des modifications ici, vous verrez le journal des modifications de la nouvelle version disponible.

## Gérer la vue

Le gestionnaire d'extensions affichera le journal des modifications de l'extension actuellement installée si disponible.

![changelog installer view](../../../en/images/developer-information/adding-changelog-extension-view.png)

Cliquer sur le numéro de version ici affichera le journal des modifications de la version actuellement installée.

## Ajouter l'étiquette changelogurl aux fichiers manifestes

La première étape consiste à mettre à jour vos fichiers manifestes qui indiquent à Joomla où trouver les détails du journal des modifications. Ajoutez le nœud suivant à vos fichiers XML manifestes :

```
<changelogurl>https://example.com/updates/changelog.xml</changelogurl>
```

Veuillez noter : L'URL dans la balise changelogurl ne doit pas comporter d'espaces ou de sauts de ligne avant ou après. Voir les exemples de code.

### Mettre à jour le manifeste du serveur

Voir cet exemple de fichier manifeste du serveur de mise à jour qui informe Joomla d'une mise à jour d'un composant nommé "com_lists". Ainsi, vous verrez le bouton Journal des modifications dans la vue de mise à jour.

```
<?xml version="1.0" encoding="utf-8"?>
<updates>
 <update>
  <name>Student List</name>
  <description>List of students</description>
  <element>com_lists</element>
  <type>component</type>
  <version>4.0.0</version>

  <changelogurl>https://example.com/updates/changelog.xml</changelogurl>

  <tags>
   <tag>stable</tag>
  </tags>
  <maintainer>Example Miller</maintainer>
  <maintainerurl>https://example.com/</maintainerurl>
  <section>Updates</section>
  <targetplatform name="joomla" version="4.?" />
  <client>1</client>
  <folder></folder>
 </update>
</updates>
```

### Manifeste d'extension

Ajoutez également la balise changelogurl au manifeste XML de l'extension. Ainsi, la version de l'extension sera liée aux journaux des modifications dans la vue de gestion.

```
<?xml version="1.0" encoding="utf-8"?>
<extension type="component" method="upgrade">
    <name>COM_LISTS</name>

... Other stuff ...

    <changelogurl>https://example.com/updates/changelog.xml</changelogurl>

    <updateservers>
        <server type="extension" name="My Extension's Updates">https://example.com/lists-updates.xml</server>
    </updateservers>
</extension>
```

## Créer un fichier de modifications

Le fichier de journal des modifications doit comporter les 3 nœuds suivants :

* élément
* type
* version

Ces informations sont utilisées pour identifier le bon journal des modifications pour une extension donnée.

Un nœud de version à l'intérieur de n'importe quel nœud de journal des modifications est toujours obligatoire. Sinon, vous verrez un message d'erreur tel que SyntaxError: JSON.parse: caractère inattendu à la ligne 1 colonne 1 des données JSON.

```
<element>com_lists</element>
<type>component</type>
<version>4.0.0</version>
```

De plus, le journal des modifications est rempli d'un ou plusieurs types de modifications. Les types de modifications suivants sont pris en charge :

* sécurité : Tous les problèmes de sécurité qui ont été corrigés
* correction : Tous les bogues qui ont été corrigés
* langue : Ceci concerne les modifications de langue
* ajout : Toutes les nouvelles fonctionnalités ajoutées
* modification : Tous les changements
* suppression : Toutes les fonctionnalités supprimées
* note : Toute information supplémentaire pour informer l'utilisateur

Chaque nœud peut être répété autant de fois que nécessaire.

Le format du texte peut être du texte brut ou du HTML, mais dans le cas du HTML, il doit être encadré de balises CDATA comme montré dans l'exemple.

```
<changelogs>
    <changelog>
        <element>com_lists</element>
        <type>component</type>
        <version>4.0.0</version>
        <security>
            <item>Item A</item>
            <item><![CDATA[<h2>You MUST replace this file</h2>]]></item>
        </security>
        <fix>
            <item>Item A</item>
            <item>Item b</item>
        </fix>
        <language>
            <item>Item A</item>
            <item>Item b</item>
        </language>
        <addition>
            <item>Item A</item>
            <item>Item b</item>
        </addition>
        <change>
            <item>Item A</item>
            <item>Item b</item>
        </change>
        <remove>
            <item>Item A</item>
            <item>Item b</item>
        </remove>
        <note>
            <item>Item A</item>
            <item>Item b</item>
        </note>
</changelog>
<changelog>
    <element>com_lists</element>
    <type>component</type>
    <version>0.0.2</version>
    <security>
        <item>Big issue</item>
    </security>
</changelog>
</changelogs>
```

Ce fichier contient 2 journaux de modifications :

* Version 0.0.2 (pour tester la vue de gestion)
* Version 4.0.0 (pour tester la vue de mise à jour)

Un journal des modifications peut avoir autant de versions que nécessaire.

*Traduit par openai.com*

