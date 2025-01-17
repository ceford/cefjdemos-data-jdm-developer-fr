<!-- Filename: J4.x:SCSS_and_Sass / Display title: Tester le CSS et le JavaScript -->

## Introduction

Dans un site Joomla en production, Joomla fournit des fichiers CSS et JavaScript compressés (ceux avec `.min` dans le nom de fichier). Les serveurs enverront les versions gzippées si elles sont disponibles (celles se terminant par un suffixe `.gz`). Ces fichiers sont créés à partir de sources non compressées. Dans le cas des fichiers CSS, ils sont souvent créés en traitant des précurseurs SCSS. Pour tester du code qui inclut des CSS ou JavaScript nouveaux ou mis à jour, il est nécessaire de reconstruire les fichiers CSS ainsi que les versions compressées et réduites à partir des sources modifiées.

## Tester l'installation

À des fins de test, vous avez besoin d'un serveur de test avec Git, Node.js et Composer installés. Allez dans le dépôt [Joomla CMS](https://github.com/joomla/joomla-cms) sur GitHub et suivez les instructions sur *Comment obtenir une installation fonctionnelle à partir de la source* vers le bas du texte README.

À la fin de l'installation de Joomla, ne supprimez pas le Répertoire d'Installation ! Cela supprimerait également le répertoire `build` nécessaire pour reconstruire les fichiers CSS et JavaScript.

Les fichiers `.scss` principaux sont dans les répertoires suivants :

- media/templates/site/cassiopeia/scss
- media/templates/administrator/atum/scss

Le script de génération CSS, le compilateur SCSS et d'autres outils de construction similaires sont situés dans le répertoire `build/`. Le répertoire de construction est uniquement disponible à partir de la source Joomlaǃ, il n'est pas inclus dans une version officielle de Joomlaǃ.

Les instructions d'installation comprennent les éléments suivants :

```sh
git checkout 5.2-dev (or whatever branch you wish to work with)
composer install
npm ci
```

La commande `npm ci` est un synonyme de `npm clean-install` utilisée dans toute situation où vous souhaitez vous assurer de faire une installation propre de vos dépendances.

## Utilisation quotidienne

Il existe des commandes alternatives à utiliser dans les situations où seuls les fichiers SCSS ou JavaScript ont été modifiés :

```sh
npm run build:css¶
    This command compiles SCSS files to CSS and also creates the minified files.

npm run build:js¶
    This command compiles and transpiles the JavaScript files to the correct format and creates minified files.
```

Les commandes explorent les répertoires media et templates et construisent les versions finales des fichiers requises par une installation Joomla.

## Sass, SCSS et CSS

> Sass est un langage de feuille de style qui est compilé en CSS. Il vous permet d'utiliser des variables, des règles imbriquées, des mixins, des fonctions, et bien plus, avec une syntaxe entièrement compatible CSS. Sass aide à garder les grandes feuilles de style bien organisées et facilite le partage de design au sein et entre les projets. Les fichiers Sass ont le suffixe `scss`.

### CSS

Le code CSS s'exprime comme suitː

```css
#header {
    margin: 0;
    border: 1px solid blue;
}
#header p {
    font-size: 14px;
    color: blue;
}
#header a {
    text-decoration: none;
}
```

### SCSS

`SCSS` est une extension de la syntaxe de CSS. Il est utilisé dans le noyau de Joomlaǃ

```css
$color:    blue;
#header {
    margin: 0;
    border: 1px solid $color;
    p {
        color: $colorRed;
        font: {
            size: 14px;
        }
    }
    a {
        text-decoration: none;
    }
}
```

Une syntaxe `Sass` plus ancienne offre une manière plus concise d'écrire du CSS.

```css
$color:    blue
#header
    margin: 0
    border: 1px solid $color
        p
            color: $colorRed
            font:
                size: 14px
        a
            text-decoration: none
```

Vous pouvez trouver plus d'informations dans la [Documentation Sass](http://sass-lang.com/documentation/syntax/).

## Du CSS existant au SCSS

Si vous souhaitez personnaliser le modèle Cassiopeia, il est judicieux de copier le modèle et de le personnaliser par la suite afin que les mises à jour de Joomla! ne remplacent pas vos personnalisations. Pour ajouter vos propres fichiers CSS à une copie du modèle Cassiopeia, renommez simplement vos fichiers `.css` en `.scss`. Vous pouvez ensuite profiter des fonctionnalités offertes par SCSS :

- Extensions de langage telles que les variables, l’imbrication et les mixins
- De nombreuses fonctions utiles pour manipuler les couleurs et d'autres valeurs
- Fonctions avancées comme les directives de contrôle pour les bibliothèques
- Sortie bien formatée et personnalisable

Voir le [site Sass](https://sass-lang.com/) pour plus de détails.

### Importez vos fichiers .css sous forme de fichiers .scss

Supposons que vous souhaitiez inclure vos fichiers personnalisés et les faire analyser par le compilateur SCSS - vous n'avez PAS besoin de réécrire votre .css en SCSS car le `.css` simple fonctionne également. Ce que vous devez faire :

- renommez vos fichiers `.css` en `.scss`
- préfixez-les avec un `_`
- ajoutez une déclaration `@import` en bas de `media/templates/site/copy_of_cassiopeia/scss/template.scss` afin qu'elle remplace les déclarations existantes :

```css
// Bootstrap functions
@import "../../../media/vendor/bootstrap/scss/functions";
//...
// 50 or more lines of import statements
// Finally add the cassiopeia colours
:root {
  @each $color, $value in $cassiopeia-colors {
    --#{$color}: #{$value};
  }
// More lines containing scss color statements, then your own styles:
@import "mystyles";
}
```

Si vous compilez maintenant votre fichier principal template.scss, vous obtenez un fichier principal template.css optimisé et minifié. Vous avez également réduit vos requêtes HTTP et le site devrait se charger plus rapidement.

*Traduit par openai.com*

