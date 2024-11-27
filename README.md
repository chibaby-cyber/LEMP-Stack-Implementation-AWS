# LEMP-Stack-Implementation-AWS
This project demonstrates how to iplement a LEMP stack on AWS.

LEMP Stack is one of the various technology stacks that exist. A technology stack is a set of frameworks and tools used to develop a software product. This set of frameworks and tools are very specifically chosen to work together in creating a well-functioning software. They are acronymns for individual technologies used together for a specific technology product. LEMP (Linux, Nginx, MySQL, PHP or Python, or Perl) is one of them. Others include; LAMP (Linux, Apache, MySQL, PHP or Python, or Perl), MERN (MongoDB, ExpressJS, ReactJS, NodeJS), MEAN (MongoDB, ExpressJS, AngularJS, NodeJS)

To perform this project, I must carry out the following steps
- Create a EC2 Instance
- Install the NGINX Web Server
- Install MYSQL
- Install PHP
- Configure NGINX to use PHP processor
- Test PHP with NGINX
- Retieve data from MYSQL database with PHP

## Create a EC2 Instance
In order to complete this project I will need an AWS account and a virtual server with Ubuntu Server OS.

AWS is the biggest Cloud Service Provider and it offers a free tier account that we are going to leverage for our projects.

I followed the instructions below to create your EC2 instance.

1. Register a new AWS account.
2. Select your preferred region (the closest to you) and launch a new EC2 instance of t2.micro family with Ubuntu Server launch EC2
   
IMPORTANT – save your private key (.pem file) securely and do not share it with anyone! If you lose it, you will not be able to connect to your server ever again!

### Connecting to EC2 terminal
- Move into the folder where the pair key is downloaded and run the following command to connect to the instance
```
      cd ~/Downloads
      sudo chmod 0400 <private-key-name>.pem
      ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>
```

## Install the NGINX Web Server




