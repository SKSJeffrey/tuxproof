# MySQL - Unix Socket Authentication
## Purpose - Authenticate users with the unix socket for ease of access to MySQL
## Procedure

## Description
The Unix socket is a method of allowing users to use the operating system credentials to authenticate with MariaDB. When a user signs into the system, the uid of the process is used by MariaDB to authenticate accordingly.

## Installation
Verify that the `server_audit.so` plugin is installed
```bash
SHOW VARIABLES LIKE 'plugin_dir';
\! ls -l /usr/lib64/mysql/plugin | grep server_audit.so
```
If the `server_audit.so` plugin does not exist:
```bash
INSTALL PLUGIN server_audit SONAME 'server_audit';
\! ls -l /usr/lib64/mysql/plugin | grep server_audit.so
```
If the `server_audit.so` plugin exists:
```bash
INSTALL PLUGIN unix_socket SONAME 'auth_socket';
```
Confirm unix_socket plugin is installed:
```bash
SHOW PLUGINS;
```
## Creating a User
```bash
CREATE USER 'jeffreywen'@'localhost' IDENTIFIED VIA unix_socket;
FLUSH PRIVILEGES;
```
## Verify unix_socket
Option 1: If you are not signed into the user you want
```bash
mysql --user=jeffreywen
```
Option 2: If you are already signed into the user you want
```bash
mysql
```
## Optional Grant Privileges
The grant option is slightly different: `IDENTIFIED BY` vs `IDENTIFIED VIA`
```bash
GRANT ALL PRIVILEGES ON *.* to 'jeffreywen'@'locahost' IDENTIFIED VIA unix_socket;
```
