---
date: 2020-11-08
title: node项目监控工具之pm2
featured_image: cover.png
tags:
  - pm2
categories: 
  - web
slug: pm2UseApi
---
用了好几年的宝塔，不知道什么时候出了个pm2管理器，才开始以为它是对node版本切换管理的，直到今天看到有一篇文章才发现理解错了。研究了一下pm2的作用和用法，也顺便玩一玩宝塔中的pm2。

<!-- more -->

>   pm2官方文档：http://pm2.keymetrics.io/docs/usage/quick-start/

## 简单教程

首先需要安装pm2：

```bash
npm install -g pm2
```

运行：

```bash
pm2 start app.js
```

开机自动运行：

```bash
pm2 start app.js --watch
```

开机自启:

```bash
pm2 startup
pm2 save
```

初次安装并运行，会有一个高大上的界面：

![img](https://image.xiaomo.info//blog/1240.png)

那么pm2与[forever](http://www.jianshu.com/p/82a64aee0710)相比，比较有哪些高大上的功能呢？我们看一下对比表格：

|       Feature       | Forever | PM2  |
| :-----------------: | :------ | :--- |
|     Keep Alive      | ✔       | ✔    |
|    Coffeescript     | ✔       |      |
|   Log aggregation   |         | ✔    |
|         API         |         | ✔    |
| Terminal monitoring |         | ✔    |
|     Clustering      |         | ✔    |
| JSON configuration  |         | ✔    |

我们可以很直观的看出，pm2相比较Forever，功能更加强大一些。



## 查看运行状态

我们可以通过简单的命令查看应用的运行状态：

```bash
pm2 list
```

效果如下：

![img](https://image.xiaomo.info//blog/1240-20201106184135599.png)

>   ANodeBlog应用正在运行，pid为31480，并且占用内存为89.113 MB。

## 追踪资源运行情况

```bash
pm2 monit
```

会看到应用资源的实时运行情况

![img](https://image.xiaomo.info//blog/1240-20201106184139553.png)

## 查看应用详细部署状态

如果我们想要查看一个应用详细的运行状态，比如`ANodeBlog`的状态，可以运行：

```bash
pm2 describe 3
```

>   "3"是指App Id。

结果如下：

![img](https://image.xiaomo.info//blog/1240-20201106184144285.png)

## 查看日志

```bash
pm2 logs
```

系统会打印出详细的logs。

## 重启应用

```bash
pm2 restart appId
```

## 停止应用

想要终止应用，只需要运行：

```bash
pm2 stop app.js
```

## 强健的API

在项目中运行：

```bash
pm2 web
```

然后浏览器访问[http://localhost:9615](http://localhost:9615/) 你会有惊喜！

## 预定义运行配置文件

我们可以预定义一个配置文件，然后制定运行这个配置文件，比如我们定义一个文件`process.json`，内容如下：

```json
{
  "apps": [
    {
      "name": "ANodeBlog",
      "script": "bin/www",
      "watch": "../",
      "log_date_format": "YYYY-MM-DD HH:mm Z"
    }
  ]
}
```

然后可以通过

```bash
pm2 start process.json
```

运行这个App。

## 总结

常用命令总结如下：

1.  安装pm2

    ```bash
    npm install -g pm2
    ```

2.  启动应用

    ```bash
    pm2 start app.js
    ```

3.  列出所有应用

    ```bash
    pm2 list
    ```

4.  查看资源消耗

    ```bash
    pm2 monit
    ```

5.  查看某一个应用状态

    ```bash
    pm2 describe [app id]
    ```

6.  查看所有日志

    ```bash
    pm2 logs
    ```

7.  重启应用

    ```bash
    pm2 restart [app id]
    ```

8.  停止应用

    ```bash
    pm2 stop [app id]
    ```

9.  开启api访问

    ```bash
    pm2 web
    ```

更多pm2内容请参考官方文档：http://pm2.keymetrics.io/docs/usage/quick-start





# 宝塔中的pm2管理器使用方法

**1、安装版本**   

windows默认支持3个版本（8.x/9.x/10.x），选择任意一个版本安装即可   安装时间可能会比较久，请耐心等待..   ![img](https://www.bt.cn/bbs/data/attachment/forum/201908/05/113406f0lvelgqogeq0yop.png)  ![img](https://www.bt.cn/bbs/data/attachment/forum/201908/05/111557qgguiyzy52nnxuuk.png)  



**2、添加项目**   

如果添加项目后无法启动，可能是当前环境变量未生效，需要重启面板或者服务器后，重新添加项目    ![img](https://www.bt.cn/bbs/data/attachment/forum/201908/05/111735k1cw3t5a1oc5cs85.png)   ![img](https://www.bt.cn/bbs/data/attachment/forum/201908/05/111735jsjqgpqwjwddg0j4.png)  



**3、给项目绑定一个域名**   

如图点击映射，出现绑定域名窗口，绑定完成之后，可在网站管理查看到对应的网站   ![img](https://www.bt.cn/bbs/data/attachment/forum/201908/05/111912i5o6eb6uabnc89bo.png)  



**4、访问网站**   

浏览器输入刚刚绑定的域名如图：[http://node.ffce.cn](http://node.ffce.cn/)   至此第一个node.js搭建成功     ![img](https://www.bt.cn/bbs/data/attachment/forum/201908/05/112153ilqp5s11sqy5rsqp.png) 

 **5、添加模块**   

如果项目需要安装其他模块，则通过模块管理安装，如图我需要安装express   ![img](https://www.bt.cn/bbs/data/attachment/forum/201908/05/112455zgqi8qwzwiy8ky9m.png)  ![img](https://www.bt.cn/bbs/data/attachment/forum/201908/05/112455ny999dz59wq3qf9c.png)  



**6、查看日志**   

  ![img](https://www.bt.cn/bbs/data/attachment/forum/201908/05/112914dbk57ooxx7q3q57k.png)      



测试js ![img](https://www.bt.cn/bbs/static/image/filetype/html.gif) [app.js](https://www.bt.cn/bbs/forum.php?mod=attachment&aid=MjgyODB8YTI4NzM0ZDl8MTYwNDY0NzY4OHwwfDM1NjA3) *(423 Bytes, 下载次数: 1191)*



# 在mac上安装bt



## 安装Docker

```bash
brew cask install docker
```

## 在Docker里安装CentOS

Docker Hub：https://hub.docker.com/_/centos

```bash
docker pull centos
```



## 映射宝塔端口

创建一个CentOS容器并映射`8888`端口，复制返回的`容器ID`

```
docker run -d -it -p 10086:10086 centos
```

![img](https://img-blog.csdnimg.cn/20190901231059858.jpg)

## 进入CentOS安装宝塔

进入容器终端

```bash
docker exec -it [容器ID] bash
```

__在CentOS中__执行宝塔安装命令

在[宝塔论坛](https://www.bt.cn/bbs/forum-36-1.html)中打开安装教程，找到最新的版本安装命令，像以下这样的命令。

```bash
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
```



安装完成后，提示给的外网IP是用不了的，所以这里需要用`本地ip`访问面板

![img](https://img-blog.csdnimg.cn/2019090123111685.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FhNDY0OTcx,size_16,color_FFFFFF,t_70)

## docker相关命令

[Docker 命令大全](https://www.runoob.com/docker/docker-command-manual.html)

列出正在运行的容器信息

```bash
docker ps
```

列出所有容器信息

```bash
docker ps
```

停止所有容器

```bash
docker stop $(docker ps -a -q)
```



# 或者通过docker-compose安装

```bash
git clone https://github.com/ifui/baota.git
```

### 3. 进入项目根目录

```bash
cd baota
```

### 4. 生成配置文件

```bash
cp .env-example .env
```

### 5. 启动宝塔镜像，在项目根目录下执行命令

```bash
docker-compose up -d app
```

### 6. 查看默认登录信息

```bash
docker-compose logs app
```



如果自己修改过env中的端口配置，那么访问的时候就需要写自己修改的端口

![image-20201106174751622](https://image.xiaomo.info//blog/image-20201106174751622.png)

比如我这里填的是18888，那我的访问地址就是 http://localhost:18888/63c795f5 ，后面的随机码可以用`docker-compose logs app`查看

