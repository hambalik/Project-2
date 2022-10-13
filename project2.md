**PROJECT 2 IMPLEMENTATION/DOCUMENTATION (LEMP)**

## STEP 1 – INSTALLING THE NGINX WEB SERVER

### Since this is our first time using apt for this session, I started off by updating server package index. Following that, I used apt install to get Nginx installed
`sudo apt update`
![sudo-apt-update](./images/sudo-apt-update.PNG)
![sudo-apt-update1](./images/sudo-apt-update1.PNG)

`sudo apt install nginx`
![nginx-installation](./images/nginx-installation.PNG)
![nginx-installation1](./images/nginx-installation1.PNG)

### To verify that nginx was successfully installed and is running as a service in Ubuntu, below code was run for confirmation
`sudo systemctl status nginx`
![nginx-status](./images/nginx-status.PNG)

### Accessing the installed server which is running locally using IP address and DNS name, the 2 commands which actually do pretty much the same task
`curl http://127.0.0.1:80`
![curl-http-127-0-0-1-80](./images/curl-http-127-0-0-1-80.PNG)

`curl http://localhost:80`
![curl-http-localhost-80](./images/curl-http-localhost-80.PNG)

### Accessing the installed server which is running on the web browser using either IP address or DNS name, the 2 commands which actually do pretty much the same task
[Log-in-the-nginx-server](http://54.90.138.220)
![nginx-.on-chrome](./images/nginx-.on-chrome.PNG)

### The public IP address can also be retrieved using the following command other than in the AWS web console
`curl -s http://169.254.169.254/latest/meta-data/public-ipv4`
![retrieving-ipadd-on-terminal](./images/retrieving-ipadd-on-terminal.PNG)

## STEP 2 – INSTALLING MYSQL

### Installation of MYSQL server
`sudo apt install mysql-server`
![mysql-installation](./images/mysql-installation.PNG)
![mysql-installation1](./images/mysql-installation1.PNG)

### MYSQL server connection as the administartive database user root
`sudo mysql`
![mysql-status](./images/mysql-status.PNG)

### Runing security script
`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';`
![alter-user-password](./images/alter-user-password.PNG)

### Exiting mysql

`mysql> exit`

![exit-mysql](./images/exit-mysql.PNG)


### INTERRACTIVE SCRIPT 
`sudo mysql_secure_installation`
![validating-password](./images/validating-password.PNG)
![password-setting1](./images/password-setting1.PNG)


### TESTING LOGIN INTO MYSQL
`sudo mysql -p`
![sudo-mysql-p](./images/sudo-mysql-p.PNG)

### THEN I EXIT MYSQL TO PROCEED TO THE NEXT STEP
`exit`

![exit-mysql](./images/exit-mysql.PNG)

## STEP 3 – INSTALLING PHP

### Installation of 2 packages of php-fpm php-mysql at once 
`sudo apt install php-fpm php-mysql`
![installing-php](./images/installing-php.PNG)
![php-installation](./images/php-installation.PNG)
![php-installation1](./images/php-installation1.PNG)

## STEP 4 – CONFIGURING NGINX TO USE PHP PROCESSOR

### Creation of the root web directory for my domain 
`sudo mkdir /var/www/projectLEMP`
![sudo-mkdir-var-www-projectLEMP](./images/sudo-mkdir-var-www-projectLEMP.PNG)

### Assigning ownership of the directory with the $USER environment variable 
`sudo chown -R $USER:$USER /var/www/projectLEMP`
![assign-ownership](./images/assign-ownership.PNG)

### Opening a new configuration file in Nginx’s sites-available directory
`sudo nano /etc/nginx/sites-available/projectLEMP`
![paste-bare-bone-configuration](./images/paste-bare-bone-configuration.PNG)

### Activation of my configuration by linking to the config file from Nginx’s sites-enabled directory
`sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`
![activating-configuration](./images/activating-configuration.PNG)

### Testing configuration for syntax error

`sudo nginx -t`

![config-test-ok](./images/config-test-ok.PNG)

### Disabling default Nginx host that is currently configured to listen on port 80
`sudo unlink /etc/nginx/sites-enabled/default`
![disabling-default-nginx](./images/disabling-default-nginx.PNG)

### Reloading nginx to apply changes

`sudo systemctl reload nginx`

![reloading-nginx](./images/reloading-nginx.PNG)

### Reloading nginx to apply changes

`sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html`

![webURL-using-ipadd](./images/webURL-using-ipadd.PNG)

## STEP 5 – TESTING PHP WITH NGINX

### creating a test PHP file in my document root

`sudo nano /var/www/projectLEMP/info.php`

![phpinfo](./images/phpinfo.PNG)
![info.php1](./images/info.php1.PNG)
![info.php2](./images/info.php2.PNG)

### Removing the file created because of the sensitive info contained, the file can always be regenerated if need be

`sudo rm /var/www/your_domain/info.php`
![Capture](./images/Capture.PNG)

## STEP 6 – RETRIEVING DATA FROM MYSQL DATABASE WITH PHP (CONTINUED)

### Firstl, I connected to the MySQL console using the root account:
`sudo -u root -p`
![mysql-root](./images/mysql-root.PNG)

### Creating a new database, by runing the following command from my MySQL console:
`CREATE DATABASE 'example_database`;`
![creating-database](./images/creating-database.PNG)

### Created new user and granted all previledges on the database created
`CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';`
![creating-new-user](./images/creating-new-user.PNG)

### Given the user permission over the created database
`GRANT ALL ON example_database.* TO 'example_user'@'%';`
![grant-all](./images/grant-all.PNG)

### Exiting MYSQL shell
`exit`
![exit-mysql](./images/exit-mysql.PNG)

### Testing the new user has the proper permission by loggin into MYSQL console again
`mysql -u example_user -p`
![mysql-example-user](./images/mysql-example-user.PNG)


### Confirmed that I have access to my database
`SHOW DATABASES;`
![show-database](./images/show-database.PNG)

### Creating a table named todo_list
`CREATE TABLE example_database.todo_list (
mysql>     item_id INT AUTO_INCREMENT,
mysql>     content VARCHAR(255),
mysql>     PRIMARY KEY(item_id)
mysql> );`
![creating-table](./images/creating-table.PNG)

### Inserting a few rows of content in the test table. With repeat of the following command a few times, using different VALUES
`INSERT INTO example_database.todo_list (content) VALUES ("My first important item");`
![4row-table](./images/4row-table.PNG)

### After confirming that I have valid data in my test table, I then exit the MySQL console:
`exit`
![exit-mysql](./images/exit-mysql.PNG)

### Creating a PHP script that will connect to MySQL and query for my content. PHP file cane be created in your custom web root directory using your preferred editor. 
`nano /var/www/projectLEMP/todo_list.php`
![after-saving-to-do-list](./images/after-saving-to-do-list.PNG)

### Then, the following PHP script connects to the MySQL database and queries for the content of the todo_list table, displays the results in a list. If there is a problem with the database connection, it will throw an exception
`exit`
![saving-list](./images/saving-list.PNG)

### And finally, I was able to log into my database via my web browser using IP address or domain name followed by /todo_list.php
![finally!](./images/finally!.PNG)

**PROJECT 2 IMPLEMENTATION/DOCUMENTATION (LEMP) IS SUCCESSFULLY DONE WITH**
