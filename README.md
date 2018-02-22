# LINUX SERVER CONFIGURATION
  This project is a baseline installation of a linux server which is prepared to host web application. The server is protected 


### IP Address
   

### URL


## Steps for Configuration
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
2200
123
      
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
2. Enable the Password Authentication 
3. Give Grader sudo permissions
```
      sudo nano /etc/sudoers.d/grader
```
      Insert the following line -
      grader ALL=(ALL) NOPASSWD
4. Login as Grader
5. Create SSH keypair
```
      ssh-keygen
```
                 