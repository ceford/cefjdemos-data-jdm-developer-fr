<!-- Filename: J4.x:Http_Header_Management / Display title: Gestion des En-têtes HTTP -->

Joomla!  4.0 Didacticiel
Comment utiliser le nouveau gestionnaire d'en-têtes HTTP dans Joomla 4.0


À partir de Joomla 4.0, Joomla a introduit un système de gestion
d'en-tête HTTP. Ce système est conçu pour aider les propriétaires de
sites à configurer les en-têtes de sécurité HTTP à partir du serveur
principal.

Dans ce didacticiel, vous trouverez les informations sur comment
paramétrer ce nouveau système.

## Plugin

### Système - En-têtes HTTP (plg_system_httpheaders)

Naviguez à **Système **→** Plugins **→** Système- En-têtes HTTP** pour
accéder à la configuration du plugin.

#### Configuration du Plugin

Sur cette page, vous pouvez choisir d'activer l'écriture des en-têtes
dans les fichiers de configuration du serveur (.htaccess et web.config)
et de définir si les en-têtes http suivants sont activés.

- <a
  href="https://scotthelme.co.uk/hardening-your-http-response-headers/#x-frame-options"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">X-Frame-Options</a>
- <a
  href="https://scotthelme.co.uk/a-new-security-header-referrer-policy/"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Referrer-Policy</a>
- <a href="https://www.chromestatus.com/feature/5432089535053824"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Cross-Origin-Opener-Policy</a>

En utilisant le formulaire "Forcer l'en-tête", vous pouvez également
forcer les en-têtes suivants avec ses valeurs:

- <a href="https://scotthelme.co.uk/hsts-the-missing-link-in-tls/"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Strict-Transport-Security</a>
- <a
  href="https://scotthelme.co.uk/content-security-policy-an-introduction/"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Content-Security-Policy</a>
- <a
  href="https://scotthelme.co.uk/content-security-policy-an-introduction/#testingapolicy"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Content-Security-Policy-Report-Only</a>
- <a href="https://scotthelme.co.uk/a-new-security-header-expect-ct/"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Expect-CT</a>
- <a href="https://scotthelme.co.uk/a-new-security-header-feature-policy/"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Feature-Policy &amp;
  Permissions-Policy</a>
- <a
  href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/report-to"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Report-to</a>

<img
src="https://docs.joomla.org/images/thumb/1/1c/Plg-system-httpheaders-options-en.png/800px-Plg-system-httpheaders-options-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/1/1c/Plg-system-httpheaders-options-en.png/1200px-Plg-system-httpheaders-options-en.png 1.5x, https://docs.joomla.org/images/thumb/1/1c/Plg-system-httpheaders-options-en.png/1600px-Plg-system-httpheaders-options-en.png 2x"
data-file-width="1837" data-file-height="1330" width="800" height="579"
alt="Plg-system-httpheaders-options-en.png" />

### Configuration Strict-Transport-Security (HSTS)

À partir de cette page, vous pouvez choisir d'activer l'en-tête
Strict-Transport-Security (HSTS) ainsi que de configurer la valeur
max-age si les sous-domaines doivent être inclus et si vous souhaitez
être ajouté à la liste de préchargement des navigateurs.

<img
src="https://docs.joomla.org/images/thumb/b/b5/Plg-system-httpheaders-options-hsts-en.png/800px-Plg-system-httpheaders-options-hsts-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/b/b5/Plg-system-httpheaders-options-hsts-en.png/1200px-Plg-system-httpheaders-options-hsts-en.png 1.5x, https://docs.joomla.org/images/thumb/b/b5/Plg-system-httpheaders-options-hsts-en.png/1600px-Plg-system-httpheaders-options-hsts-en.png 2x"
data-file-width="1837" data-file-height="1338" width="800" height="583"
alt="Plg-system-httpheaders-options-hsts-en.png" />

### Content-Security-Policy (CSP) Configuration

From this page you can choose to enable and configure the
Content-Security-Policy (CSP) header set by the plugin.

<img
src="https://docs.joomla.org/images/thumb/c/cb/Plg-system-httpheaders-options-csp-en.png/800px-Plg-system-httpheaders-options-csp-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/c/cb/Plg-system-httpheaders-options-csp-en.png/1200px-Plg-system-httpheaders-options-csp-en.png 1.5x, https://docs.joomla.org/images/thumb/c/cb/Plg-system-httpheaders-options-csp-en.png/1600px-Plg-system-httpheaders-options-csp-en.png 2x"
data-file-width="1802" data-file-height="1846" width="800" height="820"
alt="Plg-system-httpheaders-options-csp-en.png" />

Once enabled you can set the client where you want to enforce the
configured CSP on, allowing you to set set "site", "administrator" or
"both".

The following settings need a bit more explanation, thats also the
reason there are additional descriptions on all of them.

The final option called "Add Directive" allows you to configure the
allowlist per directive as you need them. For example the directive
"script-src" where the option "Value" you enter the origins you want to
allow to load scripts from.

## Notes

Lorsque vous avez configuré certains en-têtes de sécurité HTTP
directement sur le serveur, notre outil peut créer des entrées doubles.

Vérifiez la sortie de vos en-têtes HTTP après la configuration dans la
console du navigateur. Dans Google Chrome : Inspecter \> Réseau \> la
sortie sous En-têtes). Vous pouvez alors désactiver les en-têtes qui
provoquent des doubles entrées. Vérifiez également les erreurs
éventuelles dans la console de votre navigateur.

## Développeurs d'extension

Comme vous le savez peut-être, le grand avantage en matière de sécurité
concernant la politique de sécurité du contenu se manifeste lorsque nous
pouvons utiliser l'en-tête pour bloquer tout JavaScript et CSS en ligne
affectant par exemple les gestionnaires d'événements JavaScript via des
attributs HTML. So with this browser protection enabled we will block
inline JavaScript and inline CSS usage also for your extensions. That
protection is not enabled by default but can be enabled by your users.

For 4.0 it would be recommended to get the frontend of your extension
running with strict Content Security Policy enabled. For 4.1
compatibility it would be recommended that this also applies to your
backend.

Nous savons qu'il est toujours nécessaire d'avoir du JavaScript et du
CSS en ligne, c'est pourquoi nous avons implémenté la prise en charge de
nonce et de hash dans nos API de documents. Lorsque vous les utilisez,
le noyau s'assurera qu'ils sont sur une liste blanche, mais nous
bloquerons tout ce qui est malveillant pour protéger nos sites.

### Important notes for Extension Developers

Starting with Joomla 4.0 Content Security Policy:

- is shipped by the core
- is disabled by default
- can be enabled by your users
- it is strongly recommended that your extension frontend works by 4.0
  with Content Security Policy enabled
- it is recommended that your extension backend works by 4.1 with
  Content Security Policy enabled

With strict Content Security Policy enabled the following features will
be blocked:

- the execution of JavaScript via the HTML event handlers (onXXX
  handlers like onClick and similar)
- the execution of in-page JavaScript not passed to the page via the
  Document API
- the execution of JavaScript code injected into DOM APIs such as eval()
- the usage of inline in-page CSS not passed to the page via the
  Document API
- the usage of inline CSS using the HTML style attribute

Pour que vos extensions fonctionnent même lorsque la politique de
sécurité du contenu est strictement activée, le moyen le plus simple est
d'utiliser l'API Document pour appliquer votre JavaScript et CSS en
ligne, veuillez consulter les exemples ci-dessous.

### Ajout de JavaScript à l'aide de l'API Joomla

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

### Ajout de CSS à l'aide de l'API Joomla

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

Plus de détails ici:[Ajouter du JavaScript et des CSS à une
page](https://docs.joomla.org/J3.x:Adding_JavaScript_and_CSS_to_the_page "Special:MyLanguage/J3.x:Adding JavaScript and CSS to the page")

## Additional resources about Content Security Policy and HTTP Headers

- <a href="https://scotthelme.co.uk/csp-cheat-sheet/"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">CSP Cheat Sheet</a>
- <a
  href="https://scotthelme.co.uk/content-security-policy-an-introduction/"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Content Security Policy - An
  Introduction</a>
- <a href="https://securityheaders.com/%7C" class="external text"
  target="_blank" rel="nofollow noreferrer noopener">SecurityHeaders.com
  (by Scott Helme)</a>
- <a href="https://csp-evaluator.withgoogle.com/%7C" class="external text"
  target="_blank" rel="nofollow noreferrer noopener">CSP Evaluator</a>
- <a href="https://developers.google.com/web/fundamentals/security/csp%7C"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Web Fundamentals Content Security
  Policy</a>
- <a href="https://csp.withgoogle.com/docs/index.html%7C"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Google's CSP Documentation</a>
- <a href="https://research.google/pubs/pub45542/%7C"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">CSP Is Dead, Long Live CSP! On the
  Insecurity of Whitelists and the Future of Content Security Policy</a>
- <a href="https://web.dev/tags/security/%7C" class="external text"
  target="_blank" rel="nofollow noreferrer noopener">web.dev tag
  Security</a>
