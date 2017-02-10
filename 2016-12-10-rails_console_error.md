---
title: 'rails_console出错'
date: 2016-12-10 08:34
categories: [fullstack]
tags:
  - RoR
---
教程步骤：

> Rails 第二课：初级练习
> [3-6 设定首页](https://fullstack.xinshengdaxue.com/posts/45)

按照教材，terminal 输入
```ruby
rails console
```
预期结果是

```
> rails console
2.3.1 :001 > app.topics_path
=> "/topics"
2.3.1 :001 > app.topics_url
=> "http://www.example.com/topics"
```

我的结果是

```
anli@Ans-Mac-mini ⮀ ~/railsbridge/suggestotron ⮀ ⭠ master± ⮀ rails console
Running via Spring preloader in process 1566
/Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:293:in `require': dlopen(/Users/anli/.rvm/rubies/ruby-2.3.1/lib/ruby/2.3.0/x86_64-darwin15/readline.bundle, 9): Library not loaded: /usr/local/opt/readline/lib/libreadline.6.dylib (LoadError)
  Referenced from: /Users/anli/.rvm/rubies/ruby-2.3.1/lib/ruby/2.3.0/x86_64-darwin15/readline.bundle
  Reason: image not found - /Users/anli/.rvm/rubies/ruby-2.3.1/lib/ruby/2.3.0/x86_64-darwin15/readline.bundle
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:293:in `block in require'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:259:in `load_dependency'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:293:in `require'
	from /Users/anli/.rvm/rubies/ruby-2.3.1/lib/ruby/2.3.0/irb/completion.rb:10:in `<top (required)>'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:293:in `require'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:293:in `block in require'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:259:in `load_dependency'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:293:in `require'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/railties-5.0.0.1/lib/rails/commands/console.rb:3:in `<top (required)>'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:293:in `require'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:293:in `block in require'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:259:in `load_dependency'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:293:in `require'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/railties-5.0.0.1/lib/rails/commands/commands_tasks.rb:138:in `require_command!'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/railties-5.0.0.1/lib/rails/commands/commands_tasks.rb:68:in `console'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/railties-5.0.0.1/lib/rails/commands/commands_tasks.rb:49:in `run_command!'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/railties-5.0.0.1/lib/rails/commands.rb:18:in `<top (required)>'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:293:in `require'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:293:in `block in require'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:259:in `load_dependency'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:293:in `require'
	from /Users/anli/railsbridge/suggestotron/bin/rails:9:in `<top (required)>'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:287:in `load'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:287:in `block in load'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:259:in `load_dependency'
	from /Users/anli/.rvm/gems/ruby-2.3.1/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:287:in `load'
	from /Users/anli/.rvm/rubies/ruby-2.3.1/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:55:in `require'
	from /Users/anli/.rvm/rubies/ruby-2.3.1/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:55:in `require'
	from -e:1:in `<main>'
```

反复几次，无果。教材上这部分写着「可以跳过」。
于是先**跳过**。
