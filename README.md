# Ubuntu 18.04 with LAMP.

From experience, when I first started out playing around with web development it can be very confusing setting it up. This is a clean bundle build for a LAMP platform. Using docker, this will allow you to pull the build and trash the build with ease.

This container is useful for people that would want to develop and test projects on a web platform. LAMP is an open-source web development platform. LAMP contains: 
* Linux - Operating system 
* Apache - Web server
* MySQL - Relational database management system 
* PHP - Scripted object-oriented language

However, this container is not the best code of practice because the purpose of docker is to be lightweight and agile. Therefore on a production level you should limit compute processes to one per container. 

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


* Replace these variables to the desired volume name. For example: 

|        Change these         |     Example
|-----------------------------|---------------
|%mysql_data_volume_variable% | mysql_data 
|%mysql_log_volume_variable%  | mysql_log
|%apache_data_volume_variable%| apache_data
|%httpd_data_volume_variable% | httpd_data

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

## Instructions for Ubuntu 14.04, 16.04, 18.04

If you haven't got docker on your machine, follow these instructions to start you off. If you are the type that have little patients, this is great because you will see how easy and quick to install docker and the docker container in minutes. Please refer to the docker documentation if you don't wish to follow these instructions https://docs.docker.com/v17.12/install/. 

1) Type the code below in the terminal to install the repository and docker. 
```
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg |  apt-key add -
 add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
 apt-get update
 apt-get install -y docker-ce
```
2) Now that docker has been installed, install the mylesp/dockerlamp container below. The code below will show a working version below, however if wish to change the variables, refer to the document above on what to change.
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


3) Congratulations! you've installed your first docker container in minutes! Now, lets test to see if it works. You can use your domain name or IP address of the server/machine that it is running on. Here is an example below: 

Without a domain name type this into a browser:
```
 http://localhost/
```
or
```
 http://127.0.0.1/ 
```
If you have a domain name type this into a browser:

```
 http://insert_domain_here/
```
Now that you have tested it on the browser you won't see much. This is why you need to add some content inside the "/root/home/docker/html/" or the file directory that you changed it too.

4) This test is just to print out an "Hello world!" string on to your browser. Either create a HTML document your self or try this code but delete it afterwards when you want to start building:
```
echo "<html><head><\head><body><h1> Hello world!<\h1><\body><\html>" >> /root/home/docker/html/index.html
```
Now run refresh your browser and you'll see the string! You are now ready to make websites. If you don't like HTML and CSS, you could always look into wordpress or Graphical user interface software to help you build sites as well.

## Instructions for Windows
If you have a windows machine don't sweat, here is the instructions for this type of OS. If you don't want to follow my instructions, please refer to the docker documentation https://docs.docker.com/docker-for-windows/install/.

1) The first thing you'll need to do is install docker. Go on to this site to download docker https://store.docker.com/editions/community/docker-ce-desktop-windows. Here is a screenshot on what it should look like:

Inline-style: ![alt text](https://raw.githubusercontent.com/RattyMyles/DockerLAMP/master/website/docker_install_w.PNG "Logo Title Text 1")

