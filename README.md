# Installing wordpress and MySQL on EC2 instance
# Step1: launch EC2 instance
Create an EC2 instance with Ubuntu image
and configure security group with HTTP, HTTPS & SSH.

Connect ec2 with following command
ssh -i home/downloads/ec2.pem ubuntu@10.2.143.45
(replace directory path of your pem file and punlic IP of ec2 instance)

# Step2: Install Apache
Open the terminal on your Ubuntu system. The terminal is a text interface to your computer, which you will use to run all the commands.

First, update your software package list.
ubuntu@ubunu2004:~$ sudo apt-get update

The next step will be installing and configuring Apache2, the web server. Run the below command to install Apache 2 on Ubuntu 20.04.
ubuntu@ubunu2004:~$ sudo apt install apache2

Now verify Apache2 status as well.
ubuntu@ubunu2004:~$ sudo systemctl status apache2

Open your web browser, and type localhost it will display the default Apache2 index page.

# Step 3: Install MySQL

After Apache has been started, it is time to install MySQL. Run the following command in the terminal to do this:
ubuntu@ubunu2004:~$ sudo apt install mysql-server

Run below command to remove unsecure default settings and protect your database.
ubuntu@ubunu2004:~$ sudo mysql_secure_installation

You will be asked to install the validate_password plugin. So, type Y/Yes, and finally choose the default password strength.
To answer the remaining questions, press Y and hit the ENTER key for each prompt.

Verify mysql status
ubuntu@ubunu2004:~$ sudo systemctl status mysql

# Step 4: Install PHP
WordPress is a PHP-based CMS. We need PHP to process the dynamic content on our WordPress site.

We will need additional modules to allow PHP to communicate with Apache and MySQL instances. The following command will install PHP along with the MySQL and Apache modules.
ubuntu@ubunu2004:~$ sudo apt install php libapache2-mod-php php-mysql

WordPress and many plugins use PHP extensions, which you will need to install manually.
ubuntu@ubunu2004:~$ sudo apt install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip

The following command will verify that PHP 7.4 has been successfully installed.
ubuntu@ubunu2004 :$ php -v

# Step 5: Install WordPress
First, we will download the WordPress installation files and place them in the default web server root directory /var/www/html.

ubuntu@ubunu2004:~$ cd /var/www/html

Now download the latest WordPress install with the following command.
ubuntu@ubunu2004:/var/www/html$ sudo wget -c http://wordpress.org/latest.tar.gz

Extract the files
ubuntu@ubunu2004:/var/www/html$ sudo tar -xzvf latest.tar.gz

ubuntu@ubunu2004:/var/www/html$ ls -l
The extracted WordPress files will be now placed in the WordPress directory at the following location on your server 

/var/www/html/wordpress

The user of your web server must own these files.

We are using Apache as our web server. Apache is running on Ubuntu 20.04. The following command will allow you to change the owner of these files and set the appropriate permissions:
ubuntu@ubunu2004: sudo chown -R www-data:www-data /var/www/html/wordpress

# Step 6: Create a Database for WordPress
Next, we will create a WordPress database for the site and set up a user account. This will make it easier to manage the site and increase its security.

Log in to your MySQL root account via Terminal by entering:

When prompted, enter the MySQL root password we have previously set up.

ubuntu@ubunu2004:/var/www/html$ sudo mysql -u root -p
Create a separate database for WordPress to manage

CREATE DATABASE wordpressdb;
To access the new database, we will create a MySQL user account. Enter a strong password

CREATE USER userashok@localhost IDENTIFIED BY 'Pass@123';
You have just created a new user.

GRANT ALL PRIVILEGES ON wordpressdb. * TO userashok@localhost;
After completing the above, flush your privileges to allow MySQL to implement the changes.

FLUSH PRIVILEGES;
exit;

Allow the executable permission to be granted to the WordPress folder.
ubuntu@ubunu2004:/var/www/html$ sudo chmod -R 777 wordpress/

ubuntu@ubunu2004:/var/www/html$ cd wordpress/

# Step 7: Setup and Configure WordPress
Setup wordpress config file to communicate with database. 
Firstly, you need to create a configuration file for WordPress. So, rename the sample WordPress configuration file using the following command:

ubuntu@ubunu2004:/var/www/html/wordpress$ mv wp-config-sample.php wp-config.php

Edit the wpconfig. As shown below, edit the php file.
ubuntu@ubunu2004:/var/www/html/wordpress$ vim wp-config.php

Update the database settings by replacing demo_db, demo_user, and demo_password with your own details.
Save the file and close it.

Once you have done this, you can access your WordPress. Open the browser and go to https://localhost/wordpress/

Configure the minimum details and the WordPress dashboard page will greet you.
Congratulations, WordPress installation is successful!
