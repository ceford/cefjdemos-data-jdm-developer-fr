<!-- Filename: J4.x:Joomla_4_Tips_and_Tricks:_Form_Validation_Basics / Display title: Validation de formulaire -->

## Introduction

Joomla dispose d'un script de validation côté client qui peut être utilisé et étendu par tout composant. Cet article explique comment il fonctionne et comment l'utiliser. Il existe quatre validateurs standard pour les types de champs nom d'utilisateur, mot de passe, numérique et email. Il y a aussi un validateur de motif à usage plus général et un validateur de champ requis.

Notez que les navigateurs modernes fournissent également une validation de formulaire par défaut. La validation par navigateur peut être désactivée en incluant novalidate dans la balise form.

## Comment invoquer la validation

En haut du fichier edit.php contenant le code de génération de formulaire, incluez l'appel pour charger le fichier javascript du validateur :

```php
/** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
$wa = $this->document->getWebAssetManager();
$wa->useScript('keepalive')
    ->useScript('form.validate');
```

dans la balise form, assurez-vous d'inclure class="form-validate". Cet exemple provient du formulaire de modification de l'utilisateur :

```html
<form action="<?php echo Route::_('index.php?option=com_users&layout=edit&id=' . (int) $this->item->id); ?>"
    method="post" name="adminForm" id="user-form"
    enctype="multipart/form-data"
    aria-label="<?php echo Text::_('COM_USERS_USER_FORM_' . ( (int) $this->item->id === 0 ? 'NEW' : 'EDIT'), true); ?>"
    class="form-validate">
```

Dans le fichier de formulaire XML, incluez les instructions qui invoquent la validation individuelle des champs. Cet exemple concerne le champ de l'email utilisateur :

```xml
        <field
            name="email"
            type="email"
            label="JGLOBAL_EMAIL"
            required="true"
            size="30"
            validate="email"
            validDomains="com_users.domains"
        />
```

## Les expressions de validation

Cette section a pour but de donner une petite compréhension des expressions de validation pour aider à expliquer l'utilisation. Toutes sont incluses dans le code de validator.js.

### Nom d'utilisateur

```php
this.setHandler('username', value => {
      const regex = new RegExp('[<|>|"|\'|%|;|(|)|&]', 'i');
      return !regex.test(value);
    });
```

Ici, l'expression recherche la présence de l'un des caractères alternatifs dans la classe de caractères [] et renvoie false (non valide) si l'un d'eux est trouvé.

### Mot de passe

```pnp
this.setHandler('password', value => {
      const regex = /^\S[\S ]{2,98}\S$/;
      return regex.test(value);
    });
```

Ici, l'expression recherche un caractère initial qui n'est pas un espace suivi de 2 à 98 caractères qui ne sont pas un espace ou un espace et se terminant par un caractère non blanc. Joomla dispose d'un vérificateur de mot de passe plus sophistiqué qui garantit un mélange de types de caractères et une longueur minimale.

### Courriel

```php
this.setHandler('email', value => {
      const newValue = punycode.toASCII(value);
      const regex = /^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/;
      return regex.test(newValue);
    }); // Attach all forms with a class 'form-validate'
```

Ici, l'expression régulière accepte toute chaîne de caractères qui commence par un ou plusieurs des caractères dans la classe de caractères [], suivis de @, suivis d'un ou de plusieurs caractères dans la deuxième classe de caractères [], suivis de zéro ou plusieurs points pleins suivis d'un ou plusieurs caractères dans le troisième groupe de classe de caractères, et se terminant par l'un des groupes après le @.

Bien que techniquement correct, ce validateur permet certaines adresses e-mail assez absurdes comme !@me - vous pourriez vouloir créer un modèle plus restrictif.

### Numérique

```php
this.setHandler('numeric', value => {
      const regex = /^(\d|-)?(\d|,)*\.?\d*$/;
      return regex.test(value);
    });
```

Ici, la valeur doit commencer par un chiffre ou un signe moins zéro ou une fois, être suivie d'un chiffre ou d'une virgule zéro ou plusieurs fois, suivie d'un point final zéro ou une fois, se terminant par un chiffre zéro ou plusieurs fois. Cela correspondra donc à 3,142 ou -10 ou 100 000,0 et ainsi de suite. Le nombre réel doit correspondre au type de champ dans la base de données. L'expression ne correspondra pas aux nombres avec des exposants, donc 10e2 sera rejeté.

### Motif

Supposez que vous souhaitez qu'un champ de saisie soit un entier positif. Vous pouvez utiliser votre propre modèle placé dans le fichier XML du formulaire :

```xml
        <field
            name="number"
            type="text"
            label="COM_MYCOMPONENT_CAMP_NUMBER_LABEL"
            description="COM_MYCOMPONENT_CAMP_NUMBER_DESC"
            class="w-auto"
            required="true"
            pattern="\d+"
        />
```

Ici, le modèle permet un ou plusieurs chiffres. Ainsi, -1 serait invalide, tout comme 3,142.

### Requis

Assurez-vous d'inclure required="true" dans la spécification du champ, sinon l'étiquette du formulaire omettra le marqueur * couramment utilisé pour indiquer les champs obligatoires. Inclure required dans la classe n'est pas suffisant pour la validation.

*Traduit par openai.com*

