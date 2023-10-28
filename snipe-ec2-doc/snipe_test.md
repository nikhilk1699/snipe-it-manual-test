# "Deploying Snipe-IT Inventory Management System on AWS EC2: A Step-by-Step Manual Guide"

## create ec2 instance on aws console
- select ec2 dashboard
- click on launch
- AMI: Ubuntu Server 22.04 LTS (free tier eligible)
- Instance type: t2.micro
- Key pair: create and select key
- Network settings: Create your own security group allowing all traffic SSH traffic from Anywhere for the purpose of this project. Leave everything else as the default settings.
- Configure storage: 25 GiB gp2
- Launch the instance and then connect to it with SSH.

![Screenshot 2023-10-20 101330](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/0781cabc-d0a2-49bb-a760-d3525afd2e05)
![1](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/69af6c5f-b5ae-487e-b79f-27b60b9a7857)
![2](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/775432a4-ea82-4d4d-aed3-13fa1338f4f9)
![3](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/b53c4bfb-3b18-46cb-b749-3ca39781de84)
![4](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/0aa0e063-e32e-4904-872a-8c536f2441cf)
![5](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/2a5de906-4b10-4718-b082-03400abb0df4)
![6](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/338eb4dd-0f73-466c-affc-3dd4a6529093)

### update he packages using root user by running below command
```
apt update
```
### Installation apache2 using shell script
#### create directory & file.
```
mkdir scripts
cd scripts
```
#### vi apache2-setup.sh

```
#!/bin/bash

# Install Apache2
sudo apt install apache2 -y

# Start Apache2 and enable it on boot
systemctl start apache2
systemctl enable apache2

# Check the status of Apache2
systemctl status apache2

# Enable mod_rewrite for Apache2
sudo a2enmod rewrite

# Restart Apache2 to apply the changes
systemctl restart apache2

# Display a message
echo "Apache2 installation and configuration completed."
```
![7](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/98398e1a-3c04-4ba5-972a-3a2da712eae3)


### Run following command for above apache2 script 
```
chmod +x apache2-setup.sh #give executable permission
./apache2-setup.sh
```
![8](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/18170f49-cd7d-44f5-88df-9329eac1a90c)
![9](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/eb8bea3c-9821-4762-a094-550a4be295dd)

### Installation MySql/mariadb using shell script
#### vi mariadb-setup.sh
```
#!/bin/bash

# Install MariaDB Server and Client
sudo apt install mariadb-server mariadb-client -y

# Start MariaDB and enable it on boot
sudo systemctl start mariadb
sudo systemctl enable mariadb

# Check the status of MariaDB
sudo systemctl status mariadb

# secure the installation of the MySQL 
sudo mysql_secure_installation

```
![10](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/dcae797c-769b-49d4-a4ca-2a35acf9766c)
![11](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/4b43bdd9-8763-4bbb-88cf-ee3dc03356de)
![11-1](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/9d051aab-de64-4235-bc9c-1eb9d80c4459)

##### you'll secure your MySQL installation by setting a strong root password, removing anonymous users, disallowing remote root login, and removing the test database, which is protecting your MySQL server.

### installation of PHP
```
$ apt install php php-cli php-fpm php-json php-common php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath
$ apt install php php-bcmath php-bz2 php-intl php-gd php-mbstring php-mysql php-zip php-opcache php-pdo php-calendar php-ctype php-exif php-ffi php-fileinfo php-ftp php-iconv php-intl php-json php-mysqli php-phar php-posix php-readline php-shmop php-sockets php-sysvmsg php-sysvsem php-sysvshm php-tokenizer php-curl php-ldap -y
```
![php_install](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/190d41de-c6db-46b5-881f-0be70f1cfd71)
![php-2](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/013fbd02-276b-4d98-be22-b01a7a7e5136)

##### you run a command to install PHP and a basic set of extensions. Then, you run another command to make sure you have all the additional extensions needed for a software. basically adding PHP and its extensions to your system to make it compatible with Snipe-IT.

### To install PHP Composer for Snipe-IT, you can follow these steps
```
$ curl -sS https://getcomposer.org/installer | php
```
### Move the composer.phar file to the /usr/local/bin/composer 
```
$ mv composer.phar /usr/local/bin/composer
```
![compo](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/ff879e8e-e35d-4f32-b48d-ea710ae6cd8b)

#### By moving Composer to /usr/local/bin, you make it globally accessible to all users.

### To create a database in MariaDB for Snipe-IT
#### Make sure you have access to MariaDB with the root user.
```
$ mysql -u root -p
```
#### After accessing MariaDB, you can create the database, set up a user with a password, and grant the necessary privileges. You should either modify the details mentioned below or copy and paste them into a text editor for editing. Then, you can execute these commands one by one.  it grants significant administrative control over the database server.
```
CREATE DATABASE snipeitdb;
CREATE USER snipeituser@localhost IDENTIFIED BY 'snipe';
GRANT ALL PRIVILEGES ON snipeitdb.* TO snipeituser@localhost;
FLUSH PRIVILEGES;
EXIT;
```
![create snipe](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/3e92ad5b-0d7d-400b-a6e3-829a6c7a458a)

### Installation of Snipe-IT
#### In your system, you have a place the website into /var/www/.
```
$ cd /var/www/
```
#### clone github repository in /var/www/
```
$ git clone https://github.com/PearlThoughtsInternship/snipe-it.git
```
![clonegit](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/3d600aa3-c2ae-4e85-89b7-c340aad8cdd5)

#### Enter the Snipe-IT Directory
```
$ cd snipe-it
```
#### Create a copy of the configuration file named ".env.example" and name it ".env" in the same directory
```
$ cp /var/www/snipe-it/.env.example /var/www/snipe-it/.env
```
#### Open the ".env" configuration file for add db_name, username,password.
```
$ nano /var/www/snipe-it/.env
```
```
DB_DATABASE=snipeitdb
DB_USERNAME=snipeituser
DB_PASSWORD=snipe
```
![nanosinipe](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/a8b8717d-7f3d-42d9-98e3-1d9b3c9edeb6)
![settingsnipe](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/f0c10547-7d2f-469d-b70d-f8da7d71b54c)


 #### the web server has the permissions to work with the Snipe-IT files and directories. 
```
$ chown -R www-data:www-data /var/www/snipe-it
$ chmod -R 755 /var/www/snipe-it
```
![permision snipe](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/b20f482b-6640-4efe-bc6a-33c7b701f70a)

#### We started by downloading the Composer installer and placed it in the folder /var/www/snipe-it. Installed Composer with these following commands:
```
composer update --no-plugins --no-scripts
$ composer install --no-dev --prefer-source --no-plugins --no-scripts
```
![composer update](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/48188a47-6f84-4816-b2a2-fc9a3ed6b6be)
![composerinstall1](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/94b0c501-88fe-4e66-af86-c7d604e09584)

#### Now, we need to generate an APPKEY, which will automatically update the .env file. To do this, run the following command while staying in the same folder:
```
$ php artisan key:generate
```
![application-key](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/ca832d8b-5703-4f42-9523-2b2331e08f9f)

note we installed Composer and generated an APPKEY to configure our web application.

### Disable Default Virtual Host
#### You turn off the default web configuration on your server using the following command
```
$ a2dissite 000-default.conf.
```
### You create a new configuration for Snipe-IT using the command 
```
$ nano /etc/apache2/sites-available/snipe-it.conf
```
```
<VirtualHost *:80>
ServerName snipe-it.syncbricks.com
DocumentRoot /var/www/snipe-it/public
<Directory /var/www/snipe-it/public>
Options Indexes FollowSymLinks MultiViews
AllowOverride All
Order allow,deny
allow from all
</Directory>
</VirtualHost>
```
![image](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/a2b5b38b-ff76-4df3-834b-6068b0067489)

#### this configuration, you specify how the server should handle requests for your Snipe-IT website, including where the website files are located.

### Enable Snipe-it configuration
```
a2ensite snipe-it.conf
```
#### ensure that the server has the necessary permissions to read and write to specific folders by using the following commands 
```
chown -R www-data:www-data ./storage
chmod -R 755 ./storage
```

#### Restart Apache Server to apply the changes you've made.
```
systemctl restart apache2
```
![image](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/9c45b641-2d48-4d9e-890c-9def0ccbf703)

### Copy the ec2 Instnace public IP and paste into browser.

![image](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/5a9914ac-cc92-4ef6-a00c-0b0e748d3c43)

![image](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/08591f41-0f1d-4105-a82d-f94404a39d5f)

![image](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/58412fc1-1aff-4bb3-ab59-a0ed67801643)

![snipsuceesfil](https://github.com/nikhilk1699/snipe-it-manual-test/assets/109533285/76437458-cbad-4beb-9f45-04e71f37d13d)


## Sucessfully deploy application ec2 instance














