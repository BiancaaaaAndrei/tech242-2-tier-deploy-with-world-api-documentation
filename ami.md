## AMI Stage

- Start by launching an EC2 instance with the desired configuration.
- Connect to the instance and configure it according to the needs, including installing necessary software and configuring settings.
- Prepare the Database VM (User Data):
    - Add the following user data script to the EC2 instance, which is responsible for restarting the MySQL service upon instance launch [DB Script](DB_Code.md)


## Step 1. Create an AMI:

- Once the instance is configured and the user data script is added, navigate to the AWS Management Console.
- In the EC2 Dashboard, select the instance you just configured.
- Click on "Actions" -> "Create Image."
- Fill in the required details and create the AMI.
- Launching an Instance from the AMI:
  
## Step 2. Launch a New EC2 Instance

## Step 3. Configure Instance Settings:

## Step 4. Add User Data for Database AMI Instance:

- Add the following user data script for the App VM instance. Make sure to replace PUBLIC IP DATABASE AMI INSTANCE with the actual public IP of your Database AMI instance:
```
#!/bin/bash
sudo systemctl restart mysql
```

## Follow Step 1, 2, 3 and 4 to create the Application AMI Instance:
```
#!/bin/bash
export DB_HOST=jdbc:mysql://PUBLIC IP DATABASE AMI INSTANCE:3306/world
export DB_USER=root
export DB_PASS=root
sudo systemctl start apache2
cd tech242-2-tier-deploy-with-world-api/WorldProject
mvn clean package spring-boot:start
```
