# Installation de Zabbix

## Dépendances

### Apache2 et PHP7.3 

```bash
# apt install apache2 php php-mysql php-mysqlnd php-ldap php-bcmath php-mbstring php-gd php-pdo php-xml libapache2-mod-php
# systemctl status apache2
```  
### MySQL (MariaDB)

```bash
# apt install mariadb-server mariadb-client
# mysql_secure_connection
# mysql -uroot -p

MariaDB [(none)]> create database zabbix character set utf8 collate utf8_bin;
MariaDB [(none)]> create user zabbix@localhost identified by 'motdepasse';
MariaDB [(none)]> grant all privileges on zabbix.* to zabbix@localhost;
MariaDB [(none)]> quit;
```