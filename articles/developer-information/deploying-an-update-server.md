<!-- Filename: Deploying_an_Update_Server / Display title: Mettre à jour les serveurs -->

## Contexte

Il y a une bonne description des serveurs de mise à jour dans la documentation pour programmeurs de Joomla! copiée sur [ce site](jdocmanual?article=docus/install-update/update-server) ou disponible dans la [source originale](https://manual.joomla.org/docs/building-extensions/install-update/update-server/).

## Dépannage

- **Le script de mise à jour SQL n'est pas exécuté lors de la mise à jour.**

Si le script de mise à jour SQL (par exemple, dans le dossier `sql/updates/mysql`) ne s'exécute pas lors du processus de mise à jour, cela pourrait être dû à l'absence de numéro de version dans la table `#_schemas` pour cette extension *avant la mise à jour*. Cette valeur est déterminée par le nom du dernier script dans le dossier des mises à jour SQL. Si cette valeur est vide, aucun script SQL ne sera exécuté pendant ce cycle de mise à jour. Pour vous assurer que cette valeur est correctement définie, assurez-vous d'avoir un script SQL dans ce dossier avec son nom comme numéro de version (par exemple, 1.2.3.sql si la version est 1.2.3). Le fichier peut être vide ou contenir simplement une ligne de commentaire SQL. Cela doit être fait dans l'ancienne version — celle avant la mise à jour. Alternativement, vous pouvez ajouter cette valeur à `#_schemas` en utilisant une requête SQL.

*Traduit par openai.com*

