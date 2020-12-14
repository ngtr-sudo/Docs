# Installer et configurer Ansible

Pour installer Ansible 2.10 sur Debian il faut ajouter le dépôt de paquet au système :
```bash
$ echo "deb http://ppa.launchpad.net/ansible/ansible/ubuntu trusty main" >> append /etc/apt/sources.list
$ apt install gnupg2
$ apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 93C4A3FD7BB9C367
$ apt update
$ apt install ansible sudo
```
On active l'accès en ssh par root :
```bash
$ vim /etc/ssh/sshd_config

PermitRootLogin yes
```
On génère des clés ssh et les partager aux hôtes ansible (ici 10.0.0.1 et 10.0.0.2):
```bash
$ ssh-keygen

$ ssh-copy-id root@10.0.0.1

$ ssh-copy-id root@10.0.0.2
```
On crée le fichier hosts pour ansible :
```bash
$ vim $HOME/hosts

[Premiere]

10.0.0.1

[Deuxieme]

10.0.0.2
```
