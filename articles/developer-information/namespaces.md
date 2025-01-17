<!-- Filename: J4.x:Namespace_Conventions_In_Joomla / Display title: Espaces de noms -->

## Introduction

L'utilisation des **espaces de noms PHP** est un moyen de regrouper des classes, interfaces, fonctions et constantes connexes afin d'éviter les conflits de noms dans les grands projets. Ils permettent aux développeurs d'organiser le code de manière plus efficace, surtout lorsqu'ils travaillent avec des bibliothèques tierces ou de grandes bases de code qui pourraient avoir des noms de classes dupliqués.

Il y a plus d'informations dans la section [Espace de noms](jdocmanual?article=docus/namespaces/index) de la documentation des programmeurs Joomla!.

### Principales Caractéristiques des Espaces de Noms PHP

1. **Éviter les Conflits de Noms** :
   - Sans espaces de noms, si deux classes portent le même nom, PHP générera une erreur. Les espaces de noms fournissent un contexte pour identifier de manière unique les classes ou d'autres éléments.
2. **Organiser le Code** :
   - Les espaces de noms permettent un regroupement logique des classes ou fonctions connexes, rendant le code plus facile à lire et à maintenir.
3. **Accès via des Noms Complètement Qualifiés** :
   - Les classes et fonctions au sein d'un espace de noms peuvent être accédées en utilisant leur nom complètement qualifié, qui inclut le chemin de l'espace de noms.

### Pratiques courantes :
- Utilisez des espaces de noms pour refléter la structure des dossiers de votre projet pour plus de cohérence.
- Importez des espaces de noms en utilisant le mot-clé `use` pour un code plus clair.

## Le fichier manifeste

La déclaration initiale d'un espace de noms d'extension se trouve dans un fichier manifeste d'extension, comme dans cet exemple :

```xml
    <namespace path="src">Acme\Component\Wonderful</namespace>
```

Les éléments de cette déclaration sont les suivants :

- **path="src"** indique au chargeur de classes que les classes avec espace de noms se trouvent dans le dossier **src** de l'extension, par exemple com_wonderful/src.
- **Acme** est le préfixe du fournisseur. Pour les extensions de base de Joomla, cela serait *Joomla*. Vous devez inventer votre propre préfixe de fournisseur unique.
- **Component** indique le type d'extension (composant, module, plugin, template, bibliothèque, ...).
- **Wonderful** est le nom de l'extension. Choisissez un nom d'extension qui soit court, significatif et probablement unique.

## Le fichier de classe

La déclaration d'espace de noms doit être la première instruction du fichier dans lequel elle est utilisée. Par exemple :

```php
<?php

/**
 * @package     Wonderful
 * @subpackage  Site
 *
 * @copyright   (C) 2024 Merlin. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace Acme\Component\Wonderful\Controller;

use Joomla\CMS\MVC\Controller\BaseController;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

/**
 * Controller to display the default page - display of wonderful items
 *
 * @since  1.0.0
 */
class MagicController extends BaseController
{
    public function dosomething($wonderful)
    {
        // ... output something wonderful!
    }
}
```

## Utilisation du fichier de classe

Lorsqu'il est nécessaire d'utiliser la classe, par exemple dans un modèle, vous insérez une instruction **use** en haut du modèle :

```
use Acme\Component\Wonderful\Controller\MagicController;
```

Ensuite, instanciez la classe et appelez la fonction :

```
$magic = new MagicController;
$result = $magic->dosomething('wonderful');
```

## Le fichier autoload_psr4.php

Ce fichier est situé dans le dossier administrator/cache du site Joomla. Il contient une carte complète des espaces de noms extraits des fichiers manifeste. Il est généré lors de l'installation et régénéré chaque fois qu'une extension est installée ou réinstallée. Vous pouvez supprimer ce fichier et il sera régénéré lors du prochain chargement de page. Il est parfois nécessaire de faire cela si l'installation d'une extension a été tentée par une méthode inhabituelle. Le fichier contient 275 chemins ou plus vers les emplacements des classes avec espace de noms.

**PSR** est un acronyme pour [**Recommandation de Standard PHP**](https://www.php-fig.org/psr/) et [**PSR-4: Autoloader**](https://www.php-fig.org/psr/psr-4/) est une norme acceptée pour le chargement des classes PHP.

*Traduit par openai.com*

