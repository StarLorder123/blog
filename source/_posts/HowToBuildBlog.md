---
title: 使用Hexo搭建静态博客
description: 如何使用Hexo和Github搭建属于自己的静态博客
keywords: 'Hexo,博客搭建'
top_img: /HowToBuildBlog/1-2.jpg
cover: /HowToBuildBlog/1-2.jpg
tags:
  - Operation Guide
categories:
  - 月光赶海
abbrlink: 48dba841
date: 2024-09-29 22:30:55
---
# 写在前面
- 一直以来都有自己建博客的想法，买过服务器，也使用过wordpress、halo。wordpress、halo这样的框架适合建社区式内容网站，用于个人博客网站的搭建有些大材小用了。
- 兜兜转转还是回到了使用hexo搭建静态网站，然后使用github进行托管。

# 环境搭建
## Nodejs 安装
- 按照Nodejs官网的指导安装即可
- [Nodejs官方网站](https://nodejs.org/en/)

## git 安装
- 按照git官网的指导安装即可
- [git官方网站](https://git-scm.com/)

## 安装Hexo
```shell
 npm install hexo-cli -g # 安装Hexo
 hexo -v # 返回Hexo版本，确定安装成功
```
- 如果没有梯子，那可以切换到国内的淘宝源

```shell
npm config set registry https://registry.npm.taobao.org # 将npm源替换为阿里的镜像，安装更快
```

# 搭建博客
## 初始化本地博客
- 在本地创建一个目录，然后运行下述命令

```shell
 hexo init # 初始化博客
 hexo generate # 生成网站信息
 hexo server # 开始网站服务
```
- 然后在浏览器中输入 localhost:4000 , 就可以通过本地浏览器看到效果了。

![效果图](./HowToBuildBlog/1-1.png)

- 在_config.yml文件中，还需要改一个位置，那就是下面代码中的url，需要改成设置的github仓的地址，否则会出现找不到对应图片、代码的情况

``` yaml
# URL
## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
url: https://username.github.io/projectname/
permalink: :year/:month/:day/:title/
permalink_defaults:
pretty_urls:
  trailing_index: false # Set to false to remove trailing 'index.html' from permalinks
  trailing_html: false # Set to false to remove trailing '.html' from permalinks
```

## github仓库创建
- 在github上创建一个仓库，可以任意命名
- 然后对这个仓库进行初始化操作，并且建立gh-pages分支。
- 修改你博客根目录下的_config.yml文件中的deploy配置项：

```yaml
 deploy:
   type: git
   repository: git@github.com:username/username.github.io.git  # 你的仓库地址
   branch: gh-pages
```
- 他会在你的仓库的gh-pages分支部署你的页面，然后向master（或者main）分支保存你网站的源代码，不会相互覆盖。
- 注意在上传部署你的页面之前，需要将对应的分支创建好，否则上传会失败。
- 在github中添加ssh秘钥：
    - 1）在本地生成ssh秘钥，使用 ssh-keygen -t rsa -C "My-SSH" 在shell中执行便可。My-SSH位置替换成对应的github邮件地址。
    - 2）按三个回车就可以，然后在对应的地址下找到秘钥文件
        - 在Linux系统下的路径一般是：/home/username/.ssh/id_rsa.pub。
        - 在macOS系统下的路径一般是：/Users/username/.ssh/id_rsa.pub。
        - 在Windows系统下的路径一般是：C:/Users/username/.ssh/id_rsa.pub。
    - 3）打开对应的.pub文件，将文件内容添加到github账户下的settings中去。
        - 具体的位置就是：点击用户->settings->SSH and GPG keys->将.pub文件中的秘钥添加进去
- 通过如下代码进行部署：

```shell
 hexo clean # 清除缓存
 hexo generate # 生成网站
 hexo deploy # 部署至远程网络

  # 简写
 hexo cl
 hexo g
 hexo d

  # 简简写
 hexo cl
 hexo g -d  # hexo d -g
```
## 参考链接
[hexo文档](https://hexo.io/zh-cn/)
[【个人博客】Hexo+Github搭建个人博客](https://zhuanlan.zhihu.com/p/675680355)

# 个性化博客
## 挑一个你适合的主题
[流行的自定义主题](https://pengtech.net/hexo/hexo_theme_recommendation.html)

- 我选择的是butterfly主题。这里是[butterfly主题的说明文档](https://butterfly.js.org/)