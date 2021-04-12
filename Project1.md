# Project 1

**Step 1** - Installing Apache and Updating the Firewall
---

The below cmdlet updates a list of packages in package manager

`$ sudo apt update`

![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/Pics1.PNG)

The below cmdlet runs apache2 package installation

`$ sudo apt install apache2`

![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/Pics2.PNG)
![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/Pics2b.PNG)

This code verifies that apache2 is running as a Service in my Server

`$ sudo systemctl status apache2`

![Output of the apache2 installation](https://github.com/Tofumy/Tofumy-PBL/blob/main/Pics3.PNG)

Opened TCP port 80 in the EC2 Instance which is the default port that web browsers use to access web pages on the Internet

![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/Inboundrule.PNG)


Output to check if we can access the apache2 locally

![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/curl-checks-local.PNG)

Output to check if we can access the apache2 over the internet from any IP address

![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/curl-checks-internet.PNG)


Test that apache server is running over the browser

![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/apache-browser-test.PNG)



**Step 2** - Installing MySQL

The below cmdlet installs the mysql in the server

`$ sudo apt install mysql-server`

![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/install-mysql.PNG)

The below cmdlet runs a security script that removes sime insecure default settings and lock down access to the database system

`$ sudo mysql_secure_installation`

![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/secure-sql.PNG)


This code verifies that we can log in to the MySQL server

`$ sudo mysql`

![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/test-sql.PNG)




**Step 3** - Installing PHP

The below cmdlet installs *php* , *libapache2-mod-php* and *php-mysql*

`$ sudo apt install php libapache2-mod-php php-mysql`

![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/install-php.PNG)


Use the below to confirm your php version 

`$ php -v`

![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/php-version.PNG)


**LAMP stack has been installed completely and it is operational**



**Step 4** - Creating a Virtual Host for your Website using Apache 2


Created a directory for projectlamp using ‘mkdir’ command

`$ sudo mkdir /var/www/projectlamp`

Assigned ownership to the directory with this variable $USER which still referenced the system user

`$ sudo chown -R $USER:$USER /var/www/projectlamp`

Created and opened a new configuration file in Apache’s sites-available directory using "vi" command-line editor

`$ sudo vi /etc/apache2/sites-available/projectlamp.conf`

Pasted the below bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and pasted the text:

```xml

<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

```

### To save and close the file, simply follow the steps below:

- Hit the esc button on the keyboard
- Type :
- Type wq. w for write and q for quit
- Hit ENTER to save the file

Used the below cmdlet to show the new file we created in the sites-available directory

`$ sudo ls /etc/apache2/sites-available`

See screenshot of the above cmds ran

![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/Virtualhost.PNG)
![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/Virtualhost2.PNG)


Enabled the new virtualhost created with the below

`$ sudo a2ensite projectlamp`

Disabled the default website that comes installed with Apache with the below

`$ sudo dissite 000-default`

![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/enable-disableVH.PNG)


Ran the below cmd to make sure the config file does not contain any syntax error

`$ sudo apache2ctl configtest`

Used the below to reload Apache so the changes can take effect

`$ sudo systemctl reload apache2`


![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/config-test.PNG)


Created an index.html file in the */var/www/projectlamp* location for testing if the virtual host works fine

`sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html`


The below screen shot is the output which shows Virtual host is working properly


![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/config-test-output.PNG)




**Step 5** - Enable PHP on the website

We needed to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive so as to allow the php page be the landing page

`sudo vim /etc/apache2/mods-enabled/dir.conf`

```xml

<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>

```

![](https://github.com/Tofumy/Tofumy-PBL/blob/main/change-dirconf.PNG)


Create a new file named index.php inside your custom web root folder:

`$ vim /var/www/projectlamp/index.php`


```php

<?php
phpinfo();

```

Reloaded apache for changes to take place

`$ sudo systemctl reload apache2`

![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/indexphp.PNG)



The output shows that PHP installation is working as expected

![PHP landing](https://github.com/Tofumy/Tofumy-PBL/blob/main/landingphp.PNG)
