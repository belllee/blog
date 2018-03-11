---
title: centos安装mysql过程记录
date: 2017-01-10 19:39:06
tags: [工具,MySQL]
---
软件版本：centos7，mysql5.6

具体安装过程：

# 1、通过ssh上传rpm安装文件

# 2、由于centos默认安装了mariadb，需要先卸载。

    查询是否已安装：

     $ rpm -qa |grep mariadb 

    如果已经安装，则卸载：

    $ rpm -e --nodeps mariadb-libs-5.5.47-1.el7_2.x86_64

# 3、分别安装mysql server/client/devel。

    切换到安装包目录：    

    $ cd /usr/local/sw/MySql5.6

     执行安装命令：

    $ rpm -ivh MySQL-server-5.6.13-1.el6.x86_64.rpm

     $ rpm -ivh MySQL-client-5.6.13-1.el6.x86_64.rpm

     $ rpm -ivh MySQL-devel-5.6.13-1.el6.x86_64.rpm

# 4、复制服务启动文件
    $ cp /usr/share/mysql/mysql.server /etc/init.d/mysqld

# 5、启动mysql服务
    $ /etc/init.d/mysqld start

    检查mysql 是否已经启动:$ netstat -atln | grep 3306

# 6、把mysql加入开机自启动:
    $ chkconfig  --add mysqld  

# 7、设置账号密码

    关闭mysql：$ /etc/init.d/mysql stop

    进入mysql安全模式: $ mysqld_safe --user=mysql --skip-grant-tables --skip-networking &

    设置root账号密码，允许root远程连接，开发所有权限：

    mysql -u root

    mysql> use mysql;

    mysql> UPDATE user SET Password=PASSWORD('newpassword') where USER='root';

    mysql> INSERT INTO mysql.user (Host,User,Password,ssl_cipher,x509_issuer,x509_subject) VALUES ('%','root',PASSWORD('newpassword'),"","",""); 

    mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'newpassword' WITH GRANT OPTION;

    mysql> FLUSH PRIVILEGES;

    mysql> quit