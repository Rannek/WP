
# WordPress Installation Guide on Arch Linux

This guide will help you install WordPress on a fresh Arch Linux installation.

## Goal Directory

The target directory for the WordPress installation is `/wp/wordpress`.

## Steps

1. **Install Required Packages**: Install PHP, MariaDB, Apache using pacman.
   ```bash
   pacman -S apache php php-gd php-intl php-apache php-fpm mariadb unzip tar wget
   ```

2. **Create a Folder for WordPress Installation**:
   ```bash
   cd /
   mkdir wp
   wget https://wordpress.org/latest.tar.gz
   tar xvzf latest.tar.gz
   ```

3. **Configure Apache**:
   Edit the Apache configuration file.
   ```bash
   nano /etc/httpd/conf/httpd.conf
   ```
   Use the included configuration file as a starting point.

4. **Restart Apache Service**:
   ```bash
   systemctl restart httpd.service
   ```

5. **Set Up MariaDB Database**:
   Log in to MariaDB as root:
   ```bash
   sudo su
   mysql -u root -p
   ```
   Then execute the following SQL commands:
   ```sql
   CREATE DATABASE wordpress;
   GRANT ALL PRIVILEGES ON wordpress.* TO "wp-user"@"localhost" IDENTIFIED BY "choose_db_password";
   FLUSH PRIVILEGES;
   EXIT;
   ```

6. **Access Your WordPress Installation**:
   Go to your IP address in a web browser to access the WordPress setup.

db name: wordpress_db
username: wordpress_user
password:choose_db_password
host:localhost


## Additional Tips

- **Creating a PHP Info File**: You can create a `info.php` file to check your PHP installation with the following content:
   ```php
   <?php phpinfo(); ?>
   ```
