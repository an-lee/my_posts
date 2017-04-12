---
title: Capistrano部署bug
date: 2017-03-03
categories: fullstack
tags:
  - Rails
typora-copy-images-to: ipic
---

使用 Capistrano 部署项目时

```
cap production deploy:check
```

报错

```
rvm 1.29.1 (master) by Michal Papis, Piotr Kuczynski, Wayne E. Seguin [https://rvm.io/]
ruby-2.3.1
ruby 2.3.1p112 (2016-04-26 revision 54768) [x86_64-linux]
00:00 git:wrapper
      01 mkdir -p /tmp
    ✔ 01 apps@139.162.111.185 0.136s
      Uploading /tmp/git-ssh-blog-production-anli.sh 100.0%
      02 chmod 700 /tmp/git-ssh-blog-production-anli.sh
    ✔ 02 apps@139.162.111.185 0.137s
00:00 git:check
      01 git ls-remote git@github.com:an-lee/blog.git HEAD
      01 Permission denied (publickey).
      01 fatal: Could not read from remote repository.
      01
      01 Please make sure you have the correct access rights
      01 and the repository exists.
(Backtrace restricted to imported tasks)
cap aborted!
SSHKit::Runner::ExecuteError: Exception while executing as apps@xxx.xxx.xxx.xxx: git exit status: 128
git stdout: Nothing written
git stderr: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

SSHKit::Command::Failed: git exit status: 128
git stdout: Nothing written
git stderr: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

Tasks: TOP => deploy:check => git:check
(See full trace by running task with --trace)
```



![Screen Shot 2017-03-03 at 6.06.51 AM](http://okgqgpbx3.bkt.clouddn.com/blog/2017-03-02-220835.png)

google 之后，根据 [stackflow](http://stackoverflow.com/questions/28375506/sshkitrunnerexecuteerror) 这里的解法，

```
eval 'ssh-agent'
ssh-add ~/.ssh/id_rsa
```

重新运行

![Screen Shot 2017-03-03 at 6.10.39 AM](http://okgqgpbx3.bkt.clouddn.com/blog/2017-03-02-221058.png)

Done!