<!-- Filename: Adding_changelog_to_your_manifest_file / Display title: Ajouter un changelog -->

Depuis Joomla 4.0, les développeurs d’extensions peuvent exploiter la capacité de Joomla à lire un fichier de journal des modifications et à donner une représentation visuelle de celui-ci. Si une version donnée n'est pas trouvée dans le journal des modifications, le bouton du journal des modifications ne sera pas affiché.

Les modifications dans une version seront présentées de cette manière:

<img alt="Changelog modal" src="https://docs.joomla.org/images/thumb/7/7a/Changelog_modal-en.png/700px-Changelog_modal-en.png" decoding="async" width="700" height="461" srcset="https://docs.joomla.org/images/thumb/7/7a/Changelog_modal-en.png/1050px-Changelog_modal-en.png 1.5x, https://docs.joomla.org/images/thumb/7/7a/Changelog_modal-en.png/1400px-Changelog_modal-en.png 2x" data-file-width="1618" data-file-height="1066">

Le journal des modifications est utilisé à deux endroits différents.

## Vue de Mise à jour

Le programme d'installation affichera le journal des modifications de la version pouvant être installée, le cas échéant.

<img alt="Changelog button on the Update View" src="https://docs.joomla.org/images/thumb/7/79/Update_view_changelog_button-en.png/700px-Update_view_changelog_button-en.png" decoding="async" width="700" height="152" srcset="https://docs.joomla.org/images/thumb/7/79/Update_view_changelog_button-en.png/1050px-Update_view_changelog_button-en.png 1.5x, https://docs.joomla.org/images/7/79/Update_view_changelog_button-en.png 2x" data-file-width="1282" data-file-height="278">

Cliquez sur le bouton Changelog pour afficher le changelog de la nouvelle version disponible.

## Gérer la Vue

Le gestionnaire d'extensions affiche le journal des modifications de l'extension actuellement installée, s'il est disponible.

<img alt="Version number is a link to the changelog modal" src="https://docs.joomla.org/images/thumb/4/4b/Manage_view_changelog_link-en.png/700px-Manage_view_changelog_link-en.png" decoding="async" width="700" height="81" srcset="https://docs.joomla.org/images/thumb/4/4b/Manage_view_changelog_link-en.png/1050px-Manage_view_changelog_link-en.png 1.5x, https://docs.joomla.org/images/4/4b/Manage_view_changelog_link-en.png 2x" data-file-width="1274" data-file-height="148">

Cliquez sur le numéro de version ici affichera le journal des modifications de la version actuellement installée.

## Ajouter la balise changelogurl aux fichiers manifest

La première étape consiste à mettre à jour vos fichiers manifest indiquant à Joomla où trouver les détails du journal des modifications. Ajoutez le noeud suivant à vos fichiers manifest XML:

```
<changelogurl>https://example.com/updates/changelog.xml</changelogurl>
```

A noterː L'URL dans le tag changelogurl ne doit contenir aucun espace ou passage à la ligne avant ou après. Voir les exemples de code.

### Mettre à jour du manifest du serveur

Voir cet exemple pour un fichier manifeste de serveur de mise à jour informant Joomla de la mise à jour d'un composant nommé "com_lists". Ainsi, vous verrez le bouton Changelog dans la vue de la mise à jour.

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

### Extension manifest

Ajoutez également la balise changelogurl au fichier manifest XML d’extension. Ainsi, la version de l'extension sera liée aux journaux des modifications dans la vue de gestion.

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
## Créer un fichier changelog

Le fichier changelog doit comporter les 3 nœuds suivants:

* element
* type
* version

Ces informations sont utilisées pour correctement identifier le journal des modifications pour une extension donnée.

Un noeud version est obligatoire à l'intérieur de tout noeud changelog . Sinon, vous verrez un message d'erreur du type "SyntaxError: JSON.parse: caractère inattendu à la ligne 1, colonne 1 des données JSON".

```
<element>com_lists</element>
<type>component</type>
<version>4.0.0</version>
```

De plus, le journal des modifications est rempli avec un ou plusieurs types de modifications. Les types de modification suivants sont pris en charge:

* security: Tous les problèmes de sécurité qui ont été résolus
* fix: Tous les bugs corrigés
* language: Ceci est pour les changements de langage
* addition: Toutes les nouvelles fonctionnalités ajoutées
* change: Tout changement
* remove: Toutes les fonctionnalités supprimées
* note: Toute information supplémentaire pour informer l'utilisateur

Chaque nœud peut être répété autant de fois que nécessaire.

Le format du texte peut être du texte brut ou HTML, mais dans le cas du HTML, il doit être placé entre balises CDATA, comme indiqué dans l'exemple.
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

Ce fichier contient 2 changelogs:

* Version 0.0.2 (pour tester la vue de gestion)
* Version 4.0.0 (pour tester la vue de mise à jour)

Un changelog peut avoir autant de versions que nécessaire.
