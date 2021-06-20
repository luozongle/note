[TOC]

# 说明:

sysbench是一种基于LuaJIT编写的多线程基准测试工具。它最常用于数据库基准测试，但也可以用于创建不涉及数据库服务器的任意复杂工作测试。

sysbench github地址

```
https://github.com/akopytov/sysbench#installing-from-binary-packages
```



# 安装步骤



## 一、安装依赖

> 根据不同的系统版本安装不同的依赖  

- Debian/Ubuntu

```bash
apt -y install make automake libtool pkg-config libaio-dev
# For MySQL support
apt -y install libmysqlclient-dev libssl-dev
# For PostgreSQL support
apt -y install libpq-dev
```

- RHEL/Centos

```bash
yum -y install make automake libtool pkgconfig libaio-devel
# For MySQL support, replace with mysql-devel on RHEL/CentOS 5
yum -y install mariadb-devel openssl-devel
# For PostgreSQL support
yum -y install postgresql-devel
```

- Fedora

```bash
dnf -y install make automake libtool pkgconfig libaio-devel
# For MySQL support
dnf -y install mariadb-devel openssl-devel
# For PostgreSQL support
dnf -y install postgresql-devel
```

- MacOs

```bash
brew install automake libtool openssl pkg-config
# For MySQL support
brew install mysql
# For PostgreSQL support
brew install postgresql
# openssl is not linked by Homebrew, this is to avoid "ld: library not found for -lssl"
export LDFLAGS=-L/usr/local/opt/openssl/lib 
```



将github上的代码下载下来

> 用git 或者 http下载都可以(http下载可能更快一些)

```
git clone https://github.com/akopytov/sysbench.git
```



## 二、编译安装

```
./autogen.sh
# Add --with-pgsql to build with PostgreSQL support
./configure
make -j
make install
```



## 三、将mysql下的lib目录添加到动态库的查找路径中

```bash
export LD_LIBRARY_PATH=$MYSQL_HOME/lib:$LD_LIBRARY_PATH
```



## 四、验证

```bash
sysbench --verion
```