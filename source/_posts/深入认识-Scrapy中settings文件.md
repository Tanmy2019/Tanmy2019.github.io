---
title: 深入认识 Scrapy框架中settings文件
date: 2021-04-30 10:08:37
author: Tanmy
img: https://www.ipicbed.com/images/2021/04/30/6.png
top: true
cover: false
# coverImg: /images/1.jpg
toc: true
mathjax: false
categories: Python
tags:
  - Scrapy
  - Python
---

##  深入认识 Scrapy框架中settings文件

###  为什么Scrapy项目中会存在settings文件呢，他到底是什么？

- 配置文件存放一些公共的变量（比如数据库的地址、账号+密码等），方便自己和别人修改
- 一般使用全大写字母命名变量名，例如 `LOG_LEVEL="WARN"`

##  settings文件详细解读



```python
BOT_NAME = 'Sun'  # 项目名

SPIDER_MODULES = ['Sun.spiders']  # 爬虫的位置
NEWSPIDER_MODULE = 'Sun.spiders'  # 如果我们新建一个爬虫，这个新建爬虫的位置

LOG_LEVEL="WARN"  # log日志显示的最低等级，只有等级大于或等于LOG_LEVEL的才会在终端中显示

# Crawl responsibly by identifying yourself (and your website) on the user-agent
USER_AGENT = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.89 Safari/537.36"
# 浏览器的身份标识，即是"User-Agent"

# Obey robots.txt rules
ROBOTSTXT_OBEY = True
# 默认情况下为True，表示遵守这个网站的robots协议
# 如果我们设置
# ROBOTSTXT_OBEY = True  # 不遵守robots协议
# 遵守robots协议的体现：最开始请求url地址的时候会先请求对应的robots.txt，如果我们不遵守robots协议，就不会请求

# Configure maximum concurrent requests performed by Scrapy (default: 16)
#CONCURRENT_REQUESTS = 32  # 设置最大并发请求
# scrapy中封装了一个twisted异步网络框架，在发送网络请求的时候不会说等一个请求请求成功之后再去发送下一个请求，而是同时去发送很多个请求
# 大概有多少个请求可以同时被发送呢？我们可以在这里自定义数量
# 注意：默认是16个，我们可以根据自己的爬虫项目需要自定义CONCURRENT_REQUESTS的数量
# CONCURRENT_REQUESTS的值越大，爬虫爬取的速度越快
# 但是注意，CONCURRENT_REQUESTS的值越大，越有可能被对方的服务器识别为一个爬虫程序
# 所以说，要适当设置CONCURRENT_REQUESTS的数量

#DOWNLOAD_DELAY = 3  # 下载延迟
# 每次请求之前，休眠3秒
# 可以让我们的爬虫请求速度变慢一些
# The download delay setting will honor only one of:
# 就是说我们的DOWNLOAD_DELAY参数可以配合着下面的CONCURRENT_REQUESTS_PER_DOMAIN参数和CONCURRENT_REQUESTS_PER_IP参数来使用
#CONCURRENT_REQUESTS_PER_DOMAIN = 16  # 每一个域名的最大并发请求数目
#CONCURRENT_REQUESTS_PER_IP = 16  # 每一个IP的最大并发请求数目

# Disable cookies (enabled by default)
#COOKIES_ENABLED = False  # cookie是否开启
# 默认情况下是开启的，也就是说我们请求完一个url地址之后，去请求下一个url地址的时候，scrapy默认是带上前一次请求对方服务器设置在我们本地的cookie信息
# 很重要

# Disable Telnet Console (enabled by default)
#TELNETCONSOLE_ENABLED = False
# 这是我们scrapy框架的一个插件
# 默认是开启的状态

# Override the default request headers:
#DEFAULT_REQUEST_HEADERS = {
#   'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
#   'Accept-Language': 'en',
#}
# 默认的请求头，默认是关闭的状态
# 开启之后，scrapy就会用这个请求头去发送请求
# 注意：如果我们将"User-Agent"参数放在这里，是没有任何效果的，因为专门有一个参数接收"User-Agent"对应的值

# Enable or disable spider middlewares
# See https://docs.scrapy.org/en/latest/topics/spider-middleware.html
# SPIDER_MIDDLEWARES = {
#    'Sun.middlewares.SunSpiderMiddleware': 543,
# }
# 爬虫中间件

# Enable or disable downloader middlewares
# See https://docs.scrapy.org/en/latest/topics/downloader-middleware.html
#DOWNLOADER_MIDDLEWARES = {
#    'Sun.middlewares.SunDownloaderMiddleware': 543,
#}
# 下载中间件

# Enable or disable extensions
# See https://docs.scrapy.org/en/latest/topics/extensions.html
#EXTENSIONS = {
#    'scrapy.extensions.telnet.TelnetConsole': None,
#}
# 插件

# Configure item pipelines
# See https://docs.scrapy.org/en/latest/topics/item-pipeline.html
ITEM_PIPELINES = {
   'Sun.pipelines.SunPipeline': 300,
}
# 管道

# 爬虫中间件、下载中间件、插件、管道的使用方法：
# 类似于ITEM_PIPELINES管道的使用方法
# 开启之后，键--位置，值--和引擎之间的距离（注意：值越大，权重越低；值越小，权重越高）

# Enable and configure the AutoThrottle extension (disabled by default)
# See https://docs.scrapy.org/en/latest/topics/autothrottle.html
#AUTOTHROTTLE_ENABLED = True
# AUTOTHROTTLE--自动限速
# 比如说我们通过scrapy爬取对方的网站，可能会由于我们的scrapy爬虫速度太快导致把对方的网站“抓崩了”
# “抓崩了”并不是我们想要的结果，因为“抓崩了”之后我们就没有办法再在对方的网站上爬取数据了
# 我们可以根据对方网站的情况让我们的scrapy爬虫速度变慢一些
# 通过调节下面的参数我们可以达到这个自动限速的目的
# 但是往往我们是不需要这样操作的
# The initial download delay
#AUTOTHROTTLE_START_DELAY = 5
# The maximum download delay to be set in case of high latencies
#AUTOTHROTTLE_MAX_DELAY = 60
# The average number of requests Scrapy should be sending in parallel to
# each remote server
#AUTOTHROTTLE_TARGET_CONCURRENCY = 1.0
# Enable showing throttling stats for every response received:
#AUTOTHROTTLE_DEBUG = False

# Enable and configure HTTP caching (disabled by default)
# 关于HTTP的缓存，默认是关闭的
# See https://docs.scrapy.org/en/latest/topics/downloader-middleware.html#httpcache-middleware-settings
#HTTPCACHE_ENABLED = True
#HTTPCACHE_EXPIRATION_SECS = 0
#HTTPCACHE_DIR = 'httpcache'
#HTTPCACHE_IGNORE_HTTP_CODES = []
#HTTPCACHE_STORAGE = 'scrapy.extensions.httpcache.FilesystemCacheStorage'
# 如果我们想要开启HTTP缓存就可以将其打开
# 它会将我们的HTTP缓存至设置的位置，实现对应的功能
# 不常用

# 每一块内容都是有对应的官方文档的
```



## **如何在代码中使用我们在settings.py文件中所做的设置**

现在，我们在`settings.py`文件中做了一些设置

[![ ](https://www.ipicbed.com/images/2021/04/30/1.md.png)](https://www.ipicbed.com/image/n4mz)

`settings.py`文件中有一个`MONGO_HOST`，我们应该如何在spider中使用呢？

### 第一种方法：导入

1.导入

[![ ](https://www.ipicbed.com/images/2021/04/30/2.md.png)](https://www.ipicbed.com/image/n8n8)

2.现在我们就可以使用我们在`settings.py`文件中定义的这个`MONGO_HOST`参数了

3.如果我们想要在其他文件中使用`MONGO_HOST`这个参数也是一样方法：导入

以` pipelines.py` 为例，

`from .settings import MONGO_HOST`

[![ ](https://www.ipicbed.com/images/2021/04/30/3.md.png)](https://www.ipicbed.com/image/ntWC)

### 第二种方法：属性

其实我们的spider本身就有一个属性就是settings

#### **1.如何在爬虫中使用settings属性**

[![ ](https://www.ipicbed.com/images/2021/04/30/4.md.png)](https://www.ipicbed.com/image/nYJE)



```python
# -*- coding: utf-8 -*-
import scrapy
from ..items import SunItem
# from ..settings import MONGO_HOST

class SunSpider(scrapy.Spider):
    name = 'sun'
    allowed_domains = ['sun0769.com']
    start_urls = ['http://wz.sun0769.com/political/index/politicsNewest?id=1&page=1']

    def parse(self, response):
        # # spider本身就有一个属性叫做settings
        # self.settings["MONGO_HOST"]  # self.settings是一个字典
        # self.settings.get("MONGO_HOST","指定的MONGO_HOST的值")
        """
        由于self.settings是一个字典。所以我们可以对其使用
        self.settings.get("MONGO_HOST")  # 如果MONGO_HOST不存在，返回的就是一个None值
        # 或者是返回我们在get方法中给MONGO_HOST手动指定的值
        self.settings.get("MONGO_HOST","指定的MONGO_HOST的值")
        """


        # 分组
        li_list=response.xpath('//div[@class="width-12"]/ul[@class="title-state-ul"]//li[@class="clear"]')
        for li in li_list:
            item=SunItem()
            item["num"]=li.xpath('.//span[@class="state1"]/text()').extract_first()
            item["title"]=li.xpath('.//span[@class="state3"]/a[1]/text()').extract_first()
            item["response_time"]=li.xpath('.//span[@class="state4"]/text()').extract_first().strip()
            item["response_time"]=item["response_time"].split("：")[-1]
            item["ask_time"]=li.xpath('.//span[@class="state5 "]/text()').extract_first()
            item["detail_url"]="http://wz.sun0769.com"+li.xpath('.//span[@class="state3"]/a[1]/@href').extract_first()

            yield scrapy.Request(item["detail_url"],
                                 callback=self.parse_detail_url,
                                 meta={"item":item}  # 通过meta传递数据
                                 )

        # 实现翻页操作
        for page in range(2, 4):
            next_url = f"http://wz.sun0769.com/political/index/politicsNewest?id=1&page={page}"
            yield scrapy.Request(next_url,
                                 callback=self.parse
                                 )

    def parse_detail_url(self,response):
        """处理详情页的数据"""
        item=response.meta["item"]  # 取出数据

        # 注意：这样获得的内容是混杂的，包含了很多的占位符等垃圾符号
        item["content"]=response.xpath('//div[@class="details-box"]/pre/text()').extract_first()

        # 注意：图片和视频可能不止一个，也可能没有
        item["img"]=response.xpath('//div[@class="clear details-img-list Picture-img"]/img/@src').extract()
        item["video"]=response.xpath('//div[@class="vcp-player"]/video/@src').extract()

        yield item
```



#### **2.如何在其他文件中使用settings属性**

> *以pipelines.py为例*
>
> [![ ](https://www.ipicbed.com/images/2021/04/30/5.md.png)](https://www.ipicbed.com/image/nf3N)


```python
# -*- coding: utf-8 -*-

# Define your item pipelines here
#
# Don't forget to add your pipeline to the ITEM_PIPELINES setting
# See: https://docs.scrapy.org/en/latest/topics/item-pipeline.html

import logging
# from .settings import MONGO_HOST

logger=logging.getLogger(__name__)

class SunPipeline:
    def process_item(self, item, spider):
        # spider.settings
        # spider.settings就是self.settings
        """
        spider.settings["MONGO_HOST"]
        spider.settings.get("MONGO_HOST","指定的MONGO_HOST的值")
        """

        item["content"]=self.process_content(item["content"])
        # logger.warning(item)
        return item

    def process_content(self,content):
        """处理item中的字符串"""
        new_content=content.replace("\r\n","").replace("\xa0","")
        return new_content
```

**scrapy深入之pipeline的使用**


```python
class JsonWriterPipeline(object):
    def open(self,spider):
        """在爬虫开启的时候执行，仅执行一次"""
        self.file=open(spider.setting.get("SAVE_FILE","./temp.json"),"w")
    
    def close(self,spider):
        """在爬虫关闭的时候执行，仅执行一次"""
        self.file.close()
    
    def process_item(self,item,spider):
        line=json.dumps(dict(item))+"\n"  # 将字典转化为json字符串
        self.file.write(line)
        return item  # 如果不return的情况下，另一个权重较低的pipeline就不会获取到该item
```



- open_spider`在爬虫开启的时候执行一次
- `close_spider`在爬虫关闭的时候执行一次

**好处1：数据库的处理：**

- 建立连接的过程就可以放在`open_spider`中
- 断开连接的过程就放在`close_spider`中

**好处2：在爬虫开始时为爬虫添加一些属性**

1. 在爬虫开始的时候为爬虫添加了一个新的属性`hello`，值为`"world"`

   位置：`pipelines.SunPipeline`

[![ ](https://www.ipicbed.com/images/2021/04/30/6.md.png)](https://www.ipicbed.com/image/nQfu)

```python
def open_spider(self,spider):
        spider.hello="world"  # 为spider创建一个新的属性叫做"hello"，值为"world"
```

2. 在爬虫中调用这个属性

[![ ](https://www.ipicbed.com/images/2021/04/30/7.md.png)](https://www.ipicbed.com/image/npoT)

注意：现在我们至是想要看见`scrapy`在爬虫启动的时候是否执行了open_spider这个方法，不需要爬取网站的信息，所以我们修改爬取的范围，即是 `allowed_domains`

2. 启动我们的爬虫程序

`scrapy crawl sun`

[![ ](https://www.ipicbed.com/images/2021/04/30/8.md.png)](https://www.ipicbed.com/image/nq1b)

发现确实是输出了`hello`属性对应的值，说明我们的爬虫在开始的时候的确是调用了open_spider这个函数



## **pipeline中对数据库的处理**

```python
# 对数据库进行处理
from pymongo import MongoClient


class SunPipeline:
    def open_spider(self,spider):
        client=MongoClient()  # 实例化一下client
        self.collection=client["test"]["test"]

    def process_item(self, item, spider):
        # 调用self.collection，直接insert
        self.collection.insert(dict(item))
```

**当我们需要把文件写在本地的时候**

```python
class JsonWriterPipeline(object):
    def open(self,spider):
        """在爬虫开启的时候执行，仅执行一次"""
        self.file=open(spider.setting.get("SAVE_FILE","./temp.json"),"w")
    
    def close(self,spider):
        """在爬虫关闭的时候执行，仅执行一次"""
        self.file.close()
    
    def process_item(self,item,spider):
        line=json.dumps(dict(item))+"\n"  # 将字典转化为json字符串
        self.file.write(line)
        return item  # 如果不return的情况下，另一个权重较低的pipeline就不会获取到该item
```

**思路：**

- 在`open_spider`中打开一次
- 在`close_spider`中关闭一次
- 中间不停地往文件中写入数据

这种方式是可以的，但是会有问题

**问题：**

如果我们的爬虫在爬取网站的时候爬取到第3页的时候程序突然报错了，然后爬虫终止执行，这时候，前2页的数据能够保存在我们的文件中吗？

**不会**

**为什么？**

因为我们的文件打开之后写入数据只有在close之后数据才会写进去，如果我们不close的话，数据是不会写入的。