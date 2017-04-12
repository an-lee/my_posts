---
title: 安装nokogiri 出错
date: 2017-03-26 06:30
categories: fullstack
tags:
  - install
  - Rails
typora-copy-images-to: ipic
---

mac 安装 Rails 出错，报错如下

```
ERROR:  Error installing rails:
    invalid gem: package is corrupt, exception while verifying: undefined method `size' for nil:NilClass (NoMethodError) in /Users/zhangfan/.rvm/gems/ruby-2.3.1/cache/nokogiri-1.7.1.gem
```

![屏幕快照 2017-03-26 上午6.11.33](http://okgqgpbx3.bkt.clouddn.com/blog/2017-03-25-224845.png)

尝试安装 nokogiri

```
gem install nokogori
```

类似的报错

```
ERROR:  Error installing nokogiri:
    invalid gem: package is corrupt, exception while verifying: undefined method `size' for nil:NilClass (NoMethodError) in /Users/zhangfan/.rvm/gems/ruby-2.3.1/cache/nokogiri-1.7.1.gem
```

Google 之后，在 [stackoverflow](http://stackoverflow.com/questions/28958687/nomethod-error-when-installing-rails) 找到解法。

报错原因是 cache 下的 nokogiri 坏掉了，删除它，然后重装即可。

```
rm -rf /Users/zhangfan/.rvm/gems/ruby-2.3.1/cache/nokogiri-1.7.1.gem
gem install rails
```

Done.