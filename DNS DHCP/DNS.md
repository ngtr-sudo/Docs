- [Installation de Bind9](#installation-de-bind9)
- [Création des zones](#création-des-zones)

# Installation de Bind9
On installe bind9 et les utilitaires dns `$ apt install bind9 dnsutils`.

Les fichiers de zones sont situés dans le répertoire /var/cache/bind/.

Les fichiers de configurations sont dans le répertoire /etc/bind/.
# Création des zones
Les fichiers de zones peuvent être placés selon l'envie de l'administrateur, mais le répertoire de base sur Debian est `/var/cache/bind`.

Le fichier de zone classique : [/var/cache/bind/domaine.local.zone](zones/domaine.local.zone)

Le fichier de zone inverse : [/var/cache/bind/domaine.local.reverse.zone](zones/domaine.local.reverse.zone)