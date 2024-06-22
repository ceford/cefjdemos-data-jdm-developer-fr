<!-- Filename: Joomla_CodeSniffer / Display title: Joomla CodeSniffer 'Renifleur de Code' -->

## Un Nez Pour Joomla

Il s'agit d'une norme de codage personnalisée pour PHP CodeSniffer qui
tente de codifier et d'appliquer les normes de codage Joomla. Cet
article explique comment définir le vérificateur automatique de style de
code. Il se compose de trois étapes:

1.  Installez PHP CodeSniffer (phpcs).
2.  Clonez le style de code Joomla (à utiliser avec pour phpcs).
3.  Configurez votre IDE pour qu'il fonctionne avec PHP CodeSniffer et
    le Style de Code Joomla.

<img
src="https://docs.joomla.org/images/thumb/e/e4/CodeSnifferInAction.jpg/300px-CodeSnifferInAction.jpg"
class="thumbimage" decoding="async"
srcset="https://docs.joomla.org/images/thumb/e/e4/CodeSnifferInAction.jpg/450px-CodeSnifferInAction.jpg 1.5x, https://docs.joomla.org/images/thumb/e/e4/CodeSnifferInAction.jpg/600px-CodeSnifferInAction.jpg 2x"
data-file-width="995" data-file-height="641" width="300" height="193" />
<a href="https://docs.joomla.org/File:CodeSnifferInAction.jpg"
class="internal" title="Enlarge"></a>PHP Code Sniffer in action

## Pourquoi ?

- Une pratique de codage cohérente rend les fichiers plus
  professionnels. Les styles en conflit dans le même projet (ou, pire,
  dans le même fichier) ne sont pas seulement négligés, ils encouragent
  encore à plus de négligence.
- Lorsque tout le code est conforme à la même norme, il est plus facile
  pour tout le monde de détecter un code incorrect.
- Il est ainsi plus facile pour un nouveau venu dans un fichier du
  projet de rechercher et de corriger des erreurs ou d’étendre les
  fonctionnalités.
- Si aucune norme cohérente n'est maintenue, les développeurs
  reformatent parfois le code pour l'adapter à leurs besoins. Cela
  provoque une large gamme de modifications dans le référentiel de code.
  S'il y a un problème ultérieur, un changement significatif pourrait
  être perdu dans la zizanie produite par un diff.

## 1. Installation de PHP Code Sniffer (phpcs)

### Composer

PHP Code Sniffer est disponible via Composer. Il peut être installé à
l'échelle du système à l'aide de la commande suivante:  
`composer global require --dev squizlabs/php_codesniffer "~3.5"` Sous
**Linux**, PHP Code Sniffer (phpcs) sera installé dans votre dossier
utilisateur:

- Lien symbolique: `~/.composer/vendor/bin/phpcs`
- Le fichier:
  `~/.composer/vendor/squizlabs/php_codesniffer/scripts/phpcs`

Pour créer ce lien symbolique, utilisez sur la ligne de
commande:` $ sudo ln -s ~/.config/composer/vendor/squizlabs/php_codesniffer/scripts /usr/bin/phpcs `

ou (par exemple OpenSuse)

- Lien symbolique: `~/.config/composer/vendor/bin/phpcs`
- Le fichier:
  `~/.config/composer/vendor/squizlabs/php_codesniffer/scripts/phpcs`

Sous **Windows**, PHP Code Sniffer (phpcs) sera installé dans votre
dossier utilisateur:

- Lien symbolique:
  `c:\Users\Username\AppData\Roaming\Composer\vendor\bin\phpcs`
- Le fichier:
  `c:\Users\Username\AppData\Roaming\Composer\vendor\squizlabs\php_codesniffer\scripts\phpcs`

### Ajouter le répertoire bin de Composer à votre \$PATH

Pour exécuter PHP Code sniffer depuis n'importe où en ligne de commande
Linux ou Mac, vous devez ajouter `.composer/vendor/bin` à votre variable
\$PATH. Pour ce faire, éditez de manière persistante le fichier de
démarrage de votre shell, comme votre fichier .profile ou .bash_profile,
selon le shell que vous utilisez (echo \$SHELL vous indique celui que
vous utilisez).

Ajoutez la ligne suivante au fichier:
`export PATH=$PATH:~/.composer/vendor/bin`

Vous pouvez également créer un lien symbolique dans le dossier bin:
`ln -s ~/.composer/vendor/bin/phpcs /usr/local/bin/phpcs`

## 2. Installation de Joomla Coding Standards

Pour que PHP Code Sniffer (phpcs) recherche vos projets Joomla afin
d'obtenir le style de code approprié, vous devez installer les standards
de codage Joomla.

En utilisant Composer, nous pouvons exécuter:
`composer global require --dev joomla/coding-standards "~3.0@alpha"`

Notez que cela indique `@alpha` , car la norme de codage est toujours à
l'état alpha et nous devons autoriser l'installation du code d'état
alpha. Si nous n'incluons pas le `@alpha`, les normes de codage ne
seront pas installées et généreront cette erreur.

    Your requirements could not be resolved to an installable set of packages.

      Problem 1
        - The requested package joomla/coding-standards ~3.0 is satisfiable by joomla/coding-standards[3.0.0-alpha, 3.0.0-alpha2, 3.x-dev] but these conflict with your requirements or minimum-stability.

Une fois l'installation terminée, la norme de codage sera installée dans
le dossier global composer. L'emplacement de ce dossier dépend de votre
système d'exploitation. Certains endroits populaires sont:

**Mac / Linux**

    ~/.composer/vendor/joomla/coding-standards/

    ~/.config/composer/vendor/joomla/coding-standards/

**Windows**

    c:\Users\Username\AppData\Roaming\Composer\vendor\joomla\coding-standards\

Ceci utilise Composer sous Windows.

Si vous ne le trouvez pas dans l'un des dossiers ci-dessus, notez que le
dossier que vous recherchez se termine par
**\joomla\coding-standards\\**. Enfin, nous devons indiquer à Code
Sniffer que les normes de codage Joomla existent.

**Installer les normes de codage Joomla**

1.  Vérifiez si certains chemins sont déjà définis en exécutant
    `phpcs --config-show`
2.  Vous pouvez obtenir une réponse qui ressemble à ceci:
    `installed_paths: /path/to/installation`
3.  A l'étape 2, si vous voyez le **installed_paths**, vous devez le
    copier.
4.  Définissez le chemin des standards de codage Joomla dans phpcs en
    exécutant
    `phpcs --config-set installed_paths [/to/path1],[/to/path2],[/Users/user/.composer/vendor/joomla/coding-standards]`.
    Si vous avez copié des chemins à l'étape 2, incluez-les ici
    également en les séparant par une virgule. La commande ressemble à
    ça
    `phpcs --config-set installed_paths [/to/path1],[/to/path2],[/Users/user/.composer/vendor/joomla/coding-standards]`
5.  Vérifiez que le chemin est défini correctement en exécutant:
    `phpcs --config-show`
6.  Vérifiez que phpcs peut voir le style de code Joomla en lançant:
    `phpcs -i`
7.  La sortie devrait ressembler à ceci:
    `The installed coding standards are MySource, PEAR, PHPCS, PSR1, PSR2, Squiz, Zend and Joomla`

## Utilisation

Vous appelez la norme personnalisée par
`phpcs --standard=Joomla file/to/sniff`

Pour tester un fichier de plateforme en utilisant les normes de codage
de plateforme fournies, utilisez
`phpcs --standard=build/phpcs/Joomla path/to/file/or/folder`

Une documentation supplémentaire sur l'utilisation de phpcs est
disponible à l'adresse suivante:
<a href="http://pear.php.net/package/PHP_CodeSniffer/docs"
class="external autonumber" target="_blank"
rel="nofollow noreferrer noopener">[1]</a>

## 3. Integration IDE

Tout le monde aime la console. C'est sans aucun doute le moyen le plus
efficace de faire ce que vous devez faire. Parfois, même les gourous de
Linux ont besoin d'un peu de confort.

Heureusement, il existe un plug-in disponible pour PhpStorm, Eclipse et
Netbeans qui intègre CodeSniffer dans votre IDE préféré. Ainsi, toutes
les violations standard de codage sont affichées comme des erreurs
"normales".

### PhpStorm

Code Sniffer est pris en charge immédiatement dans PhpStorm. Allez dans
Paramètres et sous Editeur  **→**  Inspections, vous verrez la liste des
renifleurs que vous avez installés.

#### Définir le chemin d'accès à Code Sniffer

1.  Open Settings (CTRL-ALT-S / CMD-,)
2.  Go to Languages & Frameworks
3.  Click on PHP
4.  Click on Quality Tools
5.  Click on PHP cs fixer dropdown arrow
6.  The configuration is set to Local by default
7.  Click on the 3 dots behind it to open the configuration screen
8.  The first option is the PHP Code Sniffer (phpcs) path
9.  Click on the 3 dots behind the path to select the location of the
    phpcs file. See above on where phpcs may be installed on your site
10. Click on Validate to make sure the path is correct and phpcs is
    working
11. Click OK

##### Activer le Style de Code Joomla

1.  Open Settings (CTRL-ALT-S / CMD-,)
2.  Go to Editor
3.  Click on Inspections
4.  In the list, go to PHP
5.  Click on PHP Code Sniffer Validation
6.  Click on the check box behind it to activate it
7.  Click the Reload button (2 arrows) to force a reload of rules from
    disk
8.  Joomla should now be available in the list. See following image:<img
    src="https://docs.joomla.org/images/thumb/9/9c/PhpStorm8CodeSniffer.png/500px-PhpStorm8CodeSniffer.png"
    decoding="async"
    srcset="https://docs.joomla.org/images/thumb/9/9c/PhpStorm8CodeSniffer.png/750px-PhpStorm8CodeSniffer.png 1.5x, https://docs.joomla.org/images/thumb/9/9c/PhpStorm8CodeSniffer.png/1000px-PhpStorm8CodeSniffer.png 2x"
    data-file-width="2446" data-file-height="1624" width="500" height="332"
    alt="PHPStorm CodeSniffer" />
9.  Click OK

#### PHP PSR-0, PSR-1 et PSR-2

- <a
  href="https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md</a>
- <a
  href="https://github.com/pmjones/fig-standards/blob/psr-1-style-guide/proposed/PSR-1-basic.md"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/pmjones/fig-standards/blob/psr-1-style-guide/proposed/PSR-1-basic.md</a>
- <a
  href="https://github.com/pmjones/fig-standards/blob/psr-1-style-guide/proposed/PSR-2-advanced.md"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/pmjones/fig-standards/blob/psr-1-style-guide/proposed/PSR-2-advanced.md</a>

##### Using PHP PSR-1 and PSR-2 Sniff

A CodeSniffer sniff to check against the PSR-x Coding Standard can be
used as well. In that case you can select a different standard than
Joomla.

##### Utiliser le Style de Code Joomla

C'est bien de pouvoir vérifier que les normes sont respectées. C'est
encore mieux si PhpStorm vous aide à formater correctement pendant le
codage. To use the Joomla code style in PhpStorm

1.  Open Settings (CTRL-ALT-S / CMD-,)
2.  Go to Editor
3.  Click on Code Style
4.  Select Joomla from the Scheme select box

#### PhpStorm / Méthode Alternative

**NOTE**: This method is **outdated**, but it might be useful if you are
looking for a different integration. It also demonstrates the use of
external tools in PhpStorm - so it *shouldn't be deleted*.

The Code Sniffer can also be integrated easily as an external tool.
PhpStorm will display the output in the console, including click-able
links containing line and column numbers to the files that contain
errors.

- Click on "Settings" and search for "External tools"
- Click "Add..."
- Choose a "Name", "Group" and "Description".
- Click "Output Filters"
  - Click "Add...", Choose a name and enter under "Regular expression to
    match output" the value: `$FILE_PATH$:$LINE$:$COLUMN$`
- "Program": Search for the phpcs executable on your system. You have to
  set the path to the *phpcs.bat* from the installed PHP_CodeSniffer
  PEAR package
  - For Unix based systems, the path is something like /usr/bin/phpcs
  - In XAMPP (windows), you can find the file in the PHP root folder
    (e.g. C:\xampp\php\phpcs.bat)
- "Parameters":
  - `--standard=/build/phpcs/Joomla` The path to the Joomla! coding
    standards.
  - `--report=emacs` The will generate a simple list containing links to
    the error files
  - Optionally you may want to specify `-p` for "progress" or `-n` for
    "errors only".
  - The last parameter has to be `$FilePath$` specifying the file or
    folder you want to sniff.

A typical "Parameters" line on a Linux system might look like this:

    -np --standard=/home/elkuku/libs/joomla/build/phpcs/Joomla --report=emacs $FilePath$

You may now right click any file or folder and choose the sniffer from
the context menu or add a new tool bar button with a nice Joomla! logo
<img src="https://docs.joomla.org/images/6/67/Icon-16-joomla.png"
decoding="async" data-file-width="16" data-file-height="16" width="16"
height="16" alt="Icon-16-joomla.png" />.

### Netbeans

Netbeans has the sniffer functionality integrated into the core system.

1.  Start your Netbeans IDE
2.  Open Tools =\> Options =\> PHP =\> Code Analysis =\> Code Sniffer
3.  You have to set the path to *phpcs.bat* from the installed
    PHP_CodeSniffer PEAR package
    - For Unix based systems the path is something like /usr/bin/phpcs
    - In XAMPP (windows) you can find the file in the PHP root folder
      (e.g. C:\xampp\php\phpcs.bat)
4.  As "Default Standard," choose "Joomla" to use the Joomla! standard
5.  Now you can click *OK* to start sniffing
6.  Open from the top menu Source =\> Inspect...
7.  Enjoy

### Eclipse

<img
src="https://docs.joomla.org/images/thumb/4/44/Eclipse_pti.png/200px-Eclipse_pti.png"
class="thumbimage" decoding="async"
srcset="https://docs.joomla.org/images/thumb/4/44/Eclipse_pti.png/300px-Eclipse_pti.png 1.5x, https://docs.joomla.org/images/thumb/4/44/Eclipse_pti.png/400px-Eclipse_pti.png 2x"
data-file-width="1388" data-file-height="762" width="200"
height="110" />
<a href="https://docs.joomla.org/File:Eclipse_pti.png" class="internal"
title="Enlarge"></a>1) Eclipse PTI

Installation is a breeze and follows the usual pattern:

1.  `Help => Install New Software...`
2.  `Work with:` Fill in one of the update site URLs found here:
    <a href="http://www.phpsrc.org/eclipse/pti/" class="external free"
    target="_blank"
    rel="nofollow noreferrer noopener">http://www.phpsrc.org/eclipse/pti/</a>
3.  Select the desired tools
4.  Restart Eclipse.

<img
src="https://docs.joomla.org/images/thumb/a/a1/Eclipse_pti_settings.png/150px-Eclipse_pti_settings.png"
class="thumbimage" decoding="async"
srcset="https://docs.joomla.org/images/thumb/a/a1/Eclipse_pti_settings.png/225px-Eclipse_pti_settings.png 1.5x, https://docs.joomla.org/images/thumb/a/a1/Eclipse_pti_settings.png/300px-Eclipse_pti_settings.png 2x"
data-file-width="973" data-file-height="819" width="150" height="126" />
<a href="https://docs.joomla.org/File:Eclipse_pti_settings.png"
class="internal" title="Enlarge"></a>2) Paramètres Eclipse PTI

You are now able to sniff for code violations against common standards
like PEAR or Zend etc.

To sniff against your own standards, specify their location and activate
them (see screen shot 2)

1.  `Window => Preferences`
2.  `PHP Tools => PHP CodeSniffer`

Joyeux reniflage.

### Geany

- Open a PHP file. (Otherwise the build menu is not accessible.) See
  this <a
  href="http://static.xscreenshot.com/2016/07/13/08/screen_6bc0692cf995702a1e379b39643d0c2d"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Screenshot</a>
- On the top menu, select Build-\>Set Build Commands.
- Select the second field and name it as you wish. Enter this code in
  the Command:
  `phpcs --standard=Joomla "%f" | sed -e 's/^/%f |/' | egrep 'WARNING|ERROR'`
- Enter this code in the Error Regular Expression field:
  `(.+) [|]\s+([0-9]+)` See this
  <a href="http://view.xscreenshot.com/ef00820cf7c017ce659c8e9f0b02d3ae"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Screenshot</a>
- Select OK.
- If the Message Window is not open, display it by selecting it in the
  top View menu.
- When viewing any PHP file, press F9 to see the errors found.
  <a href="http://view.xscreenshot.com/8704b9fd1bda3f841364a0ffa28a54ae"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Screenshot</a>

### Atom

- Installez phpcs et les standards Joomla comme décrit ci-dessus.
- Dans **Atom \> File \> Settings \> Install** installer
  **linter-phpcs** ainsi que toute ses dépendances.
- Dans **Atom \> File \> Settings \> Packages \> linter-phpcs \>
  Settings** ajuster
  - **Executable Path** pour le chemin de votre phpcs
  - **Code Standard Or Config File**: Joomla
  - **Tab Width**: 4

### Références

- <a href="https://github.com/joomla/coding-standards"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla/coding-standards</a> -
  Joomla! Coding Standards
- <a href="http://www.phpsrc.org/" class="external free" target="_blank"
  rel="nofollow noreferrer noopener">http://www.phpsrc.org/</a> PTI -
  PHP tools integration pour Eclipse
- <a href="http://sourceforge.net/projects/phpmdnb/" class="external free"
  target="_blank"
  rel="nofollow noreferrer noopener">http://sourceforge.net/projects/phpmdnb/</a>
  Netbeans plugin
- <a
  href="http://hakre.wordpress.com/2010/03/06/php-code-sniffer-eclipse-and-wordpress/"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">http://hakre.wordpress.com/2010/03/06/php-code-sniffer-eclipse-and-wordpress/</a> -
  Excellent article. Il suffit de changer "Wordpress" pour "Joomla!" ;)
