---
layout: post
title: linux 下gitblit 服务器配置
tags: linux gitblit
---

1 下载需要的依赖库
```
yum install java # Gitblit 是一个纯 Java 库
yum install git
yum install -y gcc-c++ curl-devel expat-devel gettext-devel openssl-devel zlib-devel perl-devel
yum install lsof
yum install net-tools
```

2.从官网中下载GitBlit源码
```
wget http://dl.bintray.com/gitblit/releases/gitblit-1.8.0.tar.gz
```
3.解压缩GitBlit包
```
mv gitblit-1.8.0.tar.gz gitblit ##修改名称
cp gitblit /usr/  移动到usr目录下
进入usr/目录进行解压
cd /usr/
tar -zxvf gitblit
```
4.需要gitlbit/data/defaults.properties进行设置,包括端口号，ip，使用vi编辑器进行设置
```
cd gitblit/data
vim defaults.properties
```
更改ip地址与主机ip地址一样,使用/httpBind 检索如图所示：
![avatar](/assets/img/1.png)
修改自己需要的端口号如80：
![avatar](https://img2018.cnblogs.com/blog/1046754/201904/1046754-20190402203804238-311007268.png)

同时修改service-centos.sh中的端口号与在defaults.properties文件中修改的端口号一致
![avatar](https://img2018.cnblogs.com/blog/1046754/201904/1046754-20190402204258296-1814475139.png)
![avatar](https://img2018.cnblogs.com/blog/1046754/201904/1046754-20190402204315112-867148086.png)
接下来是启动gitblit服务器了,启动命令如下
```
cd /usr/gitblit 
.gitblit.sh 在gitblit 目录下启动
java -jar gitblit.jar --baseFolder data 同上
```
如图所示就启动成功了
![avatar](/assets/img/2.png)

建好版本库后上传项目时发现只能用http协议，无法用ssh协议
这个原因是在linux系统中没有开启gitblit端口号的访问权限
```
    firewall-cmd --zone=public --add-port=29418/tcp --permanent 开启端口
    firewall-cmd --reload 重启防火墙后生效
```
如此gitblit服务器搭建成功。