# 第二章 探花交友 - 环境搭建

## 概述

`react-native` 的环境搭建相比较于普通的web项目(vue、react）来说要繁琐不少，但是通过以下的方式基本都可以得到解决。

- 老师的文档
- [RN的官网文档](https://reactnative.cn/docs/getting-started.html)
- 百度+谷歌

## 安装环境介绍

- 操作系统：win10专业版
- 手机：`安卓手机真机`一部或夜神模拟器
- 必须安装的依赖有:Node、JDK、Yarn、Android SDK、Python2



## Node的安装

- 先到 [官网 ](http://nodejs.cn/)去下载node版本(使用 [nvm](https://github.com/coreybutler/nvm-windows) 工具来安装也可以)
- 老师当前是用的 **12.16.3** 版本
- 以 **管理员** 身份安装 然后一直点击下一步即可



## [Yarn](https://yarn.bootcss.com)的安装

Yarn是Facebook提供的替代npm的工具，可以加速node模块的下载

```js
npm install yarn -g  // 使用npm全局安装yarn 
```

检查是否安装成功

```js
yarn -v
```

**效果如下:**

![image-20200522155320956](medias/image-20200522155320956.png)



## JDK的安装与配置

> Java SE Development Kit

安卓系统的APP离不开JAVA环境，因此需要下载安装JDK(1.8版本)。到[该网站](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)下载JDK

![image-20200522155509416](medias/image-20200522155509416.png)

需要注意的是单击下载之后，会跳转到一个Oracler的登陆页面，得登陆之后才可以下载，如果没有账号可以注册一
个，也是比较简单，下载完成之后，以管理身份进行l默认安装。

### JDK的环境变量配置

1.右键我的电脑，点属性

![image-20200522161608862](medias/image-20200522161608862.png)

2.然后单击高级系统设置，在弹出来的对话框中单击高级，再单击环境变量

![image-20200522161628275](medias/image-20200522161628275.png)

3.在弹出来的对话框中设置如下

![image-20200522161647214](medias/image-20200522161647214.png)

4.然后在 **系统变量** 中找到path项单击，然后再单击下面的编辑，在弹出框中设置如下

![image-20200522161712608](medias/image-20200522161712608.png)

5.到此JDK的环境变量设置完毕，可以再次打开命令行终端，使用命令 `java` 与 `javac` 检测一下是否设置
成功。

![image-20200522161731987](medias/image-20200522161731987.png)

---

![image-20200522161746069](medias/image-20200522161746069.png)

## Android SDK的下载与安装

我们可以直接下载Android SDK并进行必要配置 
1.首先打开 [网站](https://www.androiddevtools.cn/)，然后一直向下拉，找到 SDK Tools 进行下载

![image-20200522161947215](medias/image-20200522161947215.png)

2.以管理员身份安装此软件，设置 允许使用此计算机的所有人 选项，其它一路默认到底，直到安装完成.
切记，切记，切记，重要的事情说三遍，一定记着Android SDK的安装路径，后面会打开这个管理器下载东
西

![image-20200522162022143](medias/image-20200522162022143.png)

## Android SDK的下载项

1.根据RN中文网的描述，编译React Native应用需要的是Android 9版本的SDK，还需要各种组件，为
了当前以及后期的稳定，总结起来，总共需要下载:

- Android SDK Tools 25.2.5
- Android SDK Platform-tools 29.0.5
- Android SDK Build-tools 29
- Android SDK Build-tools 28.0.3
- Android SDK Build-tools 28.0.2
- Android SDK Build-tools 28.0.1
- Android SDK Build-tools 28
- Android SDK Build-tools 27
- SDK Platform 29
- Intel x86 Atom System Image 29
- SDK Platform 28
- Intel x86 Atom System Image 28
- SDK Platform 27

2.接下来，打开`SDK Manager.exe`，照此截图依次下载以上SDK及组件

![image-20200522162144179](medias/image-20200522162144179.png)

---



![image-20200522162154907](medias/image-20200522162154907.png)

---

![image-20200522162228056](medias/image-20200522162228056.png)

---

![image-20200522162257510](medias/image-20200522162257510.png)

3.此下载的过程取决于自家的网速了，不过一般都会成功的，耐心等待安装完成即可。

## Android环境变量设置

1.右键选中 此电脑 点属性，再点高级，再点环境变量，设置如下

![image-20200522162417205](medias/image-20200522162417205.png)

---

![image-20200522162431658](medias/image-20200522162431658.png)

2.到此，安卓的环境变量配置完成

## 初始化项目和打包APP到手机

1. 准备一台  Android 手机, 通过数据线 连接 到电脑，设置启用 USB调试

2. 如果没有安卓手机，可以使用安卓模拟器也可以，推荐使用 夜神模拟器 ，自行百度下载安装

3. 一般的手机在 设置 中可以直接找到 开发者选项 进行开启, 如果 找不到 , 就自行百度查一下

   ![image-20200522170648131](medias/image-20200522170648131.png)

4. 手机连接电脑成功后运行检测命令  `adb devices` , 如果有输出设备列表与  ID 相关的字符串就证明
   手机和电脑是连接成功了，如果没有显示设备号，则说明连接有问题，一定要保证手机和电脑是正常连接状态

   ![image-20200522170726891](medias/image-20200522170726891.png)

5. 运行  `npx react-native init 项目名称` 命令初始化一个  React-Native 项目, 创建时需要联网 下载 依赖
   包, 可能比较慢，取决于各自的网速，一定要耐心等待，如果出错了，则重新运行命令再次初始化即可，例
   如：

   ```js
   npx react-native init myApp 
   ```

6. 使用 `cd myApp` 命令进行此项目文件夹，确保手机和电脑连接正常的情况下，然后再输入命令 ``adb
   devices `来检测一下手机是否正常连接，然后再使用命令 `yarn android`   将APP打包到手
   机上

7. 手机上出现如下画面，说明打包成功

   ![image-20200522170849831](medias/image-20200522170849831.png)

## 手机屏幕投影工具

为了在电脑上看到真实的手机屏幕,可以安装手机屏幕投影工具

网上有很多工具,百度即可看到。老师使用的是  [scrcpy](https://github.com/Genymobile/scrcpy)

### 使用教程

1. 下载好 工具 

2. 手机通过usb连接到电脑

3. 双击打开工具即可

   ![image-20200522172425782](medias/image-20200522172425782.png)

   

   
