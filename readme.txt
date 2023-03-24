Надо сменить версию php на 7.4 в этом базовом КОНТЕЙНЕРЕ, тк на пересобранный образ c php81 не удается законнекстится
ниже инструкция с моими комментариями

Запустить контеер
docker run -d --name acop_container -i -t akopkesheshyan/bitrix24:lates

The code in this package will not work on php8 as it is written for php 7
To upgrade php to 7.4:
Then in terminal execute: 

docker exec -it acop_container bash

replace the bitrix26-web-1 with your own image name.
Then execute:
yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum -y install https://rpms.remirepo.net/enterprise/remi-release-7.rpm
yum -y install yum-utils
yum-config-manager --disable 'remi-php*'
yum-config-manager --enable remi-php74
yum repolist
yum -y install php
type php -v
you should see version 7.4
Then restart apache: service httpd restart

BUT this does not solve the problem.
Start the installation process.
When you are on the page with bitrixsetup.php
You need to download a new version of bitrixsetup.php from http://www.1c-bitrix.ru/download/scripts/bitrixsetup.php
replace /home/bitrix/www/bitrixsetup.php
Then refresh the page
now execute: vi /etc/php.d/bitrixenv.ini
find mbstring.func_overload = 2 and change this to 0
Then restart apache: service httpd restart

Записать новый образ 
 docker commit 78b2e8040e50 akopkesheshyan/bitrix24:php7
 
Отредактировать docker-compose.yml
в нем сменить имя на вновь записанный образ akopkesheshyan/bitrix24:php7
