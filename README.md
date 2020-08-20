之前用的jekyll搭建的git pages静态blog，今年因为疫情，在家边远程办公边学习go，在bilibili看golang的学习视频，其中发现[李文周](https://www.liwenzhou.com)讲解的golang学习视频不错，从头学习了基础，发现之前直接拿着数据类型和流程控制看语法有很多不足，一些基础的东西忽略了
发现李文周的blog是hugo的，速度好快，尝试了解，本地搭建开发环境，测试使用后，效果真的不错，想都不想，决定迁移hugo作为静态blog

# 一、准备本地开发环境

## 1、下载hugo
我个人使用的mac，因此决定用homebrew安装，查了下hugo的官方资料也是建议用brew命令安装，结果坑爹。。。网速也马马虎虎不是特别快也不慢，完全下载不下来啊。改用git clone，稍微好一点，但是发现依赖几十个工程，下载老是失败，只好进hugo的官方git下载二进制文件放到$GOPATH/bin  
下载地址：https://github.com/gohugoio/hugo/releases 因为我的是mac所以选择当前的最新版本0.64.1 macOS 64位版本  
windows同理，选择对应的位数版本，放到$GOPATH/bin环境变量下即可

验证下是否配置成功,以本人的mac为例，出现版本号表示配置成功
```bash
>hugo version
Hugo Static Site Generator v0.64.1-C327E75D darwin/amd64 BuildDate: 2020-02-09T20:46:36Z
```

## 2、选择主题
因为有hexo和jekyll的静态blog经验，知道主题很重要，进了hugo英文官网和查找huguo漂亮简洁的主题，找到了maupassant主题，但是查找发现git上很久没更新了，再找资料发现[飞雪无情](https://www.flysnow.org/2018/07/29/from-hexo-to-hugo.html)针对该主题进行了优化，对比后觉得优化的点我也需要，就下载优化版的主题。
```bash
>hugo new site hugo_blog
>cd hugo_blog
>git clone https://github.com/flysnow-org/maupassant-hugo /theme/maupassant
```

## 3、创建第一篇blog文章，并把jekyll下的markdown记录都复制过来
```bash
>hugo new post/first.md
>rm -rf /***/hugo_blog/content/post/first.md

>cp *.md /***/hugo_blog/content/post
```
调整下导入的markdown记录的头部参数，以前不支持的语法去掉，新增的语法添加
类似：
```bash
---
title: "改用最快的静态blog-hugo"
date: 2020-02-13T00:29:47+08:00
tags: [ "blog", "hugo" ]
categories: [ "blog", "hugo" ]
keywords: [ "blog", "hugo" ]
toc: true
---
```

## 4、配置config.toml，可以参考主题exampleSite下的示例config.toml
经过阅读官方hugo和飞雪无情的模板说明文档，给config.toml新增需要的配置参数
```bash
baseURL = "https://anTtutu.github.io/"
languageCode = "zh-CN"
defaultContentLanguage = "zh-cn"                          # en / zh-cn / ... (This field determines which i18n file to use)
title = "Anttu's Blog"
theme = "maupassant"                                      # 主题
summaryLength = 140                                        # 默认是70，显示文章摘要的长度
preserveTaxonomyNames = true                                                      # 若为 false，`Getting Started` 这样的英文标签将会被转换为 `getting-started`
enableRobotsTXT = true
enableEmoji = true
enableGitInfo = false     # use git commit log to generate lastmod record # 可根据 Git 中的提交生成最近更新记录。
disablePathToLower = true                                 # 禁止将路径全部改为小写
# GA统计分析
#googleAnalytics = "GA ID"
hasCJKLanguage = true     # has chinese/japanese/korean   # 自动检测是否包含 中文\日文\韩文
paginate = 10
copyright = ""            # default: author.name ↓        # 默认为下面配置的author.name

# 一些全局开关，你也可以在每一篇内容的 front matter 中针对单篇内容关闭或开启某些功能，在 archetypes/default.md 查看更多信息。
# Some global options, you can also close or open something in front matter for a single post, see more information from `archetypes/default.md`.
toc = true                                                                            # 是否开启目录
autoCollapseToc = true    # Auto expand and collapse toc                              # 目录自动展开/折叠
fancybox = true           # see https://github.com/fancyapps/fancybox                 # 是否启用fancybox（图片可点击）

[author]
  name = "Anttu"

[sitemap]                 # essential                     # 必需
  changefreq = "weekly"
  priority = 0.5
  filename = "sitemap.xml"

[params]
  author = "Anttu"
  subtitle = "一位Java开发者，喜欢研究技术，同时也在学习Golang和Python中，对服务器、Linux使用比较熟悉。欢迎添加技术交流QQ群：655158296"
  keywords = "golang,go语言,go语言笔记,anttu,java,博客,bash,linux笔记,python笔记,公众号,小程序"
  description = "专注于IT互联网，包括但不限于Java、Go语言(golang)、Python、Bash等"

#  googleAd = ""                                           # google广告
  localSearch = true                                      # 是否开启本地搜索

  # paginate of archives, tags and categories             # 归档、标签、分类每页显示的文章数目，建议修改为一个较大的值
  archivePaginate = 50

  # show 'xx Posts In Total' in archive page ?            # 是否在归档页显示文章的总数
  showArchiveCount = true

 # 代码高亮
[markup]
  [markup.highlight]
    style = "github"

# 分类
[menu]
  [[menu.main]]
    identifier = "archives"
    name = "归档"
    url = "/archives/"
    weight = 2

  [[menu.main]]
    identifier = "tags"
    name = "分类"
    url = "/tags/"
    weight = 3

  [[menu.main]]
    identifier = "about"
    name = "关于"
    url = "/about/"
    weight = 4

# Github Issue的utteranc评论系统
[params.utteranc]
  enable = true
  repo = "anTtutu/anTtutu.github.io"     # 存储评论的Repo，格式为 owner/repo
  issueTerm = "pathname"                 # 表示你选择以那种方式让github issue的评论和你的文章关联。
  theme = "github-light"                 # 样式主题，有github-light和github-dark两种

# 不算子页面统计
[params.busuanzi]
  enable = true
  siteUV = true
  sitePV = true
  pagePV = true

# 版权信息
[params.cc]
  name = "知识共享署名-非商业性使用-禁止演绎 4.0 国际许可协议"
  link = "https://creativecommons.org/licenses/by-nc-nd/4.0/"

# 流程图
[params.flowchartDiagrams]# see https://blog.olowolo.com/example-site/post/js-flowchart-diagrams/
  enable = false
  options = ""

# 序列图
[params.sequenceDiagrams] # see https://blog.olowolo.com/example-site/post/js-sequence-diagrams/
  enable = false
  options = ""                          # default: "{theme: 'simple'}"
```

## 5、新增本地查询、归档、关于
本地查询参考下面创建目录的index文件并修改成只保留如下参数  
归档参考下面创建目录的index文件并修改成并只保留如下参数  
关于是个人介绍，自由发挥
```bash
>hugo new search/index.md
>hugo new archives/index.md
>hugo new about/index.md

>vi /***/hugo_blog/content/search/index.md
---
title: "搜索"
description: "搜索页面"
type: "search"
---

>vi /***/huguo_blog/content/archives/index.md
---
title: "归档"
description: "归档页面"
type: archives
---
content/archives/index.md
```

## 6、启动server
启动server并测试，测试地址:http://127.0.0.1:1313
```bash
hugo server --theme=maupassant --buildDrafts
```

## 7、启动后不表示全部配置完成，还有需要增加留言
这里本人采用github issue的模板utterances作为评论系统(gitment、gitalk、utterances都可以，目前已知采用后2者多一点也简单)

### 首先选择一个仓库作为issue提交的落地点  
因为我之前已经有jekyll + gitpages和gitment做过留言，所以不需要新创建仓库了。 仓库命名就是git pages所在的仓库anTtutu.github.io

如果是首次搭建gitpages和issue采用留言系统，就首先在github上创建一个空仓库，如：anTtutu.github.io，并且初始化设置，仓库必须是Public，而不是私有的，这样我们的读者才可以查看以及发表评论

### 在github安装utterances插件app
访问[utterances](https://github.com/apps/utterances)应用程序 然后点击 Install 按钮进行安装。  

在这里可以选择可以关联的存储库，可以选择前一步确定的仓库，可以跟git pages一起也可以选全部，也可以独立，个人建议跟git pages一起比较好，关联比较强。gitpages作为blog也不会有人提issue。

## 8、大功告成，可以体验hugo blog了
