---
title: Win10 通过 445 端口映射访问公网smb服务
date: 2021-04-05 16:11:50
tags: [踩坑, 环境]
---

# Win10 通过 445 端口映射访问公网smb服务

实验室新配了一台文件服务器，不过没放在实验室。。服务器做了samba服务，想要在实验室访问，只能通过smb服务，这对使用mac和linux系统的人就很舒服啦。mac直接在服务器搜索器中填写搜索的ip地址（例如：smb://11.10.11.55），然后连接，输入用户名和密码就好啦；linux应该是使用 -p 选项指定端口号的方式直接公网访问samba服务（这个没试）。但是对实验室的windows电脑就很头大，windows因为黑客攻击、比特币勒索等原因禁用了445端口。所以在win下不能通过正常方式访问，需要用端口转发实现访问。



好叭，划重点！！！

## 原理：

在本地访问\\\127.0.0.1\共享名，然后系统会在本地检索samba服务，但是根据设置的端口转发，侦听到有来自445端口的请求时，会自动转发到设定好的目标服务器ip地址和目标端口来访问，简单说就是用本地访问服务器的非标端口。

## 步骤：

1. 假设我们已经有一个搭建好非标端口的samba服务，端口为1234，公网地址为111.111.111.111(假设)。

2. 释放445端口：

   若是你没有用过“某卫士”的话，你的 445 端口应该是一直被 LanmanServer 占用的，这个 LanmanServer 就是帮你把你电脑的文件分享给别人。所以你要把这个服务禁用了（网上如是说~）

   - 使用管理员身份打开cmd，分别运行如下命令：

     ``` shell
     sc config Lanmanserver start= disabled
     
     net stop Lanmanserver
     ```

     关闭Lanmanserver服务。

   - 启动 windows 的 ip helper 服务（一般来说，不用这一步，基本都是开着的）。这个 ip helper 服务，就是用来搞端口转发的，没有了它就没法转发了。

     ```shell
     sc config iphlpsvc start= auto
     ```

     **注：要是运行后没有显示"成功"二字，那是可能你没有用管理员权限运行吧~**

3. 端口转发：

   - 使用管理员身份打开cmd，运行如下命令：

     ```shell
     netsh interface portproxy add v4tov4 listenport=445 listenaddress=127.0.0.1 connectport=1234 connectaddress=111.111.111.111
     ```

     地址和端口记得改哦~

   - 查看是否设置成功

     ```shell
     netsh interface portproxy show all
     ```

     - 应该显示如下哦

     <div align=center> {% asset_img 1.jpg This is an example image %}

     ```
     netstat -ano|findstr "445"
     ```

     - 此时可能没有显示，需要重启电脑。（等下再重启啦）

   - 如果不对的话，可以使用下面的命令重新设置哦

     ```
     netsh interface portproxy reset (删除所有)
     or
     netsh interface portproxy delete v4tov4 listenport=1234 listenaddress=111.111.111.111 (删除一个)
     ```

   - 打开控制面板，然后点击程序，找到打开或关闭Windows功能，找到smb 1.0 ，全选，全部安装。

   <div align=center> {% asset_img 5.jpg This is an example image %}

   - 重启电脑

     - 运行上面那段代码

       ```
       netstat -ano|findstr "445"
       ```

     - 应该显示如下哦，主要是第一条！！

       <div align=center> {% asset_img 2.jpg This is an example image %}

     - 打开任务管理器，上图中的进程号对应的任务名称与服务应该分别为svchost.exe与iphlpsvc

       <div align=center> {% asset_img 3.jpg This is an example image %}

       <div align=center> {% asset_img 4.jpg This is an example image %}

   - 一般来说，到这一步就可以啦，然后就可以去映射驱动器了~

     - 右击此电脑，选择映射网络驱动器，输入文件夹路径，例**\\\\127.0.0.1\\work**，选择使用其他凭据连接，完成后输入用户名和密码就ok啦！

     - 效果如下

       <div align=center> {% asset_img 6.jpg This is an example image %}

## 问题：

不知道是哪的问题，在win上总会有写奇奇怪怪的错误，如上设置应该是没问题的，但在有些电脑上就是不可以，时好时坏。

- 对于关机重启后失效的情况，解决办法为将端口映射删除后再重新映射一下就可以了，不需要重启电脑。
- 有时查看端口转发的状态只有*ESTABLISHED*，emmmm，建立了连接但没有监听，还有待研究呀。