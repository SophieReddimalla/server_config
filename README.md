# ** LINUX SERVER CONFIGURATION **
  This project is a baseline installation of a linux server which is prepared to host web application. The server is protected 

***
## IP Address
   
***
## URL

***
## Walkthrough
### Securing the server:
    1.Update and Upgrade all packages
      '''
      sudo apt-get update 
      sudo apt-get upgrade
      '''
    2.Change SSH port from 22 to 2200
      '''
      sudo nano /etc/ssh/sshd_config
      '''
    3.Configure the Lightsail Firewall open the following    ports:
      '''
      Port 2200
      Port 123
      '''
    4.Configure the Uncomplicated Firewall(UFW)
      '''
      sudo ufw status
      sudo ufw default deny incoming
      sudo ufw default allow outgoing 
      sudo ufw allow 2200
      sudo ufw allow www
      sudo ufw allow 123
      sudo ufw enable
      '''  
                  