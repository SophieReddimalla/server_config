# LINUX SERVER CONFIGURATION
  This project is a baseline installation of a linux server which is prepared to host web application. The server is protected 


### IP Address
13.126.166.84

### URL
[https://ec2-13-126-166-84.ap-south-1.compute.amazonaws.com](http://ec2-13-126-166-84.ap-south-1.compute.amazonaws.com).

## Steps for Configuration
***
### Securing the server:
1. Update and Upgrade all packages
```sh
        sudo apt-get update 
        sudo apt-get upgrade
```
2. Change SSH port from 22 to 2200
```
        sudo nano /etc/ssh/sshd_config
```
Locate the line Port 22 change it to Port 2200
Restart ssh service
```
        sudo service ssh restart
```

3. In Lightsail Firewall open the following add following custom Ports
      * 2200
      * 123
      
4. Configure the Uncomplicated Firewall(UFW)
```
      sudo ufw default deny incoming
      sudo ufw default allow outgoing 
      sudo ufw allow 2200
      sudo ufw allow www
      sudo ufw allow 123
      sudo ufw enable
```
***
### Create New User grader
1. Create User grader
```
      sudo adduser grader
```
2. Enable the Password Authentication (Temporarily till ssh keys are in place):
```
      sudo nano /etc/ssh/sshd_config
      PasswordAuthentication  yes
```       
3. Give Grader sudo permissions
```
      sudo nano /etc/sudoers.d/grader
      # Add following line:
      grader ALL=(ALL) NOPASSWD
```
4. Login as Grader with user ID and Password

5. Create SSH keypair
```
      ssh-keygen
```
  Add generated public key to the authorized_keys file :
```
      cat ~/.ssh/id_rsa.pub > ~/.ssh/authorized_keys
```
  Open private key in nano and copy the content to the clipboard:
```
     sudo nano ~/.ssh/id_rsa                   
```
   * Then open Notepad and paste the clipboard content .
   * Save the file .
   * Convert saved private key to .ppk format using PuttyGen tool.
   * Use the above private key to connect to as grader in putty.
   * After ensuring successfull authentication change PasswordAuthentication to no in /etc/ssh/sshd_config

6. Set permissions to .ssh and authorized_keys
```
      chmod 700 .ssh
      chmod 600 authorized_keys
```
***
### Enable NTP service to synchronize time
1. Set the localtimezone to UTC
   * Install ntp 
```
     sudo apt-get install ntp
```
   * Execute the following command:
``` 
     sudo dpkg-reconfigure tzdata
```
   * Scroll to the bottom of the Continents list and select "None of the above"
   * Then in the subsequent menu select UTC 
### Configure Webserver - Apache2  
1. Configure Apache2
   * Install Apache
``` 
     sudo apt-get install apache2
```
   * Install Web Server Gateway Interface for Python 2 applications
```
      sudo apt-get install libapache2-mod-wsgi   
```
***
### Configure Database Server - PostgreSQL
1. Install PostgreSQL
```
     sudo apt-get install postgresql postgresql-contrib
```
2. Create Catalog Database using postgres user
```
    sudo -u postgres createdb catalog
``` 

3. Create a database User catalog
```
     sudo -u postgres createuser catalog
```
4. Limit the permissions for the catalog user
   * On psql prompt grant following permission
```
     sudo -u postgres psql
     postgres# GRANT ALL PRIVILEGES ON DATABASE CATALOG TO CATALOG;
     \q
***

### Deploying Item catalog project

1. Install git
``` 
      sudo apt-get install git
```
2. Clone the catalog project
```    
      cd /var/www
      sudo git clone  https://github.com/SophieReddimalla/sports_catalog.git catalog
      (Above Repository contains the changes needed to run it on Amazon Web Services)
```
3. Setup the Virtual Environment
```
      sudo apt-get install python-pip
      sudo pip install virtualenv
```
      * Change the directory to catalog
```
      cd catalog
```
      * Create virtual environment
```
      virtualenv venv
      source venv/bin/activate 
```                 
      * To install the project dependencies
```
      pip install -r requirements.txt
```
      * Table creation and seeding with data
``` 
      python catalog_setup.py
```
## Create website in Apache
1. Create site config file.
      In folder /etc/apache2/site-available create file catalog.conf with following content 
      
      <VirtualHost *:80>
                  ServerName 13.126.166.84
                  ServerAdmin sophie.reddimalla@gmail.com
                  WSGIScriptAlias / /var/www/catalog/catalog.wsgi
                  <Directory /var/www/catalog/catalog/>
                              Order allow,deny
                              Allow from all
                  </Directory>
                  Alias /static /var/www/catalog/catalog/static
                  <Directory /var/www/catalog/catalog/static/>
                              Order allow,deny
                              Allow from all
                  </Directory>
                  ErrorLog ${APACHE_LOG_DIR}/error.log
                  LogLevel warn
                  CustomLog ${APACHE_LOG_DIR}/access.log combined
      </VirtualHost>
2. Enable the site.
      * Disable the existing default site
      ```
      sudo a2dissite 000-default.conf
      ```
      * Enable catalog site
      ```
      sudo a2ensite catalog.conf
      ```

## Resources 

Following websites provided lot of useful information for the completting this project. Also for correcting the errors encountered during
the process.

http://digitalocean.com
http://stackoverflow.com
http://aws.amazon.com documentation

## Acknowlegements
Samson Reddimalla



      *