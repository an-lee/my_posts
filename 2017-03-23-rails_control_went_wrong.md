---
title: rails control 出错
date: 2017-03-23 09:00
categories: fullstack
tags:
  - bug
  - Rails
---

macOS 版本 10.12.3，在 Gemfile 中已经把 spring 和 spring-wather-listen 这两个 gem 注释掉。在运行 `rails c` 时报错，如下

```
/Users/An/.rvm/rubies/ruby-2.3.1/lib/ruby/2.3.0/irb/completion.rb:10:in `require': dlopen(/Users/An/.rvm/rubies/ruby-2.3.1/lib/ruby/2.3.0/x86_64-darwin15/readline.bundle, 9): Library not loaded: /usr/local/opt/readline/lib/libreadline.6.dylib (LoadError)
  Referenced from: /Users/An/.rvm/rubies/ruby-2.3.1/lib/ruby/2.3.0/x86_64-darwin15/readline.bundle
  Reason: image not found - /Users/An/.rvm/rubies/ruby-2.3.1/lib/ruby/2.3.0/x86_64-darwin15/readline.bundle
	from /Users/An/.rvm/rubies/ruby-2.3.1/lib/ruby/2.3.0/irb/completion.rb:10:in `<top (required)>'
	from /Users/An/.rvm/gems/ruby-2.3.1/gems/railties-5.0.2/lib/rails/commands/console.rb:3:in `require'
	from /Users/An/.rvm/gems/ruby-2.3.1/gems/railties-5.0.2/lib/rails/commands/console.rb:3:in `<top (required)>'
	from /Users/An/.rvm/gems/ruby-2.3.1/gems/railties-5.0.2/lib/rails/commands/commands_tasks.rb:138:in `require'
	from /Users/An/.rvm/gems/ruby-2.3.1/gems/railties-5.0.2/lib/rails/commands/commands_tasks.rb:138:in `require_command!'
	from /Users/An/.rvm/gems/ruby-2.3.1/gems/railties-5.0.2/lib/rails/commands/commands_tasks.rb:68:in `console'
	from /Users/An/.rvm/gems/ruby-2.3.1/gems/railties-5.0.2/lib/rails/commands/commands_tasks.rb:49:in `run_command!'
	from /Users/An/.rvm/gems/ruby-2.3.1/gems/railties-5.0.2/lib/rails/commands.rb:18:in `<top (required)>'
	from bin/rails:9:in `require'
	from bin/rails:9:in `<main>'
```

在我输入 `irb` 时，也有同样类似的报错

```
Readline was unable to be required, if you need completion or history install readline then reinstall the ruby.
You may follow 'rvm notes' for dependencies and/or read the docs page https://rvm.io/packages/readline/ . Be sure you 'rvm remove X ; rvm install X' to re-compile your ruby with readline support after obtaining the readline libraries.
```

按照报错提示，重装 ruby。

```
rvm uninstall 2.3.1
rvm install 2.3.1
```

重新 `bundle install` ，因为连接不顺，把 gemfile source 改了

```
gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
```

又出现下面的错误

```
Could not load OpenSSL.
You must recompile Ruby with OpenSSL support or change the sources in your
Gemfile from 'https' to 'http'. Instructions for compiling with OpenSSL using
RVM are available at rvm.io/packages/openssl.
```

把 gem source 改回来，还是一样。根据提示，到 [rvm.io/packages/openssl](rvm.io/packages/openssl) ，按照提示安装了 RVM packages

```
rvm autolibs rvm_pkg
```

重新 `bundle install` ，还是不行。

重装 rvm。然后再重装 ruby，还是错误

```
Error running 'env CFLAGS=-O3 -I/Users/An/.rvm/usr/include LDFLAGS=-L/Users/An/.rvm/usr/lib ./configure --prefix=/Users/An/.rvm/rubies/ruby-2.3.1  --with-opt-dir=/Users/An/.rvm/usr:/usr:/usr/local/Cellar/libyaml/0.1.6_1 --disable-install-doc --enable-shared',
```

崩溃。

---

决定重新安装 rvm 和 ruby。

先删除 rvm

```
rvm implode
gem uninstall rvm
```

查看主目录下所有文件

```
ls -a ~
```

如果 `.rvm` 和 `.rvmrc` 还在，将其删除。

```
rm -rf .rvm
rm -rf .rvmrc
```

然后重新安装 rvm

```
\curl -sSL https://get.rvm.io | bash -s stable
```

安装完之后，再让 rvm 生效

```
source ~/.rvm/scripts/rvm
```

然后安装套件 libxml2

```
brew install libxml2
```

然后再安装 ruby

```
rvm install 2.3.1
```

安装 rails

```
gem install rails -v 5.0.0
```

进入专案 `bundle install`。谢天谢地，终于成功了！

再 `rails c` ，也成功了！

# 总结

由于不明的原因，导致 rvm、ruby、rails 出现了错误，解决办法是，全部删除，全部重装。