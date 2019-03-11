---
layout: post
title: Cygwin安装
date: 2017-09-12
tags: cygwin 
description: "Cygwin安装"
---

#### cygwin是windows平台上运行的unix模拟环境，它对于学习unix/linux操作环境，或者从unix到windows的应用程序移植，或者进行某些特殊的开发工作，尤其是使用gnu工具集在windows上进行嵌入式系统开发，非常有用。

#### cygwin相对安装虚拟机、vps是轻量级的，因此在条件不足情况下，练习、学习shell等都是很好的选择。

### 一、Cygwin的安装
1. 下载Cygwin，这个可以到这里下载 ，至于使用32位的还是64位的版本可以根据自己的系统而定，打开下载好的setup-x86.exe（以64bit-windws系统为例） 。

2. 我这里选择的是2.876（64位）版本
![](/images/posts/cygwin/install1.jpg)

3. 第一个选项是在线安装，第二个选项是只下载不安装（然后手动安装），第三个指你已经下载了安装包，通过已经下载的本地安装包安装，若是第一次安装，选中第一个即可（默认），【下一步】
![](/images/posts/cygwin/install2.jpg)

4. 指定unix系统的根目录，以及限定那些用户可以访问这个目录。安装目录别为中文名,以免出错,接入网站如果不行,提示错误,那就重新来换一个接入网站，建议这个路径要指定在空间比较大的硬盘，在后面的开发中，这个目录是工作目录，随着积累会越来越大
![](/images/posts/cygwin/install3.jpg)

5. 指定包的下载目录，安装完成以后可删除，下面的单选框默认即可，【下一步】
![](/images/posts/cygwin/install4.jpg)

6. 选择连接方式，如果用的是外网，选择第一个（默认）即可，如果使用的是公司网或者其他需要代理的内网，记得使用相应的代理，一般如果默认浏览器有设代理，选择第二个就好，如果默认浏览器没有设代理，则使用第三项自己配置代理，【下一步】
![](/images/posts/cygwin/install5.jpg)

7. 选择一个镜像站点，任选一个即可，按Ctrl键可选中多个。这里需要注意一下，对于国内的用户，强烈建议使用国内的镜像，这样可以在后面的下载过程中有更快的速度，比如我这里使用的是http://mirrors.163.com/cygwin/，【下一步】
![](/images/posts/cygwin/install6.jpg)

8. 这一步很关键，选择要下载和安装的包，根据你的需要选择包，选的包越多所需的下载时间越长，单击【View】可以在分类、全部、已选之间循环切换，点击每一类前面的加号可以展开，要选中每一个包，只需单击每一行前边像循环的那个图标，会在版本号和Skip之间切换，选一个最新的版本号即可，下边的那个复选框默认即可。
```
为了后面的操作，我们有必要在这里选择一些必要的包进行安装：
(1) curl
(2) git* (git, git-gui, gitk)；
(3) libreadline7, libiconv2
(4) vim, ctags
(5) python
(6) lynx
(7) wget, tar, gawk, bzip2
```
当然其中有一些是已经就默认勾选的，在选择的时候只要在search里面输入对应名称，它就会自动过滤出你要安装的包了，然后将循环Skip切换成你需要安装的版本就好了，一定要记得在搜索的时候不需要按Enter, 不然就直接跳到下一步了。
![](/images/posts/cygwin/install7.jpg)

9. 选好后【下一步】下图，会显示你选择的安装包：
![](/images/posts/cygwin/install8.jpg)
单击下一步开始安装，最后会让你让你选择是否生成快捷方式，然后OK了！
以后要安装新的安装包，或是更新，还是通过这个过程，运行setup.exe选择安装包即可。

### 二、Cygwin的配置
打开Cygwin终端,右击打开 Options...选项
Text可以设置字体的一些属性,如大小、编码,Locale 选择C, Character set 选择 UTF-8,可以避免中文显示乱码

### 三、安装apt-cyg
这时就可以打开Cygwin64 Terminal，开始像正常linux终端一样在windows下工作了，但是现在还是比较粗糙，缺少很多我们必要的比如一些依赖库和命令，而且我们比较熟悉的apt-get也没有，在Cygwin中，我们使用apt-cyg来下载和管理安装包，下面我们来介绍怎么安装它：
```
git clone https://github.com/transcode-open/apt-cyg.git
cd apt-cyg
install apt-cyg /bin

#检测是否安装成功
apt-cyg --help

#出现如下帮助信息，表示安装成功
$ apt-cyg --help
NAME
  apt-cyg - package manager utility

SYNOPSIS
  apt-cyg [operation] [options] [targets]

DESCRIPTION
  apt-cyg is a package management utility that tracks installed packages on a
  Cygwin system. Invoking apt-cyg involves specifying an operation with any
  potential options and targets to operate on. A target is usually a package
  name, file name, URL, or a search string. Targets can be provided as command
  line arguments.
  ......
  ......
  ......

#出现其他提示，可能是安装出现故障了，请删除apt-cyg包后重新下载再安装配置。  
```
ps：win10可能会在执行install出现个错误提示
```
/usr/bin/apt-cyg:行25: $'\r': 未找到命令
/usr/bin/apt-cyg:行121: 未预期的符号 `$'{\r'' 附近有语法错误
'usr/bin/apt-cyg:行121: `function wget {
由于windows上的换行符和linux上不同，因此notepad++打开apt-cyg文件，替换所有的\r为空，保存即可使用apt-cyg了
```

### 四、安装其他组件
就可以随便安装相应的软件了
```
apt-cyg install man cygwin-doc 
apt-cyg install vim screen wget subversion openssh pwgen gzip bzip2 curl rsync bash-completion lftp nc tree p7zip connect-proxy util-linux bind-utils inetutils
#有了apt-cyg这个神器后，后续安装就可以类似yum、apt-get那么方便了
```
