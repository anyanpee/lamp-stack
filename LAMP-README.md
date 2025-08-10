# Project LAMP - Ubuntu Server Setup

## Overview
This project demonstrates the setup of a complete LAMP (Linux, Apache, MySQL, PHP) stack on Ubuntu Server with a custom virtual host configuration.

## Prerequisites
- Ubuntu Server (tested on Ubuntu 20.04/22.04)
- Root or sudo access
- Basic command line knowledge

First create an Ubuntu instance from EC2 on Aws and Ssh into the instance 

![alt text](screenshots/ubuntu-instance-Screenshot-2025-08-06-075136-1.png)



## Installation Steps

### 1. MySQL Installation & Configuration
```bash
# Install MySQL
sudo apt update
sudo apt install mysql-server
```
![alt text](screenshots/ubuntu-instance-Screenshot-2025-08-06-075136-1.png)


![alt text](screenshots/ubuntu-instance-Screenshot-2025-08-06-075136-1.png)


```bash
# Secure MySQL installation
sudo mysql_secure_installation



# Change root password
mysql -u root -p
ALTER USER 'root'@'localhost' IDENTIFIED BY 'your_password';
FLUSH PRIVILEGES;
EXIT;
```

### 2. PHP Installation
```bash
# Install PHP and common modules
sudo apt install php php-mysql php-curl php-json php-mbstring

# Verify installation
php -v
php -m
```
![alt text](screenshots/ubuntu-instance-Screenshot-2025-08-06-075136-1.png)



### 3. Apache Web Server Setup
```bash
# Apache should be installed by default, verify status
sudo systemctl status apache2

# If not installed:
sudo apt install apache2
```
![alt text](screenshots/ubuntu-instance-Screenshot-2025-08-06-075136-1.png)

![alt text](screenshots/ubuntu-instance-Screenshot-2025-08-06-075136-1.png)

### 4. Project Directory Setup
```bash
# Create project directory
mkdir "Project Lamp"

# Set ownership to ubuntu user
sudo chown -R ubuntu:ubuntu "Project Lamp"

# Set proper permissions
sudo chmod 755 "Project Lamp"
```

### 5. Create PHP Info File
```bash
# Create PHP info file for testing
sudo nano "Project Lamp/info.php"
```

Add the following content:
```php
<?php
phpinfo();
?>
```
![alt text](screenshots/ubuntu-instance-Screenshot-2025-08-06-075136-1.png)

### 6. Virtual Host Configuration
```bash
# Create virtual host configuration
sudo nano /etc/apache2/sites-available/projectlamp.conf
```

Add the following configuration:
```apache
<VirtualHost *:80>
    ServerName projectlamp
    DocumentRoot "/home/ubuntu/Project Lamp"
    
    <Directory "/home/ubuntu/Project Lamp">
        AllowOverride All
        Require all granted
    </Directory>
    
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

### 7. Enable Virtual Host
```bash
# Enable the new site
sudo a2ensite projectlamp

# Disable default site (optional)
sudo a2dissite 000-default

# Test configuration
sudo apache2ctl configtest

# Restart Apache
sudo systemctl restart apache2
```

### 8. Configure Local DNS (Optional)
```bash
# Add to hosts file for local testing
sudo nano /etc/hosts
```

Add this line:
```
127.0.0.1 projectlamp
```

### 9. Set Proper Permissions
```bash
# Set directory permissions
sudo chmod 755 /home/ubuntu
sudo chmod 755 "/home/ubuntu/Project Lamp"
sudo chmod 644 "/home/ubuntu/Project Lamp/info.php"
```

## Testing the Setup

### Verify PHP Installation
```bash
php -v
curl http://projectlamp/info.php
```

### Test Apache Virtual Host
```bash
# Create test HTML file
echo "<h1>Project Lamp Works!</h1>" | sudo tee "/home/ubuntu/Project Lamp/index.html"

# Test the site
curl http://projectlamp/index.html
```

### Check Services Status
```bash
# Apache status
sudo systemctl status apache2

# MySQL status
sudo systemctl status mysql

# List enabled sites
sudo a2ensite -l
```
![alt text](screenshots/ubuntu-instance-Screenshot-2025-08-06-075136-1.png)

## Project Structure
```
Project Lamp/
├── info.php          # PHP information page
├── index.html         # Test HTML page
└── [future files]     # Additional project files
```

## Troubleshooting

### Common Issues
1. **403 Forbidden Error**: Check file permissions and directory access
2. **Apache Configuration Error**: Run `sudo apache2ctl configtest`
3. **MySQL Connection Issues**: Verify MySQL service and credentials
4. **PHP Not Working**: Ensure PHP module is enabled in Apache

### Useful Commands
```bash
# Check Apache error logs
sudo tail -f /var/log/apache2/error.log

# Restart services
sudo systemctl restart apache2
sudo systemctl restart mysql

# Test Apache configuration
sudo apache2ctl configtest
```

## Next Steps
- Install additional PHP modules as needed
- Set up SSL/HTTPS configuration
- Configure database for web applications
- Deploy actual web application files

## Author
Peter Anyankpele

## License
This project is for educational purposes.
