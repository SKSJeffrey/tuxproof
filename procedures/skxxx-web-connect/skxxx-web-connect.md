# SKXXX - SKS-Connect
## Purpose - 
## Procedure

## Update and Upgrade the System
```bash
$ yum -y update; yum -y upgrade; yum -y clean all
```

## Installing the LAMP Stack
### Apache, MySQL, and PHP
```bash
$ yum -y install httpd mariadb-server mariadb php

$ systemctl enable httpd.service
$ systemctl enable mariadb.service
   # Start the services on boot

$ systemctl start httpd.service
$ systemctl start mariadb.service

$ yum -y install mod_ldap mod_ssl openssl-devel
   # Install modules necessary for SKS-Connect
     # mod_ldap for LDAP login authentication
     # mod_ssl for SSL listening
     # openssl-devel for wkhtmltopdf to execute on HTTPS pages

$ yum -y install php-mysql php-gd php-ldap php-odbc php-pear php-xml php-xmlrpc 
$ yum -y install curl curl-devel php-snmp php-soap php-mbstring
   # Install the essential PHP modules

$ systemctl restart httpd.service
```

### Configure the Firewall
```bash
$ firewall-cmd --permanent --zone=public --add-service=http
$ firewall-cmd --permanent --zone=public --add-service=https
$ firewall-cmd --permanent --zone=public --add-service=mysql
$ firewall-cmd --reload
```

### Configure MySQL
```bash
$ mysql_secure_installation
   # Setup a root password and 'Y' to all prompts

$ mysql -u root -p
   MariaDB [(none)]> CREATE DATABASE database_name;
   MariaDB [(none)]> GRANT ALL PRIVILEGES ON database_name.* \
   -> TO 'database_user'@'hostname/ip' \
   -> IDENTIFIED BY 'password';
   MariaDB [(none)]> FLUSH PRIVILEGES;
```

### Configure PHP
```
$ vim /etc/php.ini
   short_open_tag = On
   session.save_path = "/var/lib/php/session"
   session.cookie_secure = 1
   session.cookie_httponly = 1
   expose_php = Off
```

## SELinux
```bash
$ setsebool -P httpd_unified=1
$ setsebool -P httpd_can_network_connect=1
$ setsebool -P httpd_can_network_connect_db=1
$ setsebool -P httpd_can_connect_ldap=1
$ setsebool -P httpd_can_sendmail=1
$ setsebool -P httpd_execmem=1

$ semanage port -a -t http_port_t -p tcp 9200-9231
   # Ports used for the GetScale application

$ firewall-cmd --permanent --zone=public --add-port={each port for the GetScale application}/tcp
$ firewall-cmd --reload
```

## CUPS
```bash
$ yum -y install cups samba samba-client hplip-common

$ firewall-cmd --permanent --zone=public --add-port=631/tcp
$ firewall-cmd --reload

$ vim /etc/cups/cupsd.conf
   Port 631
   Allow all (for server, admin pages, and configuration pages)
```

## Postfix
```bash
$ yum -y install postfix

$ vim /etc/postfix/main.cf
   relayhost = smtp-relay.gmail.com:25
   smtpd_recipient_restrictions = check_recipient_access hash:/etc/postfix/recipient_domains, reject

$ vim /etc/postfix/generic
   apache@hostname.com	custsalesconfirm@sks-bottle.com
   root@hostname.com	custsalesconfirm@sks-bottle.com
$ postmap /etc/postfix/generic

$ vim /etc/postfix/recipient_domains
   # Limit outgoing emails only to the following domains:
   sks-bottle.com OK
   sks-connect.com OK
   sks-portal.com OK
   sks-science.com OK
$ postmap /etc/postfix/recipient_domains

$ systemctl restart postfix.service
```
