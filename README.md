# Bash_Scripts_for_Automation

#Update the Ubuntu OS
sudo apt update
#Install Apache in non-interactive mode
sudo apt install apache2 -y
#Install mysql community server
sudo apt install mysql-server -y
#Update root-user password
sudo mysql -e "ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';"
#Login and test root user login
sudo mysql -u root -p password -e "SHOW DATABASES;"
#Install PHP, php-apache and php-mysql libraries and dependencies
sudo apt install php libapache2-mod-php php-mysql -y
#Check php version
php -v
#Create a directory to hold you website files
sudo mkdir /var/www/projectlamp
#Change ownership to current user
sudo chown -R $USER:$USER /var/www/projectlamp

#Copy configuration content into .../projectlamp.conf
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

sudo a2ensite projectlamp
sudo a2dissite 000-default
sudo apache2ctl configtest
sudo systemctl reload apache2
#Create an index.html file under projectlamp directory
sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html

#Change Direction Index to allow .php over .html
cat > /etc/apache2/mods-enabled/dir.conf <<"EOF"
<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
EOF

sudo systemctl reload apache2

echo "<?php
phpinfo();" > /var/www/projectlamp/index.php
