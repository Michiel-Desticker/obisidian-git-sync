# Wordpress

## Prerequisites

Apache with PHP enabled & MySQL (Mariadb)

## Installation 

Create a new database for Wordpress
```
mysql -u root -p CREATE DATABASE db_name; CREATE USER 'db_user'@'localhost' IDENTIFIED BY 'password'; GRANT ALL PRIVILEGES ON db_name.* TO 'db_user'@'localhost'
```

### Wordpress

Source: https://wordpress.org/documentation/article/how-to-install-wordpress/

#### Step 1: Download and Extract
```
# Download package
curl -O https://wordpress.org/latest.tar.gz
# Extract package
tar -xzvf latest.tar.gz
```

#### Step 2: Create the Database and a User

Source: https://wordpress.org/documentation/article/creating-database-for-wordpress/#using-the-mysql-client

```
$ mysql -u adminusername -p  
Enter password:  
Welcome to the MySQL monitor. Commands end with ; or \g.  
Your MySQL connection id is 5340 to server version: 3.23.54  
  
Type 'help;' or '\h' for help. Type '\c' to clear the buffer.  
  
mysql> CREATE DATABASE databasename;  
Query OK, 1 row affected (0.00 sec)  
  
mysql> GRANT ALL PRIVILEGES ON databasename.* TO "wordpressusername"@"hostname"  
-> IDENTIFIED BY "password";  
Query OK, 0 rows affected (0.00 sec)  
  
mysql> FLUSH PRIVILEGES;  
Query OK, 0 rows affected (0.01 sec)   
  
mysql> EXIT  
Bye  
$
```

#### Step 3: Set up wp-config.php

Can be done manually or automatically with an install script from Wordpress
