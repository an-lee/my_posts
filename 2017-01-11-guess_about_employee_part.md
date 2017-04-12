---
title: 应聘者部分的思路猜想
date: 2017-01-11
categories: fullstack
tags:
  - Rails
---

「Rails 实战：招聘网站」的应聘者部分，根据[教程的提示](https://fullstack.xinshengdaxue.com/posts/367)，还是没理出清晰的思路，先整理出来，再看解答。

## 提示1:上传简历的网址

- /jobs/1/resumes/new
- 提示：可以参考第三课的练习

**我的理解：**

参考第三课的练习，这里的「招聘职位 Jobs」和「简历 Resume」的关系，就像在Rails 101中「群组 Groups」和「文章 Posts」之间的关系一样。在Rails 101中，建立 Post 的 model 在，在 `rootes.rb` 里增加

```ruby
resources :groups do
  resources :posts
end
```

类似的，招聘网站应该先新建一个 `resume` 的 model，然后修改 `routes.rb`

```ruby
resources :jobs do
  resources :resumes
end
```

这样的就可以得到 `/jobs/1/resumes/new` 的网址了。

## 提示2:用户必须要登入才能提交履历

- devise
- 提示：可以参考第三课练习

**我的理解：**

参考第三课的练习，只要在 `resume` 这个 model 里增加：

```
before_filter :authenticate_user!, :only => [:new, :create]
```

## 提示3:上传空履历必须要被滤掉

- validation :attachment, presence:true

**我的理解：**

这句语句也应该放在 `resume` 这个 model 里。

## 提示4:上传履历

- Google 关键字： carrierwave pdf
- carrierwave （上传档案的 gem）
- 在 Job 这个 model 实作 attachment uploader

**我的理解：**

经过 Google，在[这里](https://richonrails.com/articles/allowing-file-uploads-with-carrierwave)找到了 carrierwave 的用法。

在以上 carrierwave 教程里，是新建一个 model `resume`，在 `resume` 里上传简历。而全栈营的教程要求，简历要放在 Job 这个 model 的 attachment 栏位。**这也是我最不理解的地方**。

如果用 Rails 101 的思路来理解，resume 和 job 的关系，就是 post 和 group 的关系的话，那么上传的附件应该属于 resume 的内容之一，而 resume 又是隶属于 job 之下。

如果按照全栈营的要求，把在 jobs 这个 model 下新建一个栏位 attachment，然后把上传的简历放在这里，那么 resume 这个 model 真正用处是什么呢？

## 整理网站的架构

先来把目前网站的整体架构整理一下看。

目前只有一个model，就是 `job`。有两个 controller，分别是`jobs_controller` 和 ` admin/jobs_controller`，然后有一个用户系统，`user`。

### model: job

`job` 这个model目前的栏位包括

| 栏位                | 说明                      |
| :---------------- | :---------------------- |
| :job_id           | 职位编号，整数型，:integer(默认自带) |
| :title            | 职位名称，字符型，:string        |
| :description      | 职位描述，文本型，:text          |
| :wage_lower_bound | 薪资下限，整数型，:integer       |
| :wage_upper_bound | 薪资上限，整数型，:integer       |
| :contact_email    | 联系方法，字符型，:string        |
| :is_hidden        | 是否隐藏，布尔型，:boolean       |

按照要求，需要增加一个栏位，attachment，就变成这样

| 栏位                | 说明                      |
| ----------------- | ----------------------- |
| :job_id           | 职位编号，整数型，:integer(默认自带) |
| :title            | 职位名称，字符型，:string        |
| :description      | 职位描述，文本型，:text          |
| :wage_lower_bound | 薪资下限，整数型，:integer       |
| :wage_upper_bound | 薪资上限，整数型，:integer       |
| :contact_email    | 联系方法，字符型，:string        |
| :is_hidden        | 是否隐藏，布尔型，:boolean       |
| **:attachment**   | **上传简历，字符型，:string**    |

### model: user

用户系统 `user` 是用 devise 生成的，本质上它也是一个 model，目前的栏位应该至少包括

| 栏位        | 说明                        |
| --------- | ------------------------- |
| :user_id  | 用户编号，整数型，:integer(默认自带)   |
| :is_admin | 是否管理员，布尔型，:boolean，用作管理权限 |

### model: resume

现在要实现新增上传简历的功能，需要新增一个model，叫做 `resume`，因为教程要求是把简历的附件放在 `job` 这个 model 里，那么 `resume` 的栏位如何设计呢？大概可以这样

| 栏位           | 说明                      |
| ------------ | ----------------------- |
| :resume_id   | 简历编号，整数型，:integer(默认自带) |
| :description | 简历说明，文本型，:text          |

### model: relationship

对于应聘者，除了搜索到理想职位，进行投递简历之外，最好还能查看自己已经投了哪些简历，网址是 `/user/2/resumes/index` ，甚至可以修改， `/user/2/resumes/edit`。所以需要一个 user/resume 的 CRUD。

管理员在接收简历以后，查看各个应聘者的简历时，需要知道哪份简历是应聘哪个职位的，所以最好把 `job` 、 `resume` 和 `user` 这三者联系起来。

这三者的关系大概是这样的

- 每个应聘者可以向多个职位投简历，所以 user has_many :resumes
- 每个职位会有多个应聘者上传简历，所以 job has_many :resumes
- 每份简历既隶属应聘者，也隶属职位，所以 resume belongs_to :jobs & belongs_to :user

建立的 `job_resume_user_relationship` 这个model，栏位包括

| 栏位         | 说明                    |
| ---------- | --------------------- |
| :id        | 编号，整数型，:integer(默认自带) |
| :job_id    | 职位编号，整数型，:integer     |
| :user_id   | 应聘者编号，整数型，:integer    |
| :resume_id | 简历编号，整数型，:integer     |

通过这个表格，在应聘者界面下捞出自己所投出的所有简历，在管理者界面下也可以捞出某个职位下所收到的所以简历了。

## 疑惑

始终想不明白，为什么要把 `attachment` 这个栏位放在 `job` 这个 model 下，既然已经新增一个 `resume` 了，为什么不直接放在 `resume` 下呢？然后建立一个关系表连接起来就可以了。

好吧，现在来看看教程的思路。

----

# update:

## 教程的解答思路

通看了一遍教程的解答，发现 attachment 根本就不是放在 `job` 这个 model 下的，而是像我说的，就是在 `resume` 下。为什么前面的提示又说放在 `job` 下呢？是故意的吗？-_-!!!

再来检查自己之前的思路，有个几个漏洞。

首先是把 `job` 、 `resume` 和 `user` 这三者的关系搞复杂了。

对于 `resume` 来说，某一份简历与职位 `job` 和应聘者 `user` 的关系都是一对一的关系，就是说，某一份简历都对应着唯一的一个职位 `job_id`，还对应着唯一的一个应聘者 `user_id`，所以这一重关系不必放在 `relationship` 这个 model 来处理。直接在 `resume` 这个 model 下增加两个栏位就可以了，所以 `resume` 的栏位设置改成这样

| 栏位           | 说明                      |
| ------------ | ----------------------- |
| :resume_id   | 简历编号，整数型，:integer(默认自带) |
| :description | 简历说明，文本型，:text          |
| :user_id     | 应聘者编号，整数型，:integer      |
| :job_id      | 职位编号，整数型，:integer       |

然后，

- job has_many :resumes
- user has_many :resumes
- resume belongs_to job
- resume belongs_to user

这样的话，用 `User.resumes` 是不是就可以捞出某个应聘者的投出的所有简历了呢？用 `Job.resumes` 也可以捞出某个职位所收到的所有简历了呢？

到这一步，应该就可以实现加分题里的功能了

- Admin 在后台可以看到每个职缺有多少使用者投递履历
- Admin 在后台可以看到每个职缺里面的投递履历内容，并且可以下载附件

如果进一步，想要建立于职位和应聘者之间的关系，应该是这样的：

- 每个职位可以接收多个应聘者投递简历
- 每个应聘者可以投递多个职位

所以 `job` 和 `user` 这两个 model 的关系是**多对多**，像 `resume` 这样简单处理是无法理清其中关系的，所以需要额外创建一个新的关系表来记录这两者的关系，因此新建一个 model，叫做 `job_user_relationship`，其中的栏位是这样的

| 栏位       | 说明                    |
| :------- | --------------------- |
| :id      | 编号，整数型，:integer(默认自带) |
| :job_id  | 职位编号，整数型，:integer     |
| :user_id | 应聘者编号，整数型，:integer    |

通过这个关系表，就可以捞出包含任意一个 `:user_id` 下的所有 `:job_id` ，也能捞出任意一个 `:job_id` 下的所有 `:user_id`，这样的话，就能看到每个职位下面都有哪些人投递了简历。不过这个功能好像不太需要。

好了，思路理得差不多了，明天早上开始实作。
