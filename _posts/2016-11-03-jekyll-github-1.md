---
layout: post
title:  "jekyll install and run locally"
categories: jekyll
tags: github jekyll
---

* content
{:toc}

关于jekyll安装以及使用jekyll调试已有的静态网页项目。




## Ruby&jekyll install

* **前置条件-Ruby安装**

1. [How To Install Ruby on Rails on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-on-ubuntu-12-04-lts-precise-pangolin-with-rvm)
2. 手动安装

首先，下载最新的ruby版本。[download](https://www.ruby-lang.org/en/downloads/)

然后，解压缩，进入解压缩后的文件夹，执行下述语句：
```
./configure --prefix=/usr/local/ruby_version
make & make install
```
查看ruby和gem是否成功安装以及安装的版本。
```
ruby --version
gem --version
```
系统默认安装了一个低版本的ruby，安装jekyll会报出下述错误信息：
```
Error:**********
```
删除默认的ruby:
```
sudo apt-get remove ruby
```

* **jekyll安装**
```
gem install jekyll
```

## Setting up your GitHub Pages site locally with Jekyll
1. **jekyll command**
```
jekyll s %way1
jekyll serve %way2
jekyll serve --watch%way3
```
其中命令1和2效果一样。
2. **bundler**

[Setting up your GitHub Pages site locally with Jekyll](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)


