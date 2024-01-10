---
title: "CentOS上运行Odoo源码实战"
date: 2024-01-10T09:13:56+08:00
draft: false
tags: ["CentOS", "Odoo"]
categories: ["documentation"]

---

## 直接安装Odoo失败历程

### 在CentOS 7.9上安装

#### 安装Odoo过程

```bash
#更新组件
yum update -y
yum install epel-release

#安装Postgresql服务器
yum install -y postgresql-server

#初始化数据库
postgresql-setup initdb

#启动postgresql服务
systemctl enable postgresql
systemctl start postgresql

#安装wkhtmltopdf
wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-3/wkhtmltox-0.12.6.1-3.fedora37.x86_64.rpm
dnf localinstall wkhtmltox-0.12.6.1-3.fedora37.x86_64.rpm

######################请勿学习，错了可以用###########################
#我输了下面错误命令导致的
dnf config-manager --add-repo=https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-3/wkhtmltox-0.12.6.1-3.fedora37.x86_64.rpm
#插曲修复
dnf repolist
dnf config-manager --disable github.com_wkhtmltopdf_packaging_releases_download_0.12.6.1-3_wkhtmltox-0.12.6.1-3.fedora37.x86_64.rpm
#############################################

#Yum配置Odoo源
yum-config-manager --add-repo=https://nightly.odoo.com/17.0/nightly/rpm/odoo.repo

#尝试安装：
yum install odoo
#失败，参见下面的错误

#尝试本地安装，也失败了
wget https://nightly.odoo.com/17.0/nightly/rpm/odoo_17.0.latest.noarch.rpm
yum localinstall -y odoo_17.0.latest.noarch.rpm

```
==错误提示==

```bash
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(docutils)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(reportlab)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(pillow)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(markupsafe)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(xlrd)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(pyopenssl)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: sassc
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(rjsmin)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(geoip2)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(python-stdnum)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(python-dateutil)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(passlib)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(polib)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(qrcode)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(babel) >= 1
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(gevent)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(decorator)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(num2words)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(chardet)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(pydot)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(ofxparse)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(werkzeug)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(idna)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(zeep)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(libsass)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(requests)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(psutil)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(xlwt)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(greenlet)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(pytz)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(pyserial)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(xlsxwriter)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(vobject)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(pypdf2)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python(abi) = 3.11
           Installed: python-2.7.5-94.el7_9.x86_64 (@updates)
               python(abi) = 2.7
               python(abi) = 2.7
           Available: python-2.7.5-89.el7.x86_64 (base)
               python(abi) = 2.7
               python(abi) = 2.7
           Available: python-2.7.5-90.el7.x86_64 (updates)
               python(abi) = 2.7
               python(abi) = 2.7
           Available: python-2.7.5-92.el7_9.x86_64 (updates)
               python(abi) = 2.7
               python(abi) = 2.7
           Available: python-2.7.5-93.el7_9.x86_64 (updates)
               python(abi) = 2.7
               python(abi) = 2.7
           Available: python3-3.6.8-17.el7.i686 (base)
               python(abi) = 3.6
               python(abi) = 3.6
           Available: python3-3.6.8-18.el7.i686 (updates)
               python(abi) = 3.6
               python(abi) = 3.6
           Available: python3-3.6.8-19.el7_9.i686 (updates)
               python(abi) = 3.6
               python(abi) = 3.6
           Available: python3-3.6.8-21.el7_9.i686 (updates)
               python(abi) = 3.6
               python(abi) = 3.6
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(pyusb) >= 1~b1
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(cryptography)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(psycopg2) >= 2.2
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(lxml)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(jinja2)
Error: Package: odoo-17.0-1.noarch (odoo-nightly)
           Requires: python3.11dist(urllib3)
 You could try using --skip-broken to work around the problem
 You could try running: rpm -Va --nofiles --nodigest
```

#### 安装Python3

我第一反应是怀疑Python的版本问题，因为CentOS上默认安装的版本是Python 2.7，尝试了

```bash
#安装python3，在CentOS 7.9上，源里最高版本也就3.6.8
yum install python3
```

#### 编译Python3.12.1

之后又尝试了通过编译Python 3.12.1，编译通过了，但是缺少模块(runpy)

```bash
#安装依赖
yum install gcc openssl-devel bzip2-devel libffi-devel -y

#下载包
curl -O https://www.python.org/ftp/python/3.12.1/Python-3.12.1.tgz

#解压
tar -xzf Python-3.12.1.tgz

#进入文件
cd Python-3.12.1

#配置
./configure --enable-optimizations

#编译安装
make
make altinstall
```

```bash
./_bootstrap_python ./Programs/_freeze_module.py abc ./Lib/abc.py Python/frozen_modules/abc.h
Fatal Python error: init_import_site: Failed to import the site module
Python runtime state: initialized
Traceback (most recent call last):
  File "/root/Python-3.12.1/Lib/site.py", line 73, in <module>
    import os
  File "/root/Python-3.12.1/Lib/os.py", line 29, in <module>
    from _collections_abc import _check_methods
SystemError: <built-in function compile> returned NULL without setting an exception
make: *** [Python/frozen_modules/abc.h] Error 1
```

还是不行，操作系统也尝试了CentOS-Stream，还是不行，只是错误换了一个形式，得了，换另一种途径吧

```bash
Error: 
 Problem: conflicting requests
  - nothing provides python3.11dist(babel) >= 1 needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(pyusb) >= 1~b1 needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides /usr/bin/python2 needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(chardet) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(decorator) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(docutils) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(geoip2) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(gevent) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(greenlet) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(jinja2) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(libsass) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(markupsafe) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(num2words) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(ofxparse) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(passlib) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(pillow) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(polib) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(psutil) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(pydot) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(pyopenssl) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(pypdf2) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(pyserial) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(python-dateutil) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(python-stdnum) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(pytz) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(qrcode) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(reportlab) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(rjsmin) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(vobject) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(werkzeug) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(xlrd) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(xlsxwriter) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(xlwt) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides python3.11dist(zeep) needed by odoo-17.0-1.noarch from odoo-nightly
  - nothing provides sassc needed by odoo-17.0-1.noarch from odoo-nightly
```

#### 尝试Python虚拟环境

```bash
#创建虚拟环境
python -m venv odoo-env

#激活
source odoo-env/bin/activate

#安装缺失依赖
pip install babel  pyusb chardet decorator docutils geoip2 gevent greenlet  jinja2 libsass markupsafe num2words ofxparse passlib  pillow polib psutil  pydot  pyopenssl pypdf2  pyserial python-dateutil python-stdnum pytz qrcode reportlab rjsmin vobject werkzeug xlrd xlsxwriter xlwt zeep
```



## 换源码运行Odoo

### 安装Python3.11和pip3

在尝试了很多次Python的编译失败之后，系统重新安装为CentOS-Stream，安装Python环境顺利了许多

```bash
dnf install python3.11

dnf install python3.11-pip
python3 -m pip install --upgrade pip
```

#### PostgreSQL安装

```bash
#安装Postgresql服务器
yum install -y postgresql-server

#初始化数据库
postgresql-setup initdb

#启动postgresql服务
systemctl enable postgresql
systemctl start postgresql
```



#### PostgreSQL创建用户和数据库

```bash
#查看版本，我这里是postgres (PostgreSQL) 13.11
postgres --version

sudo -i -u postgres

psql

#新建用户
CREATE USER odoo WITH PASSWORD 'your_password';

#修改密码，如果需要，但是odoo不支持默认用户postgres登录，所以不用尝试设置postgres
ALTER USER odoo WITH PASSWORD 'your_password';

#创建数据库
CREATEDB odooData

#退出sql命令行
\q

#退出postgresql用户
exit
```

#### 安装WKHtmlToPDF

`wkhtmltopdf` is not installed through **pip** and must be installed manually in [version 0.12.6](https://github.com/wkhtmltopdf/packaging/releases/tag/0.12.6.1-3) for it to support headers and footers. Check out the [wkhtmltopdf wiki](https://github.com/odoo/odoo/wiki/Wkhtmltopdf) for more details on the various versions.

```bash
#安装wkhtmltopdf
wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-3/wkhtmltox-0.12.6.1-3.fedora37.x86_64.rpm
dnf localinstall wkhtmltox-0.12.6.1-3.fedora37.x86_64.rpm
```



#### 下载源码并安装依赖

```bash
#安装git
dnf install git

#下载源码
git clone https://github.com/odoo/odoo.git

#进入目录
cd odoo

#安装依赖
pip install -r requirements.txt
```

得，没有成功，后面有时间再继续吧。。。
