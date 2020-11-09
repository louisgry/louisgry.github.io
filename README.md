# louisgry.github.io

- [Github+Hexo搭建博客](#Github+Hexo搭建博客)

# Github+Hexo搭建博客
### 安装部署
- GitHub
    - 安装Git
    - GitHub创建仓库：`username+github.io`
- Hexo
    - 安装Nodejs
    - 安装Hexo：管理员模式打开cmd输入`npm install hexo-cli -g`
    - 验证安装：`hexo -v`
    - `hexo init`：到一个空的blog文件夹初始化
    - `npm install`：安装需要的组件
    - `npm g`：生成静态文件
    - `hexo server`：打开http://localhost:4000/博客页面

### GitHub Pages
- 修改配置文件_config.yml（最后一行）
``` 
deploy:
  type: git
  repo: git@github.com:louisgry/louisgry.github.io.git
  branch: master
```
- 安装自动部署到GitHub的插件
```
npm install hexo-deployer-git --save
```
- 创建博客文章
```
hexo new post "blog_name" # ==> ..\source\_posts\blog_name.md
```
- 推送文章
```
hexo g # generate：生成静态文件
hexo d # deploy：部署到Github Pages
```
### next主题
- Hexo Themes：https://hexo.io/themes/
- 下载next主题：
```
git clone https://github.com/theme-next/hexo-theme-next.git themes/next
```
- 使用主题：修改站点的_config.yml
```
theme: next
```
- 修改站点配置文件
```
# Site
title: Blog · Louis 
subtitle:
description:
keywords:
author: Louis
language: en
timezone:
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://louisgry.github.io/
root: /
```
- 修改主题配置文件
```
menu:
  home: / || home
  #categories: /categories/ || th
  archives: /archives/ || archive
  tags: /tags/ || tags
```
- 创建tags页
```
hexo new page "tags"
```
- 修改tags文件夹下的md文件：添加type
```
---
title: Tags
date: 2019-10-17 19:14:58
type: tags
---
```
- 图片使用：放在source目录下跟_posts同级
```
![页、段、段页式](/images/19-10-17/1.png)
```

### pure主题
- 下载pure主题：https://github.com/cofess/hexo-theme-pure
```
git clone https://github.com/cofess/hexo-theme-pure.git themes/pure
```
- 使用主题：修改站点的_config.yml
```
theme: pure
```
- 主题信息：修改主题的_config.yml
```
profile:
  enabled: true # Whether to show profile bar
  avatar: images/avatar.jpg
  gravatar: # Gravatar email address, if you enable Gravatar, your avatar config will be overriden
  author: Louis
```
- 安装插件
```
npm install hexo-wordcount --save
npm install hexo-generator-json-content --save
npm install hexo-generator-feed --save
npm install hexo-generator-sitemap --save
npm install hexo-generator-baidu-sitemap --save
```

### bug
- 本地有样式，但是GitHub上没有
    - 修改站点_config.yml
    ```
    url: https://louisgry.github.io/blog
    root: /louisgry.github.io/
    ```
    - 然后运行
    ```
    hexo clean
    hexo g
    hexo d
    ```
