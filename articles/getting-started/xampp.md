<!-- Filename: XAMPP / Display title: XAMPP -->

## Introduction

XAMPP est un package facile à installer qui regroupe le serveur web Apache, PHP, XDEBUG et la base de données MySQL. Cela vous permet de créer l'environnement nécessaire pour faire fonctionner Joomla! sur votre machine locale. La dernière version de XAMPP est disponible sur le <a href="http://www.apachefriends.org/en/index.html" class="external text" target="_blank" rel="nofollow noreferrer noopener">site web de XAMPP</a>. Les téléchargements sont disponibles pour Linux, Windows, Mac OS X et Solaris. Téléchargez le package pour votre plateforme.

*Remarque importante concernant XAMPP et Skype :* Apache et Skype utilisent tous deux le port 80 comme alternative pour les connexions entrantes. Si vous utilisez Skype, allez dans le panneau Outils-Options-Avancé-Connexion et désélectionnez l'option "Utiliser les ports 80 et 443 comme alternatives pour les connexions entrantes". Si Apache démarre en tant que service, il prendra le port 80 avant que Skype ne démarre et vous ne verrez pas de problème. Mais, pour être sûr, désactivez l'option dans Skype.

### Installation sur Windows

L'installation pour Windows est très simple. Vous pouvez utiliser l'installateur exécutable XAMPP (par exemple, "xampp-windows-x64-7.4.4-0-VC15-installer.exe"). Des instructions d'installation détaillées pour Windows sont disponibles <a href="https://www.apachefriends.org/download.html" class="external text" target="_blank" rel="nofollow noreferrer noopener">ici</a>.

Si vous êtes sur Windows XP ou 2003, ils ne sont pas pris en charge par le package principal, mais il existe des versions compatibles de XAMPP pour ces plates-formes répertoriées sur la page de téléchargement (mais vous ne pourrez exécuter que PHP 5.4 ou inférieur - et donc vous ne pourrez tester que Joomla 3.x et inférieur).

Pour Windows, il est recommandé d'installer XAMPP dans "c:\xampp" (et non dans "c:\program files"). Si vous faites cela, votre Joomla! (et tout autre dossier de site web local) ira dans le dossier "c:\xampp\htdocs". (Par convention, tout le contenu web se place dans le dossier "htdocs".)

Si vous avez plusieurs serveurs HTTP (comme IIS), vous pouvez changer le port d'écoute de XAMPP. Dans \apache\conf\httpd.conf, modifiez la ligne Listen 80 en Listen \[numérodeport\] (ex : "Listen 8080").

Tutoriel du magazine de la communauté Joomla

Vous pouvez trouver un tutoriel détaillé sur l'installation de XAMPP sur Windows, ainsi que Joomla 4 Beta, Joomla Patch Tester et Git dans cet <a href="https://magazine.joomla.org/all-issues/june-2020/github-installing-git" class="external text" target="_blank" rel="noreferrer noopener">article du Joomla Community Magazine</a>.

### Installation sur Linux

#### Installer XAMPP

Ouvrez le Terminal et entrez :

```bash
    sudo tar xvfz xampp-linux-1.7.7.tar.gz -C /opt
```

(remplacez *xampp-linux-1.7.7.tar.gz* par la version de xammp que vous avez téléchargée). Il a été rapporté que la base de données MYSQL de xampp 1.7.4 ne fonctionne pas avec Joomla 1.5.22.

Cela installe ... Apache2, mysql et php5 ainsi qu'un serveur ftp.

```bash
    sudo /opt/lampp/lampp start
```

et

```bash
    sudo /opt/lampp/lampp stop
```

démarre/arrête tous les services

#### Testez votre serveur localhost XAMPP

Ouvrez votre navigateur et allez à

```bash
    http://localhost
```

L'index.php redirigera vers

```bash
    http://localhost/xampp
```

Vous y trouverez des instructions sur la manière de changer les noms d'utilisateur/mots de passe par défaut. Sur un PC qui ne sert pas de fichiers à Internet ou au réseau local, changer les paramètres par défaut est une décision personnelle.

#### Obtenez Joomla

Téléchargez le dernier fichier zip d'installation de Joomla
<a href="https://www.joomla.org/download.html"
class="external autonumber" target="_blank"
rel="noreferrer noopener">[1]</a>

Décompressez sur votre disque dur

Se connecter à localhost avec un client FTP par défaut

```
    nobody
    lampp
```

Créez un dossier pour votre Joomla sur le serveur localhost

Téléchargez les fichiers d'installation Joomla décompressés vers le dossier Joomla nouvellement créé.

**Important :**

- L'installation de xammp configure la propriété correcte des fichiers et les permissions.
- Utiliser la **commande CHOWN** va **causer des problèmes de propriété avec xampp**.
- **Utiliser nautilus** pour manipuler des dossiers/fichiers en local va **causer des problèmes de propriété avec xampp**.

**Informations sur la base de données**

- Hôte : localhost
- Nom par défaut de la base de données : test
- Utilisateur par défaut de la base de données : root
- Il n'y a **pas** de mot de passe par défaut.

Le mot de passe Administrateur est votre choix.

Il est recommandé d'installer les données d'exemple pour l'utilisateur novice.

Après l'installation, supprimez le répertoire d'installation et pointez votre navigateur vers :

```
    http://localhost/yournewjoomlafolder
```

ou

```
    http://localhost/yournewjoomlafolder/administrator
```

#### Créer un lien dans le menu Ubuntu

**Pour créer une interface graphique pour xampp connectée au menu Ubuntu**

Ouvrez le Terminal et tapez

```bash
    sudo gedit /usr/share/applications/xampp-control-panel.desktop
```

Ensuite, copiez ce qui suit dans gedit et enregistrez.

```bash
[Desktop Entry]
Encoding=UTF-8
Name=XAMPP Control Panel
Comment=Start and Stop XAMPP
Exec=gksudo "python /opt/lampp/share/xampp-control-panel/xampp-control-panel.py"
Icon=/usr/share/icons/Tango/scalable/devices/network-wired.svg
Terminal=false
Type=Application
Categories=GNOME;Application;Network;
StartupNotify=true
```

Si le panneau de contrôle ne parvient pas à se lancer, essayez d'exécuter la commande Exec directement dans le terminal :

```bash
    gksudo "python /opt/lampp/share/xampp-control-panel/xampp-control-panel.py"
```

Si vous recevez l'erreur :

```bash
    Error importing pygtk2 and pygtk2-libglade
```

Installez les bibliothèques manquantes :

```bash
    sudo apt-get install python-glade2
```

#### Débogueur PHP XDebug

Le paquet XAMPP pour Linux n'inclut pas le débogueur PHP XDebug.  
Pour installer XDebug sur Debian ou Ubuntu :

- Installez le paquet *build-essential* :```bash
    sudo apt-get update
    sudo apt-get install build-essential
    sudo apt-get install autoconf
```

- Téléchargez le
<a href="http://www.apachefriends.org/en/xampp-linux.html"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">paquet de développement</a> pour votre
version de XAMPP et extrayez-le sur votre installation existante :```bash
    sudo tar xvfz xampp-linux-devel-1.7.7.tar.gz -C /opt 
```

- Compiler XDebug :```bash
    wget http://xdebug.org/files/xdebug-2.1.3.tgz
    tar xzf xdebug-2.1.3.tgz
    cd xdebug-2.1.3/
    /opt/lampp/bin/phpize
```

Après cela, vous aurez la sortie suivante sur votre console…

```bash
    Configuring for:
    PHP Api Version:         20090626
    Zend Module Api No:      20090626
    Zend Extension Api No:   20090626 

    ./configure --with-php-config=/opt/lampp/bin/php-config
    make
    sudo make install 
```

Ensuite, la sortie sera celle-ci.. veuillez surveiller le répertoire spécifié.

```bash
    Installing shared extensions:     /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/ 
```

Créez un dossier dans votre dossier temporaire qui contiendra le fichier de données généré par XDebug :

```bash
    sudo mkdir /opt/lampp/tmp/xdebug
    sudo chmod a+rwx -R /opt/lampp/tmp/xdebug 
```

Installations alternatives :

Installez en utilisant la bibliothèque communautaire d'extensions PHP (PECL) incluse avec xampp :

```bash
    sudo /opt/lampp/bin/pecl install xdebug
```

Sur Ubuntu/Debian, vous pouvez installer en utilisant :

```bash
    apt-get install php5-xdebug 
```

(avertissement : cela installera également Apache et PHP à partir des dépôts apt).

**Avertissement pour les utilisateurs 64 bits**

Lors de la compilation de XDebug ou de l'installation via apt-get, vous recevrez une erreur lors du (re)démarrage de xampp :

```bash
    /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/xdebug.so: wrong ELF class: ELFCLASS64
```

C'est parce que xampp fonctionne en 32 bits mais XDebug est en 64 bits. Pour surmonter ce problème, créez xdebug.so sur une machine 32 bits ou téléchargez-le depuis :

```bash
    http://code.activestate.com/komodo/remotedebugging/
```

Téléchargez le fichier : "PHP Remote Debugging Client" pour "Linux (x86)"
Extrayez le contenu du fichier sur votre ordinateur, ce fichier compressé
contient plusieurs dossiers avec des numéros de version ex : 4.4, 5.0, 5.1 ... 5.3
et ainsi de suite, allez dans le dossier avec le numéro de version le plus élevé ou celui qui fonctionne pour vous, puis copiez manuellement le fichier "xdebug.so" à l’emplacement suivant, remplacez-le si nécessaire.

```bash
    /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/
```

N'oubliez pas que cet emplacement pourrait être différent sur votre ordinateur.

### Installation sur Mac OS X

Mac OS X inclut déjà un serveur Apache prêt à l'emploi, mais la plupart des développeurs préféreront utiliser les outils intégrés et la configurabilité fournis par XAMPP.

Comme avec la plupart des programmes sur Mac, l'installation est un jeu d'enfant. Visitez <a href="https://www.apachefriends.org/en/download.html" class="external text" target="_blank" rel="nofollow noreferrer noopener">Apache Friends - Mac OS X</a> pour télécharger le binaire universel.

Une fois le fichier téléchargé, ouvrez simplement l'image disque et
glissez le dossier XAMPP vers le raccourci du dossier "Applications".

Pour démarrer le serveur, ouvrez "XAMPP Control.app" et appuyez sur le bouton démarrer à côté d'Apache.

##### Un peu de dépannage

De nombreux utilisateurs de Mac rencontrent une petite difficulté à ce stade lorsqu'ils essaient de configurer une autre instance d'Apache sur leur machine. Si vous ne pouvez pas démarrer l'Apache de XAMPP, vous avez deux options :  
**Vous pouvez changer le port d'écoute de XAMPP.** Dans
\Applications\XAMPP\xamppfiles\etc\httpd.conf, modifiez la ligne qui dit, "Listen 80" à Listen \[portNumber\]. Par exemple :

```bash
    Listen 8080
```

**Vous pouvez changer le port d'écoute du serveur Apache pré-installé.** Dans Finder, allez à "/etc" (CMD+SHIFT+G) ; à partir de là, vous pourrez naviguer à travers les fichiers Apache normalement cachés. Trouvez le dossier nommé Apache2, et modifiez le fichier "http.conf". Modifiez la ligne qui dit, "Listen 80" en Listen \[portNumber\]. Par exemple :

```bash
    Listen 8080
```

*Remarque : Si vous choisissez de changer le port du serveur Apache préinstallé, il se peut que vous deviez redémarrer votre ordinateur pour que les changements prennent effet. Vous devrez également vous authentifier en tant qu'administrateur pour modifier ces fichiers.*

### Tester l'installation de XAMPP

Une fois que XAMPP est installé et que vous avez démarré le service Apache avec l'outil XAMPP Control Panel, vous pouvez le tester en ouvrant votre navigateur et en naviguant vers "<a href="http://localhost" class="external free" target="_blank" rel="nofollow noreferrer noopener">http://localhost</a>". Vous devriez voir l'écran d'accueil de XAMPP similaire à celui ci-dessous.

<img
src="https://docs.joomla.org/images/thumb/f/fc/Phpinfo_on_xampp.png/800px-Phpinfo_on_xampp.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/f/fc/Phpinfo_on_xampp.png/1200px-Phpinfo_on_xampp.png 1.5x, https://docs.joomla.org/images/f/fc/Phpinfo_on_xampp.png 2x"
data-file-width="1498" data-file-height="883" width="800" height="472"
alt="Phpinfo sur xampp.png" />

Sélectionnez le lien appelé "phpinfo()" dans le menu du haut. Cela affichera un long écran d'informations sur la configuration PHP, comme indiqué ci-dessous.

<img
src="https://docs.joomla.org/images/thumb/d/db/Phpinfo.png/800px-Phpinfo.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/d/db/Phpinfo.png/1200px-Phpinfo.png 1.5x, https://docs.joomla.org/images/d/db/Phpinfo.png 2x"
data-file-width="1432" data-file-height="1282" width="800" height="716"
alt="Phpinfo.png" />

À ce stade, XAMPP est installé avec succès. Remarquez le "Fichier de configuration chargé". Nous allons éditer ce fichier dans la section suivante pour configurer XDebug.

*Traduit par openai.com*

