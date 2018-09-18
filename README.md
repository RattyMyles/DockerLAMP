# Ubuntu 18.04 with LAMP.

From experience, when I first started out playing around with web development it can be very confusing setting it up. This is a clean bundle build for a LAMP platform. Using docker, this will allow you to pull the build and trash the build with ease.

This container is useful for people that would want to develop and test projects on a web platform. LAMP is an open-source web development platform. LAMP contains: 
* Linux - Operating system 
* Apache - Web server
* MySQL - Relational database management system 
* PHP - Scripted object-oriented language

However, this container is not the best code of practice  because of *** you should stack your code?

## mylesp/dockerlamp:latest
contents: 
  * Ubunbtu 18.04
  * mysql-server
  * php
  * libapache2-mod-php
  * supervisor
  * apache2
```
    mkdir -p %LOCAL_PATH_TO_HOME_DIRECTORY% 
    docker volume create %mysql_data_volume_variable% 
    docker volume create %mysql_log_volume_variable% \ 
    docker volume create %apache_data_volume_variable% 
    docker volume create %httpd_data_volume_variable% 
    
    docker run -d --name mylespLAMP18.04 \
    -p 80:80 \
    -v %mysql_data_volume_variable%:/var/lib/mysql/ \
    -v %mysql_log_volume_variable%:/var/log/mysql/ \
    -v %apache_data_volume_variable%:/etc/apache2/ \
    -v %LOCAL_PATH_TO_HOME_DIRECTORY%:/var/www/html/ \
    -v %httpd_data_volume_variable%:/var/log/httpd/ \
    --restart unless-stopped \
    mylesp/dockerlamp:latest
```

* Replace %LOCAL_PATH_TO_HOME_DIRECTORY% with the local file directory of the website content you wish to be stored in.
* For example: "/root/home/docker/html/" or "/root/home/docker/website/"

* Replace %mysql_data_volume_variable%,, %apache_data_volume_variable% and %httpd_data_volume_variable%
with reasonable variable names for the name of the volumes.
* For example: docker volume create mysql_data
* Replace these variables to the desired volume name. For example: 

|        ####Example          |
|-----------------------------|
|%mysql_data_volume_variable% |
|%mysql_log_volume_variable%  |
|%apache_data_volume_variable%|
|%httpd_data_volume_variable% |

Example of a working code line:
```
mkdir -p /root/home/docker/html
docker volume create mysql_data 
docker volume create mysql_log
docker volume create apache_data
docker volume create httpd_data

docker run -d --name mylespLAMP18.04 \
-p 80:80 \
-v mysql_data:/var/lib/mysql/ \
-v mysql_log:/var/log/mysql/ \
-v apache_data:/etc/apache2/ \
-v /root/home/docker/html/:/var/www/html/ \
-v httpd_data:/var/log/httpd/ \
--restart unless-stopped \
mylesp/dockerlamp:latest
```
