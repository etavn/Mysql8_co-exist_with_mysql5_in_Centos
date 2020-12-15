# How to install Mysql8 into CentOS7 existing Mysql5

## Setup MYSQL8.0.21
Download Mysql
```
wget https://downloads.mysql.com/archives/get/p/23/file/mysql-8.0.21-el7-x86_64.tar.gz
```

Extract Tar file
```
tar -xvf mysql-8.0.21-el7-x86_64.tar.gz -C /usr/local && \
rm mysql-8.0.21-el7-x86_64.tar.gz
```

Preparing the locate
```
cd /usr/local && \
mv mysql-8.0.21-el7-x86_64 mysql8021 && \
mkdir data && mkdir logs && mkdir tmp && \
chown mysql tmp logs data
```

Initial Mysql
```
/usr/local/mysql8021/bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql8021 --datadir=/usr/local/mysql8021/data --log-error=/usr/local/mysql8021/logs/mysqld.log
```

Create `my.cnf` file with following the below content
```
[mysqld]
port=8021
user=mysql
basedir=/usr/local/mysql8021
datadir=/usr/local/mysql8021/data
tmpdir=/usr/local/mysql8021/tmp
lc-messages-dir=/usr/local/mysql8021/share
socket=/usr/local/mysql8021/tmp/mysql.sock
log-error=/usr/local/mysql8021/logs/mysqld.log

character-set-server=UTF8MB4

log_timestamps=SYSTEM

[mysqld_safe]
log-error=/usr/local/mysql8021/logs/mysqld.log
pid-file=/usr/local/mysql8021/tmp/mysqld.pid

[client]
default-character-set=utf8
```

Start Mysql Server
```
/usr/local/mysql8021/bin/mysqld_safe --defaults-file=/usr/local/mysql8021/my.cnf &
```

## Setup alias
Create execute file `/usr/local/mysql8021/mysql8` with content below
```
#!/bin/bash
/usr/local/mysql8021/bin/mysql --port=8021 --socket=/usr/local/mysql8021/tmp/mysql.sock -p mysql "$@"
```

Setup execute permission
```
chmod +x mysql8
```

Create symbol-link
```
ln -s /usr/local/mysql8021/mysql8 /usr/bin/mysql8
```

Get and see mysql8 version
```
mysql8 --version
```

## Notice
Get Mysql Password
```
cat mysql8021/logs/mysqld.log | grep "root@localhost" | awk '{print $13}'
```

Access and edit the root password
```
mysql8 -u root -p 
```
```
alter user 'root'@'localhost' identified by '********';
```

## Author
- [Vincent Nguyen](mailto:vannhd@ethan-tech.com)
- Don't hesitate to contact me if you get any trouble when installing