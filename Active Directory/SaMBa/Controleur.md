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

## Configuration DNS


### Configuration de Bind9
Les fichiers de configurations sont à mettre dans le répertoire `/etc/bind/`

[On inclue le fichier named.conf généré par samba dans le fichier named.conf.local](Conf/named.conf.local)

[Ajout des options dans named.conf.options nécéssaires au fonctionnement de l'AD dans la configuration de Bind9](Conf/named.conf.options)

[Il faut aussi adapter le fichier généré named.conf par Samba en renseignant le nom de la zone et la version de Bind9](Conf/named.conf) 


### Modification du fichier resolvconf :
```properties
search mydomain.lan
namerserver 172.16.0.200
```

### Modification du fichier de conf kerberos:
```properties
[libdefaults]
    default_realm = MYDOMAIN.LAN
    dns_lookup_kdc = true
    dns_lookup_realm = false
```

### Test de la connexion au DNS:
La commande `kinit Administrator` nous permet de tester le serveur DNS car on apelle le protocole de kerberos qui est renseigné dans un enregistrement SRV sur le serveur DNS.
 
### Création de la zone inverse
`samba-tool dns zonecreate 172.16.0.200 .0.16.172.in-addr.arpa -U Administrator`

## Configuration DHCP

Pour avoir un serveur dhcp qui travaille en qui travaille en simultané avec le DNS il nous faudra plusieurs éléments :

1. Un script qui va à partir des informations passées en paramètres enregistrer ou supprimer le pc dans le domaine DNS.
2. Un compte utilisateur et son fichier keytab associé dont le script va se servir pour effectuer les enregistrements dans le DNS grâce à Kerberos.
3. Quelques paramètres à rajouter dans le fichier de configuration DHCPD pour exécuter le script avec le comportement voulu.

### 1. [Le script à mettre dans /usr/local/bin/](Fichiers/DHCP/dhcp-dyndns.sh)

### 2. Compte utilisateur dhcp
1. Création du compte : `samba-tool user create dhcpduser --description="Utilisateur pour la mise à jour DNS avec kerberos" --random-password`
2. Le mot de passe de l'utilisateur n'expirera jamais : `samba-tool user setexpiry dhcpduser --noexpiry`
3. Ajout du compte aux groupes DnsAdmins : `samba-tool group addmembers DnsAdmins dhpdcuser`
4. Ajout du compte au groupe Admin (pour l'adresse mac des postes): `samba-tool group addmembers "Domains Admins" dhcpduser`
5. Création du nom principal de service du compte : `samba-tool spn add host/dc1.mydomain.lan@MYDOMAIN.LAN dhcpduser`
6. Export de la keytab : `samba-tool domain exportkeytab --principal=dhcpduser@MYDOMAIN.LAN /var/lib/samba/dhcpduser.keytab`
7. On donne le controle du fichier keytab à root : `chown root:root /var/lib/samba/dhcpduser.keytab`
8. On bloque les accès du fichier : `chmod 400 /var/lib/samba/dhcpduser.keytab`

### 3. [La configuration de DHCPD](Fichiers/DHCP/dhcpd.conf)

## Config winbind Controleur de domaine
Le contrôleur de domaine doit pouvoir voir les comptes du domaine, on met active winbind et la création de répertoire home dans pam et dans nsswitch :

`pam-auth-update`

nsswitch.conf :
```properties
passwd: files systemd winbind
group:  files systemd winbind
```
