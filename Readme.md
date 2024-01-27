<h1>MySQL Set-up and connection to VPS.</h1>


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



**One of the more common problems that users run into when trying to set up a remote MySQL database is that their MySQL instance is only configured to listen for local connections. This is MySQL’s default setting, but it won’t work for a remote database setup since MySQL must be able to listen for an external IP address where the server can be reached. To enable this, open up your mysqld.cnf file:**

```
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Navigate to the line that begins with the bind-address directive. It will look like this: 

```
. . .
lc-messages-dir = /usr/share/mysql
skip-external-locking
#
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
bind-address            = 127.0.0.1
. . .
```

**By default, this value is set to 127.0.0.1, meaning that the server will only look for local connections. You will need to change this directive to reference an external IP address. For the purposes of troubleshooting, you could set this directive to a wildcard IP address, either** *, ::, or ```0.0.0.0```

After trouble shooting or finishing development. change bind-address to ```127.0.0.1``` or set your remote_ip_address


Then restart the MySQL service to put the changes you made to mysqld.cnf into effect:
```
sudo systemctl restart mysql
```

<h1> Root DB user set-up. </h1>

**On some systems, like Ubuntu, MySQL is using the Unix auth_socket plugin by default.**

Basically it means that: **db_users** using it, will be "authenticated" by the **system user credentials.** You can see if your root user is set up like this by doing the following: use ```sudo``` since its a new installation

```
sudo mysql -u root

mysql> USE mysql;

mysql> SELECT User, Host, plugin FROM mysql.user;
```


As you can see in the query, the **root** user is using the **auth_socket** plugin.

<h2>To solve this:</h2>

```
sudo mysql -u root

mysql> USE mysql;

mysql> UPDATE user SET plugin='mysql_native_password' WHERE User='root';

mysql> FLUSH PRIVILEGES;

mysql> exit;

sudo service mysql restart
```


<h1>Create a New User and grant all privileges to a database.</h1>

**Access command line and enter MySQL server:**
```
mysql
```

Then, execute the following command:
```
CREATE USER 'new_user'@'localhost' IDENTIFIED BY 'password';
```

**new_user** is the name we’ve given to our new user account and the **IDENTIFIED BY ‘password’** section sets a passcode for this user. You can replace these values with your own, inside the quotation marks.


In order to grant all privileges of the database for a newly created user, execute the following command:
```
GRANT ALL PRIVILEGES ON * . * TO 'new_user'@'localhost';
```

For changes to take effect immediately flush these privileges by typing in the command:
```
FLUSH PRIVILEGES;
```

**Once that is done, your new user account has the same access to the database as the root user.**

<h1>Set up Firewall to Allow Remote MySQL Connection</h1>


**Allow connection in VPS provider console settings if necessary. ie AWS lightsail Network settings(IPv4 & IPv6)**


```
sudo ufw allow mysql
```


**AFTER DEVELOPMENT CLOSE THE CONNECTION IF THE APP & WEB SERVERS IS IN THW SAME LOCATION WITH THE MYSQL DATABASE**

**For example, to withdraw all privileges for our non-root user we should use:**
```
REVOKE ALL PRIVILEGES ON * . * FROM 'user_name'@'localhost';
```

**Finally, you can entirely delete an existing user account by using the following command:**
```
DROP USER ‘user_name’@‘localhost’;
```

**In order to find what privileges have already been granted to a MySQL user, you can use the SHOW GRANTS command:**
```
SHOW GRANTS FOR 'user_name'@'localhost';
```