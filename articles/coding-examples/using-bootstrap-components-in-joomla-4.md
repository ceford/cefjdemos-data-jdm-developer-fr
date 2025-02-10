<!-- Filename: J4.x:Using_Bootstrap_Components_in_Joomla_4 / Display title: Utilisation des composants Bootstrap dans Joomla 4 -->

## Introduction

### Composants Bootstrap

Certaines des composantes décrites dans la documentation de Bootstrap utilisent uniquement le CSS. Par exemple, la composante Breadcrumbs est rendue avec CSS et ne nécessite aucun support JavaScript. D'autres réagissent aux actions de l'utilisateur telles que le clic ou le survol, et nécessitent un support JavaScript. Ces dernières sont appelées ici Composantes Interactives. Cet article explique comment les utiliser dans des Articles et un Module Personnalisé.

Voir la [Documentation de Bootstrap](https://getbootstrap.com/docs/5.0/getting-started/introduction/)

### Joomla 4 Introduit une Approche Modulaire pour les Composants Interactifs

- Qu'est-ce qu'une approche modulaire ?
- La fonctionnalité est décomposée en composants individuels pris en charge par des fichiers individuels. Il n'y a pas d'approche **un seul fichier** comme c'était le cas avec Bootstrap dans Joomla 3. L'approche modulaire est plus efficace et offre des gains de performance (envoyer uniquement le code nécessaire au lieu de tout livrer au cas où une page aurait besoin d'un composant).

### Utilisation des composants interactifs : Codeurs

- Chargez ce dont vous avez besoin par cas ! Il existe des fonctions d'aide pour configurer des composants individuels avec les arguments appropriés.
- Voir la liste des composants interactifs Bootstrap.

### Utilisation des composants interactifs : Non-coders

- Utiliser des composants dans les articles n'est pas si facile car les fonctions d'aide ne peuvent pas être appelées à partir d'un article ou d'un module HTML personnalisé standard. Trois solutions possibles sont suggérées plus loin dans ce tutoriel :
  - Un module personnalisé
  - Un plugin personnalisé
  - Une surcharge de module personnalisé
- Passez ou parcourez la liste des composants interactifs de Bootstrap. Vous n'utiliserez pas ces appels de fonction directement.

## Composants Interactifs de Bootstrap

### Alerte

En supposant que vous ayez déjà la partie HTML dans votre disposition, vous devrez également inclure l'interactivité (la partie JavaScript) :

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.alert', '.selector');
```

- Le **.sélecteur** fait référence au sélecteur CSS pour l'alerte. Vous pouvez appeler cette fonction plusieurs fois avec différents sélecteurs CSS.
- Aucune option supplémentaire disponible

### Bouton

En supposant que vous ayez déjà la partie HTML dans votre mise en page, vous devrez également inclure l'interactivité (la partie JavaScript) :

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.button', '.selector');
```

- Le **.sélecteur** fait référence au sélecteur CSS pour le bouton. Vous pouvez appeler cette fonction plusieurs fois avec différents sélecteurs CSS
- Aucune option supplémentaire disponible

### Carrousel

En supposant que vous ayez déjà la partie HTML dans votre mise en page, vous devrez également inclure l'interactivité (la partie JavaScript) :

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.carousel', '.selector', []);
```

- Le **.sélecteur** fait référence au sélecteur CSS pour le carrousel. Vous pouvez appeler cette fonction plusieurs fois avec différents sélecteurs CSS.
- Le troisième argument fait référence aux options disponibles pour le carrousel.

Les options pour le carrousel peuvent être :

- **interval**, nombre, par défaut : **5000**, Le temps de retard entre chaque défilement automatique d'un élément. Si false, le carrousel ne défilera pas automatiquement.
- **keyboard**, booléen, par défaut : **true** Indique si le carrousel doit réagir aux événements clavier.
- **pause**, chaîne\|booléen, **hover** Met en pause le défilement du carrousel lors du survol de la souris et reprend le défilement à la sortie de la souris.
- **slide**, chaîne\|booléen, par défaut : **false** Lance automatiquement le carrousel après que l'utilisateur a fait défiler manuellement le premier élément. Si "carousel", lance automatiquement le carrousel au chargement.

### Réduire

En supposant que vous ayez déjà la partie HTML dans votre mise en page, vous devrez également inclure l'interactivité (la partie JavaScript) :

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.collapse', '.selector', []);
```

- Le **.sélecteur** fait référence au sélecteur CSS pour le collapse. Vous pouvez appeler cette fonction plusieurs fois avec différents sélecteurs CSS.
- Le troisième argument fait référence aux options disponibles pour le collapse.

Options pour l'effondrement peuvent être :

- **parent**, chaîne de caractères, par défaut : **false** Si un parent est fourni, alors tous les éléments rétractables sous le parent spécifié seront fermés lorsque cet élément rétractable est affiché.
- **toggle**, booléen, par défaut : **true** Alterne l'élément rétractable lors de l'invocation.

### Liste déroulante

En supposant que vous avez déjà la partie HTML dans votre mise en page, vous devrez également inclure l'interactivité (la partie JavaScript) :

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.dropdown', '.selector', []);
```

- Le **.sélecteur** fait référence au sélecteur CSS pour le menu déroulant. Vous pouvez appeler cette fonction plusieurs fois avec différents sélecteurs CSS.
- Le troisième argument fait référence aux options disponibles pour le menu déroulant.

Options pour l'effondrement peuvent être :

- **flip**, booléen, par défaut:**true** Permet au menu déroulant de se retourner en cas de chevauchement sur l'élément de référence.
- **boundary**, chaîne de caractères, par défaut:**scrollParent** Limite de contrainte de débordement du menu déroulant.
- **reference**, chaîne de caractères, par défaut:**toggle** Élément de référence du menu déroulant. Accepte **toggle** ou **parent**.
- **display**, chaîne de caractères, par défaut:**dynamic** Par défaut, nous utilisons Popper pour un positionnement dynamique. Désactivez-le avec **static**.

### Modal

En supposant que vous ayez déjà la partie HTML dans votre mise en page, vous aurez également besoin d'inclure l'interactivité (la partie JavaScript) :

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.modal', '.selector', []);
```

- Le **.sélecteur** fait référence au sélecteur CSS pour la fenêtre modale. Vous pouvez appeler cette fonction plusieurs fois avec différents sélecteurs CSS.
- Le troisième argument fait référence aux options disponibles pour la fenêtre modale.

Les options pour la modale peuvent être :

- **backdrop**, chaîne\|booléen par défaut : **true** Inclut un élément de fond modal. Sinon, spécifiez static pour un fond qui ne ferme pas la fenêtre modale au clic.
- **keyboard**, booléen par défaut : **true** Ferme la fenêtre modale lorsque la touche Échap est enfoncée.
- **focus**, booléen par défaut : **true** Ferme la fenêtre modale lorsque la touche Échap est enfoncée.

### Hors-canvas

En supposant que vous ayez déjà la partie HTML dans votre mise en page, vous devrez également inclure l'interactivité (la partie JavaScript) :

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.offcanvas', '.selector', []);
```

- Le **.sélecteur** fait référence au sélecteur CSS pour le offcanvas. Vous pouvez appeler cette fonction plusieurs fois avec différents sélecteurs CSS.
- Le troisième argument fait référence aux options disponibles pour le offcanvas.

Les options pour le offcanvas peuvent être :

- **backdrop**, booléen, par défaut : **true** Appliquer un arrière-plan sur le corps lorsque l'offcanvas est ouvert.
- **keyboard**, booléen, par défaut : **true** Ferme l'offcanvas lorsque la touche Échap est enfoncée.
- **scroll**, booléen, par défaut : **false** Permettre le défilement du corps lorsque l'offcanvas est ouvert.

### Popover

En supposant que vous ayez déjà la partie HTML dans votre mise en page, vous devrez également inclure l'interactivité (la partie JavaScript) :

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.popover', '.selector', []);
```

- Le **.sélecteur** fait référence au sélecteur CSS pour le popover. Vous pouvez appeler cette fonction plusieurs fois avec différents sélecteurs CSS.
- Le troisième argument fait référence aux options disponibles pour le popover.

Les options pour le popover peuvent être :

- **animation**, booléen, par défaut : **true** Appliquer une transition CSS de fondu au popover.
- **container**, chaîne\|booléen, par défaut : **false** Ajoute le popover à un élément spécifique. Par exemple : **body**.
- **content**, chaîne, par défaut : **null** Valeur de contenu par défaut si l'attribut data-bs-content n'est pas présent.
- **delay**, nombre, par défaut : **0** Retard de l'affichage et du masquage du popover (ms) ne s'applique pas au type de déclenchement manuel.
- **html**, booléen, par défaut : **true** Insérer du HTML dans le popover. Si **false**, la propriété innerText sera utilisée pour insérer le contenu dans le DOM.
- **placement**, chaîne, par défaut : **right** Comment positionner le popover - **auto** \| **top** \| **bottom** \| **left** \| **right**. Lorsque auto est spécifié, il réorientera dynamiquement le popover.
- **selector**, chaîne, par défaut : **false** Si un sélecteur est fourni, les objets popover seront délégués aux cibles spécifiées.
- **template**, chaîne, par défaut : **null** HTML de base à utiliser lors de la création du popover.
- **title**, chaîne, par défaut : **null** Valeur de titre par défaut si la balise **title** n'est pas présente.
- **trigger**, chaîne, par défaut : **click** Comment le popover est déclenché - **click** \| **hover** \| **focus** \| **manuel**.
- **offset**, entier, par défaut : **0** Décalage du popover par rapport à sa cible.

### Défilement espion

En supposant que vous ayez déjà la partie HTML dans votre mise en page, vous devrez également inclure l'interactivité (la partie JavaScript) :

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.scrollspy', '.selector', []);
```

- Le **.sélecteur** se réfère au sélecteur CSS pour le scrollspy. Vous pouvez appeler cette fonction plusieurs fois avec différents sélecteurs CSS.
- Le troisième argument se réfère aux options disponibles pour le scrollspy.

Les options pour le Scrollspy peuvent être :

- **décalage** nombre Pixels à décaler depuis le haut lors du calcul de la position du défilement.
- **méthode** chaîne Détermine dans quelle section se trouve l'élément espionné.
- **cible** chaîne Spécifie l'élément auquel appliquer le plugin Scrollspy.

### Onglet

En supposant que vous ayez déjà la partie HTML dans votre mise en page, vous devrez également inclure l'interactivité (la partie JavaScript) :

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tab', '.selector', []);
```

- Le **.sélecteur** se réfère au sélecteur CSS pour l'onglet. Vous pouvez appeler cette fonction plusieurs fois avec différents sélecteurs CSS.
- Le troisième argument se réfère aux options disponibles pour l'onglet.

Options pour l'onglet peuvent être :

### Info-bulle

En supposant que vous ayez déjà la partie HTML dans votre mise en page, vous devrez également inclure l'interactivité (la partie JavaScript) :

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tooltip', '.selector', []);
```

- Le **.sélecteur** fait référence au sélecteur CSS pour l'infobulle. Vous pouvez appeler cette fonction plusieurs fois avec différents sélecteurs CSS.
- Le troisième argument fait référence aux options disponibles pour l'infobulle.

Les options pour l'infobulle peuvent être :

- **animation**, booléen applique une transition d'estompage CSS au popover.
- **container**, chaîne\|booléen Ajoute le popover à un élément spécifique : { container: **body** }.
- **delay**, nombre\|objet retarde l'affichage et la dissimulation du popover (ms) - ne s'applique pas au type de déclenchement manuel Si un nombre est fourni, le délai est appliqué à la fois à la dissimulation/affichage La structure de l'objet est : delay: { show: 500, hide: 100 }.
- **html**, booléen Insère du HTML dans le popover. Si false, la méthode text de jQuery sera utilisée pour insérer le contenu dans le DOM.
- **placement**, chaîne\|fonction comment positionner le popover - **top** \| **bottom** \| **left** \| **right**.
- **sélecteur** chaîne Si un sélecteur est fourni, les objets popover seront délégués aux cibles spécifiées.
- **template**, chaîne HTML de base à utiliser lors de la création du popover.
- **titre**, chaîne\|fonction valeur de titre par défaut si la balise **title** n'est pas présente.
- **trigger**, chaîne comment le popover est déclenché - hover \| focus \| manuel.
- **constraints**, tableau Un tableau de contraintes - passé à Popper.
- **offset**, chaîne Décalage du popover par rapport à sa cible.

### Toast

En supposant que vous ayez déjà la partie HTML dans votre mise en page, vous devrez également inclure l'interactivité (la partie JavaScript) :

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.toast', '.selector', []);
```

- Le **.sélecteur** fait référence au sélecteur CSS pour le toast. Vous pouvez appeler cette fonction plusieurs fois avec différents sélecteurs CSS.
- Le troisième argument fait référence aux options disponibles pour le toast.

Les options pour le toast peuvent être :

- **animation**, booléen, défaut : **true** Appliquer une transition de fondu CSS au toast.
- **autohide**, booléen, défaut : **true** Cacher le toast automatiquement.
- **delay**, nombre, défaut : **5000** Délai pour cacher le toast (ms).

### Voir aussi

- **Accordéon** utilise Collapse pour afficher des panneaux de données.
- **Groupe de liste** peut être combiné avec Tab pour afficher du contenu sous forme d'onglets.

## Utilisation des composants Bootstrap dans les articles

La balise HTML de la plupart des composants peut être incluse dans un article ou un module qui peut lui-même être inclus dans un article. Le problème est que l'appel à HTMLHelper pour configurer le support JavaScript ne peut pas y être inclus. Plusieurs approches sont possibles pour résoudre ce problème. Trois approches sont suggérées ici : utiliser un module personnalisé, utiliser un plugin ou utiliser une surcharge de template.

**Attention :** Les éditeurs TinyMCE et JCE suppriment les espaces blancs lors de l'enregistrement et rendent l'édition du code difficile ! La solution simple est d'aller en haut à droite de votre écran Administrateur et de sélectionner **Menu Utilisateur → Modifier le Compte** et de définir l'Éditeur sur Code Mirror.

## Approche 1 : Utilisation d'un module personnalisé

Il s'agit probablement de l'approche la moins sujette aux erreurs car les options de support du composant Bootstrap sont sélectionnées à l'aide de cases à cocher. Les étapes impliquées sont les suivantes :

- Téléchargez, installez et activez ce [module personnalisé](https://github.com/ceford/j4xdemos-mod-custom-bscompos/raw/master/mod_custom_bscompos.zip)
- Depuis le menu Administrateur, allez à **Contenu** → **Modules du site → Nouveau**
- Sélectionnez **Composants personnalisés BS**
- Entrez un titre
- Basculez l'éditeur en mode texte brut et collez ou tapez le code HTML du composant que vous souhaitez utiliser.
- Dans l'onglet Options, faites défiler vers le bas jusqu'à la liste des composants BS et sélectionnez le type de composant dans cette instance du module. Notez que vous pouvez en sélectionner plusieurs si vous utilisez plusieurs composants.
- Sélectionnez une position de module : soit
  - une position définie par le modèle si vous souhaitez utiliser le module dans un emplacement spécifique ou
  - tapez une position si vous souhaitez utiliser le module dans un article spécifique : dans l'article, tapez {loadposition whatever}
- Enregistrez et allez sur le site pour tester !

### Sélecteurs

Pour certains composants, l'action JavaScript est déclenchée par une **classe** spécifique dans le code HTML. Dans d'autres composants, l'action est déclenchée par un attribut **data-bs-whatever**. Voici les déclencheurs actuels qui peuvent changer :

- **Alerte** déclenchée par class="alert ..."
- **Bouton** déclenché par data-bs-toggle="button"
- **Carrousel** déclenché par data-bs-ride="whatever"
- **Effondrement** déclenché par data-bs-toggle="collapse"
- **Déroulant** déclenché par data-bs-toggle="dropdown"
- **Modale** déclenchée par data-bs-toggle="modal"
- **Hors canvas** déclenché par data-bs-toggle="offcanvas"
- **Pop-over** déclenché par class="btn ..." ou
    - balise (pourrait être changée en class="haspopover ...") ET
    - data-bs-toggle="popover"
- **Spy de défilement** déclenché par data-bs-spy="scroll"
- **Onglet** déclenché par data-bs-toggle="tab"
- **Toast** déclenché par class="toast ..."
- **Infobulle** déclenchée par class="btn ..." ou
    - balise (pourrait être changée en class="hastooltip ...") ET
    - data-bs-toggle="tooltip"

### Exemple 1 : Alerte

Les alertes peuvent être utilisées dans le code HTML sans support JavaScript. Cela n'est nécessaire que pour la capacité de fermeture. Exemple de code HTML :

```html
<div class="alert alert-warning alert-dismissible fade show" role="alert">
  <strong>Holy guacamole!</strong> You should check in on some of those fields below.
  <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
</div>
```

Exemple de résultat d'inclusion d'un module dans un article :

![Bootstrap alert](../../../en/images/coding-examples/coding-examples-alert.png)

Notez que sans support JavaScript, l'alerte apparaîtra exactement comme ci-dessus, mais un clic sur le bouton de fermeture \[X\] ne fermera pas l'alerte. De plus, l'alerte apparaîtra à chaque chargement de page.

### Exemple 2 : Boutons

Les boutons peuvent être utilisés dans le code HTML sans support JavaScript. Ceci n'est nécessaire que pour le changement parfois subtil de style appliqué aux boutons avec un changement d'état, stylé actif. Exemple de code Bootstrap :

```html
<p><button type="button" class="btn btn-primary" data-bs-toggle="button" autocomplete="off">Toggle button</button>
<button type="button" class="btn btn-primary active" data-bs-toggle="button" autocomplete="off" aria-pressed="true">Active toggle button</button>
<button type="button" class="btn btn-primary" disabled data-bs-toggle="button" autocomplete="off">Disabled toggle button</button></p>
```

```html
<p><a href="#" class="btn btn-primary" role="button" data-bs-toggle="button">Toggle link</a>
<a href="#" class="btn btn-primary active" role="button" data-bs-toggle="button" aria-pressed="true">Active toggle link</a>
<a href="#" class="btn btn-primary disabled" tabindex="-1" aria-disabled="true" role="button" data-bs-toggle="button">Disabled toggle link</a></p>
```

Avec ce style dans le modèle user.css :

```css
    .btn.btn-primary.active {
        background-color: green;
    }
```

![Bootstrap buttons](../../../en/images/coding-examples/coding-examples-buttons.png)

Les boutons basculent entre le bleu et le vert.

### Exemple 3 : Carrousel

Le Carrousel propose un diaporama parcourant une série d'images ou de volets de texte. L'exemple suivant utilise des images de l'exemple de Joomla 4. Code Bootstrap :

```html
<div id="carouselExampleCaptions" class="carousel slide" data-bs-ride="carousel">
    <div class="carousel-indicators">
        <button class="active" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="0" aria-current="true" aria-label="Slide 1"></button>
        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="1" aria-label="Slide 2"></button>
        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="2" aria-label="Slide 3"></button>
    </div>
    <div class="carousel-inner">
        <div class="carousel-item active"><img class="d-block w-100" src="images/sampledata/cassiopeia/nasa1-1200.jpg" alt="..." />
            <div class="carousel-caption d-none d-md-block">
                <h5>First slide label</h5>
                <p>Some representative placeholder content for the first slide.</p>
            </div>
        </div>
        <div class="carousel-item"><img class="d-block w-100" src="images/sampledata/cassiopeia/nasa2-1200.jpg" alt="..." />
            <div class="carousel-caption d-none d-md-block">
                <h5>Second slide label</h5>
                <p>Some representative placeholder content for the second slide.</p>
            </div>
        </div>
        <div class="carousel-item"><img class="d-block w-100" src="images/sampledata/cassiopeia/nasa3-1200.jpg" alt="..." />
            <div class="carousel-caption d-none d-md-block">
                <h5>Third slide label</h5>
                <p>Some representative placeholder content for the third slide.</p>
            </div>
        </div>
    </div>
    <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="prev">
        <span class="visually-hidden">Previous</span>
    </button>
    <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="next">
        <span class="visually-hidden">Next</span>
    </button>
</div>
```

Résultat :

![Bootstrap carousel](../../../en/images/coding-examples/coding-examples-carousel.jpg)

### Exemple 4 : Réduire

Collapse est largement utilisé dans Joomla et vous n'aurez peut-être pas besoin d'utiliser un module ou un plugin pour déclencher une action. Le clic ouvre un volet avec des informations supplémentaires. Exemple de code Bootstrap :

```html
<p>
    <a class="btn btn-primary" role="button" href="#collapseExample" data-bs-toggle="collapse" aria-expanded="false" aria-controls="collapseExample"> Link with href </a>
    <button class="btn btn-primary" type="button" data-bs-toggle="collapse" data-bs-target="#collapseExample" aria-expanded="false" aria-controls="collapseExample">
        Button with data-bs-target
    </button>
</p>
<div id="collapseExample" class="collapse">
    <div class="card card-body">
        Some placeholder content for the collapse component. This panel is hidden by default but revealed when the user activates the relevant trigger.
    </div>
</div>
```

Résultat :

![Bootstrap collapse](../../../en/images/coding-examples/coding-examples-collapse.png)

### Exemple 5 : Menu déroulant

Les menus déroulants sont des superpositions contextuelles basculantes pour afficher des listes de liens et plus encore. Exemple de code Bootstrap :

```html
<div class="btn-group">
    <button class="btn btn-danger dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false"> Action </button>
    <ul class="dropdown-menu">
        <li><a class="dropdown-item" href="#">Action</a></li>
        <li><a class="dropdown-item" href="#">Another action</a></li>
        <li><a class="dropdown-item" href="#">Something else here</a></li>
        <li><hr class="dropdown-divider" /></li>
        <li><a class="dropdown-item" href="#">Separated link</a></li>
    </ul>
</div>
```

Résultat :

![Bootstrap dropdown](../../../en/images/coding-examples/coding-examples-dropdown.png)

### Exemple 6 : Modale

Le composant Modal ouvre une boîte de dialogue au centre de l'écran. Il existe plusieurs options pour contrôler la taille et le contenu de la modale. Consultez la documentation Bootstrap pour plus de détails. Exemple de code Bootstrap :

```html
<p><button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"> Launch demo modal </button></p>
<div id="exampleModal" class="modal fade" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 id="exampleModalLabel" class="modal-title">Modal title</h5>
                <button class="btn-close" type="button" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                ...
            </div>
            <div class="modal-footer">
                <button class="btn btn-secondary" type="button" data-bs-dismiss="modal">Close</button>
                <button class="btn btn-primary" type="button">Save changes</button>
            </div>
        </div>
    </div>
</div>
```

Résultat :

![Bootstrap modal](../../../en/images/coding-examples/coding-examples-modal.png)

### Exemple 7 : Hors-canvas

Pour le moment, ce composant n'est pas pris en charge dans Joomla. Restez à l'écoute - bientôt disponible !

### Exemple 8 : Popover

Les popovers sont comme les infobulles mais avec un titre. Ils présentent quelques problèmes d'accessibilité et de performance, il convient donc de les utiliser avec prudence. Exemples de code Bootstrap :

```html
<p><button class="btn btn-lg btn-danger" title="Popover title" type="button" data-bs-toggle="popover" data-bs-content="And here's some amazing content. It's very engaging. Right?">Click to toggle popover</button></p>
```

Résultat :

![Bootstrap alert](../../../en/images/coding-examples/coding-examples-popover.png)

### Exemple 9 : Scrollspy

Texte de l'exemple de code :

```html
<div class="row">
    <div class="col-4">
        <nav id="navbar-example3" class="navbar navbar-light bg-light flex-column">
            <a class="navbar-brand" href="#">Navbar</a><nav class="nav nav-pills flex-column">
            <a class="nav-link active" href="#item-1">Item 1</a>
            <nav class="nav nav-pills flex-column">
                <a class="nav-link ms-3 my-1" href="#item-1-1">Item 1-1</a>
                <a class="nav-link ms-3 my-1" href="#item-1-2">Item 1-2</a>
            </nav>
            <a class="nav-link" href="#item-2">Item 2</a>
            <a class="nav-link" href="#item-3">Item 3</a>
            <nav class="nav nav-pills flex-column">
                <a class="nav-link ms-3 my-1" href="#item-3-1">Item 3-1</a>
                <a class="nav-link ms-3 my-1" href="#item-3-2">Item 3-2</a>
            </nav>
        </nav>
    </div>
    <div class="col-8">
        <div class="scrollspy-example" tabindex="0" data-bs-spy="scroll" data-bs-target="#navbar-example3" data-bs-offset="0">
            <h4 id="item-1">Item 1</h4>
            <p>Placeholder content for the scrollspy example. This one relates to item 1. Takes you miles high, so high, 'cause she’s got that one international smile. There's a stranger in my bed, there's a pounding in my head. Oh, no. In another life I would make you stay. ‘Cause I, I’m capable of anything. Suiting up for my crowning battle. Used to steal your parents' liquor and climb to the roof. Tone, tan fit and ready, turn it up cause its gettin' heavy. Her love is like a drug. I guess that I forgot I had a choice.</p>
            <h5 id="item-1-1">Item 1-1</h5>
            <p>Placeholder content for the scrollspy example. This one relates to the item 1-1. You got the finest architecture. Passport stamps, she's cosmopolitan. Fine, fresh, fierce, we got it on lock. Never planned that one day I'd be losing you. She eats your heart out. Your kiss is cosmic, every move is magic. I mean the ones, I mean like she's the one. Greetings loved ones let's take a journey. Just own the night like the 4th of July! But you'd rather get wasted.</p>
            <h5 id="item-1-2">Item 1-2</h5>
            <p>Placeholder content for the scrollspy example. This one relates to the item 1-2. Her love is like a drug. All my girls vintage Chanel baby. Got a motel and built a fort out of sheets. 'Cause she's the muse and the artist. (This is how we do) So you wanna play with magic. So just be sure before you give it all to me. I'm walking, I'm walking on air (tonight). Skip the talk, heard it all, time to walk the walk. Catch her if you can. Stinging like a bee I earned my stripes.</p>
            <h4 id="item-2">Item 2</h4>
            <p>Placeholder content for the scrollspy example. This one relates to item 2. Don't need apologies. There is no fear now, let go and just be free, I will love you unconditionally. Last Friday night. Don't be a shy kinda guy I'll bet it's beautiful. Summer after high school when we first met. 'Cause she's the muse and the artist. What? Wait. No, no, no, no. Thought that I was the exception.</p>
            <h4 id="item-3">Item 3</h4>
            <p>Placeholder content for the scrollspy example. This one relates to item 3. Word on the street, you got somethin' to show me, me. All this money can't buy me a time machine. Make it like your birthday everyday. So we hit the boulevard. You make me feel like I'm livin' a teenage dream, the way you turn me on Skip the talk, heard it all, time to walk the walk. Word on the street, you got somethin' to show me, me. It's no big deal, it's no big deal, it's no big deal.</p>
            <h5 id="item-3-1">Item 3-1</h5>
            <p>Placeholder content for the scrollspy example. This one relates to item 3-1. Baby do you dare to do this? This is no big deal. Yeah, you're lucky if you're on her plane. Just own the night like the 4th of July! Standing on the frontline when the bombs start to fall. So just be sure before you give it all to me.</p>
            <h5 id="item-3-2">Item 3-2</h5>
            <p>Placeholder content for the scrollspy example. This one relates to item 3-2. You're original, cannot be replaced. All night they're playing, your song. California girls we're undeniable. Like a bird without a cage. There is no fear now, let go and just be free, I will love you unconditionally. I can see the writing on the wall. You could travel the world but nothing comes close to the golden coast.</p>
        </div>
    </div>
</div>
```

Résultat :

![Bootstrap scrollspy](../../../en/images/coding-examples/coding-examples-scrollspy.png)

De plus, quelques styles sont nécessaires dans user.css :

```css
    .scrollspy-example {
        height: 350px;
        overflow-y: scroll;
    }
```

Accroc : le menu ne s'harmonise pas bien avec le contenu dans cet exemple !

### Exemple 10 : Onglet

Les onglets sont souvent utilisés comme éléments de navigation combinés avec des menus déroulants. Exemple de code Bootstrap :

```html
<ul class="nav nav-tabs">
    <li class="nav-item"><a class="nav-link active" href="#" aria-current="page">Active</a></li>
    <li class="nav-item dropdown"><a class="nav-link dropdown-toggle" role="button" href="#" data-bs-toggle="dropdown" aria-expanded="false">Dropdown</a>
        <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Action</a></li>
            <li><a class="dropdown-item" href="#">Another action</a></li>
            <li><a class="dropdown-item" href="#">Something else here</a></li>
            <li><hr class="dropdown-divider" /></li>
            <li><a class="dropdown-item" href="#">Separated link</a></li>
        </ul>
    </li>
    <li class="nav-item"><a class="nav-link" href="#">Link</a></li>
    <li class="nav-item"><a class="nav-link disabled" tabindex="-1" href="#" aria-disabled="true">Disabled</a></li>
</ul>
```

Résultat :

![Bootstrap tab](../../../en/images/coding-examples/coding-examples-tab.png)

N'oubliez pas de vérifier à la fois les options Onglet et Déroulant pour que la partie déroulante fonctionne.

### Exemple 11 : Toast

Les toasts sont des notifications légères conçues pour imiter les notifications push qui ont été popularisées par les systèmes d'exploitation mobiles et de bureau. Ils sont construits avec flexbox, ce qui les rend faciles à aligner et à positionner. Exemple de code Bootstrap :

```html
<div class="toast fade show" role="alert" aria-live="assertive" aria-atomic="true">
    <div class="toast-header">
        <img class="rounded me-2" src="..." alt="..." />
        <strong class="me-auto">Bootstrap</strong> <small>11 mins ago</small>
        <button class="btn-close" type="button" data-bs-dismiss="toast" aria-label="Close"></button>
    </div>
    <div class="toast-body">Hello, world! This is a toast message.</div>
</div>
```

Résultat :

![Bootstrap toast](../../../en/images/coding-examples/coding-examples-toast.png)

Notez que la démonstration Bootstrap qui utilise un bouton pour afficher le message Toast nécessite un peu de JavaScript supplémentaire. Il semble que ce composant ait besoin d'un programmeur pour en faire bon usage !

### Exemple 12 : Infobulle

Un infobulle est un petit morceau de texte qui apparaît lorsque l'on survole un bouton ou un élément de lien pour expliquer ce que c'est ou ce qu'il fait. L'infobulle peut être positionnée au-dessus, en dessous, à gauche ou à droite de l'élément. Si aucune position n'est spécifiée, la position par défaut est en haut. L'infobulle changera de position s'il n'y a pas assez de place dans la position spécifiée. Exemple de code Bootstrap :

```html
<p><button class="btn btn-secondary" title="Tooltip on left" type="button" data-bs-toggle="tooltip" data-bs-placement="left"> Tooltip on left </button>
<button class="btn btn-secondary" title="Tooltip" type="button" data-bs-toggle="tooltip"> Tooltip </button>
<button class="btn btn-secondary" title="Tooltip on top" type="button" data-bs-toggle="tooltip" data-bs-placement="top"> Tooltip on top </button>
<button class="btn btn-secondary" title="Tooltip on right" type="button" data-bs-toggle="tooltip" data-bs-placement="right"> Tooltip on right </button>
<button class="btn btn-secondary" title="Tooltip on bottom" type="button" data-bs-toggle="tooltip" data-bs-placement="bottom"> Tooltip on bottom </button>
<button class="btn btn-secondary" title="<em>Tooltip</em> <u>with</u> <b>HTML</b>" type="button" data-bs-toggle="tooltip" data-bs-html="true"> Tooltip with HTML </button></p>
<p>Tooltip triggered by class: <button class="btn btn-warning" title="Tooltip Message">Alert!</button></p>
```

Résultat :

![Bootstrap tooltip](../../../en/images/coding-examples/coding-examples-tooltip.png)

## Approche 2 : Utiliser un plugin de contenu

Les étapes impliquées :

- Téléchargez, installez et activez ce [plugin](https://github.com/ceford/j4xdemos-plg-bscompos/raw/master/plg_j4xdemos_bscompos.zip)
- Dans l'article, ajoutez le texte sur lequel le plugin agit, par exemple {bscompos modal carousel} déclenchera le chargement du JavaScript nécessaire pour prendre en charge une boîte de dialogue modale et un carrousel. Le plugin supprime le texte déclencheur et les balises (désormais) vides qui l'entourent.
- Incluez le code HTML du composant Bootstrap directement dans l'article ou dans un module inclus dans l'article. Il y a un exemple de code HTML ci-dessous pour un modal simple et un modal contenant un carrousel. Notez que cela ne fonctionnera pas si le code HTML se trouve dans un module à un emplacement de modèle.
- Cela fonctionnera également pour un module personnalisé standard si l'option Préparer le contenu est réglée sur Oui.
- Testez-le !

## Approche 3 : Utiliser une surcharge de modèle

Les étapes impliquées :

- Créez une surcharge de modèle mod_custom.
- Ajoutez un module mod_custom contenant le balisage du composant et les classes de déclenchement.
- Incluez le module dans un article.

### La substitution de modèle mod_custom

- Dans l'interface Administrateur, allez à **Système → Modèles de site → Détails et fichiers de Cassiopeia**.
- Sélectionnez **Créer des surcharges **→** mod_custom **→** default.php**.
- À la ligne suivant defined('\_JEXEC') or die; ajoutez le code suivant :

```php
    $module_class = $params->get('moduleclass_sfx');
    if (!empty($module_class))
    {
        $classes = explode(' ', $module_class);
        foreach ($classes as $class)
        {
        switch ($class)
            {
              case 'bs-alert':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.alert', '.alert');
                break;
              case 'bs-button':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.button', '.btn');
                break;
              case 'bs-carousel':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.carousel', '.selector', []);
                break;
              case 'bs-collapse':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.collapse', '.selector', []);
                break;
              case 'bs-dropdown':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.dropdown', '.selector', []);
                break;
              case 'bs-modal':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.modal', '.selector', []);
                break;
              case 'bs-offcanvas':
                // Not Found
                //\Joomla\CMS\HTML\HTMLHelper::_('bootstrap.offcanvas', '.btn', []);
                break;
              case 'bs-popover':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.popover', '.btn', []);
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.popover', 'a', []);
                break;
              case 'bs-scrollspy':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.scrollspy', '.selector', []);
                break;
              case 'bs-tab':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tab', '.selector', []);
                break;
              case 'bs-tooltip':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tooltip', '.btn', []);
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tooltip', 'a', []);
                break;
              case 'bs-toast':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.toast', '.selector', []);
                break;
              default:
                // do nothing
            }
        }
    }
```

Ce code recherche les noms de classes définis dans mod_custom et effectue l'appel HTMLHelper pour configurer le support JavaScript. Notez que le dernier élément de chaque appel est un sélecteur qui peut ou non être utilisé pour déclencher une action. De nombreux composants sont déclenchés par des attributs de données dans le balisage et n'utilisent pas les sélecteurs ici. Pour certains, le sélecteur est nécessaire. Par exemple, il est judicieux d'utiliser la classe **.btn** et la balise **a** pour déclencher des infobulles.

### Un module mod_custom pour un composant modal

- Allez à **Contenu** → **Modules du site** → **Nouveau**.
- Sélectionnez le module **Personnalisé**.
- Remplissez le formulaire :
  - Titre : Modal Démo
  - Dans le champ Position, tapez **demomodal** pour utilisation dans un article ;
  - Contenu du module : Désactivez l'éditeur pour une saisie en texte brut.
  - Collez le code suivant de la documentation Bootstrap :

```html
<h2>Modal</h2>
<!-- Button trigger modal -->
<p><button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"> Launch demo modal </button></p>
<!-- Modal -->
<div id="exampleModal" class="modal fade" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 id="exampleModalLabel" class="modal-title">Modal title</h5>
                <button class="btn-close" type="button" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                ...
            </div>
            <div class="modal-footer">
                <button class="btn btn-secondary" type="button" data-bs-dismiss="modal">Close</button>
                <button class="btn btn-primary" type="button">Save changes</button>
            </div>
        </div>
    </div>
</div>
```

- Sélectionnez l'onglet Avancé et dans le champ Classe de module, entrez **bs-modal**
- Optionnel : définissez Titre sur Masquer pour utiliser le H2 dans le code collé.
- Enregistrez et Fermez (ne vous inquiétez pas si la fenêtre modale semble incorrecte dans l'éditeur).

### Créer un article et un élément de menu

- Créez un nouvel article, Démo Modal, et en mode de saisie de texte brut, définissez le contenu sur

```html
<div>{loadposition demomodal}</div>
```

- Créez un élément de menu Article Unique.
- Testez-le :

![Bootstrap modal module in article](../../../en/images/coding-examples/coding-examples-modal-module.png)

### Un composant modal contenant un carrousel

- Créez un nouveau module personnalisé avec un nouveau titre : Démo Modal Carrousel
- Position : demomodalcarousel
- **Onglet Avancé**→**Classe du Module** : bs-modal bs-carousel
- Contenu personnalisé du module en texte brut :

```html
<h2>Modal with Carousel</h2>
<div class="mod-custom custom">
    <!-- Button trigger modal -->
    <button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"> Launch demo modal </button>
</div>
<div id="exampleModal" class="modal fade" style="display: none;" tabindex="-1" aria-hidden="true">
    <div class="modal-dialog" role="document">
        <div class="modal-content">
            <div class="modal-header">
                <button class="close" type="button" data-bs-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">×</span>
                </button>
            </div>
            <div class="modal-body">
                <div id="carouselExampleCaptions" class="carousel slide" data-bs-ride="carousel">
                    <div class="carousel-indicators">
                        <button class="active" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="0" aria-current="true" aria-label="Slide 1"></button>
                        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="1" aria-label="Slide 2"></button>
                        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="2" aria-label="Slide 3"></button>
                    </div>
                    <div class="carousel-inner">
                        <div class="carousel-item active">
                            <img class="d-block w-100" src="images/sampledata/cassiopeia/nasa1-1200.jpg" alt="..." />
                            <div class="carousel-caption d-none d-md-block">
                                <h5>First slide label</h5>
                                <p>Some representative placeholder content for the first slide.</p>
                            </div>
                        </div>
                        <div class="carousel-item">
                            <img class="d-block w-100" src="images/sampledata/cassiopeia/nasa2-1200.jpg" alt="..." />
                            <div class="carousel-caption d-none d-md-block">
                                <h5>Second slide label</h5>
                                <p>Some representative placeholder content for the second slide.</p>
                            </div>
                        </div>
                        <div class="carousel-item">
                            <img class="d-block w-100" src="images/sampledata/cassiopeia/nasa3-1200.jpg" alt="..." />
                            <div class="carousel-caption d-none d-md-block">
                                <h5>Third slide label</h5>
                                <p>Some representative placeholder content for the third slide.</p>
                            </div>
                        </div>
                    </div>
                    <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="prev">
                        <span class="visually-hidden">Previous</span>
                    </button>
                    <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="next">
                        <span class="visually-hidden">Next</span>
                    </button>
                </div>
            </div>
        </div>
    </div>
</div>
```

- Créez un nouvel article avec {loadposition demomodalcarousel} dans le contenu.
- Créez un nouvel élément de menu pour un article unique : Démo Modal Carrousel
- Testez-le :

![Bootstrap modal carousel](../../../en/images/coding-examples/coding-examples-modal-carousel.png)

*Traduit par openai.com*
