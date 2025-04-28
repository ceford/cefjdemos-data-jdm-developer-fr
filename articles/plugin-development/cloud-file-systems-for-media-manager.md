<!-- Filename: J4.x:Cloud_File_Systems_for_Media_Manager / Display title: Systèmes de fichiers cloud pour le gestionnaire de médias -->

<span id="main-portal-heading">GSoC 2017
Systèmes de fichiers Cloud pour le gestionnaire de médias
Documentation</span> Joomla! 4.x

## Introduction

**Joomla! 4.x** est livré avec des systèmes de fichiers cloud pour le **Gestionnaire de médias** par défaut. Avec l'ancienne API, créer des systèmes de fichiers personnalisés était une tâche difficile. Grâce à la nouvelle API, il est maintenant facile de créer un système de fichiers personnalisé. Si vous souhaitez utiliser un service cloud avec le nouveau Gestionnaire de médias, il est conseillé d'utiliser **OAuth2.0**.

Ce document vous guidera à travers des étapes importantes pour créer votre propre **Plugin de Système de Fichiers** afin d'étendre le **Gestionnaire de Médias**. Avant de procéder, veuillez vous assurer que vous avez les connaissances de base sur la façon de développer un plugin pour Joomla. [Ce tutoriel](https://docs.joomla.org/J3.x:Creating_a_Plugin_for_Joomla) devrait vous aider.

## Créez votre plugin de système de fichiers

### Configuration

Tout d'abord, nous devons créer un plugin de **système de fichiers** pour étendre le gestionnaire de médias. Ce plugin doit contenir certains attributs importants afin qu'il puisse fonctionner avec le gestionnaire de médias.

Assurez-vous que votre plugin contient `group="filesystem"`. Ainsi, dans votre fichier `[plugin-name].xml`, vous devriez avoir :

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension version="4.0" type="plugin" group="filesystem" method="upgrade">
    <name>plg_filesystem_myplugin</name>
    <author>Joomla! Project</author>
    <creationDate>April 2017</creationDate>
    <copyright>Copyright (C) 2005 - 2017 Open Source Matters. All rights reserved.</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <authorEmail>admin@joomla.org</authorEmail>
    <authorUrl>www.joomla.org</authorUrl>
    <version>__DEPLOY_VERSION__</version>
    <description>Description</description>
    <files>
        <filename plugin="myplugin">myplugin.php</filename>
        <folder>SomeFolder</folder>
    </files>

    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="display_name"
                    type="text"
                    label="YOUR_LABEL"
                    description="YOUR_DESCRIPTION"
                    default="DEFAULT_VALUE"
                />
            </fieldset>
        </fields>
    </config>
</extension>
```

Le paramètre **display_name** aide le Gestionnaire de Médias à afficher le nom de votre **Système de Fichiers** comme un nœud racine dans le Navigateur de Fichiers. Tout adaptateur appartenant à votre Système de Fichiers sera affiché comme des enfants sous la racine.

Incluez tous les paramètres requis dont vous pourriez avoir besoin, comme le `App Secret`, avec des champs de formulaire appropriés.

### Événements du plugin

- `onFileSystemGetAdapters()`
- `onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)`

#### onFileSystemGetAdapters()

**Tout** plugin de système de fichiers doit contenir un événement appelé `onFileSystemGetAdapters()` pour la fonctionnalité. Cet événement doit retourner un **tableau** de `Joomla\Component\Media\Administrator\Adapter\AdapterInterface` lorsqu'il est appelé. L'événement est déclenché lorsque vous ouvrez le Gestionnaire de médias. Chaque `AdapterInterface` sera monté sous le nœud racine de votre système de fichiers.

#### onFileSystemOAuthCallback()

Cet événement est déclenché lorsque vous utilisez le **OAuthCallbackHandler** du Media Manager pour le processus d'autorisation et d'authentification OAuth2.0 décrit ci-dessous dans le document. L'événement est uniquement déclenché sur le plugin demandé. Il n'est donc pas nécessaire de vérifier `$context` dans un scénario typique.

Un exemple d'utilisation de l'événement ressemble à :

```php
    public function onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)
    {
        // Your context
        $context = $event->getContext();

        // Get the input
        $data = $event->getInput();

        // Your code goes here

        // Set result to be returned
        $result = [
            "action" => "control-panel"
        ];

        // Pass back the result to event
        $event->setArgument('result', $result);
    }
```

**OAuthCallbackEvent** contient l'entrée transférée vers l'URI OAuthCallback du Media Manager. `$event->getInput()` renvoie un objet Input.

`$result` définit comment Joomla se comporte après une redirection vers le rappel. Il existe plusieurs solutions possibles :

- `action`
  - close : Ferme une fenêtre qui est ouverte par un JavaScript **en utilisant
    window.open()**
  - redirect : Redirige vers la page spécifiée par le **redirect_uri**
  - control-panel : Rediriger vers le panneau de contrôle
  - media-manager : Rediriger vers le gestionnaire de médias
- `redirect_uri`
  - Utilisé avec l'action **redirect** (obligatoire)
- `message`
  - Le message doit être affiché après une action
- `message_type`
  - warning
  - notice
  - error
  - message(ou laisser vide)

Après avoir tout réglé, définissez l'argument `$result` de `$event` pour le renvoyer à l'appelant.

Un exemple pour afficher un message à l'utilisateur serait :

```php
    $result = [
        "action" => "media-manager",
        "message" => "Some message",
        "message-type" => "notice"
    ];
```

Cela redirigera vers le gestionnaire de médias et affichera un avis avec le texte dans **message**.

Cette méthode peut généralement être utilisée pour obtenir des codes d'autorisation pour le processus **OAuth2.0**. En cas d'**erreur**, Joomla! reviendra au panneau de contrôle et affichera un message d'erreur.

## Authentification et Autorisation

Joomla! 4.x vous conseille d'utiliser OAuth2.0 pour ce processus. Il est courant que OAuth2.0 nécessite une **URL de redirection** pour transmettre les données d'authentification à l'application. Cela est généralement fait par un gestionnaire de rappel. Pour votre plugin, pas besoin de réinventer la roue, vous pouvez utiliser le **gestionnaire de rappel** du **Gestionnaire de médias** pour accomplir cette tâche.

Tout ce que vous avez à faire est de définir l'**URI de redirection** comme suit dans votre fournisseur OAuth2.0 et d'implémenter le
`onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)`
dans votre plugin.

`[site-name]/administrator/index.php?option=com_media&task=plugin.oauthcallback&plugin=[votre-nom-de-plugin]`

Dans cet exemple :  
`[site-name]/administrator/index.php?option=com_media&task=plugin.oauthcallback&plugin=myplugin`

Maintenant, lorsque vous effectuez une redirection depuis votre fournisseur une fois qu'il atteint l'URL fournie, le **Contrôleur** pour le Gestionnaire de médias s'assurera que votre plugin implémente `onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)` avant de lui transmettre toute donnée. Sinon, vous serez redirigé vers le Panneau de contrôle avec un message d'erreur.

Si vous avez implémenté toutes les entrées qui sont passées, le fournisseur Cloud sera inclus dans le `$event`. Vous pouvez y accéder en utilisant `$event->getInput()` et le traiter comme une `input` habituelle pour Joomla.

Après avoir reçu l'`input`, vous pouvez l'utiliser pour obtenir les détails de votre service cloud tels que **Jeton d'accès**, **Jeton de rafraîchissement**, etc.

Si vous souhaitez distinguer plusieurs comptes, vous pouvez utiliser `Session` pour cela.

**Note** : Il est conseillé de vérifier le **jeton CSRF** avant de procéder à votre demande. La plupart des fournisseurs de services cloud prennent en charge le paramètre `state` qui peut être utilisé pour accomplir la tâche.

### Pièges Courants

Il est nécessaire de `urlencode()` votre URI de redirection lorsque vous l'envoyez au fournisseur de cloud. Veuillez l'utiliser pour éviter les mauvais comportements de votre cloud.

## Servir un fichier depuis votre adaptateur

Pour servir vos fichiers multimédias depuis le Gestionnaire de médias vers le **Site Joomla!**
`Joomla\Component\Media\Administrator\Adapter\AdapterInterface` vous aide. Cette interface contient une méthode spéciale appelée `getUrl($path)`.

`$path : Le chemin est relatif à votre racine`

L'intention de la méthode est de fournir une **URL Absolue Publique** pour le fichier que vous souhaitez insérer dans le site. Par exemple, supposons que le chemin de votre fichier soit `/path/to/me.png` sur le serveur cloud. Maintenant, une URL publique pourrait ressembler à `mycloud.com/share/u/myusername/path/to/me.png`. La manière dont il est servi dépend entièrement de vous. Lorsqu'un utilisateur souhaite insérer une URL générée par un média, cette méthode sera utilisée.

Plus de détails sur `Adapter Interface` peuvent être trouvés dans
`administrator/componenents/com_media/Adapter/AdapterInterface.php`

*Traduit par openai.com*

