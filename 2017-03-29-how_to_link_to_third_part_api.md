---
title: 抓取第三方API
date: 2017-03-29 05:15
categories: fullstack
tags:
  - Rails
  - api
---

# 什么是 API

简单来理解，API 就是程序与程序之间的对话通道。

# 抓取数据

要用到的 gem 是 [rest_client](https://github.com/rest-client/rest-client)。

以聚合数据为例，一般 API 网站的用法都有示例。

在 Rails 中，建立文件 `lib/tasks/dev.rake`

```ruby
namespace :dev do
  # namespace 方法可以把 Rake 任务分组，这样可以避免一些命名冲突，也显得更有条理。
  task :fetch_city => :environment do 
  #:environment 表示执行 rake 任务时的 Rails 环境参数，默认为 development 环境，你可以通过 RAILS_ENV 参数来修改它: bundle exec rake first_student RAILS_ENV=production
    puts "Fetch city data..."
    # puts 就是打印显示的意思
    response = RestClient.get "http://v.juhe.cn/weather/citys", :params => { :key => "你申请的key放这里" }
    # rest_client 这个 gem 的用法，更多用法可以参照该 gem 的 Github 页面。注意这里抓取的数据是 JSON 格式，取决于 API 网站。
    data = JSON.parse(response.body)
    # 将抓过来的数据转成 ruby 格式，并存入 data 这个变量
    data["result"].each do |c|
      existing_city = City.find_by_juhe_id( c["id"] )
      # juhe_id 是 city 这个 model 的一个栏位，纪录聚合 API 的城市 id
      # 先确定该城市是否存在，如果不存在，则新建
      if existing_city.nil?
        City.create!( :juhe_id => c["id"], :province => c["province"],
                      :city => c["city"], :district => c["district"] )
        # 将抓到的数据写入 city 这个 model
      end
    end

    puts "Total: #{City.count} cities"
  end
end
```

然后执行 `bundle exec rake dev:fetch_city`，就可以执行上述代码，把 API 中的数据下载下来，保存到数据库中。

# 更新数据

为了在网页中更新天气的数据，需要有相应的 controller

```ruby
def update_temp
  city = City.find(params[:id])
  
  response = RestClient.get "http://v.juhe.cn/weather/index", :params => { :cityname => city.juhe_id, :key => "你申请的key放这里" }
  #请求 api 参数多了一个 :cityname ，只更新某一个城市的天气
  data = JSON.parse(response.body)
  # 抓来的 JSON 格式文件转成 ruby 格式
  city.update( :current_temp => data["result"]["sk"]["temp"] )
  # 根据聚合提供的 API 说明，实时的天气在'result'=>'sk'=>'temp'这个位置
  redirect_to cities_path
end
```

# 隐藏 API

在之前有试过用第三方的存储，API 是写到 yml 文件里的，而且不会上传到 Github。同样，其他的 API 也应该写到 yml 文件里，作为设定档。

首先在 `config/` 下面新建一个设定档，`config/juhe.yml`

```yaml
development:
  api_key: "your api key"
production:
  api_key: "your api key"
```

开发环境和部署环境可以用不同的 api。

接着在设定文档 `config/application.rb` 中新增对刚才新建设定档的引用

```ruby
JUHE_CONFIG = Rails.application.config_for(:juhe)
```

这样就得到了一个可以在 Rails 中引用的变量参数 `JUHE_CONFIG["api_key"]`，在需要使用 api 的地方，用这个变量参数替换就可以了。

需要注意的是，这些变动需要重启 rails server 才能生效。除了在目录 `app/` 下的变动，其他所有目录的修改都需要重启 rails server 才可以生效。

# 国内常用 API 服务

- [https://www.juhe.cn](https://www.juhe.cn/)
- [http://apistore.baidu.com](http://apistore.baidu.com/)
- [https://www.haoduoshuju.com](https://www.haoduoshuju.com/)
- [http://www.pm25.in/api_doc](http://www.pm25.in/api_doc)
- [https://github.com/toddmotto/public-apis](https://github.com/toddmotto/public-apis)
- [http://opendatachina.com/en/projects/](http://opendatachina.com/en/projects/)