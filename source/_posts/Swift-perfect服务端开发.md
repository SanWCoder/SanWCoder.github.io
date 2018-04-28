---
title: Swift-perfect服务端开发
comments: true
date: 2018-01-20 14:13:58
tags:
 - Swift
 - perfect
 - mysql
 - homebrow
categories:
 - iOS
photos:
thumbnail:
---

Swift-perfect服务端开发

<!-- more -->

## 准备阶段（SPM） 

### 依存关系
  
```objc
~ swfit --version // swift版本必须在3.0以上才能编译
```  

### 克隆基础模板 
 
```objc
git clone https://github.com/PerfectlySoft/PerfectTemplate.git
```  
所有的SPM项目至少要包括一个 Sources 目录和一个 Package.swift 文件  

### 编译  

```objc
swift build // 编译
.build/debug/PerfectTemplate // 运行
swift build -c release // 编译一个用于发行的版本运行后可发行版本的可执行程序被放在了.build/release/目录下
swift build --clean // 清理所有编译临时文件并产生一个干净的版本 
swift build --clean=dist // .build目录和Packages目录都会被删除。并且能够重新下载所有依存关系以获得最新版本对项目的支持。
```
### 生成xcode项目

```objc
swift package generate-xcodeproj
```  

## 下载mysql依赖包

### 在Package.swift 中添加MYSQL并重新编译  

```objc
.Package(url:"https://github.com/PerfectlySoft/Perfect-MySQL.git",majorVersion : 2)
swift build // 重新编译
```
### 安装Homebrew 

#### perfect推荐使用Homebrew安装mysql如果按照homeBrow安装完则不用配置任何东西就可以运行

```objc
// 安装Homebrew，Homebrew安装在/user/local目录下，同时它会创建/user/local/Cellar目录用于存放通过Homebrew安装的程序，运行brew -v查看安装版本

// 1.
 `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
// 2.安装完成顺势更新
 brew update 
// 3.安装完成以后检查文件运行是否正常。注意警告，如果之前手动安装过node最好先删除node文件 local/lib , local/include
 brew doctor 
```
#### Homebrew常用命令

```objc
brew search * --搜索程序，例：brew search python
brew install * --安装程序，例：brew install python
brew uninstall * --卸载程序，例：brew uninstall python
brew list --列举通过Homebrew安装的程序
brew update --更新Homebrew
brew upgrade [*] --更新某个具体程序，或者更新所有程序
brew cleanup [*] --删除某个具体程序，或者删除所有老版程序
brew outdated --查看哪些程序需要更新
brew home * --用浏览器打开
brew info * --显示软件内容信息
brew deps * --显示包依赖
brew server * --启动web服务器，可以通过浏览器访问http://localhost:4567/ 来同网页来管理包
brew -h --查看帮助
```

#### 删除Homebrew

```objc
cd `brew –prefix`
rm -rf Cellar
brew prune
rm -rf Library .git .gitignore bin/brew README.md share/man/man1/brew
rm -rf ~/Library/Caches/Homebrew
```
#### 使用Homebrew安装mysql

```objc
// 1.安装mysql
brew install mysql
// 2.开启MySQL服务
mysql.server start // brew services start mysql
// 重新启动mysql服务
mysql.server restart
// 停止mysql服务
mysql.server stop // brew services stop mysql
// 3.初始化MySQL配置向导
mysql_secure_installation
// 4.连接mysql
mysql -u root -p

```
### MySQL配置

#### 初始化MySQL配置向导mysql_secure_installation

```objc
// 启动mysql
➜  local mysql.server start        
Starting MySQL
. SUCCESS! 
// 运行配置
➜  local mysql_secure_installation

Securing the MySQL server deployment.
// validate_password为密码安全验证，如果不需要或者安全级别低 在/usr/local/etc/my.cnf文件中增加validate-password=OFF validate_password_policy=LOW
Connecting to MySQL using a blank password.
The 'validate_password' plugin is installed on the server.
The subsequent steps will run with the existing configuration
of the plugin.
Please set the password for root here.

New password: 

Re-enter new password: 

Estimated strength of the password: 0 
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.
// 删除默认无密码用户
Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
Success.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.
// 禁止远程root登录
Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y
Success.

By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.

// 删除默认自带的test数据库
Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y
 - Dropping test database...
Success.

 - Removing privileges on test database...
Success.

Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
Success.

All done! 

```
#### mysql测试

```objc
CREATE DATABASE  `unitedtrade` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
// 创建用户
CREATE USER 'trade'@'%' IDENTIFIED BY  'trade';
// 设置用户：
GRANT USAGE ON * . * TO  'trade'@'%' IDENTIFIED BY  'trade' WITH MAX_QUERIES_PER_HOUR 0 MAX_CONNECTIONS_PER_HOUR 0 MAX_UPDATES_PER_HOUR 0 MAX_USER_CONNECTIONS 0 ;
// 用户赋权：
GRANT ALL PRIVILEGES ON  `unitedtrade` . * TO  'trade'@'%' WITH GRANT OPTION ;
// 刷新权限：
flush privileges;
```
#### MySQL其他操作

```objc
// 终端退出mysql编辑
\q
// 关闭mysql:
mysql stop
// 安全模式启动MySQL
mysqld_safe --skip-grant-tables
```
#### perfect连接mysql配置

```objc
// 将mysqlclient.pc文件设置为可读写后删除-fno-omit-frame-pointer内容。
// 文件路径:  
/usr/local/lib/pkgconfig/mysqlclient.pc/
```
#### 彻底移除mysql

```objc 
  
// 通过HomeBrew安装从头开始
brew remove mysql  
brew cleanup  
launchctl unload -w ~/Library/LaunchAgents/com.mysql.mysqld.plist  // 设置了开机自动开启
rm ~/Library/LaunchAgents/com.mysql.mysqld.plist // 设置了开机自动开启
// 通过下载包直接安装下面开始
sudo rm /usr/local(/var)/mysql
sudo rm -rf /usr/local(/var)/mysql*
sudo rm -rf /Library/StartupItems/MySQLCOM // 设置了启动项
sudo rm -rf /Library/PreferencePanes/My* // 设置了启动项
(编辑 /etc/hostconfig) sudo vi /etc/hostconfig (删除行 MYSQLCOM=-YES)
sudo rm -rf /Library/Receipts/mysql*
sudo rm -rf /Library/Receipts/MySQL*
sudo rm -rf /var/db/receipts/com.mysql.*
```
## 遇到的问题及解决方法

```objc  
 1. 遇到error :Header '/usr/local/include/mysql/mysql.h'时找到mysql文件并替换/usr/local/mysql-5.7.15-osx10.11-x86_64/include/mysql.h
 
 2. 遇到找不到lmysqlclient文件时，在Target中找到MySQL，找到Library Search Paths中加上/usr/local/mysql-5.7.15-osx10.11-x86_64l/lib
 ```


