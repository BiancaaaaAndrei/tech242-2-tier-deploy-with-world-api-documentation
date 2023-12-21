# Tech242 2-Tier Deployment with World API - Database Setup

## Step 1. Update the Package Repository

```
sudo apt update

```

## Step 2. Install MySQL Server

```
sudo apt install mysql-server
```


## Step 3. Check MySQL Server Status
```
sudo service mysql status
```

## Step 4. Download SQL from Repository
```
git clone https://github.com/BiancaaaaAndrei/tech242-2-tier-deploy-with-world-api-database.git
```

## Step 5. Connect to the Database
```
sudo mysql -h localhost -P 3306 -u root -p
```

- h localhost: Specifies the host where the MySQL server is running (localhost for this case).
- P 3306: Specifies the MySQL server port (default is 3306).
- u root: Specifies the MySQL user (root in this case).
- p: Prompts for the password after executing the command.


## Step 6. Import SQL and Interact with Database

```source tech242-2-tier-deploy-with-world-api-database/world.sql```
```sudo mysql -h localhost -P 3306 -u root -p```
```SHOW DATABASES;```
```SHOW TABLES;```

## Step 7. Open MySQL Configuration File

```sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf```

## Step 8. Allow Connections

```sudo sed -i 's/^bind-address\s*=\s*127\.0\.0\.1/bind-address = 0.0.0.0/' /etc/mysql/mysql.conf.d/mysqld.cnf```

## Step 9. Restart MySQL Service

```sudo service mysql restart```

## Step 10. Verify Root User Connection

```sudo mysql -u root -p```
- Password: root

## Step 11. Create or Alter Root User

```sudo mysql -u root -p -e "CREATE USER 'root'@'%' IDENTIFIED BY 'root';"```

```sudo mysql -u root -p -e "ALTER USER 'root'@'%' IDENTIFIED BY 'root';"```


## Step 12. Grant All Privileges

```sudo mysql -u root -p -e "GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;"```

```sudo mysql -u root -p -e "FLUSH PRIVILEGES;"```

```sudo mysql -u root -p -e "EXIT;"```

