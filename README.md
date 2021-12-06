<p align="center">
    <a href="https://magento.com">
        <img src="https://avatars.githubusercontent.com/u/68074738?s=400&u=726131dd9b3d652255ccaca8ef490d581886b1fa&v=4" width="100px" alt="Magento Commerce"/>
    </a>
    <br />
    <br />
    <a href="https://www.codetriage.com/magento/magento2">
        <img src="https://www.codetriage.com/magento/magento2/badges/users.svg" alt="Open Source Helpers" />
    </a>
    <a href="https://gitter.im/magento/magento2?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge">
        <img src="https://badges.gitter.im/Join%20Chat.svg" alt="Gitter" />
    </a>
    <a href="https://crowdin.com/project/magento-2">
        <img src="https://d322cqt584bo4o.cloudfront.net/magento-2/localized.svg" alt="Crowdin" />
    </a>
</p>

# [Magento 2.4.3](#) (Community Edition) by Son Nguyen
## Magento System [ Requirements](https://devdocs.magento.com/guides/v2.4/install-gde/system-requirements.html)
  * PHP 7.4
  * Elasticsearch 7.9
  * MySQL 8.0
  * Apache 2.4
  * Composer 2.x
  * Memory limit (cli/php.ini) 2048M

## Setup LAMP and Elasticsearch

#### Apache
````
sudo apt-get update
sudo apt-get install apache2
sudo systemctl start apache2
````
#### PHP
````
sudo apt-get -y update
sudo apt-get install -y php7.4 libapache2-mod-php7.4 php7.4 php7.4-common php7.4-gd php7.4-mysql php7.4-curl php7.4-intl php7.4-xsl php7.4-mbstring php7.4-zip php7.4-bcmath php7.4-iconv php7.4-soap
sudo apt-get install -y php7.4-fpm
````
#### Switch version PHP
````
sudo a2dismod php7.x
sudo a2enmod php7.4
sudo service apache2 restart
sudo update-alternatives --set php /usr/bin/php7.4
````
#### MYSQL
````
sudo apt install mysql-server
````
#### PHPMYADMIN
````
sudo mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY '123';
mysql -u root -p
CREATE USER 'son'@'localhost' IDENTIFIED WITH caching_sha2_password BY '123';
ALTER USER 'son'@'localhost' IDENTIFIED WITH mysql_native_password BY '123';
GRANT ALL PRIVILEGES ON *.* TO 'son'@'localhost' WITH GRANT OPTION;

sudo apt install phpmyadmin php7.4-mbstring php7.4-zip php7.4-gd php7.4-json php7.4-curl
sudo ln -s /etc/phpmyadmin/apache.conf /etc/apache2/conf-available/phpmyadmin.conf      
sudo a2enconf phpmyadmin.conf
sudo gedit /etc/apache2/apache2.conf
    Include /etc/phpmyadmin/apache.conf
sudo systemctl restart apache2
````
#### [XDebug]
  * Run :
````
sudo apt install php7.4-xdebug
````
  * Edit /etc/php/7.4/apache2/php.ini display_errors = On:
  * Add below /etc/php/7.4/apache2/php.ini :
````
[XDebug]
xdebug.remote_enable=1
xdebug.remote_autostart=1
xdebug.mode=debug
xdebug.remote_handler="dbgp"
xdebug.start_with_request = yes
XDEBUG_SESSION=1
````
#### Elasticsearch
````
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
sudo apt update
sudo apt install elasticsearch
sudo service elasticsearch start
````
#### COMPOSER
  * [ Install Composer](https://getcomposer.org/download)
  
## Install Magento
* Download ZIP Archive or run command:
```
sudo apt install git
git clone https://github.com/sonit1609/keds_project
```
* Extract files
* In your Magento 2 root
* Copy files and folders from archive to that folder
* In command line, using "cd", navigate to your Magento 2 root directory
````
composer install
````
* Add file config.php and env.php into app/etc
````
rm -rf var/cache/*
rm -rf var/page_cache/*
rm -rf generated/code/*

php bin/magento setup:upgrade
php bin/magento setup:di:compile
php bin/magento setup:static-content:deploy -f
php bin/magento c:c;
php bin/magento c:f;
php bin/magento indexer:reindex

find var vendor pub/static pub/media app/etc -type f -exec chmod g+w {} +;
find var vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} +;
chown -R :www-data . # Ubuntu;
chmod u+x bin/magento;
````
#### Setup local
  * go to /etc/apache2/sites-available
  * create new file dev.keds.abc.conf with contents
````
<VirtualHost *:80>
	ErrorLog /var/log/error.log
	ServerName      dev.keds.abc
      DocumentRoot /var/www/Projects/keds_project/
      <Directory /var/www/Projects/keds_project/>
             Options +Indexes +Includes +FollowSymLinks +MultiViews
              AllowOverride All
              Require all granted
      </Directory>	
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
````
  * Note : The Projects is the directory containing your web source 
  * Run there commands line:
````
sudo gedit /etc/hosts
````
  * Add in below: 127.0.0.1	dev.keds.abc
````
sudo a2ensite dev.keds.abc.conf;
sudo a2enmod rewrite;
sudo service apache2 restart
````
## Learn More About GraphQL in Magento 2

  * [GraphQL Developer Guide](https://devdocs.magento.com/guides/v2.4/graphql/index.html)

