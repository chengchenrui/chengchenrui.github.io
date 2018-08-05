title: Hexo+GitHub Pages 搭建属于自己的博客
date: 2018-03-29 21:59:00
tags: "Hexo"
categories: [Hexo,博客搭建]
---
# Hexo+GitHub Pages+Next 搭建博客
## 准备工作：
1. 安装 node.js
[https://nodejs.org/en/](https://nodejs.org/en/)  

2. 安装Git(此过程略去，程序员必备)  

3. 使用npm安装Hexo
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
![](Hexo_1/2018022200121435.png)  
看到这个样子就说明成功了，这个就是hexo默认的博客主题。现在你已经可以在这个主题下写博客了。
    
## 选择主题nexT
1. 在站点根目录输入git clone https://github.com/iissnan/hexo-theme-next themes/next：
```
E:\>cd blog
E:\blog>cd themes
E:\blog\themes>git clone https://github.com/iissnan/hexo-theme-next themes/next
```

2. 完成后，打开blog文件夹下的_config.yml文件，找到theme字段，把landscape改为next：
```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```

3. 在终端输入：
```
hexo clean  //清除缓存
hexo g  //重新生成代码
hexo s  //部署到本地
```
然后打开浏览器访问localhost:4000 查看效果。
nexT主题有三种选择，这个只是最简洁的一种，我们选择最好看的那个。
- Muse- 默认 Scheme，这是NexT最初的版本，黑白主调，大量留白。
- Mist -Muse 的紧凑版本，整洁有序的单栏外观。
- Pisces - 双栏 Scheme，小家碧玉似的清新。

4. 配置nexT
打开blog/themes/next/_congig.yml文件：
```
# Schemes
scheme: Muse
#scheme: Mist
#scheme: Pisces
#scheme: Gemini
```

5. 在终端输入：
```
hexo clean  //清除缓存
hexo g  //重新生成代码
hexo s  //部署到本地
```
打开浏览器访问 localhost:4000 查看效果，博客类似于下面这样：  
![](Hexo_1/blog_01.png)  

## 将本地博客上传到Github
创建一个新项目，项目必须要遵守格式：账户名.github.io，不然接下来会有很多麻烦。并且需要勾选Initialize this repository with a README。  
![](Hexo_1/blog_02.png)  
在建好的项目右侧有个settings按钮，点击它，向下拉到GitHub Pages，你会看到那边有个网址，访问它，你将会惊奇的发现该项目已经被部署到网络上，能够通过外网来访问它。  

### 将Hexo与Github Page联系起来
进入目标文件夹，在其文件夹里面鼠标右键，点击Git Base Here  
输入cd ~/.ssh，检查是否由.ssh的文件夹。
```
cheng@cheng MINGW64 /e/chengchenrui.github.io (Hexo)
$ cd ~/.ssh
```

输入ls，列出该文件下的内容。下图说明存在。
```
cheng@cheng MINGW64 ~/.ssh
$ ls
id_rsa  id_rsa.pub  known_hosts
```

输入ssh-keygen -t rsa -C “你的邮箱”，连续三个回车，生成密钥，最后得到了两个文件：id_rsa和id_rsa.pub（默认存储路径是：C:\Users\Administrator\.ssh）。  
![](Hexo_1/blog_03.png)  
输入eval "$(ssh-agent -s)"，添加密钥到ssh-agent。  
再输入ssh-add ~/.ssh/id_rsa，添加生成的SSH key到ssh-agent。  
登录Github，点击头像下的settings，添加ssh。  
新建一个new ssh key，将id_rsa.pub文件里的内容复制上去。  
输入ssh -T git@github.com，测试添加ssh是否成功。如果看到Hi后面是你的用户名，就说明成功了。  

### 配置Deployment，在其文件夹中，找到_config.yml文件，修改repo值（在末尾）
```
# 这是提交到github的配置，启用本地服务器，请注释:
deploy:
  type: git
  repository: git@github.com:username/username.github.io.git
  branch: master
```
repo值是你在github项目里的ssh  

### 新建一篇博客，在cmd执行命令：hexo new post "我的第一篇文章"  
这时候在文件夹_posts目录下将会看到已经创建的文件。  
在生成以及部署文章之前，需要安装一个扩展：npm install hexo-deployer-git --save  
使用编辑器编好文章，那么就可以使用命令：hexo d -g(也可以分开)，生成以及部署了。  
部署成功后访问你的地址：https://用户名.github.io。那么将看到生成的文章。 