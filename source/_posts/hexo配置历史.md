---
title: hexo配置历史
date: 2019-07-14 22:55:40
tags: hexo
---
1. ### 主题配置
    1. `_config.yml`中,配置主题为`theme: next`
    2. 采用Hexo方式配置主题,方便升级主题
    ```yaml
    theme_config:
    scheme: Gemini
    menu:
        home: / || home
        about: /about/ || user
        tags: /tags/ || tags
        #categories: /categories/ || th
        archives: /archives/ || archive
        # Social Links
    ```
    {% blockquote %}
    使用这一方式，你的全部配置都将置于 hexo 主要配置文件中（hexo/_config.yml），并且不需要修改 next/_config.yml，或者创建什么其他的文件。但是所有的主题选项必须放置在 theme_config 后，并全部增加两个空格的缩进。
    {% endblockquote %}
    主题版本 V7.2.0
    <!--more-->
2. ### 开启搜索
    1. 安装`npm install hexo-generator-searchdb --save`
    2. 修改站点配置`_config.yml`
```yaml
    search:
      path: search.xml
      field: post
      format: html
      limit: 10000
```
    3. 修改主题配置
```yaml
    local_search:
      enable: true
```
3. ### 开启侧边栏头像
  1. 上传文件到source/uploads文件夹(如果没有该文件夹则手动创建)
  2. 修改主题配置
```yaml
    avatar:
      # In theme directory (source/images): /images/avatar.gif
      # In site directory (source/uploads): /uploads/avatar.gif
      # You can also use other linking images.
      url: /uploads/avatar.jpg
      # If true, the avatar would be dispalyed in circle.
      rounded: true
      # If true, the avatar would be rotated with the cursor.
      rotated: false
```
4. ### 返回顶部按钮的百分比
  1. scrollpercent设置为`true`
```yaml
back2top:
    enable: true
    # Back to top in sidebar.
    sidebar: false
    # Scroll percent label in b2t button.
    scrollpercent: true
```
5. ### 支持阅读全文
  当文章过长时，在要截断的地方添加`<!--more-->`,即可生成"阅读全文"按钮。
  主题自带的阅读全文，会根据文章字符数固定划分，这样容易导致原文排版出现问题，故不推荐使用