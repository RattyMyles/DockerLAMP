FROM ubuntu:18.04
 # docker version has latest updates, so no need for upgrade

ENV DEBIAN_FRONTEND noninteractive
ENV INITRD No

RUN apt-get -y update
RUN apt-get install lsb-release -y
RUN apt-get -y install apache2
RUN apt-get -y install mysql-server
RUN apt-get -y install php libapache2-mod-php php-mysql
RUN apt-get -y install supervisor

RUN ln -fs /usr/share/zoneinfo/Europe/London /etc/localtime && dpkg-reconfigure -f noninteractive tzdata

RUN echo "<?php phpinfo(); ?>" > /var/www/html/info.php

RUN mkdir -p /var/run/mysqld
RUN chown mysql:mysql /var/run/mysqld

# docker doesn't understand AUFS, we had to put the var/lib into a var volume

RUN echo "[program:mysql]" > /etc/supervisor/conf.d/mysql.conf
RUN	echo "#command=/usr/bin/mysqld_safe" >> /etc/supervisor/conf.d/mysql.conf
RUN	echo "command=/usr/bin/mysqld_safe --skip-grant-tables" >> /etc/supervisor/conf.d/mysql.conf
RUN	echo "process_name = mysql" >> /etc/supervisor/conf.d/mysql.conf

RUN echo "[program:apache]" > /etc/supervisor/conf.d/apache.conf
RUN	echo "command=/usr/sbin/apachectl -DFOREGROUND -k start" >> /etc/supervisor/conf.d/apache.conf
RUN	echo "process_name = apache" >> /etc/supervisor/conf.d/apache.conf

# setup mysql
RUN sed -i -e"s/^bind-address\s*=\s*127.0.0.1/bind-address = 0.0.0.0/" /etc/mysql/my.cnf

#clean up
RUN apt-get -y autoclean && apt-get -y autoremove 
RUN apt-get -y purge $(dpkg --get-selections | grep deinstall | sed s/deinstall//g)
RUN rm -rf /var/lib/apt/lists/* 

VOLUME /var/www/html
VOLUME /var/log/httpd
VOLUME /var/lib/mysql
VOLUME /var/log/mysql
VOLUME /etc/apache2  

ADD startup.sh /root/startup.sh
CMD ["/bin/bash", "/root/startup.sh"]

EXPOSE 80
