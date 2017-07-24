---
title: SEO在网页制作中的应用
date: 2017-07-24 23:26:19
categories:
- 前端-html+css
tags:
- 前端
- html
- SEO
---

参考：http://www.imooc.com/learn/204

## 网站结构布局优化

### 使用扁平化结构：

1. **控制首页链接数量**。链接不能太少，也不能太多，中小型网站，首页链接在100以内，包括页面导航、底部导航、锚文字链接等。
2. **扁平化目录层次**。目录层次最好不超过三层，也就是最好可能三次点击就能到达页面的任何位置（我认为app的设计也应该如此，应该最多通过三次操作就能到达app的任何位置，才能带来比较好的体验）
3. **导航seo优化**。头部、底部、内容部分，主导航、副导航、分类导航，尽量用文字，面包屑导航，在每个网站上留下面包屑，使用户可以了解网站组织形式，放于正文的左上方。

## 网页代码优化

1. `<title>`标题。每个页面不要用相同的，把重要关键词放里面。
2. `<meta keywords>`关键词。
3. `<meta description>`网页描述。
4. 语义化代码。在合适的位置使用合适的标签,使用css来规划样式：
    - H1~H6标签多用于标题。
        - 对于重要的标题，要用h1标签，浏览器会认为h1标签是最重要的；
    - 列表标签。
        - ul标签用于无需列；
        - ol标签用于有序列表；
        - dl用于定义数据列表。
    - em，strong标签用于强调，i和b标签没有强调。
    - `<strong>`标签是权重标签的代表，能够在搜索引擎中突出对应文字的内容。`<b>`标签实现的页面效果和`<strong>`一样都是加粗，但是对seo没有强调效果；
    - `<em>`和`<i>`标签都表示斜体，效果与`<strong>`和`<b>`类似，`<em>`表示强调，而`<i>`不是。
    - `<a>`标签。
        - 要加标签说明title；
        - 对于指向其他网站的a标签，要加上rel="nofollow"属性，让搜索引擎忽略该链接，减少跳出率。
    - `<p>`标签和`<br>`标签。
        - 段落应该使用p标签；
        - `<br>`标签用于文本的换行，比如应该：
        ```
        <p>
        
        &nbsp;&nbsp;&nbsp;&nbsp;第一行文字内容<br>
        
        &nbsp;&nbsp;&nbsp;&nbsp;第二行文字内容<br>
        
        &nbsp;&nbsp;&nbsp;&nbsp;第三行文字内容
        
        </p>
        ```
        不应该：
        ```
        <div>
        
        &nbsp;&nbsp;&nbsp;&nbsp;<span>第一行文字内容</span><br>
        
        &nbsp;&nbsp;&nbsp;&nbsp;<span>第二行文字内容</span><br>
        
        &nbsp;&nbsp;&nbsp;&nbsp;<span>第三行文字内容</span>
        
        </div>
        ```
    - `<table>`标签。table标签应该使用caption（表格名称）进行语义化。

    ```
    <table>
    
    &nbsp;&nbsp;&nbsp;&nbsp;<caption>表格标题</caption>
    
    &nbsp;&nbsp;&nbsp;&nbsp;<tr>
    
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<th>Month</th>
    
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<th>Savings</th>
    
    &nbsp;&nbsp;&nbsp;&nbsp;</tr>
    
    &nbsp;&nbsp;&nbsp;&nbsp;<tr>
    
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<th>January</th>
    
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<th>$100</th>
    
    &nbsp;&nbsp;&nbsp;&nbsp;</tr>
    
    &nbsp;&nbsp;&nbsp;&nbsp;<span>第三行文字内容</span>
    
    </table>
    ```
    - `<img>`标签。img标签应该使用alt属性进行图片说明。
5. 重要内容HTML代码放前面，利用css来控制布局。搜索引擎抓取内容在HTML中从上往下，所以重要代码在前面会被优先抓取。
6. 重要内容不要用JS输出，因为搜索引擎抓取不到JS的内容。
7. 尽少使用iframe框架。
8. 对于暂时不想显示的内容，应使用z-index或将其设置到浏览器显示范围外，而不是使用display: none;让内容隐藏，因为搜索引擎会过滤掉display: none;的内容。
9. 不断精简代码。
