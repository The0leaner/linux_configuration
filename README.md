# Udacity - Linux server configuration project

The application meant to be deployed is the **Item catalog app**

## Useful info
IP address:139.59.110.131.
Accessible SSH port: 2200.

## Steps
### 1 - Create a new user named *grader* and grant this user sudo permissions.

1. Log into the remote VM as *root* user through ssh: `$ ssh root@YOUR IP ADDRESS`.
2. Add a new user called *grader*: `$ sudo adduser grader`.
3. Create a new file under the suoders directory: `$ sudo nano /etc/sudoers.d/grader`. Fill that newly created file with the following line of text: "grader ALL=(ALL:ALL) ALL", then save it.
4. Configure the key-based authentication for *grader* user


### 2 - Configure the local timezone to UTC

1. Open time configuration dialog and set it to UTC with: `$ sudo dpkg-reconfigure tzdata`.

### 3 - Configure the Uncomplicated Firewall (UFW)

Project requirements need the server to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).

1. `$ sudo ufw allow 2200/tcp`.
2. `$ sudo ufw allow 80/tcp`.
3. `$ sudo ufw allow 123/udp`.
4. `$ sudo ufw enable`.

### 4 - Enforce key-based authentication
1. `$ sudo nano /etc/ssh/sshd_config`. Find the *PasswordAuthentication* line and edit it to *no*.
2. `$ sudo service ssh restart`.

### 5 - Change the SSH port from 22 to 2200
1. `$ sudo nano /etc/ssh/sshd_config`. Find the *Port* line and edit it to *2200*.
2. `$ sudo service ssh restart`.
tip: [Ubuntu forums](http://ubuntuforums.org/showthread.php?t=1739013).

### 6 - Install *fail2ban* 
1. `$ sudo apt-get install fail2ban`.
tip: [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-protect-ssh-with-fail2ban-on-ubuntu-14-04), [Reddit](https://www.reddit.com/r/linuxadmin/comments/2lravs/fail2ban_does_not_detect_my_ssh_privatekey/).

### 7 - Update all currently installed packages

1. `$ sudo apt-get update`.
2. `$ sudo apt-get upgrade`.
3. Install *finger*, a utility software to check users' status: `$ apt-get install finger`.
4. `$ sudo apt-get install unattended-upgrades`.
5. To enable it, do: `$ sudo dpkg-reconfigure --priority=low unattended-upgrades`.

### 8 - Install Apache, mod_wsgi, Git

1. `$ sudo apt-get install apache2`.
2. Mod_wsgi is an Apache HTTP server mod that enables Apache to serve Flask applications. Install *mod_wsgi* with the following command: `$ sudo apt-get install libapache2-mod-wsgi python-dev`.
3. Enable *mod_wsgi*: `$ sudo a2enmod wsgi`.
3. `$ sudo service apache2 start`.
4. `$ sudo apt-get install git`.
5. Configure your username: `$ git config --global user.name <username>`.
6. Configure your email: `$ git config --global user.email <email>`.

### 9 - Setup for deploying a Flask Application on Ubuntu VPS

tip: [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)

### 10 - Clone GitHub repository and make it web inaccessible
1. Clone project 3 solution repository on GitHub:  
  `$ git clone https://github.com/stueken/FSND-P3_Music-Catalog-Web-App.git`
2. Move all content of created FSND-P3_Music-Catalog-Web-App directory to `/var/www/catalog/catalog/`-directory and delete the leftover empty directory.
3. Make the GitHub repository inaccessible:
tip: [Stackoverflow](http://stackoverflow.com/questions/6142437/make-git-directory-web-inaccessible)

### 11 - Install needed modules & packages
1. Activate virtual environment:  
  `$ source venv/bin/activate`
2. Install httplib2 module in venv:  
  `$ pip install httplib2`
  `$ pip install requests`
  `$ *sudo pip install flask-seasurf`
  `$ sudo pip install --upgrade oauth2client`
  `$ sudo pip install sqlalchemy`
  `$ sudo apt-get install python-psycopg2`

### 12 - Install and configure PostgreSQL
tip: [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps)

#### 13 - Run application 
1. Restart Apache:  
  `$ sudo service apache2 restart`
2. Open a browser and put in your public ip-address as url

### 14 - Get OAuth-Logins Working
   
1. Open [host2ip](http://www.hcidata.info/host2ip.cgi) and receive the Host name for your public IP-address

### 15 - Install system monitor tools

1. `$ sudo apt-get update`.
2. `$ sudo apt-get install glances`.

#### Special thanks to Steve Woodings for his patient support
