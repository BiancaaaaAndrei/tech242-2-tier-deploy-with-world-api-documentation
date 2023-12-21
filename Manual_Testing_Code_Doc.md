## Tech242 2-Tier Deployment with World API - Additional VM Setup and Reversed Proxy Configuration
This section outlines the steps to create another virtual machine, connect it to the database VM, and configure a reverse proxy for the World API application.

## Step 13. Create Another Virtual Machine for the World API repo
## 13.1 Update Package Repository and Install MySQL Client

```sudo apt-get update```
```sudo apt-get install mysql-client```

## Step 14. Connect to the Database VM

```mysql -h <hostname_or_ip> -u <root> -p```

## Step 15. Ensure Connection

```SHOW DATABASES;```

## Step 16. Clone Application Repository

```git clone https://github.com/BiancaaaaAndrei/tech242-2-tier-deploy-with-world-api.git```

## Step 17. Navigate to the Application Directory

```cd tech242-2-tier-deploy-with-world-api/WorldProject```

## Step 18. Set Environment Variables

```export DB_HOST=PUBLIC IP FROM DB VM```
```export DB_USER=root```
```export DB_PASS=root```

## Step 19. Verify Environment Variables

```echo $DB_HOST```
```echo $DB_USER```
```echo $DB_PASS```

## Step 20. Maven Configuration
If using sudo for Maven commands, preserve environment variables:

```sudo apt-get update```
```sudo DEBIAN_FRONTEND=noninteractive apt-get install maven -y```
```sudo -E mvn clean install```
If encountering issues during testing, consider to:

1. Check Test Class and Configuration: Ensure the CountryLanguageTests test class is annotated with @SpringBootTest and has correct configurations.

2. Update DB_HOST to:

```export DB_HOST=jdbc:mysql://3.253.53.2:3306/world```

## Step 21. Verify the variable:

```printenv DB_USER```

## Step 22. Run Maven with preserved environment:

```sudo -E mvn package spring-boot:start```

## Step 23. Reversed Proxy Configuration
Create a bash script for configuring a reverse proxy:
```
#!/bin/bash

# Navigate back to root
cd /

# Install Apache2
sudo DEBIAN_FRONTEND=noninteractive apt install apache2 -y

# Enable relevant Apache proxy modules
sudo a2enmod proxy
sudo a2enmod proxy_http

# Restart Apache
sudo systemctl restart apache2

# Edit config file if not configured
if grep -q 'ProxyPass / http://localhost:5000/' /etc/apache2/sites-available/000-default.conf; then
    # The string exists, so nothing to do
    echo "Reverse proxy already configured."
else
    # Reverse proxy not configured yet
    echo "Reverse proxy NOT configured."
    sudo sed -i '/DocumentRoot \/var\/www\/html/ a\ProxyPreserveHost On\nProxyPass \/ http:\/\/localhost:5000\/\nProxyPassReverse \/ http:\/\/localhost:5000\/\n' /etc/apache2/sites-available/000-default.conf
fi

# Restart Apache
sudo systemctl reload apache2
```