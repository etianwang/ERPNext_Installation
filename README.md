# ERPNext_Installation

# 0.前期准备
```
宝塔
yarn 1.12+
pip 20+
wkhtmltopdf
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

