# 附：Mac 系统操作 Vue 的权限问题

## 概述

MacOS 很多命令都需要加上 sudo 来执行，这是因为 Mac 本身的保护机制，需要取得管理员权限。那么问题来了，创建的文件无法操作，不能修改，简单来说：只读。要解决这个问题其实很简单，但是每次都需要手动来修改权限：

![img](./img/20180209102429850.jpg)

有件选中文件夹，记住，使整个工程的文件夹，先是简介，最下面，如图，全都修改成读与写，这只是第一步，第二步：

![img](./img/20180209102802110.jpg)

然后再去操作你的 Vue 文件就不会再报错了。

## 原文链接

https://blog.csdn.net/codingfire/article/details/79295940