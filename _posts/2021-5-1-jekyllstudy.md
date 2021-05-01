---
layout: post
title: 来学点jekyll
---
>这是个jekyll的学习文档，准确来说是官网文档的翻译和个人理解，我自己能看懂就行了，哈哈哈

[toc]

## 1. collections 集合
就是用来整合相关内容的集合
### setup 
用一个集合之前需要先在配置文件_config.yml里面定义一下
```
collections:
  - staff_members #定义成列表，
  staff_members: #定义成字典，可以给你的字典添加点原生属性
    people: true
collections_dir: my_collections #定义成集合文件夹，
这里注意要把_posts之类的原生集合也扔进这个文件夹
```
集合怎么建立就是直接类似_posts建一个_staff_members文件夹，然后把你的md文件扔进去就好了
### add content 添加内容
jekyll在处理一个文件的时候，会先处理前提条件，也就是文档最上面的哪些属性啥的吧，如果你没有指定这些前置条件，那jekyll就会把这个文件当成静态文件不做处理

不管你写没写前置条件，要想jekyll把你写的这个页面展示出来，你就得配置一下这个集合的output属性，output=true

### output
现在你就可以在一个页面上迭代site.staff_members，并且输出这些staff的内容。类似posts，文件的内容自动被放进了content属性
```
{% for staff_member in site.staff_members %}
  <h2>{{ staff_member.name }} - {{ staff_member.position }}</h2>
  <p>{{ staff_member.content | markdownify }}</p>
{% endfor %}
```
*ps：至于这个写着html的页面放在哪我理解的是放在layout，到时候试试*

如果你想让jekyll对你集合里的所有文档进行渲染，记得把output设置为true
```
collections:
  staff_members:
    output: true
```
用集合的url属性就可以链接到你的生成文档
```
{% for staff_member in site.staff_members %}
  <h2>
    <a href="{{ staff_member.url }}">
      {{ staff_member.name }} - {{ staff_member.position }}
    </a>
  </h2>
  <p>{{ staff_member.content | markdownify }}</p>
{% endfor %}
```
### Permalinks
我不造是不是叫永久链接，反正用这个就可以控制你的输出url对整个分组进行链接

换句话说就是你的pages，posts，collections的输出路径。permalinks允许你构造源码的目录而不是输出目录

最简单的使用，就是在前置项用permalinks。
举个例子：
你有个页面在/my_pages/about-me.html而且你想让这个输出路径是/about/，所以在这个/my_pages/about-me.html页面你就设置一个前置项属性为
```
---
permalink: /about/
---
```
*ps:所以这个permalinks的意思，就是构造路由 ，--！还有一些其他的构造方式，到时候再看[文档](https://jekyllrb.com/docs/permalinks/#collections)吧*

### 给你的文档定制排序
一般默认的文档排序，是集合里的文档的date属性，如果这些文档里面都有date前置的话。但是如果个别文档没有的话就是根据他们各自的路径排序

当然你想定制化也可以，有这几种定制化方法
* 设置个排序属性呗
  ```
  collections:
  tutorials:
    sort_by: lesson
  ```
* 手动排序
  ```
  collections:
  tutorials:
    order:
      - hello-world.md
      - introduction.md
      - basic-concepts.md
      - advanced-concepts.md
  ```
### liquid attributes
我也不造这咋翻译，但是看样子是用在html里的语法，应该是用来引用的，具体有哪些变量：

**集合的一些变量:**
  
|变量|描述|
|-|-|
|label|集合的名字，比如my_collection
|docs|文档列表
|files|集合的静态文件列表
|relative_directory|相对于site souce的集合源目录
|directory|集合源目录的完整路径
|output|是否集合文档将被输出作为独立文件

**文档的一些变量**
文档除了前置属性里的一些变量，还有这些属性
|变量|描述|
|-|-|
|content|前置条件以下的内容
|output|基于content，这个文档的渲染输出
|path|全路径
|relative_path|相对路径
|url|渲染后的集合的url
|collection|属于哪个集合
|date|日期

## 2. 全局变量
提到了集合的变量再来瞅瞅全局变量，应该就是贯穿整个jekyll项目的主线了
|变量|描述|
|-|-|
|site|整个网站的所有信息和配置设置
page|页面的特别信息和前置变量，在前置属性中定制的变量，可以通过page获取
layout|布局相关信息和前置变量。通过前置变量设计的layout变量可以通过layout获取
content|post和page文件的渲染内容？
paginator|当设定了paginator 配置属性，就可以当成变量来用了。

感觉就是 $$content \in page, page \in site$$,然后layout控制page布局，paginator用来分页