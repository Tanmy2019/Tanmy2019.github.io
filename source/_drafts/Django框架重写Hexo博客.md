---
title: Django框架重写Hexo博客
top: true
cover: false
toc: true
mathjax: false
date: 2021-05-02 00:29:26
author: 栗子
img: https://www.ipicbed.com/images/2021/05/02/django.jpg
categories: Python
tags: 
  - Python
  - Django
---

##  安装Django框架

如果是Mac系统的话，在系统上会自带python2版本，本次使用python3环境进行安装所以需要指定pip安装到python3 环境上。

```bash
# 安装Django框架至python3
pip3 install django
# python2环境使用
pip install django
```

###  验证
若要验证 Django 是否能被 Python 识别，可以在 shell 中输入 python。 然后在 Python 提示符下，尝试导入 Django：

```bash
python3
>>> import django
>>> print(django.get_version())
3.2
```
当然了，你也可能安装的是其它版本的 Django。

## 创建项目
如果这是你第一次使用 Django 的话，你需要一些初始化设置。也就是说，你需要用一些自动生成的代码配置一个 Django project —— 即一个 Django 项目实例需要的设置项集合，包括数据库配置、Django 配置和应用程序配置。

打开命令行，cd 到一个你想放置你代码的目录，然后运行以下命令：

```bash
django-admin startproject mysite
```

让我们看看 startproject 创建了些什么:

```bash
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

#### 这些目录和文件的用处是：

 - 最外层的 mysite/ 根目录只是你项目的容器， 根目录名称对 Django 没有影响，你可以将它重命名为任何你喜欢的名称。
 - `manage.py`: 一个让你用各种方式管理 Django 项目的命令行工具。
 - 里面一层的 mysite/ 目录包含你的项目，它是一个纯 Python 包。它的名字就是当你引用它内部任何东西时需要用到的 Python 包名。 (比如 mysite.urls).
 - `mysite/__init__.py`：一个空文件，告诉 Python 这个目录应该被认为是一个 Python 包。
 - `mysite/settings.py`：Django 项目的配置文件。
 - `mysite/urls.py`：Django 项目的 URL 声明，就像你网站的“目录”。
 - `mysite/asgi.py`：作为你的项目的运行在 ASGI 兼容的 Web 服务器上的入口。
 - `mysite/wsgi.py`：作为你的项目的运行在 WSGI 兼容的Web服务器上的入口。

####  启动简易服务器

```
python3 managoe.py runserver
```

## 创建APP应用

```
python3 manage.py startapp index
```
