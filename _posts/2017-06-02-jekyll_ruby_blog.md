---
layout: post
title: jekyll搭建自己的静态blog
date: 2017-06-02
tags: jekyll 
description: "jekyll搭建自己的静态blog"
---

端午节，花了2天时间学习了下ruby + jekyll + github pages，踩过不少坑，记录下学习笔记。

ruby建议是在Linux环境下安装的，但是工作电脑是windows比较常见，下面记录下在windows 7 64bit版本下安装ruby + jekyll + ruby devkit的经历，进过坑，不过都爬出来了，把爬坑经历记录下方便后来者。

# 搭建环境：  windows 7 64bit电脑举例

### 步骤一：下载ruby x64、ruby devkit x64、Python 2.7 x64，ruby建议用2.3.3，亲测2.4.1有插件不支持，2.4.1才出不久，插件未经考验，python不强求2.7版本，只是个人习惯用这个版本了，ruby devkit可以用最新版本，下载地址：https://rubyinstaller.org/downloads/
![](/images/posts/jekyll/ruby-download.jpg)     
### 步骤二：下载python2.7，下载地址：https://www.python.org/downloads/release/python-2713/
![](/images/posts/jekyll/python-download.jpg)
### 步骤三：下载ruby SSL证书文件，下载地址：http://curl.haxx.se/ca/cacert.pem，该证书文件待用。

### 步骤四：下载Git x64安装包，下载地址：https://git-scm.com/download/win，下载windows setup即可。
![](/images/posts/jekyll/Git-install.jpg)    
## 下载完成后，开始安装并配置环境变量，虽然安装选项中会有是否配置环境变量提示，但是仍然是习惯自己配置环境变量。

### 1、安装python，D:\新建PYTHON_HOME，把python27安装到D:\PYTHON_HOME\Python27下，具体过程不一一详细讲解，需要excuteInstall功能

### 2、安装ruby dev kit，D:\新建RubyDevKit，把DevKit解压到D:\RubyDevKit\下，具体操作不一一详细讲解，因windows环境没有linux命令，所以需要这个工具包增加操作命令。

### 3、安装ruby，在上一环节的D:\RubyDevKit\新建Ruby目录，把ruby安装到这个目录下。
```
安装完ruby dev  kit和ruby后，需要执行如下安装操作：
cd D:\RubyDevKit
创建config.yml配置文件，初始化init，
编辑config.yml添加上ruby的安装目录，然后review添加结果，
最后执行install，没报错表示配置成功。
```
![](/images/posts/jekyll/dk.rb.jpg)
![](/images/posts/jekyll/config1.jpg)     
### 4、放置ruby SSL证书文件到D:\RubyDevKit\Ruby\ Ruby23-x64\bin目录下，最后开始设置环境变量
![](/images/posts/jekyll/env-set.jpg)
![](/images/posts/jekyll/env-set3.jpg)
### 5、安装Git，直接保持默认值，一路安装完成，注意设置环境变量时要勾选设置Path。
![](/images/posts/jekyll/Git-install2.jpg)               
## 安装完成后，开始下一步配置环境变量：
```
系统 - 高级系统设置 - 环境变量，
新增RUBY_HOME，值为D:\RubyDevKit\Ruby\Ruby23-x64，
继续新增PYTHON_HOME，值为：D:\PYTHON_HOME\Python27，
继续新增SSL_CERT_FILE，值为D:\RubyDevKit\Ruby\ Ruby23-x64\bin\cacert.pem。
```
![](/images/posts/jekyll/env-set2.jpg)
## 开始校验下安装和环境变量配置是否正确：
![](/images/posts/jekyll/env-check.jpg)  
校验无误后，开始进行jekyll安装，
执行命令：
安装bundler
```
>gem install bundler
```
安装jekyll
```
>gem install jekyll
```
安装完成后，可以创建自己的blog目录 -这里以test名称举例截图
```
>jekyll new myBlog
>cd myBlog
>jekyll s
````
![](/images/posts/jekyll/new.jpg)

## 看到如下信息表示一个初步的静态blog系统搭建好
![](/images/posts/jekyll/jekyll-start.jpg)

## 中途可能会碰到4000端口被占用情况，可以cmd进去后执行netstat -ano找到占用4000端口的pid，然后进系统进程找到对应的pid，看是哪个服务进程，可以关闭，比如我的电脑因为安装了Foxit pdf，占用了4000，可以关闭这个无用的Foxit service。
![](/images/posts/jekyll/port-check-pid.jpg)
![](/images/posts/jekyll/pid.jpg)
![](/images/posts/jekyll/service-close.jpg)
## 如果没碰到4000端口占用问题，可以不理会上面的端口冲突关闭其他4000占用进程步骤，开始进一步的完善jekyll，先查看启动的服务界面。在浏览器输入http://127.0.0.1:4000，可以看到如下界面，表示jekyll服务初步成功
![](/images/posts/jekyll/127.jpg)

## 这个只是demo，jekyll如果只是这么点功能，那就不推荐了，jekyll的好处就是模板多，我们可以去github上找更多精美的模板，进行二次开发。模板地址：http://jekyllthemes.org/，如下图，有很多精美模板可供参考：
![](/images/posts/jekyll/theme.jpg)

### 还可以下载我的个人https://github.com/anTtutu/anTtutu.github.io，我的这个也是在别人的基础上修改而来，所以下载模板进行二次开发最便捷的手段，省去美化的时间。

那么，使用他人的模板，插件库可能不一致，导致的问题有很多

### 首先，使用git命令下载模板库后，进入对方的模板库目录
```
>git clone https://github.com/anTtutu/anTtutu.github.io -- 截图以anttu工程名为例
>cd anTtutu.github.io

安装模板依赖的插件
>bundle install 
更新模板依赖的插件
>bundle update 
    
下载好模板和插件后，可以切换到模板目录再次jekyll s启动模板的工程。
>cd F:\testWorksapce\anttu   
>jekyll s   
```
![](/images/posts/jekyll/clone.jpg)
![](/images/posts/jekyll/bundle-install.jpg)
![](/images/posts/jekyll/bundle-update.jpg)

### 检查如同demo  myBlog那个启动成功没错误提示表示OK了。可以愉快的去修改、试用模板了。

### 模板的语法是用ruby + markdown写的，这点对于有开发基础的人来说不是问题。



# Tips：

中间会碰到问题gem安装或者bundle install提示如下信息：
```
C:\Users\test>gem install watir
ERROR:  Error installing watir:
        The 'ffi' native gem requires installed build tools.

Please update your PATH to include build tools or download the DevKit
from 'http://rubyinstaller.org/downloads' and follow the instructions

解决步骤和命令如下：修复RubyDevKit的dk.rb初始化和安装

D:\RubyDevKit>ruby dk.rb init
[INFO] found RubyInstaller v1.9.3 at C:/Ruby193

Initialization complete! Please review and modify the auto-ge
'config.yml' file to ensure it contains the root directories
of the installed Rubies you want enhanced by the DevKit.

D:\RubyDevKit>ruby dk.rb install
[INFO] Updating convenience notice gem override for 'C:/Ruby1
[INFO] Installing 'C:/Ruby193/lib/ruby/site_ruby/devkit.rb'

D:\RubyDevKit>gem install rdiscount --platform=ruby
Fetching: rdiscount-1.6.8.gem (100%)
Temporarily enhancing PATH to include DevKit...
Building native extensions.  This could take a while...
Successfully installed rdiscount-1.6.8
1 gem installed
Installing ri documentation for rdiscount-1.6.8...
Installing RDoc documentation for rdiscount-1.6.8...
```