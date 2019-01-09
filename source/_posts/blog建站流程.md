---
title: blog建站流程
date: 2018-09-11 00:00:00
tags: [hexo,travis-ci]
categories:
---

# 技术栈hexo + travis-ci + hexo next 主题

# hexo 快速搭建博客
hexo建blog很简单

流程：

```bash
npm install hexo-cli -g

npm install hexo-deployer-git --save

```
<!-- more -->
hexo-cli 安装hexo
hexo-deployer-git 是一个git组件，用到hexo d 时候没有的话，会报错

```bash
hexo init blog #创建hexo环境的blog目录

cd blog  #进入blog目录

hexo clean #清楚缓存，虽然第一次没缓存，但是后续会有，加上没有错的

hexo g #生成静态文件，其实直接可以下一步命令的，要是将文件发布到github还是要进行这一步

hexo s #创建一个http://localhost:4000的访问目录，修改配置文件css，html都不需要重启访问(我踩的坑，填一下)
 
```
以上就可以搭建出来一个blog

# github发布自己的线上项目

>使用travis-ci的话，可以跳过这一步

配置git密钥

```bash
cd ~ #切换根目录
ssh-key -t rsa -b 4096 -C "465298643@qq.com" #换成自己的邮箱
#一路回车就会创建.ssh目录
cd .ssh/ #查看

clip < ~/.ssh/id_rsa.pub #拷贝公钥

```

打开[打开github](https://www.github.com)这里默认你已经有了github账户，没有的话自行百度注册教程

点击自己的头像 -> Settings -> SSH and GPG keys -> New SSH key -> 粘贴刚才拷贝的公钥

测试

```bash
ssh -T git@github.com  #不用修改什么，直接粘贴就行

git config --global user.name "DEBUG" #随便用户名
git config --global user.email  "*******@qq.com" #填写自己的邮箱(随便什么邮箱)

```
这样就ok了。
然后创建一个仓库，这里就不写了，网上很详细的



打开本地主配置文件 blog/_config.yml

```bash
url: demoorbug.github.io/blog  #更改成你项目的目录


deploy:
  type: git
  repo: git@github.com:DemoorBug/blog.git #github仓库地址，就是你创建仓库的地址，可以打开github进入仓库点击绿色按钮clone or download -> Use SSH
  branch: gh-pages # github分支
```
打开命令行进入本地blog目录输入

```bash
hexo g  # 生成静态文件
hexo d  #部署网站到你的github

```
# 主题美化，我用的hexo-theme-nex 主题
先进入我们的blog目录
```bash
git clone https://github.com/iissnan/hexo-theme-next themes/next

theme: next

```
打开主配置文件 _config.yml
修改参数为:theme: next
然后执行一下代码
```bash
hexo clean #清除缓存
hexo g     #生成静态文件
hexo s     #预览

hexo d    #部署线上
```
浏览器输入 http://localhost:4000/

[next主题官网](http://theme-next.iissnan.com/)

hexo发布文章
```bash
hexo new blog #生成一个.md文件，编写后可以hexo g直接生成静态文件，要是你启用了hexo s 就不用那么麻烦了，直接刷新网页就可以了

#如果删除了blog\source\_posts\*.md 网站还没有更新，可以输入下面的代码
hexo clean

```
> 这个生成的.md文件模板可以自定义，目录 blog/scaffolds/post.md google没找到，自己找了半天，才发现

> 明天继续，今天有点晚了，9/13/03点18分，为什么把这篇文章设置到9/11呢，因为建站的时候就是这个时间，这篇文章也是本站第一篇手写文章，所以时间弄早一点，文章排序就会是第一篇，纪念一下




# 更换代码高亮