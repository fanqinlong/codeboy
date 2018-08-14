---
title: Hexo开源博客系统入门
categories:
  - 工具
tags:
  - 日常
date: 2018-07-13 18:06:02
comments: true 
---
**Hexo**是一款纯静态博客系统，通过精心的设计与技术实现，支持度与集成度都比较高，社区支持也比较好，所以我选择**Hexo**来实现自己的博客。
## **安装**
**Hexo** 如官方介绍一样，安装方便快捷。安装前请确保 **Node** 和 **Nginx** 环境已经存在，需要安装可以参考 **CentOS 7** 安装 **Node** 和 **Nginx** 安装。
只需使用如下命令即可安装 **Hexo**
``` python
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
$ hexo server
```
安装完成后目录结构如下：
``` python
├── _config.yml             # 主配置文件
├── package.json            # 应用程序的信息
├── scaffolds               # 模版文件夹，新建文章时根据这些模版来生成文章的.md文件
├── source                  # 资源文件夹
|   ├── _drafts
|   └── _posts
└── themes                  # 主题文件夹
```
**Hexo** 默认启动 4000 端口，使用浏览器访问 http://localhost:4000，即可看见 **Hexo** 最初的样子。
## **使用**
关于 **Hexo**  更详细的使用技巧，请参考[官网文档](https://hexo.io/zh-cn/docs/)，这里只列举常用的使用方法。
### **更换主题**

-------------------
**Hexo** 提供的可选 主题 比较多，总有一款你如意的，我这里主题选择了 [snippet](https://github.com/shenliyang/hexo-theme-snippet)，没有为什么，就是看起来舒服而已，后续相关配置也是基于该主题。
找到喜欢的一款后，使用如下命令安装主题：

``` python
#进入博客目录
$ cd yourblog
#克隆主题源码到hexo的themes文件夹下
$ git clone https://github.com/xxx/xxx.git themes/xxx
```

最后一步，在**_config.yml**配置中启用新主题。
``` python
theme: xxx
```

关于主题的配置，请参考主题源码中的 README.md 文档。

### **写文章**

-------------------
具体请参考官方创建文章的方法，[见这里](https://hexo.io/zh-cn/docs/writing.html)
1） 新建文章
当需要写文章时，使用如下命令新建文章，会在资源文件夹中生成与 title 对应的 .md 文件。
``` python
$ hexo new [layout] <title>
```
.md 文件就是 markdown 格式的文章表述。格式大致为：
``` python
---
title: <title>
categories:
  - 工具
tags:
  - 日常
date: 2018-07-13 18:06:02
comments: false 
---
以下为文章的markdown内容
```

文件最上方以---为分隔符，分隔符以上为 `Front-matter`，用于指定与文章相关的基本信息，分隔符以下才为文章的内容区域。
2） Front-matter
Front-matter 内容如下：
``` python
layout                 #布局
title                  #标题
date                   #建立日期
updated                #更新日期
comments               #是否开启文章的评论功能
tags                   #标签
categories             #分类
permalink              #覆盖文章网址
```

其中 title、date、tags、categories 这 4 项，在新建文章时需要进行设置，其他项采用默认值即可，不需要在每篇文章中进行设置，故可以将这 4 项基本设置移到模板文件`scaffolds\post.md`中，如下：
``` python
---
title: {{ title }}
date: {{ date }}
tags:
categories:
---
```

这样在新建文章时，就会自动在文章`.md`文件中加入 4 项基本设置。
特别说明，文章中添加了分类和标签后，c 会自动生成分类页面和统计分类的文章数。关于分类和标签的使用，如下：
``` python
categories:           # 分类存在顺序关系
- 语言                 # 1级分类
- PHP                 # 2级分类
- PDO                 # 3级分类    
tags:                 # 标签为无序
- PHP                 # 标签1
- PDO                 # 标签2
```

3） 正文
文章正文使用 markdown 格式即可，我使用的 markdown 编辑器[马克飞象](https://maxiang.io/)。马克飞象在线编辑，可以同印象笔记时时同步，但是想预览图片，就必须是线上图片地址。使用编辑器预览编辑完文章后，导出`.md`文件替换新建文章时生成的同名`.md`文件即可。编辑完文章后，使用 **hexo s**命令即可实时预览到文章效果。
### **发布**

-------------------
文章的新增和编辑都是在资源文件夹下`source`操作，完成后需要发布才能生成静态文件`public`，进而才能通过浏览器直接访问。
发布更新命令如下：
``` python
$ hexo generate     #正常
$ hexo g            #简写
```

发布后，`public`文件夹更新到最新状态，此时即可直接访问。
说明：**hexo s**并没有产生静态文件，而是实时动态解析实现及时访问。
### **插件**

-------------------
#### **搜索**
安装 [hexo-generator-search](https://github.com/wzpan/hexo-generator-search)，在`_config.yml`中添加如下配置代码：
``` python
search:
  path: search.xml
  field: all
```

#### **RSS**
安装 [hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed)，并按照说明配置`atom.xml` 的链接写在`yourblog/source/_data/link.json`的 `social` 项中,一般无需更改）

## **配置**

### **打开文章资源文件夹功能**

-------------------
在**Hexo**中，相对路径是针对资源文件夹`source`来讲，所以文章的静态图片应放置于资源文件夹下。
可以将所有文章的静态图片统一放置于`source/images`下，但是这样不方便于管理，推荐方法是将每篇文章的图片放置于与该文章同名的资源文件下，然后使用相对路径引用即可。

在配置文件`_config.yml`中开启`post_asset_folder`项，即更改为：
``` python
post_asset_folder: true
```
开启该项配置后，Hexo 将会在你每一次通过`hexo new [layout] <title>`命令创建新文章时自动创建一个文件名同 .md 文件的文件夹。将所有与你的文章有关的资源放在这个关联文件夹中之后，就可以通过相对路径来引用它们。

写文章时你只需在 markdown 中插入相对 **.md** 文件的 相对路径 的图片即可， `hexo-asset-image` 自动转化为网站 绝对路径。此时，可以直接使用 **Hexo** 提供的标签asset_img来插入图片，但是这样违背了 markdown 语法，无法及时预览，不便于编辑文章。

可以通过以下 markdown 语法在文章中插入图片，这种方式同时也支持本地 markdown 编辑器实时预览。

在配置文件`_config.yml`中开启`post_asset_folder`项，即更改为：
``` python
![alt](/post_title/image_name)
# post_title为与文章.md同名的资源文件夹名
# image_name为图片的文件名
```

### **去除代码块行号**
修改`_config.yml`配置项如下：
``` python
line_number: false
```

## **部署**
如果采用本地编辑博客，而博客部署在远程服务器上，那么你就需要部署，才能同步本地更新到远程服务器。
**Hexo** 提供了 5 种部署方案，[见这里](https://hexo.io/zh-cn/docs/deployment.html)，这里只介绍`Git`：
### **Git**

-------------------
安装 hexo-deployer-git。

_config.yml配置如下：
修改`_config.yml`配置项如下：
``` python
deploy:
  type: git
  repo: <repository url>                     # 库地址
  branch: [branch]                           # 分支名称
  message: [message]                         # 提交信息
```

该方案适用于采用 `github pages` 托管博客的用户，当然使用服务器搭建博客的用户可以使用 `webhook` 方案来实现。
### **我的方案**

-------------------
上述推荐部署方案，明显的缺点是本地需要部署 **Hexo** 环境，无法实现随时随地的更新博客。为了方便写作，我的部署方案见下次的博客。