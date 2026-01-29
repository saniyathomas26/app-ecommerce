# E-Commerce Application Deployment Guide

[![OS](https://img.shields.io/badge/OS-CentOS-blue)](https://www.centos.org/)
[![Web Server](https://img.shields.io/badge/Web_Server-Apache-orange)](https://httpd.apache.org/)
[![Backend](https://img.shields.io/badge/Backend-PHP-purple)](https://www.php.net/)
[![Database](https://img.shields.io/badge/Database-MariaDB-red)](https://mariadb.org/)
[![Version Control](https://img.shields.io/badge/Version_Control-Git-green)](https://git-scm.com/)

---

## 🧩 Tech Stack
- **OS:** CentOS  
- **Web Server:** Apache (`httpd`)  
- **Backend:** PHP  
- **Database:** MariaDB  
- **Version Control:** Git  
- **Firewall:** firewalld  

---

## 🚀 Deployment Guide (CentOS)

### 1️⃣ Install and Enable FirewallD
```bash
sudo yum install -y firewalld
sudo systemctl start firewalld
sudo systemctl enable firewalld
sudo systemctl status firewalld
2️⃣ Database Setup (MariaDB)
sudo yum install -y mariadb-server
sudo systemctl start mariadb
sudo systemctl enable mariadb

Open Database Port
sudo firewall-cmd --permanent --zone=public --add-port=3306/tcp
sudo firewall-cmd --reload

Create Database and User
sudo mysql


Inside MySQL:

CREATE DATABASE ecomdb;
CREATE USER 'ecomuser'@'localhost' IDENTIFIED BY 'ecompassword';
GRANT ALL PRIVILEGES ON ecomdb.* TO 'ecomuser'@'localhost';
FLUSH PRIVILEGES;

Create SQL Script
cat > db-load-script.sql <<-EOF
USE ecomdb;

CREATE TABLE products (
  id mediumint(8) unsigned NOT NULL auto_increment,
  Name varchar(255) default NULL,
  Price varchar(255) default NULL,
  ImageUrl varchar(255) default NULL,
  PRIMARY KEY (id)
) AUTO_INCREMENT=1;

INSERT INTO products (Name,Price,ImageUrl) VALUES
("Laptop","100","c-1.png"),
("Drone","200","c-2.png"),
("VR","300","c-3.png"),
("Tablet","50","c-5.png"),
("Watch","90","c-6.png"),
("Phone Covers","20","c-7.png"),
("Phone","80","c-8.png"),
("Laptop","150","c-4.png");
EOF


Run the script:

sudo mysql < db-load-script.sql

3️⃣ Web Server Setup
Install Required Packages
sudo yum install -y httpd php php-mysqlnd

Open HTTP Port
sudo firewall-cmd --permanent --zone=public --add-port=80/tcp
sudo firewall-cmd --reload

Configure Apache

Set PHP as the default page:

sudo sed -i 's/index.html/index.php/g' /etc/httpd/conf/httpd.conf


Start Apache:

sudo systemctl start httpd
sudo systemctl enable httpd

4️⃣ Application Code
Install Git and Clone Project
sudo yum install -y git
sudo git clone https://github.com/kodekloudhub/learning-app-ecommerce.git /var/www/html/


⚠️ Note: For this repo, code is already included.

5️⃣ Environment Configuration

Create a .env file in the project root:

cat > /var/www/html/.env <<-EOF
DB_HOST=localhost
DB_USER=ecomuser
DB_PASSWORD=ecompassword
DB_NAME=ecomdb
EOF


⚠️ For multi-node setup: Update DB_HOST with the database server IP.

6️⃣ Update index.php

Ensure index.php loads environment variables from .env.

✅ Testing

Verify application access:

curl http://localhost

🎯 Notes

All files must reside inside a Git repository to be tracked.

For production, ensure firewall and database credentials are secured.

Apache and MariaDB services should be set to start automatically for server reboots.


