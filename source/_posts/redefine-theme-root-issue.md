---
title: Redefine theme root issue
excerpt: false
date: 2023-07-03 10:32:42
tags: [url,root]
categories: redefine
thumbnail: /images/redefine-theme-root-issue/EiVTDx3U0AANQzL.jpg
cover: 
typora-root-url: .\..\
---

#

### 瀑布流相册图片地址

```
- image: ../masonry/瀑布流相册/1.jpg 图片地址需增加..相对路径，不加的话本地无法加载
  title: 浅川梨奈
  description: 
  
 
- image: https://evan.beee.top/img/2023/05/14/d5328473c414a7dce7990f70e8b1ec41.png
  title: 网络图片
  description: 网路图片地址直接引用

```

![](/images/redefine-theme-root-issue/EiVTDx3U0AANQzL.jpg)



### Hexo-redefine主题应用在子仓库中的问题

使用redefine主题，hexo-config配置文件中url地址，如果使用**根仓库** url: https://username.github.io 不会出现问题；如果使用**子仓库** url: https://username.github.io/test 则会出现以下问题：

1. hexo s本地部署，无法加载背景图片，推送GitHub后网页能加载背景图片。
2. hexo s本地部署，无法加载头像图片，推送后网页也无法加载头像图片。
3. hexo s本地部署，网页logo和logo图像索引均指向根仓库，无法指向子仓库。推送后网页logo索引也指向根仓库，无法指向子仓库。

#### 1.背景图片本地部署无法加载

***背景图片本地加载不显示的问题还没有解决***，但是不影响使用，推送后网页可以正常加载背景图片。

**PS：该问题已经解决。**

在主题配置文件图片地址前加上../成为相对路径，本地部署图片正常加载。

![](/images/redefine-theme-root-issue/%E4%B8%BB%E9%A2%98%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87%E8%AE%BE%E7%BD%AE.png)

#### 2.主页及文章页头像图片无法加载

问题已解决，路径代码错误导致图片加载路径出错。

index.html文件图片路径错误：

![](/images/redefine-theme-root-issue/路径错误.png)

原代码：

![](/images/redefine-theme-root-issue/头像图片修改前.png)

修改后：

![](/images/redefine-theme-root-issue/头像图片修改后.png)

关于头像图片加载问题，还有文章页也需要修改该行代码

![](/images/redefine-theme-root-issue/文章页头像图片.png)

#### 3.logo图片和内容索引错误

问题已解决，主要问题就是href路径代码问题。

原代码：

![](/images/redefine-theme-root-issue/logo索引修改前.png)

修改后：

![](/images/redefine-theme-root-issue/logo索引修改后.png)

修改后，本地和网页都能正确指向子仓库。

#### 后记

上周用了两天的时间，主要解决了redefine主题url放置在子仓库中无法加载背景图片和头像图片，以及主页logo和图像索引指向根仓库的问题。

如果将redefine主题应用在根仓库无/root/，则都不用更改正常使用，因为没有root路径，转化生成时不会出现路径错误。有了root之后，路径引用就容易出现问题，因为这个主题的目标就是根目录，最根本的代码还是没有找到原因。

```
<div class="article-content markdown-body">
                <%- page.content %> 这里的代码如何转义不清楚
            </div>
```

```
 <!--- Inject Part-->
    <% if (theme.inject.enable == true) { %>
        <% for (let i in theme.inject.head) { %>
            <% if (theme.inject.head[i] !== null){ %>
                <% if (theme.global.pjax == true) { %>
                    <%- theme.inject.head[i].replace("<script", "<script data-pjax") %>
                <% } else { %>
                    <%- theme.inject.head[i] %>
            <% } }%>
    <% } }%>
    <%- export_config() %> 这里的代码如何转义不清楚
```

问题解决看着很简单，就是路径的问题，可是由于对代码不熟悉，大部分时间都用在找代码上面了。先从index.html入手，看本地和网页版代码，确定路径引用错误引起指向错误和图片无法加载，然后找到主题原代码位置，确定具体代码行对其修改。



### Typora图片无法正常加载

解决redefine主题应用在子仓库中的问题，新建文章后，使用Typora插入图片，加载又出现问题，还是url中有/root/的问题，图片路径在转义时会出现错误，具体哪里的原因目前不清楚。通过调试，在Typora中添加图片根目录可解决。

修改scaffolds中post.md默认内容如下：

```
title: {{ title }}
date: {{ date }}
tags:  
categories:  
thumbnail: 将图片地址复制至此处即可。
cover: 有了thumbnail这里可以空缺。
excerpt: false
typora-root-url: .\..\ 设置图片根目录，这是有root后需要添加的。
```

图片即可正常加载，如下图实例

hexo new test5 插入图片，本地、推送网页均可正常加载。

![](/images/redefine-theme-root-issue/post%E6%A0%BC%E5%BC%8F.png)



