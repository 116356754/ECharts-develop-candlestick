# ![](/assets/Titan.png) TitanOne

---

## 写在最前面

这里主要是介绍基于开源项目Electron来开发的TitanOne框架，在框架中介绍了TitanOne的基本的原理和基本使用方法，以及我们自己开发的websocket通讯组件和消息提示等组件，最后给出了关于项目的打包和自动更新方面的内容，我们围绕着TitanOne来构建桌面应用程序涉及到的底层问题来介绍。

## TitanOne能干吗

TitanOne是基于Electron 1.2.5版本开发的，Electron集成了Chrominum浏览器和Node.js,并且提供了丰富的本地（操作系统）的API，能够使用HTML，CSS和纯JavaScript来创建桌面应用程序。这并不意味着Electron是一个绑定图形用户界面（GUI）的JavaScript库，取而代之的是，Electron使用Web页面作为它的图形界面，所以你也可以将它看作是一个由JavaScript控制的迷你的Chrominum浏览器。

## 本书主要内容

* 第一章：主要介绍Electron和传统桌面应用程序开发的不同，介绍Electron的基本组成，以及制作第一个Electron应用程序。

* 第二章：主要介绍Electron中如何划分主进程（main process）和渲染进程（renderer process），以及两者之间如何通讯。

* 第三章：主要介绍Electron中如何利用node的组件，以及如何开发基于C++开发Node组件。

* 第四章：介绍我们基于ws的node组件开发的elws，实现websocket的多地址与断线重连功能。

* 第五章：介绍我们自己开发的消息提示组件elnotifier，实现了多个消息提示框，同时附带提示音功能。

* 第六章：介绍如何对一个完整项目的图标资源更换，应用打包成asar。

* 第七章：完整安装包的制作，制作windows平台下的完整安装包。

* 第八章：增量安装包的制作，将应用的asar包转账成windows平台下的exe安装包。

* 第九章：自动更新的实现，介绍earyt-elupdater组件配合服务端的更新SVersion.json进行自动更新。

* 第十章：介绍webview标签的某些TitanOne特性。



