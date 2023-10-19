---
title: github+hexo部署blog
---

### github+hexo部署blog

* 安装node.js跟git
* 安装hexo `sudo npm install -g hexo-cli`

#### 初始化hexo

```
hexo init 文件名		// 初始化
npm install/i		
hexo g  // 生成页面
hexo s	// 启动预览
hexo d	// 部署到Github Pages
hexo new "name"       # 新建文章
hexo new page "name"  # 新建页面
hexo g                # 生成页面
hexo d                # 部署
hexo g -d             # 生成页面并部署
hexo s                # 本地预览
hexo clean            # 清除缓存和已生成的静态文件
hexo help             # 帮助

```

`npm install hexo-deployer-git --save	// 部署到github需要安装这个插件`

修改`_config.yml`末尾的Deployment部分

```
deploy:
  type: git
  repository: git@github.com:用户名/用户名.github.io.git
  branch: master
```

#### 给github配置ssh

#### 使用

```
可以使用hexo new 'name'的方式去创建,也可以直接自己创建.md文件
开头第一句用---开头,然后title: 文章名
```

#### 图片问题

安装一个依赖

`npm install https://github.com/CodeFalling/hexo-asset-image --save`

```
把_config.yml中的post_asset_folder设为true,这样每次在`hexo n  'xxx'`后就会在hexo/source/_post文件夹下生成一个.md文件和一个同名文件夹，文件夹存放文章中的图片.
使用就是![图片描述]（./包名/图片）
```





