# Documentation Git Markdown Test
## Contexte
Ce dépot à pour but d'expérimenter la gestion de documentation à l'aide de Github et de fichiers markdown.

## Avantages et désavantages
Les avantages de cette configuration sont :
* La documentation est stockée sur un dépot github distant et donc est sauvegardée meme en cas de force majeurs (perte de disques, ransomware...)
* Possibilité de faire du versioning : le versioning désigne la possibilité d'avoir plusieurs versions d'un même document ou d'un même programme, ça implique un possible retour en arrière au cas ou il y aurait des erreurs dans la documentation.
* Par rapport à une solution comme dokuwiki, readthedocs, etc, aucun serveur n'est installé

Les désavantages sont :
* Cette solution n'est pas excellente en terme de confidentialité des données (hébergée sur github donc chez Microsoft)  même si le dépot est privé donc accessible uniquement par ceux qui y sont autorisés.
* Il est nécéssaire pour l'édition des documents d'utiliser Github et un éditeur Markdown