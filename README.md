# Linux-Server-configuration

## Project 7 under the Full Stack Web Developer Nanodegree at Udacity

* public Ip: 34.210.105.100
* SSH PORT: 2200
# Tasks given and method for completion:

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
