# Install Zabbix All-in-One

#### Ambiente Utilizado 

* Sistema operacional: Ubuntu 20.04
*  Memória: 4Gb
* CPU: 2vCPU
* Disco: 50G

#### Configure Locale
```
 export LANGUAGE=en_US.UTF-8
 export LANG=en_US.UTF-8
 export LC_ALL=en_US.UTF-8
```

#### Ajustes no Sistema Operacional

```
timedatectl status
timedatectl set-timezone America/Manaus
```

#### Instalar o pacote do mysql server
```
apt install -y mysql-server mysql-client
systemctl enable --now mysql
systemctl status mysql
```

#### Configurar o mysql 
```
mysql_secure_installation

Securing the Mysql server deployment.

Connecting to Mysql using a blank password.

VALIDATE PASSWORD COMPONENT can be used to test passwords and improve security. It checks the strength of password and allows the users to set only those passwords which are secure enough. Would you like to setup VALIDATE PASSWORD component?

Press y|Y for Yes, any other key for No: y

There are three levels of password validation policy:

LOW    Length >=8
MEDIUM Length >=8, numeric, mixed case, and special characters
STRONG Length >=8, numeric, mixed case, special characters and dictionary file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 1
Please set the password for root here.

New Password: Z4bb1x5!2o2o

Re-enter new password: Z4bb1x5!2o2o

Estimated strength of the password: 100
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No): y
By default, a MySQL installation has an anymous user, allowing anyone to log into MySQL without having to have a user account created for them. This is intended only for testing, and to make the installation go a bit smoother. You should remove them before moving into a production enviroment.

Remove anonymous users? (Press y|Y for Yes, any other key for No): y 
Sucess.

Normaly, root should only be allowed to connect from "localhost", This ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No): y
Sucess.

By default, Mysql comes with a database name "test" that anyone can access. This is also intended only for testing, and should be removed before moving into a production environment.

Remove test database and access to it? (Press y|Y for Yes, any other key for No): y
- Dropping test database...
Sucess.

- Removing privileges on test database...
Sucess.

Reloading the privilege tables will ensure that all changes made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for yes, any other key for NO): y
Sucess.

All done!
```

#### Conectar o banco e criar o base e usuário do zabbix

```
mysql -u root -p
create database zabbix character set utf8 collate utf8_bin;
create user 'zabbix'@'localhost' identified by 'Curso!Zabbix5';
grant all privileges on zabbix.* to 'zabbix'@'localhost';
flush privileges;
```

#### Instalar o Zabbix Server

Instalar o repositório

```
cd /tmp
wget https://repo.zabbix.com/zabbix/5.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_5.0-1+focal_all.deb
dpkg -i zabbix-release_5.0-1+focal_all.deb
apt update
```

Instalando o Zabbix Server

```
apt install zabbix-server-mysql
```

Carregar esquema inicial do banco

```
zcat /usr/share/doc/zabbix-server-mysql/create.sql.gz | mysql -uzabbix -p zabbix 
```

Verificar se o banco foi populado

```
mysql -u zabbix -p zabbix
use zabbix;
show tables;
quit;
```

Editar arquivo de configuração do Zabbix Server

```
vim /etc/zabbix/zabbix_server.conf


### Option: DBPassword
#   Database password
#   Comment this line if no password is used.
#
# Mandatory: No
# Defaults:
DBPassword=Curso!Zabbix5
```

Habilitar inicialização do serviço e inicia-lo

```
systemctl enable --now zabbix-server
systemctl status zabbix-server
```

Verificar os logs do Zabbix Server

```
tail -n50 /var/log/zabbix/zabbix_server.log
```

#### Instalar Frontend

```
apt install zabbix-frontend-php zabbix-apache-conf
```

#### Configurando o PHP
```
vim /etc/zabbix/apache.conf

php_value date.timezone America/Manaus
```

#### Habilitar serviço na inicialização
```
systemctl enable --now apache2
systemctl restart apache2
