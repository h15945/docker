FROM idoall/nginx-php
MAINTAINER lion <lion.net@163.com>


# -----------------------------------------------------------------------------
# 安装 git、svn
# -----------------------------------------------------------------------------
RUN yum install -y git svn

# -----------------------------------------------------------------------------
# 安装 composer 1.2.4
# -----------------------------------------------------------------------------
ADD composer1.2.4.phar /home/work/_src/composer.phar
RUN cd /home/work/_src/ \
    && mv composer.phar /usr/local/bin/composer \
    && chmod +x /usr/local/bin/composer

# -----------------------------------------------------------------------------
# 安装 ansible
# -----------------------------------------------------------------------------
RUN yum makecache \
    && yum install -y ansible

# -----------------------------------------------------------------------------
# 安装 walle-web 1.2.0
# -----------------------------------------------------------------------------
ADD walle-web-1.2.0.tar.gz /home/work/_site/
ADD _site /home/work/_site
RUN cd /home/work/_site/walle-web-1.2.0 \
    && composer install --prefer-dist --no-dev --optimize-autoloader -vvvv

# -----------------------------------------------------------------------------
# 删除多余文件
# -----------------------------------------------------------------------------
RUN yum clean all \
    && rm -rf /home/work/_src/* \
    && chown -R work:work /home/work/*

# -----------------------------------------------------------------------------
# 复制启动命令
# -----------------------------------------------------------------------------
COPY walle-web-init.sh /home/work/_script/walle-web-init.sh
RUN chmod 777 /home/work/_script/walle-web-init.sh

# -----------------------------------------------------------------------------
# 通过 supervisor 启动 walle-web
# -----------------------------------------------------------------------------
ADD _app/supervisord/conf.d/walle-web.ini /home/work/_app/supervisord/conf.d/walle-web.ini

# -----------------------------------------------------------------------------
# 修改 Nginx 的配置文件支持PHP，并配置默认站点
# -----------------------------------------------------------------------------
ADD _app/nginx/conf/conf.d/default.conf /home/work/_app/nginx/conf/conf.d/default.conf