# Lampstack implementation on AWS with Bash Script

# WHAT'S NEEDED:

- Basic Linux Skill
- Cloud Service Provider - AWS, AZURE, DIGITAL OCEAN, etc.
- Internet Connection

# STEPS INVOLVED: 
- Launch an instance in the cloud using Ubuntu's package manager

![](https://github.com/BodaTolu/Bash-Scripting/blob/main/Bash%20Scripting/aws%20bash.png)

- Open port 80 and 22 in security group inbound rules
- Create a file in the root directly and name it lamp.sh

```
touch lamp.sh
sudo vim lamp.sh
```

- Add the following script into the file

```
#!/bin/bash

# Update system packages
sudo apt update
sudo apt upgrade -y

# Install Apache
sudo apt install apache2 -y

# Start and enable Apache service
sudo systemctl start apache2
sudo systemctl enable apache2

# Install MySQL Server
sudo apt install mysql-server -y

# Start and enable MySQL service
sudo systemctl start mysql
sudo systemctl enable mysql

# Secure MySQL installation
SECURE_MYSQL=$(expect -c "
set timeout 10
spawn sudo mysql_secure_installation
sleep 1
expect \"Enter password for user root:\"
send \"password\n\"
expect \"Change the password for root ?\"
send \"n\n\"
expect \"Remove anonymous users?\"
send \"Y\n\"
expect \"Disallow root login remotely?\"
send \"Y\n\"
expect \"Remove test database and access to it?\"
send \"Y\n\"
expect \"Reload privilege tables now?\"
send \"Y\n\"
expect eof
")

# Display completion message
echo "MySQL server secured!"

# Install PHP and required modules
sudo apt install php libapache2-mod-php php-mysql -y

# Restart Apache to load PHP module
sudo systemctl restart apache2

# Display PHP version
php -v

# Display Apache version
apache2 -v

# Display MySQL version
mysql --version

# Test PHP setup
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php

# Display completion message
echo "LAMP setup completed!"
```
![](https://github.com/BodaTolu/Bash-Scripting/blob/main/Bash%20Scripting/mysql.png)
After adding the script into the file, save the file with :wq

- grant the file permission and run

```
chmod u+x lamp_setup.sh

```
- Run the script
 
```
sudo ./lamp_setup.sh
```

- Run mysql_secure_installation script:

This part of the code is going to prompt you for some configurations

- To confirm installation use the Public Ip of webserver:80

![](https://github.com/BodaTolu/Bash-Scripting/blob/main/Bash%20Scripting/apache.png)

- Check for mysql and php installation

![](https://github.com/BodaTolu/Bash-Scripting/blob/main/Bash%20Scripting/mysql%20bye.png)

![](https://github.com/BodaTolu/Bash-Scripting/blob/main/Bash%20Scripting/php%202.png)
