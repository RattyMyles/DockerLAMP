FROM ubuntu:18.04
# docker version has latest updates, so no need for upgrade

ENV DEBIAN_FRONTEND noninteractive
ENV INITRD No


RUN apt-get -y update && \
	apt-get -y install --no-install-recommends apache2 \
		mysql-server \
		php \
		libapache2-mod-php \
		php-mysql \
		supervisor && \
	ln -fs /usr/share/zoneinfo/Europe/London /etc/localtime && dpkg-reconfigure -f noninteractive tzdata  && \
	echo "<?php phpinfo(); ?>" > /var/www/html/info.php && \
	mkdir -p /var/run/mysqld && \
	chown mysql:mysql /var/run/mysqld && \
	echo "[program:mysql]" > /etc/supervisor/conf.d/mysql.conf && \
	echo "#command=/usr/bin/mysqld_safe" >> /etc/supervisor/conf.d/mysql.conf && \
	echo "command=/usr/bin/mysqld_safe --skip-grant-tables" >> /etc/supervisor/conf.d/mysql.conf && \
	echo "process_name = mysql" >> /etc/supervisor/conf.d/mysql.conf && \
	echo "[program:apache]" > /etc/supervisor/conf.d/apache.conf && \
	echo "command=/usr/sbin/apachectl -DFOREGROUND -k start" >> /etc/supervisor/conf.d/apache.conf && \
	echo "process_name = apache" >> /etc/supervisor/conf.d/apache.conf && \
	sed -i -e"s/^bind-address\s*=\s*127.0.0.1/bind-address = 0.0.0.0/" /etc/mysql/my.cnf && \
	apt-get -y autoclean && apt-get -y autoremove && \
	apt-get -y purge $(dpkg --get-selections | grep deinstall | sed s/deinstall//g) && \
	rm -rf /var/lib/apt/lists/* && \
	echo "#!/bin/bash" >  /root/startup.sh && \
	echo "/usr/bin/supervisord -n" >>  /root/startup.sh

#VOLUME /var/www/html
#VOLUME /var/log/httpd
#VOLUME /var/lib/mysql
#VOLUME /var/log/mysql
#VOLUME /etc/apache2  

EXPOSE 80

CMD ["/bin/bash", "/root/startup.sh"]
