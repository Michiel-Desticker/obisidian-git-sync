# Wordpress

## Prerequisites

Apache with PHP enabled & MySQL (Mariadb)

## Installation 

Create a new database for Wordpress
```
mysql -u root -p CREATE DATABASE db_name; CREATE USER 'db_user'@'localhost' IDENTIFIED BY 'password'; GRANT ALL PRIVILEGES ON db_name.* TO 'db_user'@'localhost'
```

### Wordpress

#### Step 1: Download and Extract
```
# Download package
curl -O https://wordpress.org/latest.tar.gz
# Extract package
tar -xzvf latest.tar.gz
```


