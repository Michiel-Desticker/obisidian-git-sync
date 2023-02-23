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

Example
```
CREATE DATABASE wordpress;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL ON wordpress.* TO 'wordpressuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
#### Step 3: Set up wp-config.php

Can be done manually or automatically with an install script from Wordpress

Rename sample config file
```
mv wp-config-sample.php wp-config.php
```

Go to this section
```
// ** MySQL settings - You can get this info from your web host ** //
```

Change these values
```
DB_NAME 

The name of the database you created for WordPress in Step 2.

DB_USER 

The username you created for WordPress in Step 2.

DB_PASSWORD 

The password you chose for the WordPress username in Step 2.

DB_HOST 

The hostname you determined in Step 2 (usually localhost, but not always; see [some possible DB_HOST values](https://wordpress.org/support/article/editing-wp-config-php/#set-database-host "Editing wp-config.php")). If a port, socket, or pipe is necessary, append a colon (:) and then the relevant information to the hostname.

DB_CHARSET 

The database character set, normally should not be changed (see [Editing wp-config.php](https://wordpress.org/support/article/editing-wp-config-php/ "Editing wp-config.php")).

DB_COLLATE 

The database collation should normally be left blank (see [Editing wp-config.php](https://wordpress.org/support/article/editing-wp-config-php/ "Editing wp-config.php")).

[Enter your secret key values](https://wordpress.org/support/article/editing-wp-config-php/ "Editing wp-config.php") under the section labeled

  * Authentication Unique Keys and Salts.

Save the wp-config.php file
```

eg.
```
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpressuser');
define('DB_PASSWORD', 'password');
define('DB_HOST', 'localhost');
```
#### Step 4: Upload the files

Move files to root directory or subdirectory

On apache, the default is `/var/www/html`

##### Root directory

Copy only the content of `wordpress` directory into `/var/www/html`. Don't copy  the directory itself

###### Subdirectory

Copy entire directory `wordpress` into `/var/www` and maybe rename it if necessary eg. `/var/www/html/blog`

#### Run the Install Script

Surf to the site and finish installation process

**SELINUX**

```
sudo restorecon -R /var/www/
```

Finished!

ChatGPT script installation MariaDB and Wordpress
```
#!/bin/bash

# Install LAMP stack
sudo dnf install httpd mariadb-server php php-mysqlnd -y

# Start and enable services
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl start mariadb
sudo systemctl enable mariadb

# Secure MariaDB installation
sudo mysql_secure_installation <<EOF

y
$1
$1
y
y
y
y
EOF

# Create database and user for WordPress
sudo mysql -u root -p"$1" <<EOF
CREATE DATABASE wordpress;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY '$2';
GRANT ALL ON wordpress.* TO 'wordpressuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
EOF

# Download and extract WordPress
cd /tmp
curl -O https://wordpress.org/latest.tar.gz
tar xzvf latest.tar.gz

# Move files to document root
sudo mkdir -p /var/www/html
sudo cp -a /tmp/wordpress/. /var/www/html/

# Set permissions
sudo chown -R apache:apache /var/www/html
sudo chmod -R 755 /var/www/html

# Create wp-config.php
cd /var/www/html
sudo cp wp-config-sample.php wp-config.php
sudo sed -i "s/database_name_here/wordpress/g" wp-config.php
sudo sed -i "s/username_here/wordpressuser/g" wp-config.php
sudo sed -i "s/password_here/$2/g" wp-config.php

# Restart Apache
sudo systemctl restart httpd

echo "WordPress has been installed successfully. Please navigate to http://your_server_ip/wordpress to complete the installation."

```