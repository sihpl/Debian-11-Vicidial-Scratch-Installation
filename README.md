 Complete Guide: Install & Configure VICIdial on Debian (Single Script)
This guide will walk you through the full VICIdial installation on Debian, ensuring all dependencies, Asterisk, and Apache configurations are properly set up.

📌 Step 1: Update & Install Dependencies
First, update your system and install the required packages:

##
apt update -y && apt upgrade -y
apt install -y build-essential software-properties-common \
    mariadb-server mariadb-client apache2 curl wget \
    php7.2 php7.2-cli php7.2-mysql php7.2-gd php7.2-xml \
    php7.2-curl php7.2-mbstring libapache2-mod-php7.2 \
    unzip git sox
##
