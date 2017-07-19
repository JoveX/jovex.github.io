---
layout: post
title:  "CentOS6.8安装LNMP"
date:   2017-07-19 11:30:40
categories: 
---
# CentOS6.8安装LNMP
## 要求
MySQL
> 版本：5.6
> 安装目录：/usr/local/lnmp/mysql5.6
> 端口：6000

PHP
> 版本：5.5.38
> 安装目录：/usr/local/lnmp/php5.5.38
> 端口：9900

Nginx
> 版本：1.10.2
> 安装目录：/usr/local/lnmp/nginx1.10.2
> 端口：8800

系统
> 版本：CentOS release 6.8 (Final)

## 安装系统依赖：

```
yum -y install make gcc-c++ cmake bison-devel  ncurses-devel wget
```

## MySQL
### 安装
下载安装包：[mysql-5.6.37](https://cdn.mysql.com//Downloads/MySQL-5.6/mysql-5.6.37.tar.gz)

```
wget -c wget https://cdn.mysql.com//Downloads/MySQL-5.6/mysql-5.6.37.tar.gz
tar zxvf mysql-5.6.37.tar.gz
cd mysql-5.6.37
```

编译安装：

```
cmake \
    -DCMAKE_INSTALL_PREFIX=/usr/local/lnmp/mysql5.6 \
    -DMYSQL_DATADIR=/usr/local/lnmp/mysql5.6/data \
    -DSYSCONFDIR=/etc \
    -DWITH_MYISAM_STORAGE_ENGINE=1 \
    -DWITH_INNOBASE_STORAGE_ENGINE=1 \
    -DWITH_MEMORY_STORAGE_ENGINE=1 \
    -DWITH_READLINE=1 \
    -DMYSQL_UNIX_ADDR=/var/lib/mysql/mysql.sock \
    -DMYSQL_TCP_PORT=6000 \
    -DENABLED_LOCAL_INFILE=1 \
    -DWITH_PARTITION_STORAGE_ENGINE=1 \
    -DEXTRA_CHARSETS=all \
    -DDEFAULT_CHARSET=utf8 \
    -DDEFAULT_COLLATION=utf8_general_ci

make && make install
```

编译参数可以参考：[https://dev.mysql.com/doc/refman/5.6/en/source-configuration-options.html](https://dev.mysql.com/doc/refman/5.6/en/source-configuration-options.html)

### 配置：
查看是否有mysql用户及用户组：

```
cat /etc/passwd --查看用户列表
cat /etc/group  --查看用户组列表
```

如果没有就创建：

```
groupadd mysql
useradd -g mysql mysql
```

修改/usr/local/mysql权限：

```
chown -R mysql:mysql /usr/local/lnmp/mysql5.6
```

初始化配置：
进入安装路径并执行初始化配置脚本，创建系统自带的数据库和表

```
cd /usr/local/lnmp/mysql5.6
scripts/mysql_install_db --basedir=/usr/local/lnmp/mysql5.6 --datadir=/usr/local/lnmp/mysql5.6/data --user=mysql
```

注：在启动MySQL服务时，会按照一定次序搜索my.cnf，先在/etc目录下找，找不到则会搜索"$basedir/my.cnf"，在本例中就是 /usr/local/mysql/my.cnf，这是新版MySQL的配置文件的默认位置！

### 启动
添加服务，拷贝服务脚本到init.d目录，并设置开机启动

```
cp support-files/mysql.server /etc/init.d/mysql
chkconfig mysql on
service mysql start  --启动MySQL
```

这时可能会报错：

```
Starting MySQL.170718 20:39:54 mysqld_safe Directory '/var/lib/mysql' for UNIX socket file don't exists.
 ERROR! The server quit without updating PID file (/var/lib/mysql/lianjia.pid)
```

发现是目录不正确，修改`/etc/my.cnf`中`data`目录即可。

### 配置用户
MySQL启动成功后，root默认没有密码，我们需要设置root密码。
设置之前，我们需要先设置PATH，不然无法直接调用mysql。

修改`/etc/profile`文件，在文件末尾添加

```
PATH=/usr/local/lnmp/mysql5.6/bin:$PATH
export PATH
```

关闭文件，运行下面的命令，让配置立即生效

```
source /etc/profile
```

现在，我们可以在终端内直接输入mysql进入，mysql的环境了

执行下面的命令修改root密码

```
mysql -uroot  
mysql> SET PASSWORD = PASSWORD('XXXX_PASSWORD_XXX');
```

若要设置root用户可以远程访问，执行

```
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'XXXX_PASSWORD_XXX' WITH GRANT OPTION;
```

### 设置防火墙
防火墙的6000端口默认没有开启，若要远程访问，需要开启这个端口

```
iptables -I INPUT -p tcp --dport 6000 -j ACCEPT
/etc/init.d/iptables save
```

在终端内运行下面的命令，刷新防火墙配置：

```
service iptables restart
```
 
OK，一切配置完毕，可以访问你的MySQL了

## PHP
### 安装

安装所需依赖：

```
yum groupinstall "Development tools"
yum install bzip2-devel curl-devel libjpeg-devel libpng-devel freetype-devel libc-client-devel.i686 libc-client-devel -y
```

下载安装包:[php-5.5.38](http://mirrors.sohu.com/php/php-5.5.38.tar.gz)

```
wget -c wget http://mirrors.sohu.com/php/php-5.5.38.tar.gz
```

解压：

```
tar zxvf php-5.5.38.tar.gz
```

编译安装：

```
./configure \
    --prefix=/usr/local/lnmp/php5.5.38 \
    --enable-bcmath \
    --with-bz2 \
    --enable-calendar \
    --with-curl \
    --enable-exif \
    --enable-ftp \
    --with-gd \
    --with-jpeg-dir \
    --with-png-dir \
    --with-freetype-dir \
    --enable-gd-native-ttf \
    --with-imap \
    --with-imap-ssl \
    --with-kerberos \
    --enable-mbstring \
    --with-mcrypt \
    --with-mhash \
    --with-mysql \
    --with-mysqli \
    --with-openssl \
    --with-pcre-regex \
    --with-pdo-mysql \
    --with-zlib-dir \
    --with-regex \
    --enable-sysvsem \
    --enable-sysvshm \
    --enable-sysvmsg \
    --enable-soap \
    --enable-sockets \
    --with-xmlrpc \
    --enable-zip \
    --with-zlib \
    --enable-inline-optimization \
    --enable-mbregex \
    --enable-opcache \
    --enable-fpm

make && make instll
```

有个报错：`configure: error: xml2-config not found. Please check your libxml2 installation.`

安装libxml2：

```
yum install -y libxml2 libxml2-devel
```

重新编译又有个报错：`configure: error: mcrypt.h not found. Please reinstall libmcrypt.`

安装libmcrypt：

```
wget ftp://mcrypt.hellug.gr/pub/crypto/mcrypt/attic/libmcrypt/libmcrypt-2.5.7.tar.gz
tar -zxvf libmcrypt-2.5.7.tar.gz
cd libmcrypt-2.5.7
./configure prefix=/usr/local/libmcrypt/
```

再再重新编译安装，通过！

### 配置php-fpm服务：

拷贝配置文件：

```
cp /usr/local/lnmp/php5.5.38/etc/php-fpm.conf.default /usr/local/lnmp/php5.5.38/etc/php-fpm.conf
cp php.ini-production /usr/local/lnmp/php5.5.38/etc/php.ini
```

配置启动服务：

```
cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
chmod +x /etc/init.d/php-fpm
chkconfig php-fpm on
service php-fpm start
```

PS：可以通知修改`/usr/local/lnmp/php5.5.38/etc/php-fpm.conf`里面的`listen`配置监听端口号。

## 安装Nginx
### 安装
安装系统依赖：

```
yum install -y perl
```

安装pcre：

```
wget -c ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.41.tar.gz
tar zxvf pcre-8.41.tar.gz
cd pcre-8.41
./configure
make && make install
```

安装zlib：

```
wget -c http://zlib.net/zlib-1.2.11.tar.gz
tar zxvf zlib-1.2.11.tar.gz
cd zlib-1.2.11
./configure
make && make install
```

安装openssl：

```
wget -c https://www.openssl.org/source/openssl-1.0.2l.tar.gz
tar zxvf openssl-1.0.2l.tar.gz
cd openssl-1.0.2l
./config
make && make install
```


下载安装包:[nginx-1.10.2](http://nginx.org/download/nginx-1.10.2.tar.gz)

```
wget -c http://nginx.org/download/nginx-1.10.2.tar.gz
```

解压：

```
tar zxvf nginx-1.10.2.tar.gz
```

编译安装：

```
 ./configure \
    --sbin-path=/usr/local/lnmp/nginx1.10.2/nginx \
    --conf-path=/usr/local/lnmp/nginx1.10.2/nginx.conf \
    --pid-path=/usr/local/lnmp/nginx1.10.2/nginx.pid \
    --with-http_ssl_module \
    --with-pcre=/root/lnmp/pcre-8.41 \
    --with-zlib=/root/lnmp/zlib-1.2.11 \
    --with-openssl=/root/lnmp/openssl-1.0.2l
```

编译参数可以参考：[Building nginx from Sources](http://nginx.org/en/docs/configure.html)
创建一个Nginx的运行用户：

```
useradd -s /sbin/nologin www
```
### 启动：
Nginx装完后在`/etc/init.d/`下没有启动脚本，需要手动添加。
我使用的是yum的启动脚本，修改成跟自己本地一样的配置，如下：

```
#!/bin/sh
#
# nginx - this script starts and stops the nginx daemin
#
# chkconfig:   - 85 15 
# description:  Nginx is an HTTP(S) server, HTTP(S) reverse \
#               proxy and IMAP/POP3 proxy server
# processname: nginx
# config:      /usr/local/lnmp/nginx1.10.2/nginx.conf
# pidfile:     /usr/local/lnmp/nginx1.10.2/nginx.pid

# Source function library.
. /etc/rc.d/init.d/functions

# Source networking configuration.
. /etc/sysconfig/network

# Check that networking is up.
[ "$NETWORKING" = "no" ] && exit 0

nginx="/usr/local/lnmp/nginx1.10.2/nginx"
prog=$(basename $nginx)

NGINX_CONF_FILE="/usr/local/lnmp/nginx1.10.2/nginx.conf"

lockfile=/var/lock/subsys/nginx

start() {
    [ -x $nginx ] || exit 5
    [ -f $NGINX_CONF_FILE ] || exit 6
    echo -n $"Starting $prog: "
    daemon $nginx -c $NGINX_CONF_FILE
    retval=$?
    echo
    [ $retval -eq 0 ] && touch $lockfile
    return $retval
}

stop() {
    echo -n $"Stopping $prog: "
    killproc $prog -QUIT
    retval=$?
    echo
    [ $retval -eq 0 ] && rm -f $lockfile
    return $retval
}

restart() {
    configtest || return $?
    stop
    start
}

reload() {
    configtest || return $?
    echo -n $"Reloading $prog: "
    killproc $nginx -HUP
    RETVAL=$?
    echo
}

force_reload() {
    restart
}

configtest() {
  $nginx -t -c $NGINX_CONF_FILE
}

rh_status() {
    status $prog
}

rh_status_q() {
    rh_status >/dev/null 2>&1
}

case "$1" in
    start)
        rh_status_q && exit 0
        $1
        ;;
    stop)
        rh_status_q || exit 0
        $1
        ;;
    restart|configtest)
        $1
        ;;
    reload)
        rh_status_q || exit 7
        $1
        ;;
    force-reload)
        force_reload
        ;;
    status)
        rh_status
        ;;
    condrestart|try-restart)
        rh_status_q || exit 0
            ;;
    *)
        echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}"
        exit 2
esac
```

启动脚本也可以参考：
[Red Hat NGINX Init Script](https://www.nginx.com/resources/wiki/start/topics/examples/redhatnginxinit/)

## 配置Nginx -> PHP-FPM -> MySQL
Nginx配置：

```
server {
    listen 8800;
    # server_name example.com;
    location / {
        root /data0/www/htdocs/lnmp-team/public;
        index  index.php;
        if (!-e $request_filename){
            rewrite ^(.*)$ /index.php?_url=$1;
        }

        location ~ \.php {
            try_files                  $uri $uri/index.php @rewrite;
            fastcgi_split_path_info    ^(.+\.php)(/.+)$;
            fastcgi_pass               127.0.0.1:9900;
            fastcgi_index              index.php;
            fastcgi_param              SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include                    fastcgi_params;
        }
    }
}
```

在`/data0/www/htdocs/lnmp-team/public`下新建文件：

```
<?php
// phpinfo.php

echo phpinfo();
```

访问：http://ip:8800/phpinfo.php，看到下面页面，说明配置成功了。
![](https://ooo.0o0.ooo/2017/07/19/596ed1a8c3c00.jpg)

再新建个文件，测试数据库连接：

```
<?php
// index.php

$conn = @mysql_connect('127.0.0.1:6000', 'root', '');
if (!$conn){
    die('连接数据库失败：' . mysql_error());
}

mysql_select_db('test', $conn);
mysql_query('set character set "utf8"');
$sql = 'SELECT * FROM user';
$result = mysql_query($sql);

while( $row = mysql_fetch_array($result) ){
    echo 'ID:' . $row['id'] . '<br />';
    echo '姓名:' . $row['name'] . '<br />';
    echo '年龄:' . $row['age'] . '<br />';
    echo '<br />';
}
```

访问：http://ip:8800/index.php，看到下面页面就说明成功了：
![](https://i.loli.net/2017/07/19/596ed1a8932b2.jpg)


