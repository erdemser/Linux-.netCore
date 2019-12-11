Apache Kurulumu
---------------------------------------------------------------------------------------------------------------------------
    Step 1 — Installing Apache
    sudo apt update
    sudo apt install apache2
    
    Step 2 — Adjusting the Firewall
    sudo ufw app list
    sudo ufw allow 'Apache'
    
    Step 3 — Checking your Web Server
    sudo systemctl status apache2
    
    Step 4 — Setting Up Virtual Hosts (Recommended)
    sudo mkdir /var/www/your_domain   ---Create the directory for your_domain:
    sudo chmod -R 755 /var/www/your_domain
    
    --Açılacak domainin conf dosyası oluşturma
    sudo nano /etc/apache2/sites-available/your_domain.conf
    <VirtualHost *:80>
        ServerAdmin webmaster@localhost
        ServerName your_domain
        ServerAlias your_domain
        DocumentRoot /var/www/your_domain
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>
    
    --Virtual hostları etkinleştirmek
    sudo a2ensite your_domain.conf
    sudo a2dissite 000-default.conf
    sudo apache2ctl configtest
    sudo systemctl restart apache2
    
    --80 portundan başka portları dinlemek için gerekli config dosyası
    sudo nano /etc/apache2/ports.conf --farklı port dinlemeleri için
    

> Kaynak https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-18-04-quickstart

Directory Permissions SFTP
---------------------------------------------------------------------------------------------------------------------------
    sudo chown -R webdev:webdev /var/www/html*
> Kaynak https://devanswers.co/configure-sftp-web-server-document-root/

Install .NET Core SDK on Linux Ubuntu 18.04 - x64
---------------------------------------------------------------------------------------------------------------------------
    wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
    sudo dpkg -i packages-microsoft-prod.deb
    
    sudo add-apt-repository universe
    sudo apt-get install apt-transport-https
    sudo apt-get update
    sudo apt-get install dotnet-sdk-2.2

> Kaynak https://dotnet.microsoft.com/download/linux-package-manager/ubuntu18-04/sdk-current

Create Service for .netCore
---------------------------------------------------------------------------------------------------------------------------
    ---Reverse Proxy kullanımı için =>ProxyPreserveHost
    sudo a2enmod proxy
    sudo a2enmod proxy_http
    sudo a2enmod proxy_balancer
    sudo a2enmod lbmethod_byrequests
    sudo systemctl restart apache2
    
    
    sudo nano /etc/apache2/sites-available/your_domain.conf
    
    #FileApi
    <VirtualHost *:8025>
        ServerName localhost
        ProxyPreserveHost On
        ProxyPass / http://localhost:8029/
        ProxyPassReverse / http://localhost:8029/
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>
    
    #SecurityApi
    <VirtualHost *:8024>
        ServerName localhost
        ProxyPreserveHost On
        ProxyPass / http://localhost:8028/
        ProxyPassReverse / http://localhost:8028/
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>
    
    
    
    cd /etc/systemd/system/
    
    sudo nano FileApi.service
    
    
    [Unit]
    Description=File API App running on Ubuntu
    
    [Service]
    WorkingDirectory=/var/www/FileApi
    ExecStart=/usr/bin/dotnet /var/www/FileApi/FileApi.dll --server.urls http://127.0.0.1:8025
    # Restart service after 10 seconds if the dotnet service crashes:
    SyslogIdentifier=FileApi
    #Environment=ASPNETCORE_ENVIRONMENT=Development
    Environment=ADOTNET_CLI_HOME=/tmp
    
    [Install]
    WantedBy=multi-user.target
    
    sudo systemctl enable FileApi.service

> Kaynak https://www.telerik.com/blogs/build-deploy-asp-net-core-application-apache

Enabling mod_rewrite
---------------------------------------------------------------------------------------------------------------------------
    sudo a2enmod rewrite
    sudo systemctl restart apache2
    
    
    sudo nano /etc/apache2/sites-available/000-default.conf
    <VirtualHost *:80>
        <Directory /var/www/html>
            Options Indexes FollowSymLinks MultiViews
            AllowOverride All
            Require all granted
        </Directory>
    
        . . .
    </VirtualHost>
    
    
    sudo systemctl restart apache2
    
    .htaccess dosyası
    
    RewriteEngine on
    <IfModule mod_rewrite.c>
        RewriteEngine On
        RewriteBase /
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteCond %{REQUEST_FILENAME} !-d
        RewriteRule . /index.html [L]
    </IfModule>
    
> Kaynak https://www.digitalocean.com/community/tutorials/how-to-rewrite-urls-with-mod_rewrite-for-apache-on-ubuntu-16-04
