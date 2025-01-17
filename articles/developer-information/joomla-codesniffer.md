<!-- Filename: Joomla_CodeSniffer / Display title: Normes de codage -->

<div class="alert alert-warning">
La dernière partie de cet article doit être mise à jour !
</div>

## Un aperçu de l'IA

Les normes de codage sont importantes pour le développement de logiciels car elles :

- **Améliorer la qualité du code**
  - Les normes de codage aident à garantir que le code est fiable, sécurisé et sûr. Elles peuvent également aider à réduire les problèmes de performance et les préoccupations de sécurité qui pourraient découler de mauvaises pratiques de codage.
- **Rendre le code plus lisible et maintenable**
  - Les normes de codage aident à rendre le code plus facile à comprendre, à lire et à maintenir. Cela peut aussi faciliter le travail des nouveaux développeurs avec le code.
- **Accélérer le développement**
  - Les normes de codage peuvent aider les développeurs à éviter les erreurs courantes qui peuvent ralentir les processus de codage.
- **Améliorer la collaboration**
  - Les normes de codage peuvent faciliter la collaboration entre développeurs, même dans de grandes équipes.
- **Assurer la cohérence**
  - Les normes de codage aident à garantir que le code est cohérent à travers les projets. Cela peut faciliter le travail des développeurs ensemble sur les mêmes projets.
- **Améliorer l'évolutivité**
  - Les normes de codage peuvent aider à s'assurer que le code peut évoluer sans devenir ingérable.
- **Fournir des critères clairs pour les revues de code**
  - Les normes de codage peuvent aider à fournir des critères clairs pour les revues de code, ce qui peut conduire à des retours plus efficaces.

Les normes de codage incluent souvent des règles pour l'indentation, la longueur des lignes, le placement des accolades et l'espacement.

Joomla utilise la norme de codage [PSR-12](https://www.php-fig.org/psr/psr-12/). Vous pouvez activer cette norme de codage dans votre IDE et obtenir des indications si vous ne suivez pas la norme de codage ou utilisez un correcteur automatique également. Nous vous recommandons de suivre cette norme lors du développement de vos propres extensions pour rester compatible avec le cœur et pour vous assurer que votre code fonctionnera, espérons-le, avec les futures versions.

## PHP Code Sniffer

Veuillez consulter la source actuelle de [PHP_CodeSniffer](https://github.com/PHPCSStandards/PHP_CodeSniffer/) pour obtenir des informations sur cet utilitaire et comment l'installer. Soyez conscient que la source originale est abandonnée mais qu'elle peut être listée par les moteurs de recherche. Il y a deux scripts :

- **phpcs** détecte les violations d'une norme de codage et produit un rapport.
- **phpcbf** tente de corriger les violations des normes de codage et produit un rapport sur ce qu'il a fait.

Vous pouvez installer les scripts individuels ou utiliser composer pour les installer. Vous pouvez également les trouver dans un clone de Joomla situé dans libraries/vendor/bin ainsi qu'avec d'autres utilitaires utiles.

## Tester PHP Code Sniffer

Si vous placez une installation globale dans votre `$PATH`, vous pouvez exécuter le détecteur de code depuis la ligne de commande à la racine d'un projet. Essayez ceci pour afficher une liste de normes de codage :

```sh
phpcs -i
The installed coding standards are MySource, PEAR, PSR1, PSR2, PSR12, Squiz and Zend
```

À titre d'exemple, la commande suivante a été utilisée dans un ancien dossier de projet qui utilisait l'ancienne norme de codage personnalisée Joomla. Entre autres, elle utilisait des tabulations plutôt que des espaces pour la mise en page. De nombreuses lignes similaires ont été omises ci-dessous pour des raisons de concision.

```sh
phpcs --standard=PSR12 .

FILE: /Users/ceford/git/j4xdemos-plg-toc/j4xdemostoc/j4xdemostoc.php
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
FOUND 132 ERRORS AND 3 WARNINGS AFFECTING 116 LINES
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   1 | WARNING | [ ] A file should declare new symbols (classes, functions, constants, etc.) and cause no other side effects, or it should execute logic with side effects, but should not do both. The
     |         |     first symbol is defined on line 22 and the first side effect is on line 10.
   1 | ERROR   | [x] Header blocks must be separated by a single blank line
  22 | ERROR   | [ ] Each class must be in a namespace of at least one level (a top-level vendor name)
  24 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
  25 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
  26 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
  ...
  42 | ERROR   | [x] Expected 1 space after closing parenthesis; found newline
...
  48 | ERROR   | [x] No space found after comma in argument list
  48 | ERROR   | [x] Expected 1 space after closing parenthesis; found newline
...
  67 | ERROR   | [x] Expected 0 spaces before closing parenthesis; 1 found
...
  75 | ERROR   | [x] Space after opening parenthesis of function call prohibited
  75 | ERROR   | [x] Expected 0 spaces before closing parenthesis; 1 found
...
 138 | WARNING | [ ] Line exceeds 120 characters; contains 168 characters
 139 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
 139 | WARNING | [ ] Line exceeds 120 characters; contains 141 characters
...
 156 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
 157 | ERROR   | [x] Expected 1 newline at end of file; 0 found
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
PHPCBF CAN FIX THE 131 MARKED SNIFF VIOLATIONS AUTOMATICALLY
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Time: 123ms; Memory: 8MB
```

L'exemple de projet précédent contenait un seul fichier PHP et aucun fichier CSS ou JS. phpcs produit un rapport pour chaque fichier PHP, CSS et JS dans un projet. Voici quelques exemples pour les fichiers CSS et JS :

```sh
FILE: /Users/ceford/git/j4x-demos-com-mediacat/com_mediacat/media/css/mediacat.css
----------------------------------------------------------------------------------
FOUND 33 ERRORS AFFECTING 32 LINES
----------------------------------------------------------------------------------
  3 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
...
 51 | ERROR | [x] Whitespace found at end of line
...
 79 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
----------------------------------------------------------------------------------
PHPCBF CAN FIX THE 33 MARKED SNIFF VIOLATIONS AUTOMATICALLY
----------------------------------------------------------------------------------

FILE: /Users/ceford/git/j4x-demos-com-mediacat/com_mediacat/media/css/file-icon-classic.min.css
-----------------------------------------------------------------------------------------------
FOUND 0 ERRORS AND 1 WARNING AFFECTING 1 LINE
-----------------------------------------------------------------------------------------------
 1 | WARNING | File appears to be minified and cannot be processed
-----------------------------------------------------------------------------------------------

FILE: /Users/ceford/git/j4x-demos-com-mediacat/com_mediacat/media/js/mediacat-site.js
-------------------------------------------------------------------------------------
FOUND 38 ERRORS AFFECTING 30 LINES
-------------------------------------------------------------------------------------
  3 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
  5 | ERROR | [x] Expected 1 space after FUNCTION keyword; 0 found
  6 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
...
 13 | ERROR | [x] Opening brace should be on a new line
...
 29 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
 29 | ERROR | [x] Expected at least 1 space before "+"; 0 found
 29 | ERROR | [x] Expected at least 1 space after "+"; 0 found
...
 36 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
-------------------------------------------------------------------------------------
PHPCBF CAN FIX THE 38 MARKED SNIFF VIOLATIONS AUTOMATICALLY
-------------------------------------------------------------------------------------
```

## Variations de Commande

Vous pouvez obtenir de l'aide avec les commandes phpcs :

```sh
phpcs --help
```

### Exclure un ou plusieurs fichiers

Utilisez une liste de motifs de fichiers séparés par des virgules pour exclure les fichiers de la validation du style de code. Exemple

```php
phpcs --standard=PSR12 --ignore='libraries/*' .
```

### Exclure une ou plusieurs règles

Joomla permet des lignes plus longues que la norme PSR, 560 au lieu de 120. La commande suivante peut être utilisée pour omettre les avertissements de lignes longues :

```sh
phpcs --standard=PSR12 --ignore='libraries/*' --exclude=Generic.Files.LineLength .
```

Vous pouvez trouver la règle qui est enfreinte avec cette commande :

```sh
phpcs -s yourfile.php
```

### Exceptions Joomla

Pour le développement d'une extension en utilisant un IDE, vous pouvez décider d'utiliser le standard PSR12 sans aucune exception. Joomla dispose d'un [ensemble de règles personnalisé](https://github.com/joomla/joomla-cms/blob/5.2-dev/ruleset.xml) qui permet de nombreuses exceptions. Il est utilisé pour la validation de l'ensemble de l'installation de Joomla lors des tests système.

## Correction des infractions

Le fichier php unique qui `A DÉTECTÉ 132 ERREURS ET 3 AVERTISSEMENTS AFFECTANT 116 LIGNES` montré ci-dessus peut être principalement corrigé comme suit :

```sh
phpcbf --standard=PSR12 .

PHPCBF RESULT SUMMARY
-----------------------------------------------------------------------------------
FILE                                                               FIXED  REMAINING
-----------------------------------------------------------------------------------
/Users/ceford/git/j4xdemos-plg-toc/j4xdemostoc/j4xdemostoc.php     131    4
-----------------------------------------------------------------------------------
A TOTAL OF 131 ERRORS WERE FIXED IN 1 FILE
-----------------------------------------------------------------------------------

Time: 232ms; Memory: 8MB
```

Exécuter à nouveau l'utilitaire phpcs montre quels problèmes subsistent :

```sh
phpcs --standard=PSR12 .

FILE: /Users/ceford/git/j4xdemos-plg-toc/j4xdemostoc/j4xdemostoc.php
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
FOUND 1 ERROR AND 3 WARNINGS AFFECTING 4 LINES
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   1 | WARNING | A file should declare new symbols (classes, functions, constants, etc.) and cause no other side effects, or it should execute logic with side effects, but should not do both. The first
     |         | symbol is defined on line 23 and the first side effect is on line 11.
  23 | ERROR   | Each class must be in a namespace of at least one level (a top-level vendor name)
 132 | WARNING | Line exceeds 120 characters; contains 168 characters
 133 | WARNING | Line exceeds 120 characters; contains 141 characters
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Time: 127ms; Memory: 8MB
```

Conclusion : une extension codée pour une version antérieure de Joomla et utilisant un standard de codage différent nécessite maintenant quelques ajustements !

## Installation dans les IDE

La plupart des environnements de développement intégrés peuvent utiliser le détecteur de code pendant que vous travaillez ou lorsque vous enregistrez un fichier.

### Installation dans VSCode

Dans VSCode (et/ou VScodium), sélectionnez l'icône Extensions, recherchez PHP_CodeSniffer et installez-le. Il nécessite une configuration :

1. Dans **Paramètres**, trouvez PHP_CodeSniffer
2. Réglez le champ **PHP Code Sniffer → Exécutable :** pour correspondre à l'emplacement de votre binaire phpcs.
3. Dans la liste **PHP Code Sniffer: Standard**, sélectionnez **PSR12**.

Fermez et vous êtes prêt à agir.

Certains des problèmes montrés ci-dessus peuvent être corrigés avec les directives PSR1.

```php
<?php

/**
 * @package     Whatever
 *
 * @phpcs:disable PSR1.Classes.ClassDeclaration.MissingNamespace
 */

use joomla\CMS\...

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects
```

D'autres nécessitent une réflexion!

Après l'installation dans VSCode, les violations du style de code seront détectées et marquées d'un soulignement ondulé rouge. Survolez le problème pour voir un message et une solution potentielle, comme dans cet exemple :

```txt
Header blocks must be separated by a single blank line
PHP_CodeSniffer(PSR12.Files.FileHeader.SpacingAfterBlock)
```

<div class="alert alert-warning">
Les sections suivantes doivent être mises à jour !
</div>

### Installation dans PhpStorm

Code Sniffer est pris en charge par défaut dans PhpStorm. Allez dans Paramètres et sous **Éditeur → Inspections** vous verrez la liste des sniffers que vous avez installés.

#### Définir le chemin vers le vérificateur de code

1.  Ouvrir les Paramètres (CTRL-ALT-S / CMD-,)
2.  Aller à Langues et Cadres
3.  Cliquer sur PHP
4.  Cliquer sur Outils de Qualité
5.  Cliquer sur la flèche déroulante de PHP cs fixer
6.  La configuration est réglée sur Local par défaut
7.  Cliquer sur les 3 points derrière pour ouvrir l'écran de configuration
8.  La première option est le chemin du PHP Code Sniffer (phpcs)
9.  Cliquer sur les 3 points derrière le chemin pour sélectionner l'emplacement du fichier phpcs. Voir ci-dessus où phpcs peut être installé sur votre site
10. Cliquer sur Valider pour s'assurer que le chemin est correct et que phpcs fonctionne
11. Cliquer sur OK

#### Activer le style de code

1.  Ouvrir Paramètres (CTRL-ALT-S / CMD-,)
2.  Aller à Éditeur
3.  Cliquer sur Inspections
4.  Dans la liste, aller à PHP
5.  Cliquer sur Validation PHP Code Sniffer
6.  Cliquer sur la case à cocher derrière pour l'activer
7.  Cliquer sur le bouton Recharger (2 flèches) pour forcer un rechargement des règles depuis le disque
8.  Joomla devrait maintenant être disponible dans la liste. Voir l'image suivante.
9.  Cliquer sur OK

![CodeSniffer in PHPStorm](../../../en/images/getting-started/core-phpstorm-code-sniffer.png)

### Installation dans Netbeans

Netbeans a la fonctionnalité sniffer intégrée dans le système central.

1. Lancez votre IDE Netbeans
2. Ouvrez **Outils → Options → PHP → Analyse de Code → Code Sniffer**
3. Vous devez définir le chemin vers *phpcs.bat* à partir du package PEAR PHP_CodeSniffer installé
    - Pour les systèmes basés sur Unix, le chemin est quelque chose comme /usr/bin/phpcs
    - Dans XAMPP (Windows), vous pouvez trouver le fichier dans le dossier racine PHP (par ex. C:\xampp\php\phpcs.bat)
4. Comme "Standard par Défaut", choisissez "Joomla" pour utiliser le standard Joomla!
5. Maintenant, vous pouvez cliquer sur *OK* pour commencer l'analyse
6. Ouvrez depuis le menu du haut **Source → Inspecter...**
7. Profitez-en

### Installation dans Eclipse

1. **Aide → Installer un nouveau logiciel...**
2. **Travailler avec** Remplissez l'une des URL du site de mise à jour.
3. Sélectionnez les outils désirés.
4. Redémarrez Eclipse.
5. **Fenêtre → Préférences**
6. **Outils PHP → PHP CodeSniffer**

![Eclipse PTI settings](../../../en/images/getting-started/core-eclipse-pti-settings.png)

Vous êtes maintenant capable de détecter les violations de code par rapport aux normes courantes.

![Codesniffer in Eclipse](../../../en/images/getting-started/core-eclipse-pti.png)

### Installation dans Geany

- Ouvrez un fichier PHP. (Sinon, le menu de construction n'est pas accessible.)
- Dans le menu supérieur, sélectionnez **Build → Set Build Commands**.
- Sélectionnez le deuxième champ et nommez-le comme vous le souhaitez. Entrez ce code dans la commande :<br>
  `phpcs --standard=Joomla "%f" | sed -e 's/^/%f |/' | egrep 'WARNING|ERROR'`
- Entrez ce code dans le champ d'expression régulière d'erreur : `(.+) [|]\s+([0-9]+)`
- Sélectionnez OK.
- Si la fenêtre de message n'est pas ouverte, affichez-la en la sélectionnant dans le menu supérieur Affichage.
- Lors de la visualisation de tout fichier PHP, appuyez sur F9 pour voir les erreurs trouvées.

### Installation dans Atom

- Installez phpcs et les standards Joomla comme décrit ci-dessus.
- Dans **Atom → Préférences → Installer**, installez **linter-phpcs** et toutes ses exigences.
- Dans **Atom → Préférences → Packages → linter-phpcs → Paramètres**, ajustez
  - **Chemin exécutable** au chemin de votre phpcs
  - **Norme de code ou fichier de configuration** : PSR12
  - **Largeur de tabulation** : 4

*Traduit par openai.com*

