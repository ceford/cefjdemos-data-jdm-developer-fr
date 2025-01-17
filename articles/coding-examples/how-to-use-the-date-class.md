<!-- Filename: How_to_use_JDate / Display title: Comment utiliser la classe Date -->

## Introduction
La classe Date de Joomla est une classe d'assistance, dérivée de la classe DateTime de PHP, qui permet aux développeurs de gérer le formatage des dates de manière plus efficace. La classe permet aux développeurs de formater les dates pour des chaînes lisibles, l'interaction avec MySQL, le calcul des timestamps UNIX, et fournit également des méthodes d'assistance pour travailler dans différents fuseaux horaires.

## Création d'une instance de date

Toutes les méthodes d'assistance de date nécessitent une instance de la classe Date. Pour commencer, vous devez en créer une. Un objet Date peut être créé de deux manières. L'une d'elles est la méthode native typique consistant simplement à créer une nouvelle instance :

```php
use Joomla\CMS\Date\Date;

$date = new Date(); // Creates a new Date object equal to the current time.
```

Vous pouvez également créer une instance en utilisant la méthode statique définie dans Date :

```php
use Joomla\CMS\Date\Date;

$date = Date::getInstance(); // Alias of 'new Date();'
```

Il n'y a pas de différence entre ces méthodes, car Date::getInstance crée simplement une nouvelle instance de Date exactement comme la première méthode présentée.

Alternativement, vous pouvez également récupérer la date actuelle (en tant qu'objet Date) à partir de l'objet Application, en utilisant :

```php
use Joomla\CMS\Factory;

$date = Factory::getDate();
```

## Arguments

Le constructeur Date (et la méthode statique getInstance) accepte deux paramètres optionnels : une chaîne de date à formater et un fuseau horaire. Ne pas passer de chaîne de date créera un objet Date avec la date et l'heure actuelles, tandis que ne pas passer de fuseau horaire permettra à l'objet Date d'utiliser le fuseau horaire par défaut défini.

Le premier argument, si utilisé, doit être une chaîne qui peut être analysée en utilisant le constructeur DateTime natif de PHP. Par exemple :```php
use Joomla\CMS\Date\Date;

$currentTime = new Date('now'); // Current date and time
$tomorrowTime = new Date('now +1 day'); // Current date and time, + 1 day.
$plus1MonthTime = new Date('now +1 month'); // Current date and time, + 1 month.
$plus1YearTime = new Date('now +1 year'); // Current date and time, + 1 year.
$plus1YearAnd1MonthTime = new Date('now +1 year +1 month'); // Current date and time, + 1 year and 1 month.
$plusTimeToTime = new Date('now +1 hour +30 minutes +3 seconds'); // Current date and time, + 1 hour, 30 minutes and 3 seconds
$plusTimeToTime = new Date('now -1 hour +30 minutes +3 seconds'); // Current date and time, + 1 hour, 30 minutes and 3 seconds
$combinedTimeToTime = new Date('now -1 hour -30 minutes 23 seconds'); // Current date and time, - 1 hour, +30 minutes and +23 seconds

$date = new Date('2012-12-1 15:20:00'); // 3:20 PM, December 1st, 2012
```

Un horodatage Unix (en secondes) peut également être passé comme premier argument, auquel cas il sera converti en interne en une date. Si un fuseau horaire a été spécifié comme deuxième argument du constructeur, il sera converti dans ce fuseau horaire.

## Affichage des dates

Une note de prudence lors de l'affichage des objets Date dans un contexte utilisateur : ne vous contentez pas de les imprimer à l'écran. La méthode toString() de l'objet Date appelle simplement la méthode format() de son parent, sans tenir compte des fuseaux horaires ou du formatage localisé de la date. Cela ne donnera pas une bonne expérience utilisateur et entraînera des incohérences entre le formatage dans votre extension et ailleurs dans Joomla. Au lieu de cela, vous devriez toujours afficher les Dates en utilisant les méthodes montrées ci-dessous.

### Formats de date courants

Un certain nombre de formats de date sont prédéfinis dans Joomla dans le cadre des packs de langue de base. Ceci est bénéfique car cela signifie que les formats de date peuvent être facilement internationalisés. Un échantillon des chaînes de format disponibles est ci-dessous, extrait du pack de langue en-GB. Il est fortement recommandé d'utiliser ces chaînes de format lors de l'affichage des dates, afin que vos dates soient automatiquement reformatées en fonction de la locale de l'utilisateur. Elles peuvent être récupérées de la même manière que toute chaîne de langue (voir ci-dessous pour des exemples).

```ini
DATE_FORMAT_LC="l, d F Y"
DATE_FORMAT_LC1="l, d F Y"
DATE_FORMAT_LC2="l, d F Y H:i"
DATE_FORMAT_LC3="d F Y"
DATE_FORMAT_LC4="Y-m-d"
DATE_FORMAT_LC5="Y-m-d H:i"
DATE_FORMAT_LC6="Y-m-d H:i:s"
DATE_FORMAT_JS1="y-m-d"
DATE_FORMAT_CALENDAR_DATE="%Y-%m-%d"
DATE_FORMAT_CALENDAR_DATETIME="%Y-%m-%d %H:%M:%S"
DATE_FORMAT_FILTER_DATE="Y-m-d"
DATE_FORMAT_FILTER_DATETIME="Y-m-d H:i:s"
```

### La méthode HtmlHelper (Recommandée)

Comme pour de nombreux éléments de sortie courants, la classe HtmlHelper est là pour... aider ! La méthode date() de HtmlHelper acceptera toute chaîne de date-heure que le constructeur Date accepterait, ainsi qu'une chaîne de formatage, et affichera la date de manière appropriée selon les paramètres de fuseau horaire de l'utilisateur actuel. En tant que tel, c'est la méthode recommandée pour afficher les dates pour l'utilisateur.

```php
use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;

$myDateString = '2012-12-1 15:20:00';
echo HtmlHelper::date($myDateString, Text::_('DATE_FORMAT_FILTER_DATETIME'));
```

### La méthode format() de l'objet Date

Une autre option consiste à formater la date manuellement. Si cette méthode est utilisée, vous devrez également récupérer et définir manuellement le fuseau horaire de l'utilisateur. Cette méthode est plus utile pour formater des dates en dehors de l'interface utilisateur, comme dans les journaux système ou les appels d'API.

```php
use Joomla\CMS\Language\Text;
use Joomla\CMS\Date\Date;
use Joomla\CMS\Factory;

$myDateString = '2012-12-1 15:20:00';
$timezone = Factory::getUser()->getTimezone();

$date = new Date($myDateString);
$date->setTimezone($timezone);
echo $date->format(Text::_('DATE_FORMAT_FILTER_DATETIME'));
```

## Autres exemples de code utiles

### Afficher rapidement l'heure actuelle

Il existe deux moyens simples de le faire.
- La méthode date() de HtmlHelper, si aucune valeur de date n'est fournie, utilisera par défaut l'heure actuelle.
- Factory::getDate() obtient la date actuelle sous forme d'objet Date, que nous pouvons ensuite formater.

```php
use Joomla\CMS\Factory;
use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;

// These two are functionally equivalent
echo HtmlHelper::date('now', Text::_('DATE_FORMAT_FILTER_DATETIME'));

$timezone = Factory::getUser()->getTimezone();
echo Factory::getDate()->setTimezone($timezone)->format(Text::_('DATE_FORMAT_FILTER_DATETIME'));
```

### Ajouter et soustraire des dates

Étant donné que l'objet Joomla Date étend l'objet PHP DateTime, il offre des méthodes pour ajouter et soustraire des dates. La plus simple de ces méthodes à utiliser est la méthode modify(), qui accepte toute chaîne de modification relative que la méthode PHP [strtotime](https://www.php.net/manual/en/function.strtotime.php) accepterait. Par exemple :

```php
use Joomla\CMS\Date\Date;

$date = new Date('2012-12-1 15:20:00');
$date->modify('+1 year');
echo $date->toSQL(); // 2013-12-01 15:20:00
```

Il existe également des méthodes séparées add() et sub(), pour ajouter ou soustraire du temps à partir d'un objet date respectivement. Celles-ci acceptent des objets [DateInterval](https://www.php.net/manual/fr/class.dateinterval.php) standard PHP :

```php
use Joomla\CMS\Date\Date;

$interval = new \DateInterval('P1Y1D'); // Interval represents 1 year and 1 day

$date1 = new Date('2012-12-1 15:20:00');
$date1->add($interval);
echo $date1->toSQL(); // 2013-12-02 15:20:00

$date2 = new Date('2012-12-1 15:20:00');
$date2->sub($interval);
echo $date2->toSQL(); // 2011-11-30 15:20:00
```

### Affichage des dates au format ISO 8601

```php
$date = new Date('2012-12-1 15:20:00');
$date->toISO8601(); // 20121201T152000Z
```

### Affichage des dates au format RFC 822

```php
$date = new Date('2012-12-1 15:20:00');
$date->toRFC822(); // Sat, 01 Dec 2012 15:20:00 +0000
```

### Production de dates au format date-heure SQL

```php
$date = new Date('20121201T152000Z');
$date->toSQL(); // 2012-12-01 15:20:00
```

### Affichage des dates en tant que timestamps Unix

Un horodatage Unix est exprimé en tant que nombre de secondes écoulées depuis l'époque Unix (la première seconde du 1er janvier 1970).

*Traduit par openai.com*
