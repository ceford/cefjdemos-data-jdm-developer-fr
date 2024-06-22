<!-- Filename: Deploying_an_Update_Server / Display title: Le déploiement d'un serveur de mise à jour -->

Joomla!  2.5 Joomla!  3.x Joomla!  4.x

## Introduction

Ce didacticiel a pour vocation d'aider les développeurs à créer un
serveur de mise à jour pour une intégration avec le système de mise à
jour implémenté dans Joomla. En ajoutant une liste de serveurs de mise à
jour dans le fichier manifest de leurs extensions, les développeurs vont
permettre aux utilisateurs de mettre à jour leurs extensions via le
gestionnaire des mises à jour des extensions (voir l'écran d'aide [Mise
à Jour de Joomla
3.x](https://docs.joomla.org/Help31:Extensions_Extension_Manager_Update "Special:MyLanguage/Help31:Extensions Extension Manager Update"))
en un clic seulement.

## Définition d'un serveur de mise à jour

Pour pouvoir utiliser cette fonction, un serveur de mise à jour doit
être défini dans le fichier "manifest" de votre extension. Cette
définition peut être utilisée pour toutes les extensions compatibles
avec les versions 2.5 et plus récentes de Joomla! mais n'est pas
disponible pour les templates. Vous pouvez utiliser deux options pour
votre type de serveur : collection ou extension. Celles-ci seront
expliquées en détail par la suite. Ce code doit être ajouté au fichier
"manifest" de l'extension, à l'intérieur de la balise pour l'élément
*extension*. Le serveur de mise à jour est définie comme suit pour
chaque type :

```xml
<extension>
<...>
<updateservers>
  <server type="collection">https://example.com/list.xml</server>
  <server type="extension" priority="2" name="My Extension's Updates">http://example.com/extension.xml</server>
</updateservers>
</extension>
 ```

Plusieurs serveurs peuvent être définis à l'aide de balises . Si vous
disposez de plusieurs serveurs de mise à jour, vous pouvez indiquer une
priorité différente pour chacun d'eux. De cette manière, vous contrôlez
l'ordre dans lequel les serveurs de mise à jour vont être utilisés.

## Types de serveur

### Collection

Le serveur de type `"collection"` permet aux développeurs de définir un
fichier "manifest" pour une extension afin de charger des mises à jour à
partir d'une collection. Ce type de serveur peut être utilisé par les
développeurs qui souhaitent définir les mises à jour pour leurs
extensions dans un seul fichier (non recommandé) ou si leurs extensions
disposent de plusieurs sous-extensions qui ne sont pas distribués ou
mises à jour toutes en même temps (comme un type pack d'extension).
L'exemple ci-dessous est la définition de `"collection"` utilisée par le
programme de mise à jour du noyau Joomlaǃ :

```xml
<extensionset name="Joomla Core" description="Joomla! Core">
    <extension name="Joomla" element="joomla" type="file" version="1.7.0" detailsurl="https://update.joomla.org/core/extension.xml"/>
</extensionset>
```

Toutes les définitions doivent être présentes entre les balises dans
votre "manifest" collection. La balise peut avoir deux paramètres
facultatifs : `name` et `description`. Pour chaque extension que cette
collection références, une balise est nécessaire. La balise présente les
paramètres suivants, qui sont tous requis pour que le processus de mise
à jour se déroule correctement :

- **name** (nom) - le nom de l'extension
- **element** (élément) - le nom non traduit de l'extension c'est à dire
  mod_custom
- **type** - le type d'extension (composant, module, plugin, etc.)
- **version** - la dernière version de l'extension
- **detailsurl** - L'URL du fichier XML qui contient les définitions
  indivielles de mises à jour de l'extension

### Extensions

Le type de serveur d'`"extension"` permet aux développeurs de définir un
manifest d'extension pour obtenir les mises à jour depuis un seul
manifest d'extension. Tous les manifests de collection pointent
finalement vers ce fichier XML. Toutes les mises à jour dans ce fichier
doivent être définies après une balise au début du fichier. L'exemple
ci-dessous est la définition de la mise à jour pour la version Joomla!
3.9.6 :

```xml
<updates>
	<update>
		<name>Joomla! 3.9</name>
		<description>Joomla! 3.9 CMS</description>
		<element>joomla</element>
		<type>file</type>
		<version>3.9.6</version>
		<infourl title="Joomla!">https://www.joomla.org/announcements/release-news/5765-joomla-3-9-6-release.html</infourl>
		<downloads>
			<downloadurl type="full" format="zip">https://downloads.joomla.org/cms/joomla3/3-9-6/Joomla_3.9.6-Stable-Update_Package.zip</downloadurl>
			<downloadsource type="full" format="zip">https://github.com/joomla/joomla-cms/releases/download/3.9.6/Joomla_3.9.6-Stable-Update_Package.zip</downloadsource>
			<downloadsource type="full" format="zip">https://update.joomla.org/releases/3.9.6/Joomla_3.9.6-Stable-Update_Package.zip</downloadsource>
		</downloads>
		<tags>
			<tag>stable</tag>
		</tags>
		<sha256>05157273aadd3045564ee44373ea3643b437fa5321d17993a3119b38b04578e2</sha256>
		<sha384>ebd9b0666fbe84e20a420a4bcd6c10d306fc4dee4edbbe8e2133c85f0fb84e59be5a50aa97cb38c068b77f77f6bbc091</sha384>
		<sha512>a4c47644ceeaeec28944e0c74160203cf12037e0ea1439022e95055dfb6716de172667ce6d9164f12bb519d9cfcf1fdc728abea00f853b41debc7d2740f2b711</sha512>
		<maintainer>Joomla! Production Department</maintainer>
		<maintainerurl>https://www.joomla.org</maintainerurl>
		<section>STS</section>
		<targetplatform name="joomla" version="3.[789]" />
		<php_minimum>5.3.10</php_minimum>
</update>
</updates>
```

La section suivante décrit les éléments d'une mise à jour unique.

- **name** - le nom de l'extension, ce nom apparaîtra dans la colonne
  "Nom" de la vue de mise à jour du gestionnaire d'extension
  (obligatoire).
- **description** – une courte description de l'extension (facultatif) —
  si vous choisissez d'utiliser , les guillemets doubles vont briser le
  formatage HTML. Utilisez des guillemets simples avec vos entités HTML.
- **element** - le nom de l'extension installée (obligatoire). Pour les
  plugins, ce doit être le même que la valeur de l'attribut du plugin
  dans le manifest du fichier principal du plugin. Pour
  `nomduplugin.php`, la valeur de l'élément doit être **nomduplugin**.
- **type** - le type de l'extension (composant, module, plugin, etc.)
  (obligatoire).
- **folder** (dossier) - spécifique aux plugins, cette balise décrit le
  type du plugin mis à jour (contenu, système, etc.) (obligatoire pour
  les plugins).
- **client** – Le client de l'extension. Requis pour les modules et
  modèles de site depuis la version 3.2.0. – Les valeurs possibles
  actuellement sont "site" ou "administrator". **Attention !** Les
  plugins et les modules du site public sont automatiquement installés
  avec le client à 0 (site), mais vous devez tout de même indiquer le
  client dans une mise à jour ou il sera mis par défaut à 1
  (administrator) et, du coup, la mise à jour trouvée ne sera pas
  affichée parce qu'elle ne correspondra à aucune extension. Les
  composants sont automatiquement installés avec un client à
  "administrator", ce qui est la valeur actuelle par défaut.
  - **Attention** : Concernant Joomla! 4.0 seule une chaîne de caractère
    est autorisée. L'utilisation de nombres pour le champ client a été
    déprécié en 2012 et supprimé de Joomla! 4.0 (remplacer "0" par
    "site" et "1" par "administrator").
  - **Attention** : le nom de la balise est pour Joomla! 2.5 et pour les
    versions 1.6 et 1.7. Si vous utilisez plutôt que de sur un site de
    2.5, la balise sera ignorée.
- **version** - les références de la version (obligatoire)
- **infourl** - l'URL pour diriger les utilisateurs vers des
  informations complémentaires concernant la mise à jour (facultatif).
- **downloads** - la section qui liste tous les emplacements pour le
  téléchargement.
  - **downloadurl** - l'URL à partir de laquelle il est possible de
    télécharger l'extension. La balise doit contenir deux paramètres
    obligatoires :
    - **type** - le type du package (complète ou mise à jour).
    - **format** - Le format de l'archive (zip, tar, etc.).
  - **downloadsource** - Facultatif. Depuis la version Joomlaǃ 3.8.3.,
    une URL alternative depuis laquelle il est possible de télécharger
    l'extension lorsque la connexion depuis échoue. Plusieurs balises
    sont autorisées. La balise nécessite de renseigner deux paramètres
    obligatoires :
    - **type** - Le type du package (complète ou mise à jour).
    - **format** - Le format de l'archive (zip, tar, etc.).
  - **NB** - il ne doit pas y avoir de saut à la ligne avant ou après
    l'URL ; tout doit être écrit sur une seule est même ligne où vous
    aurez une erreur de connexion au serveur : requête malformée quand
    la mise à jour sera exécutée
- **changelogurl** - Un lien vers un fichier xml du journal des
  modifications. Les versions Joomlaǃ 4.0 et ultérieures vous
  permettront d'afficher un bouton vers le journal des modifications
  dans la page de mise à jour de l'extension. Pour plus de détails pour
  utiliser cela, consulter : [Ajouter un journal de modifications à
  votre fichier
  manifest](https://docs.joomla.org/Adding_changelog_to_your_manifest_file/fr "Adding changelog to your manifest file/fr")
- **tags** - Une liste de mots clés pertinents pour cette version. Les
  versions Joomla! 3.4 et suivantes utilisent cela pour déterminer le
  niveau de stabilité de la mise à jour. Les valeurs des tags valides
  sont :
  - *dev* : versions de développement, très instables et pré-alpha (par
    exemple, les nightly builds).
  - *alpha* : Niveau de qualité de logiciel Alpha (fonctionnalités non
    implémentées, bogues bloquants)
  - *beta* : Niveau de qualité de logiciel Beta (toutes les
    fonctionnalités sont implémentées, possibilité de bogues bloquants,
    certitude de présence de bogues mineurs)
  - *rc* : Niveau de qualité de logiciel Release Candidate (plus de
    bogues bloquants, des bogues mineurs peuvent encore subsister)
  - *stable* : Niveau de qualité de logiciel Production. Tous les autres
    tags sont actuellement ignorés. Si vous fournissez plus d'un tag
    contenant l'un des mots clés de stabilité du code ci-dessus, seul le
    DERNIER tag sera pris en compte. Si aucun tag n'est fourni, Joomla!
    va considérer que la version est stable.
- **maintainer** – Le nom de la personne qui maintient l'extension
  (similaire à la balise dans le manifest) (optionnel)
- **maintainerurl** – Le site web de la personne qui maintient
  l'extension (similaire à la balise du manifest) (optionnel)
- **section** – Facultatif (utilisation inconnue)
- **targetplatform** – Une balise pour définir les exigences de la
  plateforme (depuis la version
  <img src="https://docs.joomla.org/images/5/55/Compat_icon_3_10.png"
  decoding="async" data-file-width="40" data-file-height="17" width="40"
  height="17" alt="Joomla 3.10" /> ceci est également utilisé pour
  détecter la compatibilité des extensions pour le composant de mise à
  jour de Joomla), et qui a besoin des éléments suivant ː
  - **name** – Le nom de la dépendance de la plateforme ; actuellement,
    SEUL "joomla" est supporté
  - **version**– La version de Joomla! prenant en charge l'extension.
  - **min_dev_level** et **max_dev_level** – Ces attributs ont été
    ajoutés en 3.0.1 pour permettre de sélectionner une plateforme cible
    pour un certain niveau de développement ("z" dans x.y.z). Ils sont
    optionnels. Vous pouvez en spécifier un seul ou les deux. S'il sont
    omis, les niveaux de développements correspondent tous. Par exemple,
    la balise suivante inclut les versions 4.0.0 et 4.0.1.
    - **Note :** Si votre extension est compatible Joomla! 2.5 et/ou
      3.1, vous devrez avoir des définitions séparées de pour chaque
      version à cause de la manière dans le moteur de mise à jour va
      vérifier la version, si vous spécifiez une valeur. Cependant, pour
      montrer votre extension sur toutes les versions Joomla qui
      supportent les mises à jour automatiques (et de fait indique la
      compatibilité avec toutes les versions futures de Joomla dans le
      système de mise à jour de Joomla), ajoutez . Si vous voulez que la
      mise à jour de votre extension s'affiche pour toutes les versions
      <img src="https://docs.joomla.org/images/4/4d/Compat_icon_3_x.png"
      decoding="async" data-file-width="40" data-file-height="17" width="40"
      height="17" alt="Joomla 3.x" />, alors plutôt que de spécifier une
      version dans le tag de version, ajoutez . Ceci va montrer la mise
      à jour dans toutes les versions 3.x de 3.0 à 3.5. Si vous voulez
      inclure la version 3.10, vous pouvez utiliser un `|` comme ceci :
      . Si vous voulez montrer les mises à jour pour toutes les versions
      3.8.x et toutes les versions 3.10.x, vous pouvez utiliser
- **php_minimum** – Depuis la version 3.2.2, une version minimale PHP
  supportée peut être fournie dans le flux de mise à jour. Si le serveur
  n'a pas cette version minimale, un message indique à l'utilisateur que
  la mise à jour est accessible mais ne peut pas être installée du fait
  d'une exigence non couverte.
- **supported_databases** – Depuis la version 3.7, une vérification de
  la base de données et de sa version minimale supportée peut être
  fournie dans le flux de mise à jour. Lorsque le serveur n'a pas cette
  version minimale, un message indique à l'utilisateur que la mise à
  jour est accessible mais ne peut pas être installée du fait d'une
  exigence non couverte. A noter :
  <a href="https://github.com/joomla/joomla-cms/pull/26079"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Comme pour la version 3.9.12</a> il
  y a également un cas spécial pour mariadb et ainsi vous pouvez
  indiquer une version spécifique minimale pour cela aussi.
  - Un exemple pourrait être :
- **sha256**, **sha384**, **sha512** – Facultatif. Depuis la version
  Joomlaǃ 3.9.0, vous pouvez ajouter des checksums sur vos fichiers pour
  ces formats hash. Notez que dans Joomlaǃ 3, seule une information est
  présente dans **mises à jour** si un checksum n'est pas correct. C'est
  tout. La mise à jour ne s'arrête pas pour autant. Pour les mises à
  jour avec Joomlaǃ 4 et les installations, si un checksum fourni n'est
  pas correct, alors la mise à jour s'arrêtera.

La définition d'un séparé sera requis pour chaque version de l'extension
que vous mettez à disposition.

Les valeurs de **element**, **type**, **client_id** et **folder**
doivent correspondre à ceux de la table \#\_\_extensions.

**Important pour les plugins :** les plugins doivent intégrer les
éléments et pour fonctionner correctement.

## Résolution de problèmes

- **Le script de mise à jour SQL n'est pas exécuté durant une mise à
  jour.**

Si le script de mise à jour SQL (par exemple, dans le répertoire
`sql/updates/mysql`) n'est pas exécuté durant le processus de mise à
jour, cela peut être dû au fait qu'il n'y avait pas de numéro de version
dans la table `#__schemas` de cette extension *avant la mise à jour*.
Cette valeur est déterminée par le dernier nom de script dans le
répertoire de mises à jour SQL. Si cette valeur est vierge, aucun script
SQL ne sera exécuté durant le cycle de mise à jour. Pour vous assurer
que cette valeur est correctement spécifiée, assurez vous que vous avez
un script SQL dans ce répertoire dont le nom est le numéro de version
(par exemple, 1.2.3.sql si la version est 1.2.3). Le fichier peut être
vide ou avoir juste une ligne de commentaire SQL. Ceci doit avoir été
fait dans une version antérieure — celle avant la mise à jour. Comme
alternative, vous pouvez ajouter cette valeur dans la table `#__schemas`
en utilisant une requête SQL.

## Outils de support
