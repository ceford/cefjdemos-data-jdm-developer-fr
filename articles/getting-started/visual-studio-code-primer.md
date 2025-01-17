<!-- Filename: Visual_Studio_Code_Primer / Display title: Visual Studio Code -->

## VS Code - Un IDE Gratuit Populaire

Depuis [Wikipedia](https://en.wikipedia.org/wiki/Visual_Studio_Code) :

> Visual Studio Code, également couramment appelé VS Code, est un éditeur de code source créé par Microsoft pour Windows, Linux et macOS. Les fonctionnalités incluent la prise en charge du débogage, la coloration syntaxique, la complétion de code intelligente, les snippets, le refactoring de code, et Git intégré. Les utilisateurs peuvent changer le thème, les raccourcis clavier, les préférences, et installer des extensions qui ajoutent des fonctionnalités supplémentaires.

## Installation

La page par défaut du site [VS Code](https://code.visualstudio.com/) comporte une liste déroulante pour chaque plateforme prise en charge. Il est probable que votre plateforme soit présélectionnée. Téléchargez donc, installez et vous êtes prêt à partir.

### Commencer

La page *Commencer* de VS Code comporte quelques éléments *Début*, une liste d'éléments *Récents* et une courte liste de *Guides*. Si vous êtes totalement novice avec VS Code, il est recommandé de les consulter. Cela ne prend que quelques minutes.

La documentation de VS Code est accessible depuis le menu *Aide / Documentation*. Les vidéos d'introduction valent vraiment le détour. Chacune dure de 2 à 6 minutes et offre une excellente introduction aux fonctionnalités de VS Code :

[Vidéo VS Code](https://code.visualstudio.com/docs/getstarted/introvideos)

La documentation officielle est l'endroit où aller si vous souhaitez rechercher des informations spécifiques.

### Extensions VS Code

VS Code peut être utilisé pour tout type de texte, y compris une large gamme de langages de programmation. Il fonctionne avec JavaScript sans ajouter d'extensions. D'autres langages sont détectés par le contexte, donc si vous commencez à créer et à enregistrer du code PHP, vous serez probablement invité à installer un pack de prise en charge PHP.

Cliquez sur l’icône *Extensions* dans la *Barre d'Activités* à gauche pour voir ce que vous avez installé et ce qui est recommandé. Vous aurez besoin de l'extension PHP Debug !

### La disposition de l'écran VS Code

Certains termes utilisés dans les instructions suivantes :

- **Barre d'activité :** la barre étroite à gauche de l'écran. Sélectionnez une icône pour ouvrir ou fermer la barre latérale principale.
- **Barre latérale principale :** lorsqu'elle est ouverte, elle affiche les détails de l'activité sélectionnée.
- **Barre d'état :** en bas de l'écran. Elle montre ce qui se passe.
- **Panneau :** une zone sous les éditeurs de texte pour afficher d'autres informations.

Sélectionnez une icône de mise en page en haut à droite pour ouvrir ou fermer l'un de ces éléments.

## Programmer une extension Joomla

Pour créer une extension, votre objectif est de créer un fichier zip que vous pouvez installer sur un site Joomla fonctionnel. Vous avez donc besoin d'un dossier pour contenir votre code. Celui-ci doit être dans votre espace de fichiers personnel sur votre ordinateur portable ou de bureau utilisé pour le développement local. Il ne doit pas être dans l'arborescence de votre site web. Par exemple, vous pourriez utiliser *~/jextensions* pour contenir des sous-dossiers pour différentes extensions. J'utilise *~/git* parce que c'est court et facile à épeler, bien que potentiellement déroutant car chaque sous-dossier utilise un dépôt git séparé.

### Exemple de code

Si vous souhaitez du code d'exemple sur lequel travailler, une extension est disponible sur GitHub nommée *mod_debugme*. Comme son nom l'indique, c'est un module avec quelques bugs. En plus du code du module, il y a un fichier *build.xml* pour illustrer une manière d'automatiser la construction pour les tests et la création d'un fichier zip.

Le module est conçu pour afficher les quelques prochains événements (3 par défaut) (anniversaires) à partir d'une liste stockée dans une table de base de données. Vous pouvez imaginer cela être utilisé dans un site de bureau ou de famille dans l'attente d'un gâteau.

Il peut être préférable de commencer en utilisant les commandes git à partir de la ligne de commande. Créez d'abord un dossier pour votre code, puis clonez le dépôt distant :

```sh
    mkdir ~/git
    cd ~/git
    git clone https://github.com/ceford/j4xdemos-mod-debugme
```

La réponse devrait prendre juste quelques secondes :

```sh
    Cloning into 'j4xdemos-mod-debugme'...
    remote: Enumerating objects: 23, done.
    remote: Counting objects: 100% (23/23), done.
    remote: Compressing objects: 100% (16/16), done.
    remote: Total 23 (delta 3), reused 23 (delta 3), pack-reused 0
    Unpacking objects: 100% (23/23), done.
```

Prenez un moment pour regarder le contenu du dossier :

```sh
    cd j4xdemos-mod-debugme
    ls -al
    total 16
    drwxr-xr-x   6 ceford  staff   192  2 Sep 17:48 .
    drwxr-xr-x   3 ceford  staff    96  2 Sep 17:48 ..
    drwxr-xr-x  12 ceford  staff   384  2 Sep 17:48 .git
    -rw-r--r--   1 ceford  staff  1402  2 Sep 17:48 README.md
    -rw-r--r--   1 ceford  staff   927  2 Sep 17:48 build.xml
    drwxr-xr-x   8 ceford  staff   256  2 Sep 17:48 mod_debugme
```

Le dossier *.git* contient des informations sur le dépôt. Le fichier *README.md* est un document markdown qui décrit ce dépôt. Le fichier *build.xml* est un fichier utilisé pour construire l'extension avec un outil externe, Phing - décrit plus tard. Le dossier *mod_debugme* contient le code de l'extension.

### Installer dans Joomla

Compressez le dossier d'extension pour créer un fichier zip installable :

```sh
    zip -r mod_debugme.zip mod_debugme
```

Vous pouvez maintenant installer le fichier zip sur le site Joomla que vous utilisez pour les tests. Après l'installation, vous devez créer un module de site et l’assigner à une position de module. Étant donné que c'est un module en cours de développement, vous pourriez l'assigner à une position sur *Toutes les pages* pendant que vous y travaillez ; ou vous pourriez l'assigner à une position sur une seule page ; ou le positionner dans un article qui a son propre élément de menu.

Après l'installation, supprimez le fichier zip.

### Activer le mode débogage

Dans la Configuration Globale de Joomla, réglez *Système de débogage* sur *Oui* et *Rapport d'erreurs* sur *Maximum*.

Lorsque vous ouvrez une page contenant le module défectueux, vous verrez une trace de pile indiquant où une erreur a été déclenchée.

![Stack trace](../../../en/images/getting-started/vscode-primer-stack-trace.png)

Parfois, l'erreur de codage se situe sur la première ligne de la trace de la pile. Sinon, si l'erreur est déclenchée dans le code de la bibliothèque, par exemple en transmettant des données invalides à une fonction de base de données, l'erreur de codage peut se trouver plus loin dans la liste des appels de fonction.

## Ouvrir le dossier d'extension dans VS Code

Dans VS Code, utilisez l'élément de menu Fichier / Ouvrir un dossier pour localiser et ouvrir le dossier contenant votre copie locale du code de l'extension *mod_debugme*. Vous devriez voir quelque chose de similaire à ce qui suit :

![VS Code screen](../../../en/images/getting-started/vscode-primer-screen.png)

Vous pourrez peut-être diagnostiquer le problème simplement en lisant le code. Dans le cas de l'erreur *Classe "DebugHelper" non trouvée*, vous verrez qu'une instruction *use* a été commentée quelques lignes plus haut. Oublier d'insérer une instruction *use* est une erreur courante lors du développement initial !

Corrigez ce problème, puis compressez et installez à nouveau le module. Cette étape devient un peu fastidieuse lorsque vous avez plusieurs problèmes. C'est là que les outils de construction deviennent utiles.

## L'outil de construction Phing

Phing est un outil en ligne de commande, disponible pour toutes les plateformes, utilisé pour créer des paquets logiciels en utilisant des instructions stockées dans un fichier xml, nommé build.xml par défaut. Pour travailler avec le code d'extension, il peut être utilisé pour faire deux choses :

- copiez les fichiers modifiés de votre dossier source d'extension vers les emplacements corrects dans votre dossier de site web.
- générez un nouveau fichier zip pour les nouvelles installations.

Téléchargez et installez [Phing](https://www.phing.info/). D'autres outils de construction sont disponibles ! Vous pouvez installer Phing dans votre propre dossier bin ou dans un dossier bin système. Vous devez noter le chemin vers votre code Phing. Dans cet exemple, c'est *~/bin/phing-latest.phar*. Vous pouvez l'essayer à partir de la ligne de commande après avoir changé de dossier pour celui contenant votre code d'extension :

```sh
    cd ~/git/j4xdemos-mod-debugme
    php ~/bin/phing-latest.phar
```

Réponse :

```sh
    Buildfile: /Users/ceford/git/j4xdemos-mod-debugme/build.xml

    mod_debugme > main:
          ... Any copied files will be mentioned here
          [zip] Building zip: /Users/ceford/zips/mod_debugme.zip

    BUILD FINISHED

    Total time: 0.0863 seconds
```

## Tâches VS Code

Pour exécuter Phing depuis VS Code, vous devez créer un fichier *tasks.json* dans le dossier *.vscode* à la racine du dossier *j4xdemos-mod-debugme*. Si ce dernier n'existe pas, créez-le d'abord. Ensuite, créez le fichier *tasks.json* et saisissez le code suivant :

```json
    {
        // See https://go.microsoft.com/fwlink/?LinkId=733558
        // for the documentation about the tasks.json format
        "version": "2.0.0",
        "tasks": [
          {
            "label": "Build mod_debugme",
            "type": "shell",
            "command": "php ~/bin/phing-latest.phar",
            "windows": {
              "command": "php ~/bin/phing-latest.phar"
            },
            "group": "build",
            "presentation": {
              "reveal": "always",
              "panel": "shared"
            }
          }
        ]
    }
```

Les utilisateurs de Windows doivent corriger la commande spécifique à Windows. Vous pouvez maintenant construire l'extension en utilisant le menu *Terminal / Exécuter la tâche de construction*. Le résultat de la commande devrait apparaître dans le panneau du terminal sous la zone de modification.

```sh
      *  Executing task: php ~/bin/phing-latest.phar

    Buildfile: /Users/ceford/git/gitdemo/j4xdemos-mod-debugme/build.xml

    mod_debugme > main:

          [zip] Nothing to do: /Users/ceford/zips/mod_debugme.zip is up to date.

    BUILD FINISHED

    Total time: 0.1031 seconds

     *  Terminal will be reused by tasks, press any key to close it.
```

Il peut y avoir des messages d'erreur incompréhensibles. La cause la plus probable est d'avoir des chemins invalides vers des dossiers dans le fichier *build.xml* ou un dossier n'a pas été créé. Juste un autre type de problème à déboguer !

## Débogage

Vous devriez être en mesure de corriger le premier bug par inspection du code. Les problèmes plus compliqués nécessitent de parcourir le code avec le débogueur. Cela vous permet d'inspecter les variables pour voir si elles contiennent les valeurs que vous attendez, par exemple lors du passage d'arguments aux fonctions de la bibliothèque.

### Paramètres *php.ini*

Pour configurer le débogage avec Xdebug, vous devez effectuer quelques entrées au début de votre fichier *php.ini*.

```sh
    zend_extension="xdebug.so"
    xdebug.mode="debug"
    xdebug.client_port=9003
    xdebug.start_with_request=yes
    xdebug.log_level=0
```

Après avoir enregistré toutes les modifications, redémarrez votre serveur Apache.

### Ajouter une fenêtre de site web

Votre dossier d'extension contient juste quelques fichiers, les ***sources*** des fichiers installés sur votre site web. Le débogage à l'exécution implique de définir des points d'arrêt dans vos fichiers de ***site***, vous avez donc besoin d'accéder à ces fichiers. Vous pourriez utiliser le menu *Fichier / Ajouter un dossier à l'espace de travail...* pour ajouter le dossier du site à votre espace de travail. Cependant, il est très probable que vous finissiez par apporter des modifications aux fichiers du site au lieu des fichiers sources. Il est donc probablement préférable d'ouvrir une fenêtre VS Code distincte pour le débogage.

- **Ouvrir une nouvelle fenêtre :** Dans le menu VS Code, sélectionnez *Fichier / Nouvelle fenêtre* et sélectionnez le dossier contenant votre site Joomla.
- **Ouvrir le dossier :** Dans la nouvelle fenêtre ouverte, sélectionnez *Fichier / Ouvrir le dossier...* dans le menu VS Code. Trouvez votre dossier de site et sélectionnez-le. Vous devriez voir une liste de tous les fichiers de votre site Joomla dans la barre latérale principale.

### Configuration de Lancement

Pour que le débogage fonctionne réellement dans VS Code, vous avez besoin d'une configuration de lancement. À la racine de votre site web, créez un dossier nommé *.vscode* (notez le point initial) contenant un fichier nommé *launch.json* avec le contenu suivant :

```json
    {
        "configurations": [
            {
                "name": "Listen for XDebug",
                "type": "php",
                "request": "launch",
                "hostname": "0.0.0.0",
                "port": 9003,
                "stopOnEntry": true,
                "pathMappings": {
                    "/Users/ceford/Sites/j421rc2": "${workspaceFolder}"
                }
            }
        ]
    }
```

N'oubliez pas de remplacer l'élément pathMappings dans cet exemple par les pathMappings réels sur votre propre site. L'élément stopOnEntry mettra en pause l'exécution sur la toute première ligne de code PHP exécutée.

### Déboguer *mod_debugme*

Vous êtes maintenant prêt à trouver et corriger les bogues dans le module installé.

- **Trouver le code du module :** Trouver le premier bug à la ligne 16 de JROOT/modules/mod_debugme/mod_debugme.php.
- **Définir un point d'arrêt :** Cliquez dans l'espace à gauche du numéro 16. Une tache rouge pâle apparaîtra lorsque vous survolerez et deviendra rouge vif après avoir cliqué pour indiquer qu'un point d'arrêt a été défini.
- **Lancer le débogage :** Dans le menu VS Code, sélectionnez *Exécuter / Démarrer le débogage*. Dans votre navigateur, rechargez votre site. Votre fenêtre VS Code devrait réapparaître avec le code arrêté à la première ligne du fichier *index.php* du site. En haut de l'écran se trouvent quelques icônes pour contrôler le processus de débogage. Elles devraient être explicites. Sinon, consultez l'Aide / Documentation de VS Code.
- **Continuer :** Sélectionnez le bouton continuer - le code s'exécutera jusqu'à votre premier point d'arrêt. Examinez le code pour voir quel est le problème.
- **Survoler :** Si vous survolez une variable à laquelle une valeur a été attribuée, une petite Info-bulle apparaîtra résumant les attributs de cette variable. Il n'y a pas d'Info-bulle pour les variables auxquelles aucune valeur n'a été attribuée.
- **Variables :** La colonne de gauche contient plus d'informations sur l'état du code au point d'arrêt. Il y en a trop pour toutes les couvrir ici. Explorez-les selon vos besoins !
- **Arrêter le débogage :** Il est probablement préférable de sélectionner l'icône Continuer, sinon la page Web est livrée blanche. Sinon, vous pouvez utiliser le bouton Arrêter ou le menu Exécuter / Arrêter le débogage.

### Corriger un bogue

**Rappelez-vous :** Ne corrigez pas le bug dans le code du site web ! Corrigez-le dans le code source !

Corrigez le code source, puis utilisez *Terminal / Exécuter la tâche de build...*.

Redémarrer le débogage.

### Conseils

Quelques problèmes pas si évidents :

- Vous corrigez le premier bogue mais il plante toujours sur cette ligne. Consultez le fichier mod_debugme.xml pour voir où la source des classes avec espace de noms est définie. Une fois corrigé dans la source, vous devez réinstaller le fichier zip ou supprimer *administrator/cache/autoload_psr4.php*. Lorsqu'il est absent, Joomla reconstruit ce fichier à partir des fichiers manifest. Mais s'il contient une entrée incorrecte ou manquante, il ne sera pas corrigé tant que l'extension n'est pas réinstallée.
- Parfois, vous devez définir un point d'arrêt quelques lignes avant la ligne où l'erreur se produit, surtout si vous souhaitez vérifier les valeurs passées aux appels de fonction.
- La table *xxx.yyy\\debugme* n'existe pas. Ah oui, le code pour créer une table lors de l'installation et la supprimer lors de la désinstallation n'a jamais été créé. Vous devrez exécuter une requête SQL dans phpMyAdmin en utilisant le contenu du fichier *mod\\debugme.sql*. N'oubliez pas de changer *\#\\* dans les noms de table pour le préfixe de votre base de données. Et lorsqu'il échoue encore, vérifiez le nom de la table dans le code.

## Capture d'écran

Lorsque tout est réparé, voici ce que vous pourriez voir :

![Site view of debugged module working](../../../en/images/getting-started/vscode-primer-debugme-fixed.png)

Jours de gâteau ?

## Références

De la documentation Joomla! : [Visual Studio Code](https://docs.joomla.org/Visual_Studio_Code "Visual Studio Code") qui couvre également la configuration d'autres outils, par exemple CodeSniffer et PHPUnit.

*Traduit par openai.com*

