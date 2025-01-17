<!-- Filename: J4.x:Web_Assets / Display title: Actifs Web -->

## À propos des ressources web

Il y a une description complète des ressources web dans la documentation des programmeurs Joomla!, reproduite sur [ce site](jdocmanual?article=docus/web-asset-manager/index) ou disponible depuis la [source originale](https://manual.joomla.org/docs/general-concepts/web-asset-manager/).

## Exemples supplémentaires

Dans Jdocmanual, de nombreux articles contiennent des blocs de code qui bénéficient de la coloration syntaxique. Voici comment cela est accompli :

```php
/** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
$wa = $this->document->getWebAssetManager();

// Register and attach a custom item in one run
$wa->registerAndUseStyle('highlight', 'https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/styles/default.min.css', [], [], []);

// Register and attach a custom item in one run
$wa->registerAndUseScript('highlight','https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/highlight.min.js', [], [], ['core']);

// Add an inline content as usual, will be rendered in flow after all assets
$wa->addInlineScript('hljs.highlightAll();');
```

Ce code se trouve dans le fichier `default.php` qui est utilisé pour afficher cette page ! Sa mise en œuvre est un peu délicate car l'appel `hljs.highlightAll()` doit être fait après le chargement des fichiers `css` et `js` et à nouveau chaque fois que le contenu de la page est modifié avec un appel JavaScript.

D'autres surligneurs de syntaxe sont disponibles !

*Traduit par openai.com*

