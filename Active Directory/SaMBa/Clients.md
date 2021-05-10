# Client Active Directory linux et windows

## Windows
Pour windows il suffit de paramétrer le serveur AD en serveur DNS, puis dans les propriétés de l'ordinateur, se connecter au domaine correspondant.

## Linux
### Manuellement
On commence par installer tous les paquets requis : `sudo apt install -y sssd sssd-tools libnss-sss libpam-sss realmd oddjob oddjob-mkhomedir adcli samba-common samba-common-bin packagekit python-pexpect python3-pexpect nfs-common krb5-user`

On joint la machine au domaine : `sudo realm join -U Administrator (domaine)`

On déplace l'ancien fichier sssd.conf et on le remplace par [celui-la](Fichiers/Client/sssd.conf)

On active la création de répertoires utilisateur à la création de compte au cas-où `sudo pam-auth-update --enable mkhomedir`

Si le répertoire /home/(domaine) n'existe pas, on le crée avec `sudo mkdir /home/domaine`

On sauvegarde l'ancien fichier fstab `sudo copy /etc/fstab /etc/fstab.ori`

On commente les lignes faisant mention de répertoire /home dans fstab et on les remplace par :
> (serveur).(domaine):/home/(domaine)   /home/(domaine) nfs defaults    0   0

On termine par un reboot pour tout vérifier.

### Avec Ansible
Un role à été prévu pour joindre la machine au domaine.
Il suffit d'appeler le role dans un playbook et l'installation sera effectuée.