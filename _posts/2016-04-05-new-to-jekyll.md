---
layout: post
title:  "迁移博客 - wordpress to jekyll"
date:   2016-04-05 02:27:10 +0800
categories: jekyll update
---  
趁着放假，好好捣鼓一下    

## 迁移花费

- `服务器` -- 阿里云 100RMB 前三个月(5折)随便折腾。
- `域名`  -- 阿里云 4RMB   
- `服务器配置` -- 请教了万能的哥哥
- `jekyll` -- 参考了万能的google

同样的配置在几年前想都不敢想，怕麻烦。    
如今一个不小心就会在手游里花去648软妹币的时代，上面的花销就跟买个app一样简单。

## 云端

今年初有很长一段时间，我的邮箱里每天都能收到几封阿里云server的邮件。
正好年前有折腾的打算，于是就点开看了。    
正好有新手5折优惠。

附上的我推荐码 `ypbpi1` ，也不知道什么用，应该能省点钱叭 ^__^

### - 登录
购买后进入网站后台，就可以看到自己服务器 ip地址 `xx.xx.xx.xx`，一开始以为是共享ip，后来发现它是独享的，也就是说，你花了100块买了一个完全属于你的ip。
你直接往上面丢 羞羞的东西 直接用ip访问都没人知道，当然蓝皮肤找你喝茶我可不管。

那么这个时候你可以直接 ssh 到你的服务器。

	ssh root@xx.xx.xx.xx  

然后输入密码就可以了，密码是什么？ 应该是购买 阿里云服务的时候你填写的一个叫密码的东西叭！！！


### - 安装nginx 

	yum nginx

安装完了？完了！就是这么任性    
这个东东，我在一本叫《深入理解Nginx》中看了100多页还有些懵懂。
现在一句就ok

然后运行
	
	nginx

。。。

。。。。

。。。。。 怎么看运行了没？

直接在你的地址栏里敲 xx.xx.xx.xx 你的ip地址，看有了没。

没有的话，我也不知道了。    啊哈哈哈哈哈，反正我这里有了。

这个时候如果没有意外，应该就能看到标准的Nginx欢迎页面，    
意思是你的服务器终于有脸见人了！！！(づ￣ 3￣)づ    
接下来咱们倒腾 Jekyll

### - 安装Jekyll

Jekyll 是一个运行在ruby下的博客程序    
具体介绍可以看这里 [http://jekyllcn.com](http://jekyllcn.com)

首先当然是安装ruby

	sudo yum install ruby

因为正常情况下是可以不用sudo，因为你登录的是root账号。    

然后是安装一些ruby 依赖

	sudo yum install ruby-devel

安装 rubygems 用来拉取jekyll

	sudo yum install rubygems


上面都安装ok 后，就可以按照 http://jekyllcn.com 上的提示来安装 jekyll了。

	gem install jekyll

### - 运行Jekyll

然后是新建/运行 jekyll

	~ $ gem install jekyll
	~ $ jekyll new Blog
	~ $ cd Blog
	~/Blog $ jekyll s

现在只是简单的运行了 jekyll ，就像在你自己的 mac上运行jekyll s 一样，可以用localhost:4000 端口访问。
	
但是！如果你直接用上面的 `xx.xx.xx.xx:4000` 端口去访问其实是访问不了的，因为服务器默认是不开放 `4000` 端口的，需要Nginx 做一下代理。

### - 设置Nginx 代理

首先 `ctrl + c` 关闭 `Jekyll`
打开 Nginx 配置文件

	vi /etc/nginx/nginx.conf

--- vim 使用方法我就不告诉你了。    
找到 `location` 位置

    location / {
    }

添加一行 

    location / {
        proxy_pass  http://127.0.0.1:4000;
    }

因为Jekyll是运行在本地 `127.0.0.1:4000端口` 所以只需要把所有请求都代理到这个下面就可以让服务器访问博客了，是不是很机智！！！！！ (づ￣ w￣)づ
	
	:wq

保存退出配置文件。
重启 Nginx
	
	nginx -s stop
	nginx -s reload

重新运行 jekyll s

。。。

。。。。

。。。。。

直接访问 xx.xx.xx.xx ！！！！
是不是有了？

恭喜！！！

### - 后台运行Jekyll

然后你会发现另外一个问题，就是你在服务器的ssh Term 面板上运行的东西，如果关闭后，Jekyll 就会自动关闭。

这个时候需用 `nohup` 语法，让Jekyll后台自动运行。

	nohup jekyll s &

这个时候关闭 Term 都不会让jekyll 终止运行。是不是很骚气.

。。。

那我怎么关掉它！！！

请充值！

·~

首先运行下面的命令从进程里找到 jekyll 

	ps xf | grep jekyll

然后会输出类似下面的行目

	7307 pts/0    S+     0:00          \_ grep --color=auto jekyll
	7034 ?        Sl     0:02 /usr/bin/ruby /usr/local/bin/jekyll s

可以发现 7034 就是 运行 jekyll s 语法的进程。

然后杀掉这个进程就可以了

	kill -9 7034

好了，杀掉了。

现在去访问 xx.xx.xx.xx 

是不是打不开了！ >.< 智障！！！


## 后记

花100来块钱其实也挺好玩的吧。    
3个月可以玩好多东西。

- 弄个Jekyll主题
- Nginx反向代理多个网站
- 做个 Node 爬虫
- 搭个多人VR聊天室
- 写本小黄书


玩着玩着就想续费了， 真贵@&（……%￥￥……%