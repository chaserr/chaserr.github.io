---
title: 最简便的方法搭建Hexo+Github博客,基于Next主题
date: 2016-06-01 12:08:29
tags: 
	- Hexo
	- Next
	- github
categories: Hexo博客教程

---

## 前言
Hexo，反正我第一眼看到就喜欢上了它的简约，[感受一下吧](http://chaserr.github.io/)。
如果你喜欢写作，我觉得你可以试试gitbook或者跟着本文搭建一个属于自己的博客空间（即使你不是IT行业的一员），不再受限于第三方博客地址，当然Hexo搭建的博客也是基于github托管的，但是并不需要你购买域名。
经过两天的探索加爬坑，终于把博客在git上安家了，感谢开源的大哥大姐们，由于并非js开发，所以遇到了很多坑，于是也想整理一篇比较完整的博客。这篇博客分为三部分：

**1.搭建you_site服务器环境**

<!-- more -->

**2.部署到github上**

**3.优化博客的主题** 
>ps:我选择的主题是Next


## Hexo的安装
### Hexo简介
在搭服务器环境之前，先简单介绍一下Hexo。Hexo 是一个简单地、轻量地、基于Node.js的一个静态博客框架。通过Hexo我们可以快速创建自己的博客，仅需要几条命令就可以完成。
Hexo特性：
- Hexo基于Node.js，支持多进程，几百篇文章也可以秒生成。
- 支持GitHub Flavored Markdown和所有Octopress的插件。
- Hexo支持EJS、Swig和Stylus。通过插件支持Haml、Jade和Less.

[Hexo的官方网站](http://brew.sh/index_zh-cn.html)，也是基于Github构建的网站。
### Hexo环境配置
在安装Hexo之前需要先安装一些它所依赖的环境配置
这里说一下，mac上安装一些软件的时候，我个人建议先安装一下Homebrew --- [Github传送门](https://github.com/mxcl/homebrew)，它相当于一个软件管理工具，不管是安装还是卸载都比较方便。
不多说，安装Homebrew也很简单，利用mac自带的ruby脚本功能，打开终端输入：

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

安装完成后，Homebrew 的主程序安装在 /usr/local/bin/brew。
接下来安装git和node.js就简单多了。
#### Git安装
- **安装过Xcode可以直接跳过这步，因为Xcode自带Git**

个人建议使用这种方式安装，不要使用网上的安装包进行安装，安装node.js也是。因为有些博客里的安装包有些过时。
打开终端输入：

    $ brew install git 
使用 Homebrew 方式安装，Git 被安装在 /usr/local/Cellar/git/ 中
#### Node.js安装

    $ brew install node 
使用 Homebrew 方式安装，node 被安装在 /usr/local/Cellar/node/ 中，自带安装好npm组件
想测试自己是否安装成功了，可以新建一个简单的hello.js文件:

```
var http = require('http');
http.createServer(function(req, res){
    res.writeHead(200, {'Content-Type': 'text/plain'});
    res.end('Hello World\n');
}).listen(8808, '127.0.0.1');
console.log('Server running at http://127.0.0.1:8808');
```

放到根目录下，然后在终端输入：

    node hello_node.js
    Server running at http://127.0.0.1:8124/
    
然后用浏览器打开用浏览器打开 http://127.0.0.1:8124/就可以看到Hello World 字样了。

当然安装node.js还可以手动安装或者利用nvm安装，这里就不介绍了。
接下来就可以安装Hexo了
#### Hexo安装
在终端输入：

    npm install hexo -g

这样，就把 Hexo 本体和其相依套件安装完毕了，很简单吧。
以后更新Hexo到最新版直接在终端输入：

    npm update hexo -g
接下来，就可以初始化一个放置你以后写文章的专属文件夹：

    hexo init <folder>
这里的`folder`参数如果指定，便会在终端当前的资料夹建立一个名为 `folder` 的新资料夹；否则会在当前资料夹初始化。所以建议都指定。因为初始化之后会有很多的文件夹出现。就像我刚开始接触电脑解压一个文件里面会有很多东西直接出现在我的桌面一样，那种感觉，好恶心。
初始化之后会稍等一会，然后终端会出现

    INFO  Start blogging with Hexo!
这样的字样就成功了，去目录查看，可以看见生成了
![Snip20160531_1](http://7xuupy.com1.z0.glb.clouddn.com/Snip20160531_1.png)
这几个文件夹，以后我们的操作基本都是在这里面了。

终端输入：

    pwd
    
   显示当前路径，如果不是在我们刚才init的文件夹里，那么就切换进取。
接下来，我们终端cd进入刚刚init的文件夹，我的文件夹是TongXingBlog
![Snip20160531_3](http://7xuupy.com1.z0.glb.clouddn.com/Snip20160531_3.png)

进入我们的Blog文件夹后，在终端输入：

    hexo s
`s`是简写：`hexo server`

终端结果显示：
![Snip20160531_5](http://7xuupy.com1.z0.glb.clouddn.com/Snip20160531_5.png)
复制地址，在浏览器打开，结果显示：
![Snip20160531_4](http://7xuupy.com1.z0.glb.clouddn.com/Snip20160531_4.png)
说明Hexo在你的电脑已经安装成功了，里面出现的是默认的一篇文章，当然你也可以自己新建一篇文章:
Ctrl+C停止服务器，在终端输入：

    hexo new "Hello Hexo"

新建的文章，可以在根目录的source/_posts目录下找到，并且可以使用支持Markdown语法的编辑器来对它进行编辑。
再次启动服务器，复制地址打开浏览器，发现刚刚新建的文章已经出现了。
这样第一部分就完成了，吐呀吐个槽，但是有没有发现，系统默认的样式很丑，反正我觉得很丑.......，还有怎么让别人看见你发表的文章呢，你现在只能在自己的电脑上看到，那么接下来就要借助github来实现对我们文章的托管。

---

## 将文章部署到github
### 创建github仓库
首先，你得有一个github账号，没有的话去申请一个吧,
> **没有github账号的童鞋申请的时候用户名想好了哦，因为你的博客域名将会以它为基础。**
- 这里推荐一个翻墙软件，Lantern，我手上有解版永久有效，之前传到CSDN上不知道为什么一直没通过。

> **注:** 已经有github账号的跳过这节

登录github账号后，点击右上角的+号创建一个新的创库：![Snip20160531_6](http://7xuupy.com1.z0.glb.clouddn.com/Snip20160531_6.png)

进入创建创库界面：
![Snip20160531_7](http://7xuupy.com1.z0.glb.clouddn.com/Snip20160531_7.png)

一切填完之后，就可以点击Create按钮了。
这里，切记切记：
> Repository name 不可以乱写，是固定的写法
也就是`必须是`你的`用户名+github.io`这种格式，它也将是你个人博客的域名。也就是别人输入这个网址来访问你的博客。

到这里创库就创建完成了，它将会与你本地init的那个文件夹相关联，大致就是将你本地的文件夹上传到服务器，供别人访问你的博客内容。
### 配置本地文件
打开我们init好的那个文件夹，找到_config.yml文件，这就是全局配置文件，通过配置里面的参数，来与我们的github上得仓库进行关联。
打开这个文件，找到最下面：

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type:
  
```

这段代码，然后修改成，我这样

``` 
deploy:
	type: git #部署类型，使用github
	repository: https://github.com/chaserr/chaserr.github.io.git #部署的仓库的SSH
	branch: master #部署分支， 一般使用master主分支
	message: update #默认类型
```

这里有两个重要的参数：（其他两个参数可以参照着写）
1. `type`：Hexo之前的版本好像是填github，但是Hexo3.0之后，必须填git，我的Hexo是最新的3.2.0，填写git。
2. `repository`这个参数，很重要，它就是用来链接我们在github上创建的创库。看网上有的人使用SSH，但是SSH配置起来相对有些复杂，我这里用的是HTTPS方式，也是可以成功的。

打开github的Repositories，找到我们创建的仓库，点进去
![Snip20160531_88](http://7xuupy.com1.z0.glb.clouddn.com/Snip20160531_8.png)

![Snip20160531_9](http://7xuupy.com1.z0.glb.clouddn.com/Snip20160531_9.png)
点击Use HTTPS,然后复制文本框里的网址，填到上面的`repository`参数后面即可，
> **注意**每个参数`：`后面必须留有一个空格，否则会出现语法错误。

然后在终端输入：

    npm install hexo-deployer-git --save

会出现信息：

    └── hexo-deployer-git@0.1.0 
接下来就可以把你的文章部署到github上去了。
在终端输入命令

    hexo d
`d`是 `hexo deployer`的缩写，

待终端出现

    INFO  Deploy done: git
就表示成功啦。
现在你可以在浏览器输入http://your_username.github.io
这里的`your_username`就是你的github账号昵称

以后每次新建文章后，待你完成编辑。即可以此执行下面几个命令来把新的文章部署到服务器上:
- hexo clean
- hexo g
- hexo d

附：
> Hexo部署常见问题解决方案 -- [传送门](http://wp.huangshiyang.com/hexo%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88)

至此，Hexo的部署算是完成了。接下来就是优化你的主题了。
我使用的是Next主题，如果喜欢其他主题的童鞋，可以跳走了。

## 优化主题

[样式预览](http://chaserr.github.io/)
### Next主题的安装使用
首先从github上clone到本地，在终端cd / 切换到你通过Hexo init生成的根目录，
然后在终端输入命令：

    $ git clone https://github.com/iissnan/hexo-theme-next themes/next

进入站点的全局配置文件：_config.yml
找到theme字段：设置为

    theme: next
到这里可以验证一下主题是否被启用。终端输入：

    hexo s -- debug
然后本地访问http://localhost:4000，看看效果，在没有部署到github上之前，一般都可以这样在本地进行预览。

关于站点全局_config.yml配置文件的其他一些参数：

```
# Site
title: 朝夕 #博客名
subtitle: 朝闻道，夕死可以。 #博客副标题
description: #给搜索引擎看的，对站点的描述，可以自定义
author: 童星 #作者名称，显示在网站最底部
language: zh-Hans #语言选择，这里表示中文
email: #你的联系邮箱
timezone:

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: chaserr.github.io  #可以填写你的站点域名
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:

# Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:

# Writing , 设置生成博文的默认格式
new_post_name: :title.md # File name of new posts
default_layout: post #默认布局
titlecase: false # Transform title into titlecase
external_link: true # Open external links in new tab
filename_case: 0 #把文件名称转换为（1）小写或者大写
render_drafts: #显示草稿
post_asset_folder: false  #启动Asset文件夹
relative_link: false #把链接改为与根目录相对位置
future: true #显示未来的文章
highlight: #代码块的设置
  enable: true
  line_number: true
  auto_detect: false
  tab_replace:

```

默认的大致就这些，另外给主题添加相关功能的时候后面会慢慢加上其他一些参数。

#### 给站点添加rss和sitemap功能
打开终端，切换到站点根目录，输入：

```
$ npm install hexo-generator-feed
$ npm install hexo-generator-sitemap
```

在全局配置文件_config.yml进行插件配置：（为防止好找，在最下面进行配置）

```
#插件配置
plugins: hexo-generator-feed 
#- hexo-generator-sitemap 

feed:
  type: atom ##feed类型 atom或者rss2
  path: atom.xml ##feed路径
  limit: 20  ##feed文章最小数量
```

#### 给站点添加本地搜索功能
- 使用swiftype添加站内搜索

去swiftype官网注册一个账号，按照步骤选择自己喜欢的搜索样式，配置完成后，选择install，然后会出现![Snip20160531_10](http://7xuupy.com1.z0.glb.clouddn.com/Snip20160531_10.png)
如图所示，我不知道为什么，这个代码框无法滚动，所以我们必须将它全部复制出来

```
<script type="text/javascript">
  (function(w,d,t,u,n,s,e){w['SwiftypeObject']=n;w[n]=w[n]||function(){
  (w[n].q=w[n].q||[]).push(arguments);};s=d.createElement(t);
  e=d.getElementsByTagName(t)[0];s.async=1;s.src=u;e.parentNode.insertBefore(s,e);
  })(window,document,'script','//s.swiftypecdn.com/install/v2/st.js','_st');
  
  _st('install','7Qoo1zkKbfSsp5bzUjQu','2.0.0');
</script>
```
然后复制install ----2.0.0之间的一段代码，添加到全局配置文件里：

```
# 搜索插件 hexo-generator-sitemap
swiftype_key: 7Qoo1zkKbfSsp5bzUjQu
```
- 添加本地搜索

直接在全局配置文件添加参数：

```
search:
  path: search.xml
  field: post
```
search.xml这些都是默认带有的
#### 给站点添加友情链接功能
直接在全局配置文件添加参数：

```
links_title: 友情链接
links:
	#百度: http://www.baidu.com/
	#新浪: http://example.com/
```
其实也可以新建一个文件专门放置这些需要链接的网站，这样方便管理，不必每次都对全局配置文件进行更改
>如果没有链接，那么友情链接不会显示，为了测试，可以随便写一个


#### 给站点添加多说评论、热评、分享功能

直接在全局配置文件添加参数：

```
# Disqus  disqus评论,  与多说类似, 国内一般使用多说
#disqus_shortname: 
duoshuo_shortname: chaser

# 多说热评文章 true 或者 false
duoshuo_hotartical: true

# 多说分享服务
duoshuo_share: true
duoshuo_info:
  ua_enable: true
  admin_enable: false
  user_id:
	  admin_nickname:
```

> **注意：**这里的`duoshuo_shortname`是需要你去多说主页 -- [传送门](http://duoshuo.com/) 注册一个账号，然后填写你的多说域名，这里填写的就是你在多说填写的域名
> 例如：我的是：`http://chaser.duoshuo.com/`，那么我填写的就是`chaser`

> * 这里配置的是所有页面都支持评论，但是后面有的页面不需要，比如标签页，分类页，关于等等，那么就单独设置。

### Next主题的优化
这之前，先对主题配置文件进行一些配置，打开博客根目录/themes/next，这个路径，然后打开，主题配置文件_config.yml：
#### 配置个人头像
在主题配置文件里找到avatar字段，(如果没有就添加)

```
# Sidebar Avatar
# in theme directory(source/images): /images/avatar.jpg
# in site  directory(source/uploads): /uploads/avatar.jpg
avatar: http://ww4.sinaimg.cn/small/937882b5jw1f4db4lroy9j20hs0npmy6.jpg
```
>我采用的是地址，并没有将图片放在本地（采用新浪博客相册）

#### Next主题选择
在主题配置文件_config.yml里，找到scheme字段

```
# ---------------------------------------------------------------
# Scheme Settings
# ---------------------------------------------------------------

# Schemes
#scheme: Muse
#scheme: Mist
scheme: Pisces
```

- Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
- Mist - Muse 的紧凑版本，整洁有序的单栏外观
- Pisces - 双栏 Scheme，小家碧玉似的清新
我选择的是第三种，去掉`#`，然后把第一个前面加上`#`
> 每进行一步都可以自己去进行预览：
> 
    hexo s --debug

#### Next主题菜单配置
```
# ---------------------------------------------------------------
# Menu Settings
# ---------------------------------------------------------------

# When running the site in a subdirectory (e.g. domain.tld/blog), remove the leading slash (/archives -> archives)
menu:
  home: / #主页
  categories: /categories #分类页面
  about: /about #关于
  archives: /archives #归档
  tags: /tags #标签
  commonweal: /404.html  #公益404


# Enable/Disable menu icons.
# Icon Mapping:
#   Map a menu item to a specific FontAwesome icon name.
#   Key is the name of menu item and value is the name of FontAwsome icon. Key is case-senstive.
#   When an question mask icon presenting up means that the item has no mapping icon.
menu_icons:
  enable: true #是否显示图标
  #KeyMapsToMenuItemKey: NameOfTheIconFromFontAwesome
  home: home
  about: user
  categories: th
  tags: tags
  archives: archive
  commonweal: heartbeat
```
>1. 如果需要添加你自己自定义的菜单，那么就只需要在menu下添加相关的菜单项，然后新建页面与之匹配，然后去next主题目录的`languages`目录下找到我们之前配置的主题所使用的语言，（我们用的是`zh-Hans`）对其进行国际化。

>2. menu_icon的设置采用`key-value`。key对应上面的menu里面的菜单项，大小写一致，value就是对应[FontAwsome icon](http://www.bootcss.com/p/font-awesome/#icons-web-app)这个网站的图标的名字，去掉前缀icon

> 关于这里的所有的图标都支持从`FontAwsome icon`这个网站获取，当然也可以自己放在资源文件夹里（一般比如个人头像(`avatar`)，网站图标(`favicon`)等)。
> 当然也支持从网址获取，推荐使用`七牛`云存储。当然也可以使用新浪博客的相册，谷歌相册，甚至QQ空间，都可以，只要能获取到图片网址，并且你不会轻易删掉。

##### 添加关于，标签，分类，公益404页面
菜单配置好了，但是我们还得新建一些页面与之相匹配，否则点击进去找不到。这里说一下，对于`标签页`和`分类页`，我们需要在新建一篇文章的时候指定它的标签和分类。对于刚开始建立的博客，点进去可能是空的，所以等会新建一篇文章试试，这之前，先建立相关页面。

###### 添加标签页
打开终端，进入博客根站点，输入：

    hexo new page tags

进入博客根目录/source路径，找到tags文件夹，可以看到生成了index.md文件。可以使用编辑器打开
在里面添加`tags`和`comments`
![Snip20160531_11](http://7xuupy.com1.z0.glb.clouddn.com/Snip20160601_11.png)
>1. `tags`设置页面的类型
>2. `comments`用来控制是否显示评论。

###### 添加分类页
    hexo new page categories

进入博客根目录/source路径，找到categories文件夹，可以看到生成了index.md文件。可以使用编辑器打开
在里面添加`tags`和`comments`
![Snip20160531_11](http://7xuupy.com1.z0.glb.clouddn.com/Snip20160601_11.png)
>1. `tags`设置页面的类型
>2. `comments`用来控制是否显示评论。


###### 添加关于页
    hexo new page about

进入博客根目录/source路径，找到categories文件夹，可以看到生成了index.md文件。可以使用编辑器打开
在里面添加`tags`和`comments`
![Snip20160531_11](http://7xuupy.com1.z0.glb.clouddn.com/Snip20160601_11.png)
>1. `tags`设置页面的类型
>2. `comments`用来控制是否显示评论。

about这个页面一般都是填写一些你的个人信息，要不要无所谓，我这里给about页面添加了一个背景音乐盒.去网易云音乐，创建一个自己的歌单，然后分享，然后找到自己分享的动态，点击链接，![Snip20160531_12](http://7xuupy.com1.z0.glb.clouddn.com/Snip20160601_12.png)
出现如图的页面，然后点击生成''外链播放器''字样。将出现的代码复制，粘贴到你的about页面
![Snip20160531_13](http://7xuupy.com1.z0.glb.clouddn.com/Snip20160601_13.png)

新建一篇文章给它添加分类和标签试试吧：

    hexo new "Hexo教程"
通过mou编辑器打开：添加`tags`和`categories`

```
---
title: title #文章標題
date: 2016-06-01 23:47:44 #文章生成時間
categories: "Hexo教程" #文章分類目錄 可以省略
tags: #文章標籤 可以省略
	 - 标签1
	 - 标签2
 description: #你對本頁的描述 可以省略
---
```

###### 添加公益404页面
直接在根目录的source路径下，新建一个404.html文件，就可以了
附：404.html


```
<!DOCTYPE HTML>
<html>
<head>
  <meta http-equiv="content-type" content="text/html;charset=utf-8;"/>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="robots" content="all" />
  <meta name="robots" content="index,follow"/>
</head>
<body>

<script type="text/javascript" src="http://www.qq.com/404/search_children.js"
        charset="utf-8" homePageUrl="your site url "
        homePageName="回到我的主页">
</script>

</body>
</html>

```
> 提示：`homePageUrl`记得修改成你的博客域名


### Next主题侧边栏社交链接
打开主题配置文件_config.yml：

```
# ---------------------------------------------------------------
# Sidebar Settings
# ---------------------------------------------------------------
# Social Links
social:
	GitHub: https://github.com/chaserr
	新浪微博: http://weibo.com/lovaxiang
	FaceBook: https://www.facebook.com/x.tongxing
	GooglePlus : https://plus.google.com/u/0/117848542581702766384
	豆瓣: https://www.douban.com/people/lovax/

# Social Links Icons
social_icons:
 enable: true #控制是否显示图标
  # Icon Mappings.
  # KeyMapsToSocalItemKey: NameOfTheIconFromFontAwesome
  GitHub: github
  新浪微博: weibo
  FaceBook: facebook
  GooglePlus: google-plus
  豆瓣: globe  #douban
```
>这里的配置和Menu里一样，
>1. `social`后面跟着的是你的社交网站的主页地址
>2. `social_icons`是FontAwesome网站的图标名称

>不过豆瓣icon好像是没有的，我在github上issue过，他们给我的解决方案就是去官网找到图标，放到本地资源、。、
>所以亲们也不用去issue了。直接用别的代替吧，如果真想显示你的豆瓣的话。

### 开启打赏功能
很简单，打开主题配置文件_config.yml
添加字段：

```
reward_comment: 坚持原创技术分享，您的支持将鼓励我继续创作！
alipay: http://ww3.sinaimg.cn/small/937882b5jw1f4dalzfc5nj20p00vu0um.jpg
```

>`alipay:` 填写的是支付宝或者微信的收款二维码图片地址。


### 给文章添加阅读量
#### 配置LeanCloud
打开[LeanCloud](https://leancloud.cn/login.html#/signin)官网，注册一个账号，完成激活
![Snip20160531_14](http://7xuupy.com1.z0.glb.clouddn.com/Snip20160601_14.png)
点击创建新应用之后，进入新创建的应用
![Snip20160531_15](http://7xuupy.com1.z0.glb.clouddn.com/Snip20160601_15.png)
点击创建Class，类名叫做`Counter`。
#### 修改主题配置文件_config.yml
打开主题配置文件，找到`leancloud_visitors`字段或者创建

```
# Show number of visitors to each article.
# You can visit https://leancloud.cn get AppID and AppKey.
leancloud_visitors:
  enable: true
  app_id: ytnok33cvEchgidigtb0WumC-gzGzoHsz #<AppID>
  app_key: SrcG8cy1VhONurWBoEBGGHML #<AppKEY>
```
> 1. `app_id`
> 2. `app_key` 
> ![Snip20160531_18](http://7xuupy.com1.z0.glb.clouddn.com/Snip20160601_18.png)
> 
将对应key的value复制填写即可。

#### 添加lean-analytics.swing
在主题目录下的\layout\_scripts路径下，新建名为：`lean-analytics.swing`的文件，
这里贴上`lean-analytics.swing`代码

```
{% if theme.leancloud_visitors.enable %}

  {# custom analytics part create by xiamo #}
  <script src="https://cdn1.lncld.net/static/js/av-core-mini-0.6.1.js"></script>
  <script>AV.initialize("{{theme.leancloud_visitors.app_id}}", "{{theme.leancloud_visitors.app_key}}");</script>
  <script>
    function showTime(Counter) {
      var query = new AV.Query(Counter);
      var entries = [];
      var $visitors = $(".leancloud_visitors");

      $visitors.each(function () {
        entries.push( $(this).attr("id").trim() );
      });

      query.containedIn('url', entries);
      query.find()
        .done(function (results) {
          var COUNT_CONTAINER_REF = '.leancloud-visitors-count';

          if (results.length === 0) {
            $visitors.find(COUNT_CONTAINER_REF).text(0);
            return;
          }

          for (var i = 0; i < results.length; i++) {
            var item = results[i];
            var url = item.get('url');
            var time = item.get('time');
            var element = document.getElementById(url);

            $(element).find(COUNT_CONTAINER_REF).text(time);
          }
        })
        .fail(function (object, error) {
          console.log("Error: " + error.code + " " + error.message);
        });
    }

    function addCount(Counter) {
      var $visitors = $(".leancloud_visitors");
      var url = $visitors.attr('id').trim();
      var title = $visitors.attr('data-flag-title').trim();
      var query = new AV.Query(Counter);

      query.equalTo("url", url);
      query.find({
        success: function(results) {
          if (results.length > 0) {
            var counter = results[0];
            counter.fetchWhenSave(true);
            counter.increment("time");
            counter.save(null, {
              success: function(counter) {
                var $element = $(document.getElementById(url));
                $element.find('.leancloud-visitors-count').text(counter.get('time'));
              },
              error: function(counter, error) {
                console.log('Failed to save Visitor num, with error message: ' + error.message);
              }
            });
          } else {
            var newcounter = new Counter();
            /* Set ACL */
            var acl = new AV.ACL();
            acl.setPublicReadAccess(true);
            acl.setPublicWriteAccess(true);
            newcounter.setACL(acl);
            /* End Set ACL */
            newcounter.set("title", title);
            newcounter.set("url", url);
            newcounter.set("time", 1);
            newcounter.save(null, {
              success: function(newcounter) {
                var $element = $(document.getElementById(url));
                $element.find('.leancloud-visitors-count').text(newcounter.get('time'));
              },
              error: function(newcounter, error) {
                console.log('Failed to create');
              }
            });
          }
        },
        error: function(error) {
          console.log('Error:' + error.code + " " + error.message);
        }
      });
    }

    $(function() {
      var Counter = AV.Object.extend("Counter");
      if ($('.leancloud_visitors').length == 1) {
        addCount(Counter);
      } else if ($('.post-title-link').length > 1) {
        showTime(Counter);
      }
    });
  </script>

{% endif %}
  
```

>网上也有人贴出来这部分代码，但是会出现阅读量出现重复情况，这里的代码是经过测试后出现的，准确无误。

#### 修改post.swing文件
打开主题目录\layout\_macro路径,找到post.swig文件，找到`LeanCould PageView #`字样,去掉 其中一个`&nbsp; | &nbsp`，否则部署之后会出现双`||`。

#### 修改layout.swing文件
打开主题目录\layout路径，找到_layout.swing
搜索`</body>`标签，在其上方添加代码：

```
  {% if theme.leancloud_visitors.enable %}
  {% include '_scripts/lean-analytics.swig' %}
  {% endif %}
```

### 给页面添加High一下
打开`博客根目录\themes\next\layout_partials\header.swig`，在`<ul> ... /ul>`标签之间加入以下代码：

```
<li> <a title="把这个链接拖到你的Chrome收藏夹工具栏中" href='javascript:(function() {
    function c() {
        var e = document.createElement("link");
        e.setAttribute("type", "text/css");
        e.setAttribute("rel", "stylesheet");
        e.setAttribute("href", f);
        e.setAttribute("class", l);
        document.body.appendChild(e)
    }
 
    function h() {
        var e = document.getElementsByClassName(l);
        for (var t = 0; t < e.length; t++) {
            document.body.removeChild(e[t])
        }
    }
 
    function p() {
        var e = document.createElement("div");
        e.setAttribute("class", a);
        document.body.appendChild(e);
        setTimeout(function() {
            document.body.removeChild(e)
        }, 100)
    }
 
    function d(e) {
        return {
            height : e.offsetHeight,
            width : e.offsetWidth
        }
    }
 
    function v(i) {
        var s = d(i);
        return s.height > e && s.height < n && s.width > t && s.width < r
    }
 
    function m(e) {
        var t = e;
        var n = 0;
        while (!!t) {
            n += t.offsetTop;
            t = t.offsetParent
        }
        return n
    }
 
    function g() {
        var e = document.documentElement;
        if (!!window.innerWidth) {
            return window.innerHeight
        } else if (e && !isNaN(e.clientHeight)) {
            return e.clientHeight
        }
        return 0
    }
 
    function y() {
        if (window.pageYOffset) {
            return window.pageYOffset
        }
        return Math.max(document.documentElement.scrollTop, document.body.scrollTop)
    }
 
    function E(e) {
        var t = m(e);
        return t >= w && t <= b + w
    }
 
    function S() {
        var e = document.createElement("audio");
        e.setAttribute("class", l);
        e.src = i;
        e.loop = false;
        e.addEventListener("canplay", function() {
            setTimeout(function() {
                x(k)
            }, 500);
            setTimeout(function() {
                N();
                p();
                for (var e = 0; e < O.length; e++) {
                    T(O[e])
                }
            }, 15500)
        }, true);
        e.addEventListener("ended", function() {
            N();
            h()
        }, true);
        e.innerHTML = " <p>If you are reading this, it is because your browser does not support the audio element. We recommend that you get a new browser.</p> <p>";
        document.body.appendChild(e);
        e.play()
    }
 
    function x(e) {
        e.className += " " + s + " " + o
    }
 
    function T(e) {
        e.className += " " + s + " " + u[Math.floor(Math.random() * u.length)]
    }
 
    function N() {
        var e = document.getElementsByClassName(s);
        var t = new RegExp("\\b" + s + "\\b");
        for (var n = 0; n < e.length; ) {
            e[n].className = e[n].className.replace(t, "")
        }
    }
 
    var e = 30;
    var t = 30;
    var n = 350;
    var r = 350;
    var i = "//7xuupy.com1.z0.glb.clouddn.com/tongxingSibel%20-%20Im%20Sorry.mp3";
    var s = "mw-harlem_shake_me";
    var o = "im_first";
    var u = ["im_drunk", "im_baked", "im_trippin", "im_blown"];
    var a = "mw-strobe_light";
    var f = "//s3.amazonaws.com/moovweb-marketing/playground/harlem-shake-style.css";
    var l = "mw_added_css";
    var b = g();
    var w = y();
    var C = document.getElementsByTagName("*");
    var k = null;
    for (var L = 0; L < C.length; L++) {
        var A = C[L];
        if (v(A)) {
            if (E(A)) {
                k = A;
                break
            }
        }
    }
    if (A === null) {
        console.warn("Could not find a node of the right size. Please try a different page.");
        return
    }
    c();
    S();
    var O = [];
    for (var L = 0; L < C.length; L++) {
        var A = C[L];
        if (v(A)) {
            O.push(A)
        }
    }
	})()    '>High一下</a> </li>
```

> `//7xuupy.com1.z0.glb.clouddn.com/tongxingSibel%20-%20Im%20Sorry.mp3"`可以替换成任意你想要的音乐地址



### 设置网站的图标Favicon
favicon图标也就是我们打开一个网页，出现在最浅的图标样式，可以自定义，首先我们需要一个favicon.ico的图标，可以去[比特虫](http://www.bitbug.net/)网站制作，网上的做法是将其放在根目录的source文件夹里。然后在主题目录的layout\_partials路径下，修改header.swig的meta标签，我实验了一下，并不能配置成功呢，所以代码就不贴了，这里介绍我的做法:
图表制作好后，上传到云存储空间，获取图片的网址，然后打开主题配置文件_config.yml，找到favicon字段，将图片网址粘贴在后面，即可。

```
favicon: http://ww4.sinaimg.cn/square/937882b5jw1f4db4lroy9j20hs0npmy6.jpg #网站图标
```

到此，Next的主题算是简单的美化了一下，你现在可以在本地预览，也可以将其部署到github上去哦。
三部曲：
终端执行

```
hexo clean #清空缓存
hexo g #生成静态网页
hexo d #部署到github
```

关于博客的迁移，文章标题的搜索优化等，这里就不说了。网上也有很多大神写过这方面的文章。再次感谢开源大帝。
不说了，我也得考虑做迁移了。




