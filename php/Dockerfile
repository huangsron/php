#选择镜像
FROM php:5.6.36-fpm-stretch

COPY phpinfo.php /var/www/html/

#因为官方镜像是用的debain stretch 所以第一步先替换源 如果是Jessie 要替换对应的源
RUN echo "deb http://mirrors.aliyun.com/debian stretch main contrib non-free" > /etc/apt/sources.list && \
    echo "deb-src http://mirrors.aliyun.com/debian stretch main contrib non-free" >> /etc/apt/sources.list  && \
    echo "deb http://mirrors.aliyun.com/debian stretch-updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.aliyun.com/debian stretch-updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.aliyun.com/debian-security stretch/updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.aliyun.com/debian-security stretch/updates main contrib non-free" >> /etc/apt/sources.lis

#安装后面所使用的工具 composer会用到git php gd扩展会用到libpng-dev
RUN apt-get update && apt-get install -y \
    git\
    libpng-dev\
    libmcrypt-dev \
    libxml++2.6-dev \
    libxslt-dev\
    unzip

#添加扩展的其中一种方式 就是把命令组合在了一起
RUN curl -fsSL 'https://pecl.php.net//get/redis-4.0.2.tgz' -o redis-4.0.2.tar.gz \
    && tar -xvf redis-4.0.2.tar.gz \
    && rm redis-4.0.2.tar.gz \
    && ( \
    cd redis-4.0.2 \
    && phpize \
    && ./configure  \
    && make \
    && make install \
    ) \
    && rm -r redis-4.0.2 \
    && docker-php-ext-enable redis

#添加扩展的其中一种方式 就是把命令组合在了一起
RUN curl -fsSL https://codeload.github.com/phalcon/cphalcon/tar.gz/v3.4.4 -o phalcon.tar.gz \
    && tar -xf phalcon.tar.gz \
    && cd cphalcon*/build && ./install \
    && docker-php-ext-enable phalcon

#添加扩展的其中一种方式 就是把命令组合在了一起
RUN curl -k -fsSL https://download.suhosin.org/suhosin-0.9.38.tar.gz -o suhosin.tar.gz \
    && tar -zxvf suhosin.tar.gz \
    && ( \
    cd suhosin-0.9.38 \
    && phpize\
    && ./configure \
    && make && make install\
    ) \
    && echo "suhosin.executor.include.whitelist = phar" >> /usr/local/etc/php/conf.d/docker-php-ext-suhosin.ini \
    && docker-php-ext-enable suhosin


#安装php编译时官方没有安装的扩展,哪个版本的php镜像,就下载哪个版本的源码
RUN curl -fsSL 'http://ba1.php.net/get/php-5.6.36.tar.gz/from/this/mirror' -o php-5.6.36.tar.gz \
    && tar -xvf php-5.6.36.tar.gz \
    && rm php-5.6.36.tar.gz \
    && ( \
    cd php-5.6.36/ext/mysql \
    && phpize \
    && ./configure  \
    && make \
    && make install \
    ) \
    && ( \
    cd php-5.6.36/ext/pdo_mysql \
    && phpize \
    && ./configure  \
    && make \
    && make install \
    ) \
    && ( \
    cd php-5.6.36/ext/mysqli \
    && phpize \
    && ./configure  \
    && make \
    && make install \
    ) \
    && ( \
    cd php-5.6.36/ext/bcmath \
    && phpize \
    && ./configure  \
    && make \
    && make install \
    ) \
    && ( \
    cd php-5.6.36/ext/gd \
    && phpize \
    && ./configure  \
    && make \
    && make install \
    ) \
    && ( \
    cd php-5.6.36/ext/exif \
    && phpize \
    && ./configure  \
    && make \
    && make install \
    ) \
    && ( \
    cd php-5.6.36/ext/intl \
    && phpize \
    && ./configure  \
    && make \
    && make install \
    ) \
    && ( \
    cd php-5.6.36/ext/mcrypt \
    && phpize \
    && ./configure  \
    && make \
    && make install \
    ) \
    && ( \
    cd php-5.6.36/ext/pcntl \
    && phpize \
    && ./configure  \
    && make \
    && make install \
    ) \
    && ( \
    cd php-5.6.36/ext/shmop \
    && phpize \
    && ./configure  \
    && make \
    && make install \
    ) \
    && ( \
    cd php-5.6.36/ext/soap \
    && phpize \
    && ./configure  \
    && make \
    && make install \
    ) \
    && ( \
    cd php-5.6.36/ext/sockets \
    && phpize \
    && ./configure  \
    && make \
    && make install \
    ) \
    && ( \
    cd php-5.6.36/ext/xmlrpc \
    && phpize \
    && ./configure  \
    && make \
    && make install \
    ) \
    && ( \
    cd php-5.6.36/ext/xsl \
    && phpize \
    && ./configure  \
    && make \
    && make install \
    ) \
    && ( \
    cd php-5.6.36/ext/zip \
    && phpize \
    && ./configure  \
    && make \
    && make install \
    ) \
    # && rm -r php-5.6.36 \
    && pecl install swoole-2.0.9 \
    # && docker-php-ext-enable mysql pdo_mysql mysqli bcmath gd exif intl mcrypt pcntl shmop soap sockets xmlrpc xsl zip
    && docker-php-ext-enable mysql pdo_mysql mysqli bcmath gd exif intl mcrypt pcntl shmop soap sockets xmlrpc xsl zip swoole
#安装composer 我个人觉得这样方便使用
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer && \
    composer self-update && \
    composer config -g repo.packagist composer https://packagist.phpcomposer.com

#命令
CMD ["php-fpm"]
