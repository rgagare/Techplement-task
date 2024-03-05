Skip to content

    rgagare
    /
    Techplacement-task

Code
Issues
Pull requests
Actions
Projects
Wiki
Security
Insights

    Settings

Home
Rohit Gagare edited this page Mar 5, 2024 · 1 revision

1

2 Deploying WordPress and MySQL on AWS EC2 This guide outlines the steps to deploy a WordPress application in both monolithic and microservices architectures on AWS EC2.

Prerequisites:

Free-tier AWS account: Sign up for a free AWS account if you haven't already. Monolithic Architecture (1 EC2 Instance)

    Launch EC2 Instance:

Login to the AWS Management Console and navigate to the EC2 service. Click on "Launch Instance". Choose an Amazon Machine Image (AMI): Select "Ubuntu Server 20.04 LTS (Focal Fossa)". Choose an Instance Type: Select "t2.micro" for a free-tier eligible instance. Configure Security Groups: Create a new security group and allow inbound traffic on ports: SSH (port 22) for remote access. HTTP (port 80) and HTTPS (port 443) for WordPress access. Launch the instance. 2. Install and Configure MySQL:

Connect to the EC2 instance using SSH. Update package lists: sudo apt update Install MySQL server: sudo apt install mysql-server Secure the MySQL installation by following the official guide: https://dev.mysql.com/doc/refman/8.0/en/mysql-secure-installation.html Create a database and user for WordPress: Login to MySQL: sudo mysql Create a database: CREATE DATABASE wordpress; Create a user: CREATE USER wordpress_user@localhost IDENTIFIED BY 'StrongPassword'; Grant privileges to the user: GRANT ALL PRIVILEGES ON wordpress.* TO wordpress_user@localhost; Exit MySQL: exit; 3. Install and Configure Apache:

Install Apache web server: sudo apt install apache2 Enable Apache to start at boot: sudo systemctl enable apache2 4. Install and Configure WordPress:

Download the latest WordPress version: wget https://wordpress.org/latest.tar.gz Extract the downloaded file: tar -xzf latest.tar.gz Move the extracted files to the Apache document root: sudo mv wordpress /var/www/html Create a virtual host configuration file for WordPress: Edit /etc/apache2/sites-available/wordpress.conf Add the following content, replacing <public_ip> with your EC2 instance's public IP: Apache <VirtualHost *:80> ServerAdmin webmaster@example.com DocumentRoot /var/www/html/wordpress

<Directory /var/www/html/wordpress>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>

ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined

Use code [with caution.](https://gemini.google.com/faq#coding) Enable the virtual host configuration: sudo a2ensite wordpress.conf Restart Apache: sudo systemctl restart apache2 5. Access WordPress:

Open a web browser and navigate to your EC2 instance's public IP address (e.g., http://<public_ip>). Follow the on-screen instructions to complete the WordPress installation, including creating a user and configuring settings. Microservices Architecture (2 EC2 Instances)

    Launch EC2 Instances:

Follow steps 1.1 and 1.2 from the monolithic architecture section twice to launch two separate EC2 instances. Configure separate security groups for each instance: WordPress instance: Allow inbound traffic on ports 22 (SSH), 80 (HTTP), and 443 (HTTPS). MySQL instance: Allow inbound traffic on port 3306 (MySQL) only from the WordPress instance's security group. 2. Install and Configure MySQL (on dedicated MySQL instance):

Follow steps 2.1 to 2.4 from the monolithic architecture section on the dedicated MySQL instance. 3. Install and Configure Apache (on WordPress instance):

Follow steps 3.1 and 3.2 from the monolithic architecture section on the WordPress instance. 4. Install and Configure WordPress:

Follow steps 4.1 and 4.2 from the monolithic architecture section on the WordPress instance. 5. Configure WordPress Database Connection:

During WordPress installation, enter the following database connection details: Database Host: The private IP address of the MySQL instance. Database Name: The name of the database created on Sources linuxapt.com/blog/1010-install-wordpress-on-rocky-linux-8 www.alibabacloud.com/blog/how-to-install-tiki-wiki-groupware-cms-on-alibaba-cloud_595721
Add a custom footer


   

