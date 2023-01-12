# INSTALL APACHE ON LINUX UBUNTU

Install

```bash
sudo apt-get install apache2 php8.x libapache2-mod-php8.x
```

Verify Modulde

```bash
a2query -m php8.x
```

Load Module :

```bash
sudo a2enmod php8.x
```

Restart Apache

```bash
sudo service apache2 restart
```

## Change Apache Root Directory

Enter Directory :

```bash
cd /etc/apache2/sites-available
```

Find and open file :

```bash
nano 000-default.conf
```

Edit or change

DocumentRoot /path/to/my/project

```bash
sudo service apache2 restart
```

Fix Permission Error When Change Directory (Forbidden You don't have permission to access / on this serve)

Find and Edit :
nano apache2.conf

Before :

```
<Directory />
	Options Indexes FollowSymLinks
	AllowOverride All
	Require all denied
</Directory>
```

After :

```text
<Directory />
	Options Indexes FollowSymLinks Includes ExecCGI
	AllowOverride All
	Require all granted
</Directory>
```

## Create Custome Virtual Server

Create Dir :

```bash
sudo mkdir /var/www/your_domain
```

Assign User

```
sudo chown -R $USER:$USER /var/www/your_domain
```

Create new config

```bash
sudo nano /etc/apache2/sites-available/your_domain.conf
```

Documents :

```
<VirtualHost *:80>
    ServerAdmin webmaster@[nama domain]
    ServerName [domain_name]

    DocumentRoot /var/www/domain_name/
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>


<Directory /var/www/domain_name>
    Options  FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>

#Add Subdomain

<VirtualHost *:80>
    ServerName subdomain
    DocumentRoot /var/www/subdomain

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

<Directory /var/www/subdomain>
   	Options  FollowSymLinks
  	AllowOverride All
 	Require all granted
</Directory>
```

Enable Config :

```bash
sudo a2ensite your_domain.conf
```

Disable default Config

```bash
sudo a2dissite 000-default.conf
```

Enable Rewrite Mod :

```
sudo a2enmod rewrite
```

Test Config :

```
sudo apache2ctl configtest

```

Restart Apache2 :

```
sudo systemctl restart apache2
```

## Check Enable Module

```bash
apache2ctl -M
```

## Enable Rewrite Module

Rewrite module digunakan untuk menggunakan setingan apache dari file .htaccess

```bash
sudo a2enmod rewrite
```
