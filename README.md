# Bash_Scripts_for_Automation

#Update the Ubuntu OS <br>
sudo apt update <br>
#Install Apache in non-interactive mode <br>
sudo apt install apache2 -y <br>
#Install mysql community server <br>
sudo apt install mysql-server -y <br>
#Update root-user password <br>
sudo mysql -e "ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';" <br>
#Login and test root user login <br>
sudo mysql -u root -p password -e "SHOW DATABASES;" <br>
#Install PHP, php-apache and php-mysql libraries and dependencies <br>
sudo apt install php libapache2-mod-php php-mysql -y <br>
#Check php version <br>
php -v <br>
#Create a directory to hold you website files <br>
sudo mkdir /var/www/projectlamp <br>
#Change ownership to current user <br>
sudo chown -R $USER:$USER /var/www/projectlamp <br>

#Copy configuration content into .../projectlamp.conf <br>
```cat > /etc/apache2/sites-available/projectlamp.conf <<"EOF"
<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
EOF
```

sudo a2ensite projectlamp <br>
sudo a2dissite 000-default <br>
sudo apache2ctl configtest <br>
sudo systemctl reload apache2 <br>
#Create an index.html file under projectlamp directory <br>
sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html <br>

#Change Direction Index to allow .php over .html <br>
```
cat > /etc/apache2/mods-enabled/dir.conf <<"EOF"
<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
EOF
```

sudo systemctl reload apache2 <br>

echo "<?php
phpinfo();" > /var/www/projectlamp/index.php <br>
