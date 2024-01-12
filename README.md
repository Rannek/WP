
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

```db name: wordpress_db```

```username: wordpress_user```

```password:choose_db_password```

```host:localhost```


## Additional Tips

- **Creating a PHP Info File**: You can create a `info.php` file to check your PHP installation with the following content:
   ```php
   <?php phpinfo(); ?>
   ```


# Attach Domain Name to Your Apache Server on Arch Linux

## 1. Attach Domain Name to Your Installation

- **Buy a domain name.**
- **Edit DNS Settings:**
  - Edit your DNS settings at your domain registrar.
  - Point your domain to your server's IP address.
  - You can check your server IP by running `ip addr`.

## 2. Configure Apache

- **Create a Configuration File for Your Domain:**
  - Run `sudo nano /etc/httpd/conf/extra/yourdomain.com.conf`
  - Add the following configuration:

    ```apache
    <VirtualHost *:80>
        ServerAdmin webmaster@yourdomain.com
        DocumentRoot "/path/to/wordpress"
        ServerName yourdomain.com
        ServerAlias www.yourdomain.com

        <Directory "/path/to/wordpress">
            AllowOverride All
            Require all granted
        </Directory>

        ErrorLog "/var/log/httpd/yourdomain.com-error.log"
        CustomLog "/var/log/httpd/yourdomain.com-access.log" common
    </VirtualHost>
    ```

- **Include the Configuration in Apache:**
  - Edit the main Apache configuration file: `sudo nano /etc/httpd/conf/httpd.conf`
  - Add the following line to include your domain configuration:
    ```
    Include conf/extra/yourdomain.com.conf
    ```

- **Restart Apache:**
  - Restart Apache to apply the changes: `sudo systemctl restart httpd`

- **Wait for DNS Propagation:**
  - It might take a few hours for the DNS changes to propagate.

## 3. Set Up WordPress

- **Change WordPress URLs:**
  - In your WordPress admin area, go to **Settings > General**.
  - Update the following:
    - WordPress Address (URL)
    - Site Address (URL)
  - Change them to your website address.

- **Update Permalinks (Optional):**
  - You can also change permalinks if desired.

## 4. Install SSL Certificate

- **Install Certbot:**
  - Run `sudo pacman -S certbot certbot-apache` to install Certbot and its Apache plugin.

- **Obtain and Install SSL Certificate:**
  - Run `sudo certbot --apache` to obtain and install the SSL certificate.

- **Update WordPress Settings:**
  - Change your domain in the WordPress settings to `https://www.yourdomain.com`.

