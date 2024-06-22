<!-- Filename: J4.x:Dependency_Injection_in_Joomla_4 / Display title: Injection de dépendances dans Joomla 4 -->

Joomla!  4.0

Joomla 4 introduit la pratique d'injection de dépendance des conteneurs
(DIC's) dans Joomla. Cet article vise à expliquer pourquoi nous les
présentons et comment les utiliser dans Joomla.

## Introduction

Les DIC font partie de l'écosystème PHP depuis longtemps pour soutenir
les objectifs de l'injection de dépendance. Par exemple, Symfony <a
href="http://fabien.potencier.org/do-you-need-a-dependency-injection-container.html"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">a introduit le concept en 2009</a>.

Il existe diverses raisons pour lesquelles le moment est venu de les
intégrer à Joomla 4:

1.  **Test** - l'un des thèmes de Joomla 3 a eu des publications
    boguées. Nous devons pouvoir tester les classes et les composants de
    manière plus simple. L'injection de dépendance permet une injection
    beaucoup plus facile des classes factices, ce qui nous permet,
    espérons-le, de réduire le nombre de bogues.
2.  '*Réduisez la quantité de magie dans Joomla* - Joomla possède un
    grand nombre de fichiers magiques dont vous devez deviner les noms.
    Cela augmente le temps que les personnes qui découvrent Joomla
    doivent consacrer à la recherche de ces conventions. Exposer une
    classe concrète dans les extensions nous permet de tester facilement
    la compatibilité des extensions avec d'autres extensions (par
    exemple les catégories et les associations).

## Le conteneur global

L'injection de Dépendance Globale remplace très vaguement la classe
JFactory. Cependant, il ne devrait pas être confondu avec un
remplacement direct.

Note de mise à niveau

En règle générale, vous ne devez pas remplacer directement les appels
JFactory au conteneur. La plupart du temps, vous devez extraire l'objet
de l'application OU utiliser l'injection de dépendance dans votre
classe.

Ainsi, par exemple, dans vos contrôleurs du système de gestion de
contenu au lieu de remplacer par `\Joomla\CMS\Factory::getDocument()`
envisager d'utiliser `$this->app->getDocument()`. Ceci utilise
l'application injectée et permet donc un test plus facile.

### Créer un objet dans un conteneur

Pour placer quelque chose dans le DIC Global, le moyen le plus simple
consiste à transmettre une fonction anonyme. Un exemple pour un
journaliseur est ci-dessous

    // Assuming we have an instance of a Joomla Container
    $container->share(
        LoggerInterface::class,
        function (Container $container)
        {
            return \Joomla\CMS\Log\Log::createDelegatedLogger();
        },
        true
    );

La fonction de partage prend deux paramètres obligatoires et un
troisième paramètre facultatif.

- Un nom pour le service qui est presque toujours le nom de la classe
  que vous créez.
- Une fonction anonyme qui prend un seul paramètre - l'instance de
  conteneur (cela vous permet de récupérer toutes les dépendances hors
  du conteneur). Le retour est le service que vous souhaitez placer dans
  le conteneur.
- (facultatif) Ce booléen contrôle si le service est protégé
  (c'est-à-dire si quelqu'un d'autre est autorisé à le remplacer dans le
  conteneur). En règle générale, cela est vrai pour les services de base
  Joomla tels que les objets de session.

Prenons maintenant un exemple plus compliqué :

    $container->alias('AmazingApiRouter', Joomla\CMS\Router\ApiRouter::class)
        ->share(
        \Joomla\CMS\Router\ApiRouter::class,
        function (Container $container)
        {
            return new \Joomla\CMS\Router\ApiRouter($container->get(\Joomla\CMS\Application\ApiApplication::class));
        },
        true
    );

Ici, vous pouvez voir que nous avons fait deux choses supplémentaires:
nous avons commencé à utiliser des dépendances (le routeur api extrait
l'application api du conteneur) et nous avons également créé un alias
pour ApiRouter. Cela signifie que le conteneur reconnaît que s'il veut
créer une instance ApiRouter, il peut le faire. Mais dans notre code,
pour garder les choses simples, nous pouvons également exécuter
`Factory::getContainer()->get('AmazingApiRouter')` pour récupérer notre
routeur.

Tandis que dans Joomla, nos fournisseurs peuvent paraître plus
compliqués que cela, car la logique pour créer des objets dans la
fonction anonyme est plus compliquée - ils suivent tous cette idée de
base.

### Fournisseurs

Les fournisseurs dans Joomla sont un moyen d’enregistrer une dépendance
dans un conteneur de services. Pour ce faire, créez une classe qui
implémente `Joomla\DI\ServiceProviderInterface`. Cela vous donne une
méthode de registre qui contient le conteneur. Vous pouvez ensuite
utiliser à nouveau la méthode de partage pour ajouter un nombre
quelconque d'objets dans le conteneur. Vous pouvez ensuite l'enregistrer
dans le conteneur avec la méthode
`\Joomla\DI\Container::registerServiceProvider` dans le conteneur. Vous
pouvez voir où nous enregistrons tous les principaux fournisseurs de
services <a
href="https://github.com/joomla/joomla-cms/blob/4.0-dev/libraries/src/Factory.php#L570-L594"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">ici dans la méthode <code
class="mw-highlight mw-highlight-lang-php"
dir="ltr">\Joomla\CMS\Factory::createContainer</code></a>

## Conteneurs de composants

Chaque composant a également son propre conteneur (qui se trouve dans la
section administrateur de Joomla). Cependant ce conteneur n'est pas
exposé. Il est juste là pour obtenir les dépendances du système et
permettre à une classe de représenter votre extension. Cette classe est
la classe Extension et doit au minimum implémenter l'interface de type
d'extensions appropriée. Par exemple, un composant doit implémenter
l'interface composant `\Joomla\CMS\Extension\ComponentInterface`
(disponible à l'adresse <a
href="https://github.com/joomla/joomla-cms/blob/4.0-dev/librarieshttps://docs.joomla.org/J4.x:Developing_an_MVC_Component"
class="external text" target="_blank" rel="nofollow noreferrer noopener"
title="Special:MyLanguage/J4.x:Developing an MVC Component">Développement
d’un composant MVC</a>

### Utilisation d'un conteneur de composants dans une autre extension

Vous pouvez facilement récupérer le conteneur d'une autre extension via
l'objet CMSApplication. Par exemple

    Factory::getApplication()->bootComponent('com_content')->getMVCFactory()->createModel('Articles', 'Site');

Obtiendra le conteneur com_content, la MVC Factory et le module
ArticlesModel à partir de l’interface de Joomla. Et cela fonctionnera
dans n’importe quelle extension dans le site, l'administration ou dans
l’API de Joomla (contrairement à l’ancienne méthode
LegacyModel::getInstance())

## En savoir plus

La documentation Joomla Framework illustre parfaitement l’utilité de
l’injection de dépendance pour votre application et la structure à
l’aide de DIC. <a
href="https://github.com/joomla-framework/di/blob/2.0-dev/docs/why-dependency-injection.md"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">Lisez-le ici</a>
