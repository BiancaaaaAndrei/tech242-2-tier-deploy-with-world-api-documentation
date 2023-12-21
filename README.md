# Tech242 2-Tier Deployment with World API

This repository documents the deployment process for a 2-tier application on AWS, consisting of an app/API running on one VM and a MySQL database running on another VM.

## Repository Structure

- [App Code Repository](https://github.com/BiancaaaaAndrei/tech242-2-tier-deploy-with-world-api.git)
- [Database Repository](https://github.com/BiancaaaaAndrei/tech242-2-tier-deploy-with-world-api-database.git)

## Prerequisites

- AWS Account
- 2 Virtual Machines (VMs)
  - VM1: For running the app/API
  - VM2: For running the MySQL database

## Deployment Steps

### 1. Manual Deployment

#### 1.1 Set Up VMs

- Create two VMs on AWS: VM1 for the app/API and VM2 for the MySQL database.
- Started off with the database VM because our application heavily depends on the database, it makes sense to ensure that the database is up and running before deploying the application. This approach helps avoid issues where the application is deployed, but it cannot connect to the database due to its unavailability.
- Ensure proper security group settings for each VM to allow necessary communication.

#### 1.2 Deploy App/API

- Clone the app code repository to VM1.
- Install dependencies and configure the app on VM1.
- Start the app/API.

#### 1.3 Deploy MySQL Database

- Clone the MySQL database code repository to VM2.
- Install and configure MySQL on VM2.
- Create necessary databases and tables.

## Step by step on how I manually tested the script:
- Documentation on how I [manually tested the DB script](Manual_Testing_Doc.md)
- Documentation on how I [manually tested the app code script](Manual_Testing_Code_Doc.md)

### 2. Automation on AWS

- [Database Script](App_Code.md)
- [App Code Script](App_Code.md)

### 3. User Data on AWS

- Both machines were then automated via user data.
- Handling user data became a seamless process for my logic tier.
- Method: copy and paste my script for the specific VM
- For the Application VM made sure that the Public IP address from the database machine was correct in order to succesfuly connect with the MySQL database. 
  
### 4. AMI Instances

Tutorial [AMI Instance](ami.md)