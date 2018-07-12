# Linux-Server-Configuration
This repository is for Udacity Full Stack Web Developer Nanodegree Program Submission


## User Management
1. Create user
```
$ sudo adduser grader
```
2. Give `grader` sudo access
```
$ sudo visudo
```
Add line: `grader ALL=(ALL) NOPASSWD:ALL`

3. Disable remote login as `root`
```
$ sudo nano /etc/ssh/sshd_config
```
Change line to : `PermissionRootLogin no`

## SSH Login with Key
1. Change the SSH default port to `2200`
```
$ sudo nano /etc/ssh/sshd_config
```
Change line to : `Port 2200`

2. Change user to `grader` and generate key pair
```
$ sudo su - grader
```
Follow this [instruction](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2) or udacity course.

3. Reload ssh
```
$ sudo service ssh restart
```

4. You can log in using the following command
```
$ ssh -i [key file path] grader@18.179.61.173 -p 2200
```

## Firewall Settings
Only allow connections for `SSH`(port 2200), `HTTP`(port 80) and `NTP`(port 123)
```
$ sudo ufw allow 2200/tcp
$ sudo ufw allow www
$ sudo ufw allow ntp
$ sudo ufw enable
```

## Update System Packages
```
$ sudo apt-get update
$ sudo apt-get upgrade
```

## Install Apache and mod_wsgi
```
$ sudo apt-get install apache2
$ sudo apt-get install libapache2-mod-wsgi
$ sudo service apache2 restart
```

## Install PostgreSQL and Create Database
1. Install PostgreSQL
```
$ sudo apt-get install postgresql
```

2. Login as user `postgres`
```
$ sudo su - postgres
```

3. Create database
```
$ psql
```
```
postgres=# CREATE DATABASE catalog;
postgres=# CREATE USER catalog;
postgres=# ALTER ROLE catalog WITH PASSWORD 'catalogpassword';
```
4. Give user permission
```
postgres=# GRANT ALL PRIVILEGES ON DATABASE catalog TO catalog;
postgres=# \q
```
5. Exit from user `postgres`
```
$ exit
```

## Clone Catalog App
1. Change directory
```
$ cd /var/www/
```
2. Create directory and move inside it
```
$ sudo mkdir catalog; cd catalog
```
3. Clone Smartphone Catalog App project
```
$ git clone https://github.com/yuyuJP/Smartphone-Catalog-App.git
```

4. Create `catalog` directory in catalog directory
```
$ sudo mkdir catalog
```

5. Move files to catalog directory
```
$ mv Smartphone-Catalog-App/vagrant/catalog/* ./catalog
```

## Setup Catalog App
1. Install pip
```
$ sudo apt-get install python-pip
```

2. Install dependencies
```
$ sudo pip install sqlalchemy flask-sqlalchemy psycopg2-binary bleach requests
$ sudo pip install flask packaging oauth2client redis passlib flask-httpauth
```

3. Setup database
```
$ sudo python database_setup.py
```
4. Add items to database
```
$ sudo python addmanyitems.py
```

## Configure Web Server
1. Create catalog.conf file
```
sudo nano /etc/apache2/sites-available/catalog.conf
```
2. Add the following lines
```
  <VirtualHost *:80>
        ServerName catalog-app
        ServerAdmin yu.yu.jp0@gmail.com
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

```
