# ERPNext_Installation

# 0.前期准备
```
Debian 11+
宝塔
yarn 1.12+
pip 20+
wkhtmltopdf (version 0.12.5 with patched qt)
mysql-mariadb
```
# 宝塔配置
## 安装宝塔面板
[宝塔](https://www.bt.cn/new/download.html)

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
·sudo apt-get update && sudo apt-get upgrade -y`  

# 配置环境
'sudo apt-get update && sudo apt-get install -y build-essential python3-dev python3-setuptools python3-pip python3-distutils python3.10-venv software-properties-common wkhtmltopdf redis-server mariadb-server libmysqlclient-dev libssl-dev libffi-dev libxrender1 libjpeg-dev zlib1g-dev libncurses5-dev liblcms2-dev libwebp-dev libharfbuzz-dev libfribidi-dev libxcb1-dev'  
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
·sudo make install·  




