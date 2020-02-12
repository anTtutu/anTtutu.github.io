# Anttu

### 使用手册

使用Jekyll搭建个人博客


### 使用条件

Jekyll 支持 Mac 、Windows、ubuntu 、Linux 操作系统                     
Jekyll 需要依赖：Ruby、bundler


#### 安装Jekyll

[Jekyll中文官方文档](http://jekyll.bootcss.com/) ， 如果你已经安装过了 Jekyll，可以忽略此处。

> $ gem install jekyll

### 安装bundler

> $ gem install bundler

#### 获取博客模板

> $ git clone https://github.com/anTtutu/anTtutu.github.io.git

或者直接[下载博客](https://github.com/anTtutu/anTtutu.github.io/archive/master.zip)   

进anTtutu.github.io/ 目录下， 开启本地服务 

> $ jekyll s

在浏览器输入 [127.0.0.1:4000](127.0.0.1:4000) ， 就可以看到博客效果了。

### Error修复

如果有错误提示，可以尝试

> $ bundle install  
> $ bundle update

### 提示

>* 如果你想使用我的模板，请把 _posts/ 目录下的文章都去掉。
>* 修改 _config.yml 文件里面的内容为你自己的个人信息。

如果在部署博客的时候发现问题，可以直接在[Issues](https://github.com/anTtutu/anTtutu.github.io/issues)里面提问。        


### 把这个博客变成你自己的博客

根据上面【提示】修改过后，在你的github里创建一个username.github.io的仓库，username指的值你的github的用户名。      
创建完成后，把我的这个模板使用git push到你的username.github.io仓库下就行了。

#### 感谢   

本博客在[leopardpan](https://github.com/leopardpan/leopardpan.github.io/)基础上修改的。  
