<!-- Filename: J4.x:Developer:_Required_Software / Display title: Installer Joomla -->

## Résumé

Ceci est juste un court résumé des étapes impliquées :

- **Téléchargez** la dernière version complète depuis la page [Téléchargements Joomla](https://downloads.joomla.org/).
- **Enregistrez** le fichier téléchargé dans votre répertoire racine ou un sous-dossier de la racine.
- **Décompressez** le fichier téléchargé sur place. Certains systèmes d'exploitation créeront un dossier avec le même nom que le fichier zip téléchargé, moins le zip. C'est bien - vous pouvez simplement renommer le dossier avec un nom court et le déplacer si nécessaire. D'autres systèmes d'exploitation décompresseront les fichiers et dossiers sur place. Cela pose problème si vous avez mis le téléchargement dans votre dossier racine où il y a des dossiers existants avec d'autres sites. Vous devrez créer un nom de dossier court et y déplacer tous les nouveaux fichiers et dossiers extraits. L'objectif est d'obtenir un dossier accessible avec votre navigateur web via localhost/j4test.
- **Installez** en pointant votre navigateur sur localhost/j4test et en remplissant les formulaires d'installation Joomla.

## Configuration

Quelques suggestions :

- **Configuration Globale / Site / Cookie / Chemin du Cookie** défini sur le dossier contenant votre installation Joomla (/j4test).
- **Configuration Globale / Système / Débogage / Système de Débogage** défini sur Oui.
- **Configuration Globale / Système / Durée de Session** définie sur 60 - sinon vous serez déconnecté pendant que vous réfléchissez.
- **Configuration Globale / Serveur / Serveur / Rapport d'erreurs** défini sur Maximum.
- **Plugin Système - Débogage / Plugin / Actualiser les ressources** défini sur Non sauf si vous déboguez le CSS ou le JavaScript.

C'est tout. Si Joomla fonctionne, vous êtes prêt à l'utiliser pour le développement d'extensions.

## Plus de sites

Vous pouvez installer autant de sites que vous le souhaitez sur un seul ordinateur. Selon la façon dont vous avez configuré votre serveur, cela peut se faire via différents sous-dossiers accessibles via localhost/test1, localhost/test2, etc., ou par des hôtes virtuels accessibles par test1.localhost, test2.localhost, etc. Dans les deux cas, vous pouvez avoir une nouvelle base de données et un nouveau site Joomla vierge en quelques minutes. Choisissez des noms courts significatifs ! *test1* et *test2* deviendront bientôt confus.

## Installation pour les tests

Il existe une procédure différente si vous souhaitez aider avec les tests de Joomla. Dans ce cas, vous devez installer **git** et suivre les instructions dans le [dépôt GitHub](https://github.com/joomla/joomla-cms).

- Dans GitHub, sélectionnez la branche de Joomla sur laquelle travailler. Cela modifie les instructions visibles plus bas sur la page.
- Sur votre ordinateur portable ou de bureau, suivez les instructions à partir de *Cloner le dépôt :* et suivantes.
- Ne supprimez pas le répertoire d'installation à la fin de l'installation de Joomla !
- Renommez le clone de joomla-cms à autre chose pour pouvoir tout refaire plus tard.
- Installez le Joomla Patchtester.

*Traduit par openai.com*

