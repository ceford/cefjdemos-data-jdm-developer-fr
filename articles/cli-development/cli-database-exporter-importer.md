<!-- Filename: J4.x:CLI_Database_Exporter_Importer / Display title: CLI Exportateur / Importateur de base de données -->

Joomla!  4.0

## À propos

Avant de mettre à jour Joomla! ou [d'installer une extension
tierce](https://docs.joomla.org/J4.x:CLI_Update "Special:MyLanguage/J4.x:CLI Update"),
il est fortement recommandé de sauvegarder votre site.  
La Console Joomla! 4.x fournit des commandes pour exporter (sauvegarder)
et importer (restaurer) votre base de données Joomla!. A noter que cette
opération ne réalise pas de sauvegarde de votre système de fichiers ;
ceci est à faire à part.

## Pré-requis

Pour utiliser ces commandes, vous avez besoin d'un accès via un shell
sécurisé (SSH) à votre hébergeur auquel PHP CLI (interface en ligne de
commande) est installé. Prenez en considération d'avoir quelques
connaissances élémentaires d'usage des commandes shell.

## Instructions

Se connecter à votre hébergeur et aller au dossier racine de votre
site.  
Je recommande d'utiliser le dossier 'tmp' de Joomla dans votre pour
avoir les autorisations en lecture/écriture.

- Lister toutes les commandes disponibles de la Console Joomla:  
  `php cli/joomla.php list`
- Exporter la base de données dans le dossier:  
  `php cli/joomla.php database:export --all --folder `
- Importer la base de données à partir du dossier:  
  `php cli/joomla.php database:import --all --folder `

Vous pouvez aussi:

- Exporter la base de données sous forme de fichier .zip:  
  `php cli/joomla.php database:export --all --zip`
- Exporter une table spécifique:  
  `php cli/joomla.php database:export --table `
- Exporter une table comme un fichier .zip:  
  `php cli/joomla.php database:export --table --zip`
- Importer une table:  
  `php cli/joomla.php database:import --table `
- Si vous avez besoin d'aide:  
  `php cli/joomla.php database:export --help`
  `php cli/joomla.php database:import --help`

## Sauvegarder et restaurer

Pour faire une sauvegarde complète (avec les dossiers, les fichiers et
la base de données) de votre site, vous pouvez exécuter ces commandes:

1.  Archiver votre répertoire racine de Joomla:  
    `tar --exclude='./tmp/joomla_bak.*' -zcvf tmp/joomla_bak.tgz . > tmp/joomla_bak.log`
2.  Exporter toute la base de données Joomla:  
    `php cli/joomla.php database:export --all --folder tmp/db_bak`

Et pour la restaurer, exécutez ces commandes:

1.  Importer toute la base de données Joomla:  
    `php cli/joomla.php database:import --all --folder tmp/db_bak`
2.  Extraire l'archive:  
    `tar --recursive-unlink -xvf tmp/joomla_bak.tgz .`
