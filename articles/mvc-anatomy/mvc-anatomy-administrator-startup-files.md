<!-- Filename: J4.x:MVC_Anatomy:_Administrator_Startup_Files / Display title: MVC Anatomie : Fichiers de démarrage de l'administrateur -->

## Aperçu des fichiers administrateur

Il y a plus de fichiers dans la partie administrateur du composant, en partie parce qu'il y a deux tables, chacune avec une vue liste et une vue édition, et en partie parce que certains des fichiers contrôlent des aspects du comportement du composant à la fois dans les interfaces du site et de l'administrateur.

La création de code pour une vue en liste a été abordée dans la partie du tutoriel traitant des fichiers du site et ne sera donc pas répétée ici. Cette section traitera des fichiers communs à la plupart des composants. Un tutoriel ultérieur couvrira l'une des vues d'édition nécessaires pour mettre à jour ou ajouter de nouveaux enregistrements.

## services/provider.php

Ce fichier est utilisé lorsque l'application Joomla appelle le ComponentHelper pour rendre le composant. C'est le tout premier code de composant exécuté à la fois pour le Site et l'Administrateur. Son rôle est d'enregistrer le composant :

```php
    public function register(Container $container)
    {
        $container->registerServiceProvider(new MVCFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->registerServiceProvider(new ComponentDispatcherFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->registerServiceProvider(new RouterFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->set(
            ComponentInterface::class,
            function (Container $container) {
                $component = new CountrybaseComponent($container->get(ComponentDispatcherFactoryInterface::class));
                $component->setMVCFactory($container->get(MVCFactoryInterface::class));
                $component->setRouterFactory($container->get(RouterFactoryInterface::class));

                return $component;
            }
        );
    }
```

## src/Extension/CountrybaseComponent.php

La fonction d'enregistrement de ce fichier est appelée depuis l'instruction \$component = new CountrybaseComponent(...); dans services/provider.php. Son but est de démarrer le composant dans les interfaces Site et Administrateur.

```php
       public function boot(ContainerInterface $container)
        {
        }
```

## access.xml

Ce fichier définit les contrôles d'accès utilisés dans ce composant. Les éléments listés ici apparaissent dans l'onglet Options Permissions du composant.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<access component="com_countrybase">
    <section name="component">
        <action name="core.admin" title="JACTION_ADMIN" />
        <action name="core.options" title="JACTION_OPTIONS" />
        <action name="core.manage" title="JACTION_MANAGE" />
        <action name="core.create" title="JACTION_CREATE" />
        <action name="core.delete" title="JACTION_DELETE" />
        <action name="core.edit" title="JACTION_EDIT" />
    </section>
</access>
```

## config.xml

Ce fichier est utilisé pour contrôler si des options apparaissent dans le formulaire *Options* du composant et sous quel onglet elles apparaissent. Pour com_countrybase, il n'y a pas d'autres options que les options de permissions standard de Joomla. Il serait possible d'ajouter ici des options, par exemple, pour afficher ou masquer une colonne particulière dans l'affichage des résultats. Cela irait dans un ensemble de champs séparé nommé Options.

```xml
<?xml version="1.0" encoding="utf-8"?>
<config>

    <fieldset
        name="permissions"
        label="JCONFIG_PERMISSIONS_LABEL"
        description="JCONFIG_PERMISSIONS_DESC"
        >

        <field
            name="rules"
            type="rules"
            label="JCONFIG_PERMISSIONS_LABEL"
            filter="rules"
            validate="rules"
            component="com_countrybase"
            section="component"
         />
    </fieldset>
</config>
```

*Traduit par openai.com*

