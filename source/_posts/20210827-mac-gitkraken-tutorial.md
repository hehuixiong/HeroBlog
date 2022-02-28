---
title: Mac 破解GitKraken教程
date: 2021-08-27 15:08:00
sticky: 1
description: 本篇文章介绍了Mac版GitKraken的破解方法
keywords: GitKraken
cover: https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20220224085359.png
photos: https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20220224085359.png
categories:
  - Mac
tags:
  - gitkraken
---

## GitKraken是什么？

> GitKraken是基于Git代码管理的一个UI管理器，拥有非常精美的界面，可以配合Github、Gitee来使用。

## 下载GitKraken

我们选择在GitKraken官网安装最新版本的[GitKraken](https://www.gitkraken.com/)，在官网安装的是收费的，不要管，先安装一遍并且登录一次
<font color='red'> ps：一定要用收费版登录一次，可以免去以后使用破解版时每次打开都要登录 </font>

## 安装GitKraken

下载完成后，即可得到一个installGitKraken.dmg文件，双击安装即可，安装完成之后，双击图标打开它。
打开之后会出现7天试用期，需要在7天后进行收费，不用管它，打开过就可以关闭了。
ps：如果以上步鄹都完成了，就可以跳过直接使用破解版的即可。
Vue3
## 使用破解版的GitKraken

破解版的GitKraken，我存放在了百度网盘，大家直接下载即可版本是@7.0.0
链接: https://pan.baidu.com/s/1VWWlRZNtKw_41x7x-L-k2A 提取码: giko
下载完成之后，双击打开，里面有一个@7.0.0版本的GitKraken，把它拖动到mac访达里的应用程序，然后替换掉。

![](https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20220224085401.png)
<font color='red'> 注：双击打开它的时候，会出现文件已损坏（这个时候别将它移到垃圾桶），执行后面的操作 </font>

## “Mac应用”已损坏，打不开解决办法
![](https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20220224085402.png)

## 问题说明：
通常在非 Mac App Store下载的软件都会提示“xxx已损坏，打不开。您应将它移到废纸篓”或者“打不开 xxx，因为它来自身份不明的开发者”。
![](https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20220224085403.png)

## 原因：
Mac电脑启用了安全机制，默认只信任Mac App Store下载的软件以及拥有开发者 ID 签名的软件，但是同时也阻止了没有开发者签名的 “老实软件”

## 解决方法：
#### 1.打开任何来源选项
打开「终端.app」，输入以下命令并回车，输入开机密码回车
```
sudo spctl --master-disable
```
此行代码可以让 Mac 允许安装第三方来源的应用
![](https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20220224085404.png)

#### 2.macOS Catalina 10.15系统：
打开「终端.app」，输入以下命令并回车，输入开机密码回车
```
sudo xattr -rd com.apple.quarantine 空格 软件的路径
```
如Sketch.app
```
sudo xattr -rd com.apple.quarantine /Applications/Sketch.app
```
如CleanMyMac X.app
```
sudo xattr -rd com.apple.quarantine /Applications/CleanMyMac X.app
```
附1：
/Applications/Sketch.app
与
/Applications/CleanMyMac X.app
就是
软件的路径
附2：
软件路径快速获取方法：
将软件拖入「终端app」即可获得路径
![](https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20220224085405.gif)
```
1
```

#### 3.macOS Catalina 10.15.4 系统：更新10.15.4系统后软件出现意外退出，可按照下面的方法给软件签名
- 1.打开「终端app」输入如下命令：
```
xcode-select --install
```
- 2.给软件签名
打开终端工具输入并执行如下命令：
```
sudo codesign --force --deep --sign - (应用路径)
```
![](https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20220224085406.png)
注意：空格不能漏
![](https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20220224085407.png)
- 3.错误解决
如出现以下错误提示：
<font color='red'> /文件位置 : replacing existing signature </font>
<font color='red'> /文件位置 : resource fork,Finder information,or similar detritus not allowed </font>
那么，先在终端执行：
```
xattr -cr /文件位置（直接将应用拖进去即可）
```
然后再次执行如下指令即可：
```
codesign --force --deep --sign - /文件位置（直接将应用拖进去即可）
```