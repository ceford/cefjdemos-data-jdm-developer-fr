<!-- Filename: J4.x:Triggering_content_plugins_in_your_extension / Display title: Déclenchement des plugins de contenu dans votre extension -->

Joomla!  4.x

Un exemple typique [d'utilisation de plug-ins dans votre
composant](https://docs.joomla.org/Supporting_plugins_in_your_component "Special:MyLanguage/Supporting plugins in your component")
est celui de **plugin de contenu** sur du texte. Ceci est utile si vous
voulez utiliser des plugins qui fonctionnent habituellement sur un
contenu depuis une extension personnalisée. Pour le déclenchement de la
préparation du contenu vous pouvez simplement appeler :

```php
    $text = \Joomla\CMS\HTML\HTMLHelper::_('content.prepare', $text);
```

Pour tout autre déclenchement de contenu, vous devez appeler :

```php
    // Ces trois instructions doivent être placées en haut de votre fichier, juste en dessous de l'instruction JEXEC.
    utiliser Joomla\CMS\CMSObject;
    utiliser Joomla\CMS\Factory;
    utiliser Joomla\CMS\Plugin\PluginHelper;

    // Ce code va dans la méthode de votre composant qui déclenche les plugins.
    $article = new \stdClass;
    $article->text = $text;

    // ajouter d'autres paramètres si nécessaire
    $params = new CMSObject;

    PluginHelper::importPlugin('content');
    $app = Factory::getApplication();
    $app->triggerEvent('onContentPrepare', array('some.context', &$article, &$params, 0));
```

Vous souhaiterez peut-être consulter les composants du noyau (par
exemple com_content) pour avoir un exemple. Lisez la [page des
déclenchements](https://docs.joomla.org/Plugin/Events "Special:MyLanguage/Plugin/Events")
pour les déclenchements possibles de plugins de contenus.
