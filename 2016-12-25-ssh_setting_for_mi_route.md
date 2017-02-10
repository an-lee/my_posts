---
title: 小米路由设置SSH
categories: fullstack
tags:
  - cheatsheet
---

家里几个电脑，N 部移动设备，每次上 Google 总要先挂 VPN，就很烦人。家里用的是小米路由器，可以直接挂 shadowsocks，稍加设置就免除很多烦恼。

## step 1 安装开发者版 ROM

1. 到[官网这里](http://www1.miwifi.com/miwifi_download.html)下载对应型号的开发者版本 ROM
2. 确保电脑与路由器连接正常
3. 打开[miwifi.com](http://miwifi.com/)，会自动跳转到路由器设置页面
4. 右上角箭头>系统升级，然后选择刚下载的 ROM 文件，确认升级


![Screen Shot 2016-12-25 at 4.31.29 PM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1242804/BdG7ep87QuO2DygmZwG7_Screen%20Shot%202016-12-25%20at%204.31.29%20PM.png)

## step 2 登录 shadowsocks

1. 到[官网这里](https://d.miwifi.com/rom/ssh)拿到 SSH 登录密码
2. 打开terminal，输入`ssh root@miwifi.com`
3. 根据提示输入刚才的 SSH 登录密码，成功后会出现这样的画面

![Screen Shot 2016-12-25 at 4.15.22 PM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1242804/cLBvDWt8SnGjOexP4Dfu_Screen%20Shot%202016-12-25%20at%204.15.22%20PM.png)

## step 3 设置 shadowsocks

1. 输入以下命令，回车
```
cd /tmp && rm -rf *.sh && wget ftp://joerv.gicp.net/miwifi/miwifi.sh && chmod +x miwifi.sh && sh ./miwifi.sh && rm -rf *.sh
```
2. 根据提示输入相关的 ssh 信息

![Screen Shot 2016-12-25 at 4.19.47 PM.png](http://user-image.logdown.io/user/22009/blog/21058/post/1242804/nON7L2zRpaFhEcFGsmxg_Screen%20Shot%202016-12-25%20at%204.19.47%20PM.png)

3. 打开[google](http://www.google.com/)试试看
4. 打开[IP 查询]看一下 IP 地址是否本地，确认已经智能区分


## 参考资料
1. [http://liangpir.com/post/2016-01-21](http://liangpir.com/post/2016-01-21)
2. [http://bbs.xiaomi.cn/t-12863925](http://bbs.xiaomi.cn/t-12863925)
