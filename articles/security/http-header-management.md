<!-- Filename: J4.x:Http_Header_Management / Display title: En-têtes HTTP -->

## Introduction

Joomla 4 a introduit un système de gestion des en-têtes HTTP conçu pour aider les propriétaires de sites à configurer les en-têtes de sécurité HTTP. Il est mis en œuvre à l'aide d'un plugin *Système - En-têtes HTTP*. Il existe un tutoriel complet pour les [utilisateurs](jdocmanual?article=user/security/http-headers) sur ce sujet. Ce tutoriel est plus court et couvre certains points pertinents pour les développeurs.

## Le Système - Plugin d'En-têtes HTTP

### Onglet plugin

Accédez à **Système → Plugins → Système - HTTP Headers** pour accéder au formulaire de configuration du plugin.

![System http headers plugin form](../../../en/images/security/security-http-headers-plugin.png)

- **X-Frame Options** Ceci est activé par défaut mais la [documentation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options) indique qu'il est obsolète et qu'une politique *frame-ancestors* devrait être utilisée à la place.
- **Referrer-Policy** La valeur par défaut est *strict-origin-when-cross-origin*.
- **Cross-Origin-Opener-Policy** La valeur par défaut de Joomla est `same-origin`.
- **Force HTTP Headers** Il n'y a pas d'en-têtes forcés définis par défaut. C'est ici qu'une alternative à *X-Frame Options* peut être spécifiée. La valeur de `Content-Security-Policy` peut être l'une des suivantes :
    - `'none'`
    - `'self' https://www.example.org;`
    - `'self' https://example.org https://example.com https://store.example.com;`

En utilisant le sous-formulaire **Forcer les en-têtes HTTP**, vous pouvez également forcer les en-têtes suivants :

- [Politique de sécurité de contenu](https://scotthelme.co.uk/content-security-policy-an-introduction/)
- [Politique de sécurité de contenu uniquement pour les rapports](https://scotthelme.co.uk/content-security-policy-an-introduction/#testingapolicy)
- [Politique d'ouverture cross-origin](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cross-Origin-Opener-Policy)
- [Expect-CT](https://scotthelme.co.uk/a-new-security-header-expect-ct/)
- [Politique de fonctionnalités & Politique de permissions](https://scotthelme.co.uk/a-new-security-header-feature-policy/)
- [NEL (Journalisation des réponses réseau)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/NEL)
- [Politique du référent](https://scotthelme.co.uk/a-new-security-header-referrer-policy/)
- [Rapporter à](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
- [Sécurité stricte du transport](https://scotthelme.co.uk/hsts-the-missing-link-in-tls/)
- [Options X-Frame](https://scotthelme.co.uk/hardening-your-http-response-headers/#x-frame-options)

### Onglet Strict-Transport-Security (HSTS)

![strict transport security settings](../../../en/images/security/security-http-headers-hsts.png)

Utilisez le bouton *Basculer l'aide en ligne* pour obtenir des informations sur chaque paramètre. Référence illustrée :

[Formulaire pour vérifier ou définir le statut de préchargement HSTS et l'éligibilité](https://hstspreload.org/)

### Onglet Content-Security-Policy (CSP)

![Content security policy options](../../../en/images/security/security-http-headers-csp.png)

Une fois activée, vous pouvez définir le client où vous souhaitez appliquer le CSP configuré, vous permettant de définir `site`, `administrator` ou `both`. Un CSP devrait être appliqué à la fois sur le frontend et le backend. Références illustrées :

- [Politique de Sécurité du Contenu](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)
- [Nonce](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/nonce)
- [Hachages de Script et de Style](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/script-src)
- [Directives de Politique](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/child-src) les descriptions src sont disponibles dans le menu de cette page.

L'option finale appelée *Ajouter une directive* vous permet de configurer la liste blanche par directive selon vos besoins. Par exemple, pour la directive *script-src*, le champ *Valeur* doit contenir les origines que vous souhaitez autoriser pour le chargement de scripts.

## Notes

Lorsque vous avez configuré certains en-têtes de sécurité HTTP directement sur le serveur, il peut y avoir des doublons.

Vérifiez la sortie de vos en-têtes HTTP dans la console du navigateur. Dans Google Chrome ou Firefox : Inspecter → Réseau → la sortie sous En-têtes. Vous pouvez ensuite désactiver les en-têtes qui causent des entrées doublées. Vérifiez également la console de votre navigateur pour d'éventuelles erreurs.

## Développeurs d'extensions

L'avantage principal d'une politique de sécurité de contenu se manifeste lorsque l'en-tête bloque tout JavaScript en ligne et CSS en ligne affectant les gestionnaires d'événements JavaScript via des attributs HTML. Avec la protection du navigateur activée, le JavaScript en ligne et le CSS en ligne sont également bloqués pour les extensions. Cette protection n'est pas activée par défaut, mais peut l'être par les utilisateurs.

Là où JavaScript et CSS en ligne sont nécessaires, la prise en charge des nonce et des hash est incluse dans les API Document. Lorsqu'elles sont utilisées, le noyau veillera à ce qu'elles soient sur la liste blanche mais bloquera toujours le code malveillant.

### Notes importantes pour les développeurs d'extensions

À partir de Joomla 4.0, la Politique de Sécurité du Contenu :

- est livré avec le noyau.
- est désactivé par défaut.
- peut être activé par les administrateurs du site.
- il est fortement recommandé que le frontend et le backend de votre extension fonctionnent avec la Politique de Sécurité de Contenu activée.

Avec la politique de sécurité de contenu stricte activée, les fonctionnalités suivantes seront bloquées :

- L'exécution de JavaScript via les gestionnaires d'événements HTML (gestionnaires onXXX comme onClick et similaires).
- L'exécution de JavaScript intégré à la page et non transmis à la page via l'API Document.
- L'exécution de code JavaScript injecté dans les API DOM comme eval().
- L'utilisation de CSS en ligne intégré à la page et non transmis à la page via l'API Document.
- L'utilisation de CSS en ligne avec l'attribut style HTML.

Pour que vos extensions fonctionnent même avec une politique de sécurité de contenu stricte activée, le moyen le plus simple est d'utiliser l'API Document pour appliquer votre JavaScript et CSS en ligne. Voir les exemples ci-dessous.

### Ajouter du JavaScript en utilisant l'API Joomla

```php
    use Joomla\CMS\Factory;

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Add JavaScript from URL
    $wa->registerAndUseScript('com_example.sample', 'https://example.org/sample.js', [], ['defer' => true]);

    // Add inline JavaScript
    $wa->addInlineScript('
        document.addEventListener("DOMContentLoaded", function(event) {
            alert("An inline JavaScript Declaration");
        });
    ');
```

### Ajouter du CSS en utilisant l'API Joomla

```php
    use Joomla\CMS\Factory;

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Add Style from URL
    $wa->registerAndUseStyle('com_example.sample', 'https://example.org/sample.css');

    // Add inline Style
    $wa->addInlineStyle('
        body {
            background: #00ff00;
            color: rgb(0,0,255);
        }
    ');
```

## Ressources supplémentaires

- [CSP Cheat Sheet](https://scotthelme.co.uk/csp-cheat-sheet/)
- [Introduction à la politique de sécurité du contenu](https://scotthelme.co.uk/content-security-policy-an-introduction/)
- [Security Headers](https://securityheaders.com/) fait partie de Probely et a été initialement créé par Scott Helme ! C'est un outil gratuit et facile à utiliser conçu pour vous aider à mieux déployer et comprendre les fonctionnalités de sécurité modernes disponibles pour votre site web.
- [CSP Evaluator](https://csp-evaluator.withgoogle.com/)
- [Web Fundamentals Content Security Policy](https://developers.google.com/web/fundamentals/security/csp)
- [Documentation CSP de Google](https://csp.withgoogle.com/docs/index.html)
- [CSP Is Dead, Long Live CSP!](https://research.google/pubs/pub45542/) Sur l'insécurité des listes blanches et l'avenir de la politique de sécurité du contenu
- [Recherche web.dev pour la sécurité](https://web.dev/s/results?q=security#gsc.tab=0&gsc.q=security&gsc.sort=)

*Traduit par openai.com*

