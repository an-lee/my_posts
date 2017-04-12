---
title: 利用API做一个股票查询系统
date: 2017-03-31 18:00
categories: fullstack
tag:
  - Rails
  - api
typora-copy-images-to: ipic
---

# User Story

- 查询某支股票（按照股票编号）
  - 股票名字
  - 当前价格
  - 今日开盘价
  - 昨日收盘价
  - 涨跌百分比
  - 涨跌额
  - 市盈率
  - 贝塔系数
  - 更新时间
- 查询某支股票的 K 线图

# 找到数据接口

在[聚合数据](https://www.juhe.cn/docs/api/id/21/aid/75)可以找到股票数据接口。使用之前需要注册和实名认证。

![Screen Shot 2017-03-31 at 9.20.46 PM](http://okgqgpbx3.bkt.clouddn.com/blog/2017-03-31-132234.png)

用 Postman 先测试一下。

![029B5C7D-E5C2-4BA6-8AF9-D7A742F79B64](http://okgqgpbx3.bkt.clouddn.com/blog/2017-03-31-029B5C7D-E5C2-4BA6-8AF9-D7A742F79B64.png)

进入 `irb` 测试

```
require 'rest-client'
require 'json'
response = RestClient.get "http://web.juhe.cn:8080/finance/stock/usa", :params => { :gid => "aapl", :key => "申请到的 API"}
data = JSON.parse(response.body)
```

苹果公司的数据就以 ruby 的格式储存在 data 变量中了。

```
data.keys # => ["resultcode", "reason", "result", "error_code"]
data["result"][0] # => {"data"=>{"gid"=>"aapl", "name"=>"苹果"...略}
```

# 建立 model

创建 `usstock` model，栏位包括

```ruby
class CreateUsstocks < ActiveRecord::Migration[5.0]
  def change
    create_table :usstocks do |t|
      t.string :juhe_gid
      t.string :name
      t.string :lastestpri
      t.string :openpri
      t.string :formpri
      t.string :limit
      t.string :uppic
      t.string :priearn
      t.string :beta
      t.date   :chtime
      t.timestamps
    end
  end
end
```

# 创建 task 获取数据

在`lib/tasks/` 下创建 `dev.rake`，测试获取数据

```ruby
namespace :dev do
  task :fetch_stock => :environment do
    puts "Fetch stock data..."
    response = RestClient.get "http://web.juhe.cn:8080/finance/stock/usa", :params => {:gid => "aapl", :key => "你的 API KEY"]}
    data = JSON.parse(response.body)

    data["result"].each do |c|
      existing_stock = Usstock.find_by_juhe_gid(c["data"]["gid"])
      if existing_stock.nil?
        Usstock.create!(:juhe_gid => c["data"]["gid"], :name => c["data"]["name"],
                        :lastestpri => c["data"]["lastestpri"], :openpri => c["data"]["openpri"],
                        :formpri => c["data"]["formpri"], :limit => c["data"]["limit"],
                        :uppic => c["data"]["uppic"], :priearn => c["data"]["priearn"],
                        :beta => c["data"]["beta"], :chtime => c["data"]["chtime"])
      end
      puts "#{Usstock.last.name}(#{Usstock.last.juhe_gid})'s data is fetched"
    end
  end
end

```

这里需要注意的是，`data["result"]` 虽然只有一组数据，但仍然是数组形式，`.each` 不是必须的，可以直接用 `data["result"][0]` 代替。

# 完成网站架构

创建 controller，在 `usstocks_controller` 下，

```ruby
  def create
    @usstock = Usstock.new(usstock_params)
    @usstock.save
    response = RestClient.get "http://web.juhe.cn:8080/finance/stock/usa", :params => {:gid => @usstock.juhe_gid, :key => "你的 API KEY"]}
    data = JSON.parse(response.body)
    data["result"].each do |c|
      @usstock = Usstock.update(:juhe_gid => c["data"]["gid"], :name => c["data"]["name"],
                      :lastestpri => c["data"]["lastestpri"], :openpri => c["data"]["openpri"],
                      :formpri => c["data"]["formpri"], :limit => c["data"]["limit"],
                      :uppic => c["data"]["uppic"], :priearn => c["data"]["priearn"],
                      :beta => c["data"]["beta"], :chtime => c["data"]["chtime"])
    end
    redirect_to usstock_path(@usstock)
  end

  def show
    @usstock = Usstock.find(params[:id])
  end
```

思路是

- 从查询页面传来要查询的股票编号 `:juhe_id`
- 以 `:juhe_id` 创建一条 model 数据
- 以 `:juhe_id` 去获取 API 数据
- 将获取到的数据更新至刚刚新建的 model 数据中

目前只想到这个解决办法，但是在 `create` 下用 `update` 是不是有点怪怪的？

# 成果

![](http://okgqgpbx3.bkt.clouddn.com/blog/2017-04-01-Screen%20Shot%202017-04-01%20at%209.02.30%20AM.png)

![](http://okgqgpbx3.bkt.clouddn.com/blog/2017-04-01-Screen%20Shot%202017-04-01%20at%209.02.40%20AM.png)

注：省略了以下步骤

- 安装 bootstrap
- 安装 simple_form
- `new.html.rb` 和 `show.html.rb`

详细代码见 https://github.com/an-lee/api_stock