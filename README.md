# LEMP-Stack-Implementation-AWS
This project demonstrates how to implement a LEMP stack on AWS.

LEMP Stack is one of the various technology stacks that exist. A technology stack is a set of frameworks and tools used to develop a software product. This set of frameworks and tools are very specifically chosen to work together in creating a well-functioning software. They are acronymns for individual technologies used together for a specific technology product. LEMP (Linux, Nginx, MySQL, PHP or Python, or Perl) is one of them. Others include; LAMP (Linux, Apache, MySQL, PHP or Python, or Perl), MERN (MongoDB, ExpressJS, ReactJS, NodeJS), MEAN (MongoDB, ExpressJS, AngularJS, NodeJS)

To perform this project, I must carry out the following steps
- Create a EC2 Instance
- Install the NGINX Web Server
- Install MYSQL
- Install PHP
- Configure NGINX to use PHP processor
- Test PHP with NGINX
- Retrieve data from MYSQL database with PHP

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
- Start off by updating your server’s package index. Following that, you can use apt install to get Nginx installed:
```
     sudo apt update
     sudo apt install nginx
```
- To verify that nginx was successfully installed and is running as a service in Ubuntu, run:
```
      sudo systemctl status nginx
```
- Now it is time for us to test how our Nginx server can respond to requests from the Internet. Open a web browser of your choice and try to access following url
```
      http://<Public-IP-Address>:80
```
![Screenshot (314)](https://github.com/user-attachments/assets/8af853e5-537b-4ed7-8c81-6c53b51874ae)

## Install MYSQL
- Use ‘apt’ to acquire and install this software:
```
      sudo apt install mysql-server
```
- It’s recommended that you run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system.
```
      sudo mysql_secure_installation
```
## Install PHP
- Having Nginx installed to serve your content and MySQL installed to store and manage your data. Now you can install PHP to process code and generate dynamic content for the web server.
```
   sudo apt install php-fpm php-mysql
```
## Configure NGINX to use PHP processor
- Create the root web directory for your_domain as follows:
```
      sudo mkdir /var/www/projectLEMP
```
- Assign ownership of the directory with the $USER environment variable, which will reference your current system user:
```
      sudo chown -R $USER:$USER /var/www/projectLEMP
```
- Open a new configuration file in Nginx’s sites-available directory using your preferred command-line editor. Here, we’ll use nano:
```
      sudo nano /etc/nginx/sites-available/projectLEMP
```
- This will create a new blank file. Paste in the following bare-bones configuration:
```
#/etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
```
- Activate your configuration by linking to the config file from Nginx’s sites-enabled directory:
```
      sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```
- This will tell Nginx to use the configuration next time it is reloaded. You can test your configuration for syntax errors by typing:
```
      sudo nginx -t
```
- You shall see following message:
```
      nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
      nginx: configuration file /etc/nginx/nginx.conf test is successful
```
- We also need to disable default Nginx host that is currently configured to listen on port 80, for this run:
```
      sudo unlink /etc/nginx/sites-enabled/default
```
- When you are ready, reload Nginx to apply the changes:
```
sudo systemctl reload nginx
```
- Your new website is now active, but the web root /var/www/projectLEMP is still empty. Create an index.html file in that location so that we can test that your new server block works as expected:
```
      sudo echo 'Hello LEMP from hostname' > /var/www/projectLEMP/index.html
```
- Now go to your browser and try to open your website URL using IP address:
```
      http://<Public-IP-Address>:80
```
![Screenshot (320)](https://github.com/user-attachments/assets/705c3986-003d-4bbc-b68b-0d05656d762f)

## Test PHP with NGINX
- Open a new file called info.php within your document root in your text editor:
```
      sudo nano /var/www/projectLEMP/info.php
```
- Type or paste the following lines into the new file. This is valid PHP code that will return information about your server:
```
      <?php
      phpinfo();
```
- You can now access this page in your web browser by visiting the domain name or public IP address you’ve set up in your Nginx configuration file, followed by /info.php:
```
      http://`server_domain_or_IP`/info.php
```
- You will see a web page containing detailed information about your serv

- After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment and your Ubuntu server. You can use rm to remove that file:
  ```
      sudo rm /var/www/your_domain/info.php
  ```
![Screenshot (322)](https://github.com/user-attachments/assets/1b5b19e9-a1b8-46d3-9e40-7965f378e954)

 ## Retrieve data from MYSQL database with PHP
 - You will create a test database (DB) with simple "To do list" and configure access to it, so the Nginx website would be able to query data from the DB and display it.
 - First, connect to the MySQL console using the root account:
```
      sudo mysql
```
- To create a new database, run the following command from your MySQL console:
```
      mysql> CREATE DATABASE `example_database`;
```
- Now you can create a new user and grant him full privileges on the database you have just created.
```
      mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
```
- Now we need to give this user permission over the example_database databas 
```
      mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';  
```
- Now exit the MySQL shell with:
```
      mysql> exit
```
- You can test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials: 
```
      mysql -u example_user -p
```
- After logging in to the MySQL console, confirm that you have access to the example_database database:
```
      mysql> SHOW DATABASES;
```
-  We’ll create a test table named todo_list. From the MySQL console, run the following statement:
```
      CREATE TABLE example_database.todo_list (
mysql>     item_id INT AUTO_INCREMENT,
mysql>     content VARCHAR(255),
mysql>     PRIMARY KEY(item_id)
mysql> );
```
- Insert a few rows of content in the test table. You might want to repeat the next command a few times, using different VALUES:
```
      mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
      mysql> INSERT INTO example_database.todo_list (content) VALUES ("My second important item");
      mysql> INSERT INTO example_database.todo_list (content) VALUES ("My third important item");
```
- To confirm that the data was successfully saved to your table, run:
```
      mysql>  SELECT * FROM example_database.todo_list;mysql>  SELECT * FROM example_database.todo_li
```
- After confirming that you have valid data in your test table, you can exit the MySQL console:
```
      mysql> exit
```
- Create a new PHP file in your custom web root directory using your preferred editor.
```
      nano /var/www/projectLEMP/todo_list.php
```
- Copy this content into your todo_list.php script:
```
      <?php
      $user = "example_user";
      $password = "password";
      $database = "example_database";
      $table = "todo_list";

      try {
        $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
        echo "<h2>TODO</h2><ol>";
        foreach($db->query("SELECT content FROM $table") as $row) {
        echo "<li>" . $row['content'] . "</li>";
        }
        echo "</ol>";
      } catch (PDOException $e) {
          print "Error!: " . $e->getMessage() . "<br/>";
          die();
      }
```
Save and close the file when you are done editing.

You can now access this page in your web browser by visiting the domain name or public IP address configured for your website, followed by /todo_list.php:
```
      http://<Public_domain_or_IP>/todo_list.php
```
- You should see a page like this, showing the content you’ve inserted in your test table:

![Screenshot (323)](https://github.com/user-attachments/assets/d49e77ac-3534-4523-86c8-820ab016b916)
