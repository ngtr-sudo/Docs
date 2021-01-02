# Installation de Bind9
On commence par installer bind9 et les utilitaires dns `$ apt install bind9 dnsutils`
Les fichiers de zones sont situés dans le répertoire /var/cache/bind/.
Les fichiers de configurations sont dans le répertoire /etc/bind/.
# Création des zones
Les zones sont situées dans le répertoire /var/cache/bind.
domaine.local.zone:
> $ORIGIN domaine.local. ; Nom de domaine
> 
> $TTL 1800 ; Temps d'expiration
> 
>   
> 
> @IN  SOA     dns-dhcp.domaine.local. admin.domaine.local. (
> 
>                         2021010201
> 
>                         3600
> 
>                         600
> 
>                         86400
> 
>                         1800 )
> 
>   
> 
> @IN  NS      dns-dhcp.domaine.local.
> 
>   
> 
> dns-dhcp    IN  A       192.168.0.250
> 
> client1     IN  A       192.168.0.1
> 
> client2     IN  A       192.168.0.2
> 
> client3     IN  A       192.168.0.3

domaine.local.reverse.zone:
> $ORIGIN domaine.local. ; Nom de domaine
> 
> $TTL 1800 ; Temps d'expiration
> 
>   
> 
> @IN  SOA     dns-dhcp.domaine.local. admin.domaine.local. (
> 
>                         2021010201
> 
>                         3600
> 
>                         600
> 
>                         86400
> 
>                         600 )
> 
>   
> 
> @IN  NS      dns-dhcp.domaine.local.
> 
>   
> 
> 250         IN  PTR     dns-dhcp.domaine.local.
> 
> 1           IN  PTR     client1.domaine.local.
> 
> 2           IN  PTR     client2.domaine.local.
> 
> 3           IN  PTR     client3.domaine.local.