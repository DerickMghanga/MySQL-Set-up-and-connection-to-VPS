MySQL Set-up and connection to VPS


Install MySQL 8.0 Server
First, update the repository by running command:

```
sudo apt-get update
```


Then install the mysql server

```
sudo apt-get install -y mysql-server
```


Start MySQL Service
At the end of installation start mysql server.

```
sudo systemctl start mysql

sudo systemctl status mysql
```


Confirm the mysql version is 8.0

```
mysql --version
```


Secure the MySQL Installation
 
```
sudo mysql_secure_installation
```


Create MySQL Database
Log into mysql server as root.

```
mysql -u root –p
```


Enter the password for root in the prompt.



Create new database called ‘myfirstdb’.

```
mysql> CREATE DATABASE myfirstdb;
```


List all databases in the mysql-server

```
mysql> SHOW DATABASES;
```



Once you done with all commands, you can quit the mysql window by using this command:

```
mysql> quit;
```
