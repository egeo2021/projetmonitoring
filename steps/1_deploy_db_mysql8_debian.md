# Deploy DB

- [Deploy DB](#deploy-db)
  - [Timezone](#timezone)
    - [Check the timezone](#check-the-timezone)
    - [Set timezone](#set-timezone)
  - [Tuning S.O](#tuning-so)
    - [Check updates](#check-updates)
    - [Install utilities](#install-utilities)
  - [Install MySQL 8](#install-mysql-8)
    - [Check version available in the Debian repository](#check-version-available-in-the-d-repository)
    - [Install the MySQL 8 package](#install-the-mysql-8-package)
    - [Enable automatic startup, start the service and validate the status](#enable-automatic-startup-start-the-service-and-validate-the-status)
    - [Set MySQL root user password](#set-mysql-root-user-password)
    - [Connect to mysql, create the Zabbix database and user](#connect-to-mysql-create-the-zabbix-database-and-user)
    - [Create zabbix user allowing connection through a remote server](#create-zabbix-user-allowing-connection-through-a-remote-server)    
	- [Not Upgrade MySQL 8.0 version] (#not-upgrade-mySQL8.0-version)

## Timezone

### Check the timezone

```bash
timedatectl status
```

Output:

```bash
               Local time: Mon 2021-01-25 13:45:09 UTC
           Universal time: Mon 2021-01-25 13:45:09 UTC
                 RTC time: Mon 2021-01-25 13:45:10
                Time zone: Etc/UTC (UTC, +0000)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```

### Set timezone

```bash
timedatectl set-timezone America/Sao_Paulo
```
Verific date
```bash
               Local time: Mon 2021-01-25 10:45:32 -03
           Universal time: Mon 2021-01-25 13:45:32 UTC
                 RTC time: Mon 2021-01-25 13:45:33
                Time zone: America/Sao_Paulo (-03, -0300)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```


## Tuning S.O

### Check updates

```bash
apt update
apt upgrade -y
```

### Install utilities

```bash
apt install -y net-tools vim nano wget curl tcpdump telnet
```

## Install MySQL 8

```bash
cd tmp
wget https://dev.mysql.com/get/mysql-apt-config_0.8.15-1_all.deb
dpkg -i mysql-apt-config_0.8.15-1_all.deb
```
Note: MySQL 8.0 is pre-selected, if you want to install MySQL 5.7, select MySQL Server & Cluster (currently selected: mysql-8.0) and select your preferred MySQL version. Here, MySQL 8.0 is already selected, so we are going to select "OK" as shown below screenshot to continue.

### Check version available in the Debian repository

```bash
apt info mysql-server
```

Output:

```bash
Package: mysql-server
Version: 8.0.22-0ubuntu0.20.10.2
Priority: optional
Section: database
Source: mysql-8.0
Origin: Ubuntu
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Original-Maintainer: Debian MySQL Maintainers <pkg-mysql-maint@lists.alioth.debian.org>
Bugs: https://bugs.launchpad.net/ubuntu/+filebug
Installed-Size: 108 kB
Depends: mysql-server-8.0
Homepage: http://dev.mysql.com/
Task: lamp-server
Download-Size: 9544 B
APT-Manual-Installed: yes

```

### Install the MySQL 8 package

```bash
apt -y install mysql-server
```

### Enable automatic startup, start the service and validate the status

```bash
systemctl enable --now mysql
systemctl status mysql
```

## Verificar Password Temporario
```bash
cat /var/log/mysqld.log | grep password
```
Terá uma saida parecida com esta
```bash
A temporary password is generated for root@localhost: CCwI9s32=&h!
```

### Digite o comando abaixo para realizar a primeira configuração do mysql, será solicitado uma nova senha para o root
mysql_secure_installation

```bash
mysql_secure_installation

Securing the MySQL server deployment.

Enter password for user root:

The existing password for the user account root has expired. Please set a new password.

New password: Pa$$w0rd
Re-enter new password: Pa$$w0rd
The 'validate_password' component is installed on the server.
The subsequent steps will run with the existing configuration
of the component.
Using existing password for root.

Estimated strength of the password: 100
Change the password for root ? ((Press y|Y for Yes, any other key for No) :

 ... skipping.
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y
Success.

By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y
 - Dropping test database...
Success.

 - Removing privileges on test database...
Success.

Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
Success.

All done!

```

### Connect to mysql, create the Zabbix database and user


```bash
mysql -u root -p
create database zabbix character set utf8 collate utf8_bin;
create user 'zabbix'@'%' identified with mysql_native_password by 'ZaBBix2021!';
grant all privileges on zabbix.* to 'zabbix'@'%';
```

`character set utf8 - Suporte para multilingal
`collate utf8_bin - suporte a case sensitiveness para os dados guardados`
