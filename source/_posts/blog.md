---
title: Github+Hexo 个人博客搭建过程（基础）
date: 2021-03-30 19:16:45
tags: 踩坑
---

# 简单的Github+Hexo 个人博客搭建过程

昨天心血来潮，突然想搭一个自己的博客。然后说干就干，昨天晚上尝试了一下Github+jekyll，过程就不说了，一路辛酸，各种错误，不想回忆。

本来不想弄了，结果今天上午尝试了一下Hexo，诶？这个好像蛮简单，虽然后面也掉进去了挺多的坑，但参考了挺多的教程，结果总还是好的。至少有了属于自己的博客~

晚上吃了一顿饭，回来竟然快忘了过程，啊~ 赶紧记录下。

**参考链接**

```
https://zhuanlan.zhihu.com/p/26625249
```

---

好啦，下面来记录一下配置的过程：

### 1、Github 创建个人仓库

首先，恩，默认大家有配置好的Github账户了哈~

- 个人仓库的命名应该为：**Github账户名.github.io**，然后可以直接next创建仓库。

```
注：Github账户名一定输入对喔~ 如果输错了就去仓库的setting里改呗
```

<div align=center> {% asset_img 1.png This is an example image %}



### 2、安装Node

- node下载网站：

  ```
  https://nodejs.org/en/download/
  ```

  下载慢的话可以尝试下面这个（如果还可以用的话）

  ```
  http://nodejs.cn/download/
  ```

- 安装完成后可以使用如下的命令测试node.js及npm是否安装成功：

  ```
  node -v
  npm -v
  ```



---

有木有很简单~

---

### 3、安装Hexo

- 首先需要在电脑上选择一个空文件夹，在这个文件夹中打开命令行

  小tips：在文件夹路径中输入cmd，回车即可~

- 使用npm命令安装Hexo：

  ```
  npm install -g hexo-cli
  ```

- 安装完成后，进行初始化

  ```
  hexo init blog
  ```

- 此时，会多出blog文件夹，点击进入，此时我们就可以开始测试我们的博客雏形了，按序输入如下命令：

  ```
  hexo new test_my_site
  hexo g
  hexo s
  ```

- 完成后，在浏览器中输入

  ```
  localhost:4000
  ```

- 有没有看到属于你的博客~

### 4、 关联Github

- 打开blog文件夹下的**_config.yml**，将文件的最下方deploy改成如下形式：

  repo为第一步新建的Github仓库

  branch为master（后面如果修改git分支不要忘了改喔）

<div align=center> {% asset_img 2.png This is an example image %}

- 保存站点配置文件

- 安装Git部署插件，命令行中输入

  ```
  npm install hexo-deployer-git --save
  ```

- 安装完成后接着输入

  ```
  hexo clean
  hexo g 
  hexo d
  ```

- 此时，我们已经将网站部署好了。打开浏览器，输入**xxxx.github.io**(xxxx为Github用户名)

- 是不是感觉很有成就感~

### 5、更换主题

来弄这个的是不是都看到了别人的高端的博客，酸了半天？大家肯定不满足于Hexo默认的主题，好，让我们现在来挑一个beautiful的主题~

主题传送门： https://hexo.io/themes/

我们可以在这上面挑选自己喜欢的主题，啊，不知道你们有木有找到怎么下载，反正我没有。好叭，只能换种方法了

- 在Github网页中使用关键字搜索**hexo-theme-xxxx**  (xxxx为你想要的主题的名字)，恩，大概率可以找到，反正我都找到了，如果你没找到，那那那去找找别的方法吧，害~

  教程中大多数都是用的Next主题，我用的是black-blue，虽然大致相同，但主题的配置还是有点不同的，大家因主题而异啦。

- 在Github中copy主题的git地址，然后在blog目录的命令行中输入

  ```
  git clone git地址 themes/文件夹名称
  以tomotoes主题为例：git clone https://github.com/Tomotoes/hexo-theme-tomotoes.git themes/tomotoes
  ```

  注：也可以直接去themes文件夹下直接clone，但一定要在themes文件夹下哈，名字也可以不用改，后面记得对应就好啦~

- 再次打开_config.yml文件，修改themes为clone的文件夹的名字：

  <div align=center> {% asset_img 3.png This is an example image %}

- 再次执行第4步中的三条命令，将新的主题部署好，再去浏览器中看一下，有咩有满足你呢？

- 然后就可以去主题文件夹下的_config.yml文件中修改一些简单的配置啦，照着网页改就好啦，像改自己头像啦、改网页背景啦、网站名字图标啦，加油，我相信你可以的~~~

### 6、 绑定域名

- 在下就是简单的弄一下，并没有买域名，害
- 有需求的可以移步其他教程啦

### 7、 写第一篇博客

- 首先，先满足一下博客中必不可少的插入图片的小需求，打开站点的_config.yml文件，将 post_asset_folder 改为 true。

- 在blog目录的命令行中输入

  ```
  hexo new xxx
  ```

  此时，source/_posts文件夹下会新建一个xxx文件夹下和一个xxx.md，此时我们就可以编辑xxx.md，写自己的博客内容啦~

  其中需要的图片不能通过常规的markdown语法插入，参考如下的标签插件

  ```
  {% asset_img example.jpg This is an example image %}
  ```

  其中example.jpg为图片的名字，后面为图片的描述。

- 编辑完成后，再次执行第4步中的三条命令，就可以将文章部署出去啦

- 其中部署push的过程中，可能会报错，类似于github的timeout，多尝试几次就可以啦，不是push的时候的错不能靠多次尝试呢

### 8、多台电脑的hexo博客同步实现

如果大家想在自己的电脑上和公司或者实验室的电脑上同时编辑博客怎么办呢？

**参考链接**

```
https://blog.csdn.net/weixin_43669250/article/details/103588588
```

- 网上最多的办法就是新建一个分支，通过在GitHub上新建一个分支，来保存本地的原始文件，另一个分支来保存hexo生成的静态网页。

  

**假设要从电脑A将博客迁移到电脑B**

**在电脑A上**

- 首先在仓库中新建一个分支*Hexo*，并且把它设置为**默认分支**，来保存本地的原始文件。此时该仓库有两个分支，一个是原来的存静态页面的仓库*master*，一个是新建的*Hexo*。

- 在git bash中执行

  ```
  git clone https://github.com/username/username.github.io.git
  ```

  将hexo分支拷贝到本地。

- 将本地文件夹*username.github.io*文件夹里的所有文件删除，仅保留*.git*文件夹。

- 将之前的blog目录拷贝至*username.github.io*文件夹下，一定要将*.gitingore.txt*拷贝过来。

- 如果使用了主题，在主题文件夹下可能有*.git*的文件夹，要删除掉，否则无法push到GitHub上。

- 依次执行git add .、git commit -m “”、git push origin Hexo将本地文件push到hexo分支上。

**在电脑B上**

- 准备好之前建博客需要的Git、node.js等步骤。

- 在git bash中执行

  ```
  git clone https://github.com/username/username.github.io.git
  ```

  将仓库克隆到本地，因为之前设置了默认分支，所以此时克隆的就是Hexo分支。

此时，我们在Hexo分支上储存静态网页代码，master分支上对网页进行部署。

**在平时修改博客的时候，除了像以往用hexo g等指令发布，也要git add .、git commit -m “”、git push origin Hexo将改动推送到GitHub上。这样我们就实现了博客文件的版本控制，可以随时随地得修改博客了。**

---

同步功能目前仅支持代码同步，也就是在电脑B上只能修改文件，但不能进行网站部署，只能将代码同步到电脑A上，在进行部署，有待解决。

---

<br>

<br>

好啦，搞了一天，只记得这么多了，希望对大家有帮助~

<div align=center> {% asset_img cy.jpg This is an example image %}