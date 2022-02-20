# 我的博客模板Jekyll使用说明

## 1. 分页
1. config.yml:
开启分页功能很简单，只需要在 _config.yml 里边加一行，指明每页该展示多少项目：    
paginate: 5    
这个数字应当是你希望在生成的站点中每页展示博客数目的最大值。
你可能还需要指定分页页面的目标路径：     
paginate_path: "blog/page:num"
```yaml
# Old jekyll-paginate pagination logic
paginate: 3
paginate_path: "/legacy/page:num/"
```
2. 应用i18n后自动分页失效，我采用liquid+paginator属性的方式手动生成了html分页组件：
可用的 Liquid 属性Permalink，分页功能插件使得 paginator liquid 对象具有下列属性：       
属性	描述
page
当前页码
per_page
每页文章数量
posts
当前页的文章列表
total_posts
总文章数
total_pages
总页数
previous_page
上一页页码 或 nil（如果上一页不存在）
previous_page_path
上一页路径 或 nil（如果上一页不存在）
next_page
下一页页码 或 nil（如果下一页不存在）
next_page_path
下一页路径 或 nil（如果下一页不存在）

本项目应用jekyll-paginate-v2，项目地址：https://github.com/sverrirs/jekyll-paginate-v2

## 2. i18n
本项目使用jekyll-multiple-languages-plugin作为i18n的插件，github地址：https://github.com/kurtsson/jekyll-multiple-languages-plugin

1. _config.yml
   增加想要的语言类型，例如languages: ["en", "zh"]，数组的第一个语言是默认语言

2. 目录结构
Create a folder called _i18n and add sub-folders for each language, using the same names used on the languages setting on the _config.yml:  
* /_i18n/sv.yml
* /_i18n/en.yml
* /_i18n/de.yml
* /_i18n/fr.yml
* /_i18n/sv/pagename/blockname.md
* /_i18n/en/pagename/blockname.md
* /_i18n/de/pagename/blockname.md
* /_i18n/fr/pagename/blockname.md

3. 使用方法:   
To add a translated string into your web page, use one of these liquid tags:
{% t key %}
or
{% translate key %}
This will pick the correct string from the language.yml file during compilation.
The language.yml files are written in YAML syntax which caters for a simple grouping of strings.

例如在en.yml中:
```yaml
global:
  swedish: Svenska
  english: English
pages:
  home: Home
  work: Work
```
在zh.yml中:
```yaml
global:
  swedish: Svenska
  english: 英语
pages:
  home: Home
  work: Work
```
通过liquid tag调用yml里面的内容:
{% t global.english %}
or
{% translate global.english %}
这样就可以在生成的中文页面中显示“英语”，英文页面中显示“English”


## 3. 博客url
```
---
layout: post
title:  web开发之利用jquery-ajax传输表单数据
date:   2020-08-16 00:43:26 +0800
author: Cai Borui
categories: work javascript
# 封面图应为300*260px或者等比例倍数，推荐600*520px
# cover_image: /assets/images/frt.png
---
```
jekyll默认url的生成与categories属性和时间相关，会按顺序生成目录结构，比如这里是/work/javascript/2020/08...，如果换成cat：javascript work， 就会变成/javascript/work/...

## 4. layout模板页面
假设您想添加一个默认的布局给站点中的所有页面和文章。 你要将这添加到你的 _config.yml 文件：
```
defaults:
  -
    scope:
      path: "" # 一个空的字符串代表项目中所有的文件
    values:
      layout: "default"
      type: "posts" # 以前的 `post`， 在 Jekyll 2.2
```
在具体页面头信息中使用layout属性可以覆盖默认设定
```
# In projects/foo_project.md
---
author: "John Smith"
layout: "foobar"
---
The post text goes here...
```