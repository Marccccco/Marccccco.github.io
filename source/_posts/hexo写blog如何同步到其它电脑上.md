---
title: hexo写blog如何同步到其它电脑上
date: 2023-10-19 16:02:03
tags:
---

### github端

新增一个分支`hexo`

更改默认分支为`hexo`(在settings->genneral->default branch中)

### 本地

git跟node.js下载

git配置ssh

git clone `hexo`分支,要用`SSH`链接,用`https`提交不上来.

#### 把本地项目第一次更到github上请按下面操作

把clone下来的文件除了`.git`文件夹以外的全部删除掉(**记得把隐藏文件显示出来**)

把之前写blog的源文件除了`.deploy_git`文件全部复制过来.

看有没有`.gitignore`文件,这个是git上传会忽略的文件,如果没有记得新建一个,内容是

```
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```

看一下`theme`中有没有`.git`文件,有的话记得删除,因为git不能嵌套上传.

然后

```
git add .
git commit –m "add branch"
git push 
```

这样就把本地的文件备份到githu上了

#### 新电脑要写blog

git跟node.js下载

git配置ssh

git clone `hexo`分支,要用`SSH`链接,用`https`提交不上来.

安装hexo

```
sudo npm install hexo-cli -g
```

**不用再`hexo init`了**

然后进入克隆到的文件夹：

```text
cd xxx.github.io
npm install
npm install hexo-deployer-git --save
```

生成，部署：

```text
hexo g
hexo d
```

然后就可以开始写你的新博客了

每次改完记得提交到github上

每次用之前要记得`git pull`















