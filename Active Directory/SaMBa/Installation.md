# Serveur Active Directory Debian 10 

## Prérequis
### Installation de tous les paquets requis :

`apt install acl attr samba samba-dsdb-modules samba-vfs-modules winbind libpam-winbind libnss-winbind libpam-krb5 krb5-config krb5-user bind9 isc-dhcp-server`

### Supression des fichiers de configurations générés à l'installation des paquets:

`rm /etc/samba/smb.conf`

`rm /etc/krb5.conf`

### Ajout du domaine dans le fichier hosts :
```properties
...
172.16.0.200    dc1.mydomain.lan    dc1
...
```

## Création du domaine
### Génération des fichiers de configurations
Pour créer le domaine on se sert de l'utilitaire samba-tool qui va générer un fichier de configuration smb.conf et tous les dossiers dont nous avons besoin.

`samba-tool domain provision --use-rfc2307 --server-role=dc --dns-backend=BIND9_DLZ --realm=MYDOMAIN.LAN --domain=MYDOMAIN --adminpass=Passw0rd`

### Configuration DNS

#### [Configuration du serveur Bind9 et lien avec samba](../BIND9/Installation.md)


#### Modification du fichier resolvconf :
```properties
search mydomain.lan
namerserver 172.16.0.200
```

#### Test de la connexion au DNS:
La commande `kinit Administrator` nous permet de tester le serveur DNS car on apelle le protocole de kerberos qui est renseigné dans un enregistrement SRV sur le serveur DNS.
 
#### Création de la zone inverse
`samba-tool dns zonecreate 172.16.0.200 .0.16.172.in-addr.arpa -U Administrator`