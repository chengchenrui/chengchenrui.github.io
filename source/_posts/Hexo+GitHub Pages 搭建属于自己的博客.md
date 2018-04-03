title: Hexo+GitHub Pages 搭建属于自己的博客
tags: []
categories: []
date: 2018-03-29 21:59:00
---
# Hexo+GitHub Pages+Next 搭建博客
## 准备工作：
1. 安装 node.js
[https://nodejs.org/en/](https://nodejs.org/en/)


2. 安装Git(此过程略去，程序员必备)


3. 使用==npm==安装==Hexo==
```
npm install -g hexo-cli
```
## 开始搭建博客
1. 随便找个文件夹打开终端（我是在E盘），输入：
```
hexo init blog//blog是项目名
cd blog //切换到站点根目录
npm install //安装
hexo server //开启服务
```
2. 打开浏览器输入localhost:4000查看：
    
## 选择主题nexT
1. 在站点根目录输入git clone https://github.com/iissnan/hexo-theme-next themes/next：
```
E:\>cd blog
E:\blog>cd themes
E:\blog\themes>git clone https://github.com/iissnan/hexo-theme-next themes/next
```
2. 完成后，打开blog文件夹下的_config.yml文件，找到theme字段，把landscape改为next：

3. 在终端输入：
```
hexo clean  //清除缓存
hexo g  //重新生成代码
hexo s  //部署到本地
```

