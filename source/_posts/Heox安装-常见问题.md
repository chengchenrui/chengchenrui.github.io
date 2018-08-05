---
title: Heox安装及常见问题
date: 2018-04-04 12:18:26
tags: "Hexo"
categories: Hexo
---
# Hexo安装
## 安装node.js
- [node.js下载](https://nodejs.org/en/)
- 从上面的链接下载node.js，并安装。  

## 设置npm淘宝镜像站
- npm默认的源的下载速度可能很慢，建议使用淘宝镜像替换。
- 执行下面的命令，将npm的源设置成淘宝镜像站。
```
npm config set registry "https://registry.npm.taobao.org"
```

## Github相关配置
### 配置ssh
- 生成ssh密匙
```
ssh-keygen -t rsa -C "你的GitHub注册邮箱"
```
- 此时，在用户文件夹下就会有一个新的文件夹.ssh，里面有刚刚创建的ssh密钥文件id_rsa和id_rsa.pub

### 将公匙添加到github上
- 详细教程自行baidu
- 用户头像→Settings→SSH and GPG keys→New SSH key→将id_rsa.pub中的内容复制到Key文本框中，然后点击Add SSH key(添加SSH)按钮

## 安装hexo
- 执行以下命令安装hexo
```
# 安装hexo
npm install hexo-cli g
# 初始化博客文件夹
hexo init blog
# 切换到该路径
cd blog
# 安装hexo的扩展插件
npm install
# 安装其它插件
npm install hexo-server --save
npm install hexo-admin --save
npm install hexo-generator-archive --save
npm install hexo-generator-feed --save
npm install hexo-generator-search --save
npm install hexo-generator-tag --save
npm install hexo-deployer-git --save
npm install hexo-generator-sitemap --save
```

## 初探hexo
- 第一次使用hexo，在本地创建服务器使用
```
# 生成静态页面
hexo generate
# 开启本地服务器
hexo s
```
- 打开浏览器，地址栏中输入：http://localhost:4000/,应该可以看见刚刚创建的博客了。
- 问题：为什么访问http://localhost:4000/，无反应？
    - 解决方法：可能是由于端口问题引起的。使用Ctrl+C中断本地服务，使用命令hexo s -p 5000重新开启本地服务，访问http://localhost:5000/可以看到博客页面了。

## 将hexo博客部署到github上
- 修改配置文件blog/_config.yml，修改deploy项的内容，如下所示：
```
# Deployment 注释
## Docs: https://hexo.io/docs/deployment.html
deploy:
  # 类型
  type: git
  # 仓库
  repo: git@github.com:username/username.github.io.git
  # 分支
  branch: master
```
- 注意：type: git中的冒号后面由空格。
## 部署hexo
- 输入下面的命令将hexo博客部署到github中：
```
# 清空静态页面
hexo clean
# 生成静态页面
hexo generate
# 部署 
hexo deploy
```
- 打开网页，输入http://username.github.io，打开github上托管的博客。如我的博客地址是：http://chengchenrui.github.io

## hexo命令缩写
- hexo支持命令缩写，如下所示。hexo g等价于hexo generate
```
hexo g：hexo generate
hexo c：hexo clean
hexo s：hexo server
hexo d：hexo deploy
```

## hexo组合命令
```
# 清除、生成、启动
hexo clean && hexo g -s
# 清除、生成、部署
hexo clean && hexo g -d
```

## 常见问题
### hexo deploy没有反应？
- 修改配置文件：_config.yml时，冒号后面没加空格。

### hexo s 网站打不开？
- 端口占用，换个端口就好了。执行命令hexo s -p 5000，并在浏览器地址栏输入http://localhost:5000，回车访问。

### 如何换主题？
- 将主题下载后，放到themes文件夹中即可。例如，下面命令安装next主题：git clone https://github.com/iissnan/hexo-theme-next themes/next