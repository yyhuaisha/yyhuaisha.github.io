---
layout: post
title: Npm 与 移动端web开发调试
category: tech
tags: 技术
keywords: Npm,Weinre
---

### npm与换源
npm

全称Node Package Manager，是node.js的模块依赖管理工具。由于npm的源在国外，所以国内用户使用起来各种不方便。下面整理出了一部分国内优秀的npm镜像资源，国内用户可以选择使用。
常用的npm源主要有 

> 1. http://www.cnpmjs.org

> 2. http://npm.taobao.org

以下操作以淘宝npm为例。

镜像使用方法（建议使用第4种，将配置写死，下次用的时候配置还在）:

##### 1.通过config命令指向国内镜像源

``` python
npm config set registry https://registry.npm.taobao.org 
npm info underscore   //如果上面配置正确这个命令会有字符串response
```


##### 2.命令行指定下载源

``` python
https://registry.npm.taobao.org info underscore 
```


##### 3.通过cnpm
使用 

``` python
npm install -g cnpm --registry=https://registry.npm.taobao.org 
```


##### 4.在配置文件 ~/.npmrc 文件写入源地址

``` python
registry = https://registry.npm.taobao.org 
```

配置后壳通过以下指令判断是否配置成功：

``` python
npm config get registry // 或npm info express
```


### 移动端web开发远程实时调试
调试前端页面一直使用着神器 Chrome开发人员工具。但当我们要调试移动设备的Web页面时,就感觉有点力不从心了。平时开发时我们可以一直都是在桌面调试这个页面, 但最终的运行环境是移动设备, 最终还是要在手机上验证一下, 这个时候各种兼容性问题就有可能接踵而来。我们使用技术将手机网页调试信息分离，实现一种能在大屏幕、高配置PC上来调试小屏幕、低配置的手机浏览器访问的网页的开发工具——RemoteInspector（简称RI）。
以下提供几种解决的方法：


#### 1.移动端安装谷歌浏览器


##### 1.1 准备
PC端安装最新的chrome，手机端安装最新的chrome ( Android机 )。


##### 1.2 连接手机与PC
USB设置 在你的手机里打开"设置"->"开发人员工具"->"USB调试" 打开USB调试。


##### 1.3 调试方式
打开电脑的chrome 在地址栏输入 chrome://inspect 选中 Discover USB devices 可以检测到你的设备，勾选。打开手机上的chrome，也可以通过PC端chrome的 inspect里chrome后面那个输入框直接打开某个链接。可以点击弹出的审查元素框右上角的方形小图标切换到视图模式 这时会把你手机打开的页面拉到pc上显示。


#### 2.安装UC浏览器开发者版

##### 2.1 准备
下载Android平台的UC浏览器开发者版，安装到手机中；PC机一台，并在PC上安装Chrome或Safari（推荐使用Chrome）。支持Chrome15–Chrome21，以及Safari5.1.4以上版本。


##### 2.2 连接手机与PC
Android平台UC浏览器开发者版，远程调试支持USB连接、Wi-Fi连接两种模式。MAC平台请参考：UC浏览器开发者版使用手册(Android平台).pdf。


##### 2.3 调试方式
在手机上启动UC浏览器开发者版，并打开需要调试的页面；在PC上打开Chrome或Safari。

- 若是Wi-Fi连接模式，则在地址栏输入：手机IP+:9998。
例，手机IP为192.168.112.244，则输入192.168.112.244:9998。此时手机端的UC浏览器开发者版会弹出对话框。

- 若是USB连接模式，则在地址栏输入：http://localhost:9998。
成功访问该网址后，即可看到UC浏览器开发者版已打开索引页面。

更多内容请参考[UC浏览器开发者版使用手册(Android平台).pdf](http://plus.uc.cn/attachment/459)

#### 3. Weinre （[参考](http://www.cnblogs.com/yuzhongwusan/p/4277453.html)）

##### 3.1 安装
通过NPM来安装 weinre，命令行输入：

``` python
npm -g install weinre
```


##### 3.2 启动
命令行输入：

``` python 
weinre -httpPort 8081 -boundHost -all-
```


##### 3.3 添加脚本

``` python
<script src="http://你的IP地址:8081/target/target-script-min.js#anonymous"></script>  //将此脚本加入要调试的页面中
```


##### 3.4 调试方式
[http://localhost:8081](http://localhost:8081/)，在 weinre Access Points 中开始远程调试，点击anonymous Access Points。