FROM amazonlinux:latest

#USER PARAMS

USER root

# INSTALL NGINX
RUN yum -y update
COPY ./conf/nginx/nginx.repo /etc/yum.repos.d/
RUN yum -y update && yum -y install nginx && yum -y install initscripts

# INSTALL PING COMMAND
RUN yum install iputils

# INSTALL AND CONFIGURE PHP
RUN yum install gcc-c++ zlib-devel amazon-linux-extras -y
RUN amazon-linux-extras enable php7.4 && amazon-linux-extras install php7.4 -y
RUN yum -y install php php-bcmath php7-dev php-pecl-memcache php-cli g++ make php-pear php-devel php-json php-xml php-curl php-opcache php-fpm php-intl php-mbstring gd gd-devel php-gd php-zip wget unzip

# INSTALL XDEBUG
RUN pecl install xdebug 

# INSTALL AND CONFIGURE GIT
RUN yum -y install git  

# INSTALL AND CONFIGURE COMPOSER
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"   \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer  \
    && php -r "unlink('composer-setup.php');"                                   \
    && ln -s /usr/local/bin/composer /usr/local/bin/composer.phar               \
    && composer self-update                                               \
    && composer --version

# INSTALL AND CONFIGURE DRUSH
RUN git clone https://github.com/drush-ops/drush.git /usr/local/src/drush \
    && cd /usr/local/src/drush && git checkout 10.x      \
    && ln -s /usr/local/src/drush/drush /usr/bin/drush
RUN cd /usr/local/src/drush && composer install

# INSTALL AND CONFIGURE DRUPAL CONSOLE -> https://github.com/hechoendrupal/drupal-console-launcher
RUN cd /tmp && curl https://github.com/hechoendrupal/drupal-console-launcher/releases/download/1.9.7/drupal.phar -L -o drupal.phar  \
    && mv drupal.phar /usr/local/bin/drupal \
    && chmod +x /usr/local/bin/drupal

# SEPTUP SSH WITH SECURE ROOT LOGIN
RUN yum -y update \
    && yum -y install openssh-server netcat \
    && mkdir /var/run/sshd \
    && echo 'root:password' | chpasswd \
    && sed -i 's/\#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# CHANGE HOSTS FILE
COPY ./conf/hosts/hosts /etc/hosts

# LOAD CONFIG FOR NGINX
COPY ./conf/nginx/default.conf /etc/nginx/conf.d/default.conf

# LOAD PHP.INI CUSTOM
COPY ./conf/php/php.ini /etc/php.ini

# INSTALL SSL SELF SIGNED CERTIFICATE FOR DOMAIN TBO
RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout nginx-selfsigned.key -out nginx-selfsigned.crt -subj "/C=CO/ST=BOG/L=BOG/O=Bits Americas, Inc./OU=IT/CN=local.tbo"

# RESTART NGIX AND PHP-FPM
RUN systemctl enable nginx.service && systemctl enable php-fpm.service

CMD ["/usr/sbin/init"] 