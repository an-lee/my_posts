---
title: 服务器sudo失败
date: 2017-03-21 06:00
categories: fullstack
tags:
  - ubuntu
  - 部署
---

在阿里云的服务器上部署 Rails 程序，按照 nodejs 时

```
sudo apt-get install npm
```

出现错误

```
sudo: unable to resolve host iZwz9d81yfrglsq67nvjmkZ
[sudo] password for apps:
E: Could not get lock /var/lib/dpkg/lock - open (11: Resource temporarily unavailable)
E: Unable to lock the administration directory (/var/lib/dpkg/), is another process using it?
```

按照 PostgreSQL

```
sudo apt-get install postgresql postgresql-contrib libpq-dev
```

同样出现错误

```
sudo: unable to resolve host iZwz9d81yfrglsq67nvjmkZ
E: Could not get lock /var/lib/dpkg/lock - open (11: Resource temporarily unavailable)
E: Unable to lock the administration directory (/var/lib/dpkg/), is another process using it?
```

![Screen Shot 2017-03-21 at 6.04.31 AM](http://okgqgpbx3.bkt.clouddn.com/blog/2017-03-20-222759.jpg)

搜索一通，因为是 Ubuntu 环境，解法都看不太懂。但是看懂了其中一条，就是重启！

于是，我在阿里云的控制后台，点击重启。

然后……问题解决。