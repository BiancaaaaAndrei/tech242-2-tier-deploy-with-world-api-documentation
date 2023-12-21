```
#!/bin/bash

#Update and upgrade
sudo DEBIAN_FRONTEND=noninteractive apt update -y
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y

#Install relevant packages
sudo DEBIAN_FRONTEND=noninteractive apt install mysql-client -y
sudo DEBIAN_FRONTEND=noninteractive apt install maven -y
sudo DEBIAN_FRONTEND=noninteractive apt install openjdk-17-jdk -y
sudo DEBIAN_FRONTEND=noninteractive apt install git -y

#!!!!!!!!
mysql -h <PUBLIC IP FROM THE DATABASE VM> -u <root> -p

#Fresh clone of the repo
git clone https://github.com/BiancaaaaAndrei/tech242-2-tier-deploy-with-world-api.git

cd tech242-2-tier-deploy-with-world-api/WorldProject

#Set environment variables
export DB_HOST=jdbc:mysql://PUBLIC IP FROM THE DATABASE VM:3306/world
export DB_USER=root
export DB_PASS=root

#Springboot start
sudo -E mvn package spring-boot:start

#Back to roo
cd /

# Install Apache2
sudo DEBIAN_FRONTEND=noninteractive apt install apache2 -y

# Enable relevant apache proxy modules
sudo a2enmod proxy
sudo a2enmod proxy_http

# Restart Apache
sudo systemctl restart apache2

# Edit config file if not configured
if grep -q 'ProxyPass / http://localhost:5000/' /etc/apache2/sites-available/000-default.conf; then
    # The string exists, so nothing to do
    echo "Reverse proxy already configured."
else
    # reverse proxy not configured yet
    echo "Reverse proxy NOT configured."
    sudo sed -i '/DocumentRoot \/var\/www\/html/ a\ProxyPreserveHost On\nProxyPass \/ http:\/\/localhost:5000\/\nProxyPassReverse \/ http:\/\/localhost:5000\/\n' /etc/apache2/sites-available/000-default.conf

fi

# Restart Apache
sudo systemctl restart apache2

```