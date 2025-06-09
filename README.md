# ERPNext_Installation
# 环境安装ERPnext 带生产环境
# 前期准备
```
Debian 11+
宝塔
yarn 1.12+
pip 20+
redis
wkhtmltopdf (version 0.12.5 with patched qt)
mysql-mariadb
```
# 宝塔配置
## 安装宝塔面板
[宝塔](https://www.bt.cn/new/download.html)  
```
if [ -f /usr/bin/curl ];then curl -sSO https://download.bt.cn/install/install_panel.sh;else wget -O install_panel.sh https://download.bt.cn/install/install_panel.sh;fi;bash install_panel.sh ed8484bec
```

删除宝塔安全入口
`rm -f /www/server/panel/data/admin_path.pl`
宝塔自选安装mysql-mariadb 10.3+
安装npm node.js版本管理器
## 添加用户
`adduser frappe`
## 添加权限
`usermod -aG sudo frappe`
>切换新用户执行接下来的安装
## 更新系统
`sudo apt-get update && sudo apt-get upgrade -y`  

# 配置环境
'sudo apt-get update && sudo apt-get install -y build-essential python3-dev python3-setuptools python3-pip python3-distutils python3.10-venv software-properties-common wkhtmltopdf redis-server mariadb-server libmysqlclient-dev libssl-dev libffi-dev libxrender1 libjpeg-dev zlib1g-dev libncurses5-dev liblcms2-dev libwebp-dev libharfbuzz-dev libfribidi-dev libxcb1-dev'  
</br>
wkhtmltopdf安装  

[Github链接](https://github.com/wkhtmltopdf/wkhtmltopdf/releases)  

## 安装python
安装 python 3.10.13  
`sudo wget https://mirrors.huaweicloud.com/python/3.10.13/Python-3.10.13.tgz`   
解压  
`sudo tar -xvf Python-3.10.13,tgz`  
配置编译  
`cd Python-3.10.13`  
配置  
`sudo ./configure --enable-optimizations`  
编译  
`sudo make install`  
添加软连接  
`sudo update-alternatives --install /usr/bin/python python  /usr/local/bin/python3.10 1`  
更换Pypi镜像(中国)  
`pip3 config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple`  
## 安装环境包及redis
`sudo apt-get install -y git python2-dev python3-dev python3-setuptools python3-pip python3-distutils redis-server curl xvfb libfontconfig1 libxrender1 libjpeg-dev zlib1g-dev libssl-dev libffi-dev libmysqlclient-dev`  </br>
`sudo apt-get install git python2-dev python3-dev python3-setuptools python3-pip python3-distutils redis-server python3-venv xvfb libfontconfig wkhtmltopdf -y`  
### 配置mariadb的my.cnf
将下文中的内容复制到my.cnf文件中[mysqld]  
```
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
```  
[mysql]  
`default-character-set = utf8mb4`  

### 配置mariadb
```mysql
mysql -u root -p
USE mysql;
ALTER USER root@localhost IDENTIFIED VIA mysql_native_password;
```
>修改密码123456

```mysql
SET PASSWORD = PASSWORD('123456');
FLUSH PRIVILEGES;
exit
```
### 重启mariadb数据库
`sudo service mysql restart`

# 安装bench命令控制台
>操作用当前用户而非root  
`sudo pip3 install frappe-bench`  
## 安装frappe
安装官方版本(github)  
`bench init frappe --verbose`  
安装develop版本(中国gitcode)  

`bench init frapp --frappe-path=https://gitcode.net/frappe/frappe.git --verbose`  
进入frappe文件夹目录  
`cd frappe`  
运行bench  
`bench start`  
>运行bench后新开一个ssh窗口来安装frappe  
`cd frappe`  

## 新建站点
站点名称site.local自己改
`bench new-site site.local`  
`bench use site.local`  

## 安装ERPNext
### 下载应用
安装官方版本(github)
`bench get-app erpnext`  
安装develop版本(gitcode中国)
`bench get-app erpnext https://gitcode.net/frappe/erpnext.git`  
## 安装应用
`bench --site site.local install-app erpnext`  
## 设置默认站点
`bench use site.local`  
## 重建文件
`bench migrate` 
>如果报错，删除重装  
### 卸载应用
`bench uninstall-app erpnext`  
---  
>至此安装完毕，后续为生产环境的设置  

# 设置生产环境
启用计划程序  
`bench --site site.local enable--scheduler`  
添加主机  
`bench --site site.local add-to-hosts`  
关闭维护模式  
`bench --site site.local set-maintenance-mode off`  
恢复调度程序  
`bench --site site.local scheduler resume`  
安装snapd  
```nash
sudo apt-get install snapd -y
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```  
生产站点  
`sudo bench setup production frappe`  
# 安装supervisor
```bash
bench setup supervisor
sudo ln -s pwd/config/supervisor.conf /etc/supervisor/.conf.d/frappe-bench.conf
```  
修改supervisor配置文件  
`sudo vim /etc/supervisor/supervisord.conf`  
```
[unix_http_server]
file=var/tmp/supervisord.sock
chmod=0700
```
在这个位置加上这一行，USERNAME如frappe:frappe  
`chown=frappe:frappe`  
重启supervisor
`sudo -A systemctl restart supervisor`  
`sudo bench setup sudoers $(whoami)`  
`sudo service supervisor stop`  
`sudo service nginx stop`  
`sudo service supervisor start`  
`sudo service nginx start`  
`sudo bench setup production frappe`  


