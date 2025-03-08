Complete Guide: Install & Configure VICIdial on Debian (Single Script)
This guide will walk you through the full VICIdial installation on Debian, ensuring all dependencies, Asterisk, and Apache configurations are properly set up.

📌 Step 1: Update & Install Dependencies
First, update your system and install the required packages:

bash
Copy
Edit
apt update -y && apt upgrade -y
apt install -y build-essential software-properties-common \
    mariadb-server mariadb-client apache2 curl wget \
    php7.2 php7.2-cli php7.2-mysql php7.2-gd php7.2-xml \
    php7.2-curl php7.2-mbstring libapache2-mod-php7.2 \
    unzip git sox
📌 Step 2: Install Asterisk (VICIdial Supported)
Download and install Asterisk 16 for VICIdial:

bash
Copy
Edit
cd /usr/src
wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-16-current.tar.gz
tar -xvzf asterisk-16-current.tar.gz
cd asterisk-16.*/
./configure --libdir=/usr/lib/x86_64-linux-gnu
make -j$(nproc) && make install && make samples
Ensure Asterisk starts on boot:

bash
Copy
Edit
systemctl enable asterisk
systemctl restart asterisk
📌 Step 3: Install VICIdial
Clone the VICIdial repository and set up the web directory:

bash
Copy
Edit
cd /usr/src
git clone https://github.com/VICIdial/vicidial.git
cd vicidial
mkdir -p /var/www/html/vicidial
cp -r www/* /var/www/html/vicidial/
chown -R www-data:www-data /var/www/html/vicidial
chmod -R 755 /var/www/html/vicidial
📌 Step 4: Configure Apache for VICIdial
Edit the Apache VirtualHost file:

bash
Copy
Edit
nano /etc/apache2/sites-available/000-default.conf
Replace its content with:

apache
Copy
Edit
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/vicidial

    <Directory /var/www/html/vicidial>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
Save (CTRL + X, then Y, then ENTER).

📌 Step 5: Enable Required Apache Modules
Run the following commands:

bash
Copy
Edit
a2enmod rewrite
a2enmod php7.2
echo "ServerName 127.0.0.1" >> /etc/apache2/apache2.conf
systemctl restart apache2
📌 Step 6: Configure MySQL for VICIdial
Run MySQL secure setup:

bash
Copy
Edit
mysql_secure_installation
Then, log into MySQL:

bash
Copy
Edit
mysql -u root -p
Create a VICIdial database:

sql
Copy
Edit
CREATE DATABASE vicidial;
CREATE USER 'vicidial'@'localhost' IDENTIFIED BY 'StrongPassword';
GRANT ALL PRIVILEGES ON vicidial.* TO 'vicidial'@'localhost';
FLUSH PRIVILEGES;
EXIT;
Import VICIdial database:

bash
Copy
Edit
mysql -u vicidial -p vicidial < /usr/src/vicidial/VICIdial.sql
📌 Step 7: Start VICIdial Services
bash
Copy
Edit
systemctl restart asterisk apache2
📌 Step 8: Open VICIdial in Browser
Go to:

arduino
Copy
Edit
http://YOUR_SERVER_IP/vicidial/admin.php
Default login:

Username: 6666
Password: 1234
✅ VICIdial is now fully set up!

