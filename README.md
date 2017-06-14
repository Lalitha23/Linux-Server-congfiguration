# Linux-Server-configuration

## Project 7 under the Full Stack Web Developer Nanodegree at Udacity

* public Ip: 34.210.105.100
* SSH PORT: 2200
# Tasks given and method for completion:
* **Create Linux server on Amazon lightsail**
  * Start a new Ubuntu Linux server instance on Amazon Lightsail.
  * Launch Amazon Lightsail terminal
  * Visit this link and press Create new instance of Ubuntu.
  * Download the default key-pair and copy to /.ssh folder.
  * Open your terminal and type in chmod 600 ~/.ssh/key.pem
  * Now Use the command ssh -i ~/.ssh/key.pem ubuntu@34.201.114.178 to create the instance on your terminal

* **Update all currently installed packages**
  * Find updates:sudo apt-get update
  * Install updates:sudo sudo apt-get upgrade Hit Y for yes and give yourself a break while it installs.
* **Change the SSH port from 22 to 2200 and other SSH configuration required from grading rubic**
  * nano /etc/ssh/sshd_config add port 2200 below port 22
  * while in the file also change PermitRootLogin prohibit-password to PermitRootLogin no to disallow root login
  * Change PasswordAuthentication from no to yes. We will change back after finishing SHH login setup
  * save file(nano: ctrl+x, Y, Enter)
  * restart ssh servicesudo service ssh reload
* **Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)**
  * Check UFW status to make sure its inactivesudo ufw status
  * Deny all incoming by defaultsudo ufw default deny incoming
  * Allow outgoing by defaultsudo ufw default allow outgoing
  * Allow SSH sudo ufw allow ssh
  * Allow SSH on port 2200sudo ufw allow 2200/tcp
  * Allow HTTP on port 80sudo ufw allow 80/tcp
  * Allow NTP on port 123sudo ufw allow 123/udp
  * Turn on firewallsudo ufw enable
* **Create grader user and give sudo permissions**
  * sudo adduser grader
  * sudo visudo
  * inside the file add grader ALL=(ALL:ALL) ALL below the root user under "#User privilege specification"
  * save file(nano: ctrl+x, Y, Enter)
  * Add grader to /etc/sudoers.d/ and type in grader ALL=(ALL:ALL) ALLby command sudo nano /etc/sudoers.d/grader
  * Add root to /etc/sudoers.d/ and type in root ALL=(ALL:ALL) ALLby command sudo nano /etc/sudoers.d/root
* **Create SSH keys and copy to server manually**
  * On your local machine generate SSH key pair with: ssh-keygen
  * save youkeygen file in your ssh directory /home/ubuntu/.ssh/ example full file path that could be used: /home/ubuntu/.ssh/item-catalog
  * You can add a password to use encase your keygen file gets compromised(you will be prompted to enter this password when you connect with key pair)
  * Change the SSH port number configuration in Amazon lightsail in networking tab to 2200.
  * login into grader account using password set during user creation ssh -v grader@*Public-IP-Address* -p 2200
  * Make .ssh directorymkdir .ssh
  * make file to store keytouch .ssh/authorized_keys
  * On your local machine read contents of the public key cat .ssh/project5.pub
  * Copy the key and paste in the file you just created in grader nano .ssh/authorized_keys paste contents(ctr+v)
  * save file(nano: ctrl+x, Y, Enter)
  * Set permissions for files: chmod 700 .ssh chmod 644 .ssh/authorized_keys
  * Change PasswordAuthentication from yes back to no. nano /etc/ssh/sshd_config
  * save file(nano: ctrl+x, Y, Enter)
  * login with key pair: ssh grader@Public-IP-Address* -p 2200 -i ~/.ssh/project5
* **Install and configure Apache to serve a Python mod_wsgi application**
  * sudo apt-get install apache2 Check if "It works!" at you public IP address given during setup.
  * install mod_wsgi: sudo apt-get install libapache2-mod-wsgi
  * configure Apache to handle requests using the WSGI module sudo nano /etc/apache2/sites-enabled/000-default.conf
  * add WSGIScriptAlias / /var/www/html/myapp.wsgi before </VirtualHost> closing line
  * save file(nano: ctrl+x, Y, Enter)
  * Restart Apache sudo apache2ctl restart
* **install git**
  * sudo apt-get install git
* **install python dev and verify WSGI is enabled**
  * Install python-dev packagesudo apt-get install python-dev
  * Verify wsgi is enabled sudo a2enmod wsgi
* ** Create flask app taken from digitalocean**
  * cd /var/www
  * sudo mkdir webapp
  * cd catalog
  * sudo mkdir catalog
  * cd catalog
  * sudo mkdir static templates
  * sudo nano __init__.py
  
    <code> from flask import Flask
app = Flask(__name__)
@app.route("/")
def hello():
    return "Hello, world (Testing!)"
if __name__ == "__main__":
app.run() </code>
* **Install Flask**
  * sudo apt-get install python-pip
  * pip install Flask
  * Make sure that the test flask __init__.py is working 
* **Create the wsgi file**
  * cd /var/www/catalog`
  * sudo nano catalog.wsgi
  ''''
    #!/usr/bin/python
  import sys
  import logging
  logging.basicConfig(stream=sys.stderr)
  sys.path.insert(0,"/var/www/catalog/")

  from catalog import app as application
  application.secret_key = 'Add your secret key' ''''
  
