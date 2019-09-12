---

title: (原创)Tutorial for Markdown
date: 2017-08-30 15:31:00
tags: [Markdown]
categories: [Tutorial]

---

<!-- vim-markdown-toc GFM -->

* [Markdown 基本语法](#markdown-基本语法)
    * [加载custom样式](#加载custom样式)
    * [块注释](#块注释)
    * [字体](#字体)
        * [斜体](#斜体)
        * [粗体](#粗体)
    * [列表](#列表)
        * [无序列表](#无序列表)
        * [有序列表](#有序列表)
        * [定义型列表](#定义型列表)
        * [包含引用](#包含引用)
        * [Task List](#task-list)
    * [链接](#链接)
        * [内联](#内联)
        * [引用](#引用)
        * [自动连接](#自动连接)
    * [锚点](#锚点)
    * [图片](#图片)
    * [代码](#代码)
    * [引用](#引用-1)
        * [简单](#简单)
        * [嵌套](#嵌套)
        * [引用其他要素](#引用其他要素)
    * [注脚](#注脚)
    * [段落](#段落)
        * [列表缩进](#列表缩进)
    * [换行](#换行)
    * [表格](#表格)
        * [无表头](#无表头)
        * [有表头](#有表头)
    * [其他](#其他)
        * [分割线](#分割线)
        * [反斜杠](#反斜杠)
* [非标用法](#非标用法)
    * [hexo](#hexo)
        * [blockquote](#blockquote)
        * [codeblock](#codeblock)
        * [Emoji](#emoji)

<!-- vim-markdown-toc -->

<!-- more -->

# Markdown 基本语法

Pandoc pass attributes via {}. see [锚点](#myanchor)

[]{#blockquote}

## 加载custom样式

```
<link href="/static/css/qrs.css" rel="stylesheet">
```

<link href="/static/css/qrs.css" rel="stylesheet">

## 块注释

每行后面2个空格,不多不少, 嵌套时层级之间加一个` > `

```
> 块注释0  
>
> > 块注释1  
>  
> > > 块注释2  

> ## This is a header.
> 
> 1.   This is the first list item.
> 2.   This is the second list item.
> 
> Here's some example code:
> 
>     return shell_exec("echo $input | $markdown_script");

> This is a block quote. This
> paragraph has two lines.
>
> 1. This is a list inside a block quote.
> 2. Second item.

> This is a block quote.
>
> > A block quote within a block quote.

> This is a block quote. with latex
the sum denotes $\sum_{a=0}^{n}$

```

> 块注释0  
>
> > 块注释1  
>  
> > > 块注释2  

> ## This is a header.
> 
> 1.   This is the first list item.
> 2.   This is the second list item.
> 
> Here's some example code:
> 
>     return shell_exec("echo $input | $markdown_script");

> This is a block quote. This
> paragraph has two lines.
>
> 1. This is a list inside a block quote.
> 2. Second item.

> This is a block quote.
>
> > A block quote within a block quote.

> This is a block quote. with latex
the sum denotes $\sum_{a=0}^{n}$

## 字体

### 斜体

```
*斜体0*
_斜体1_
```

*斜体0*  
_斜体1_  

### 粗体

```
**粗体0**
__粗体1__
```

**粗体0**  
__粗体1__  

## 列表

### 无序列表

```
* 无序0
- 无序1
+ 无序2
```

* 无序0
- 无序1
+ 无序2

### 有序列表

```
1. 有序0
2. 有序1
3. 有序2
```

1. 有序0
2. 有序1
3. 有序2

### 定义型列表

```
Markdown
:   轻量级文本标记语言

Code2
:   这是代码块的定义

        import numpy as np
```

Markdown
:   轻量级文本标记语言

Code2
:   这是代码块的定义

        import numpy as np

### 包含引用

```
*   阅读方法：

    > 打开书本 <br/>
    > 打开电灯
```

*   阅读方法：

    > 打开书本 <br/>
    > 打开电灯

### Task List

GFM(github flavoured markdown)支持, pandoc2.6 high

```
- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media
```

- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media

## 链接

### 内联

```
内联方式 [百度](http://www.baidu.com)

```

内联方式 [百度](http://www.baidu.com)

### 引用

```
引用方式 [谷歌][1] [百度][2]

[1]: http://www.google.com  "谷歌"
[2]: http://www.baidu.com   "百度"

```

引用方式 [谷歌][1] [百度][2]

[1]: http://www.google.com  "谷歌"
[2]: http://www.baidu.com   "百度"

### 自动连接

<http://www.baidu.com/>

[]{#myanchor}

## 锚点

```

Pandoc在标题行直接加`{#myanchor}`, 可以实现锚点, 不需要[]{#myanchor}

[AnchorText]{#mytext}

<span id="m1">锚点1:</span>
anchor1  
anchor1  
anchor1  
<span id="m2">锚点2:</span>
anchor2  
anchor2  
anchor2  

[锚点1](#m1 "anchor alt text")

[锚点2][anchor2]  

[anchor2]:#m2 "anchor alt text"

[锚点3](#mytext)
```

[AnchorText]{#mytext}

<span id="m1">锚点1:</span>
anchor1  
anchor1  
anchor1  
<span id="m2">锚点2:</span>
anchor2  
anchor2  
anchor2  

[锚点1](#m1 "anchor alt text")

[锚点2][anchor2]  

[anchor2]:#m2 "anchor alt text"

[锚点3](#mytext)

## 图片

```
<div align='center'>
内联方式:![alt text](/img/avatar.jpg "Title")
引用方式:![alt text][id] 
</div>
[id]: /img/avatar.jpg    "Title"

pandoc:

![link text](/img/avatar.jpg){.float-right width=20px height=10%}

```

<div align='center'>
内联方式:![alt text](/img/avatar.jpg "Title")
引用方式:![alt text][id] 
</div>
[id]: /img/avatar.jpg    "Title"

pandoc:

![float-right](/img/avatar.jpg){.float-right width=20px height=10%}

## 代码

```
\`one line\`

\`\`\`java
for (int i = 0; i < 100; ++i)  
    printf(i);
\`\`\`

\`\`\`
for (int i = 0; i < 100; ++i)  
    printf("longggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggg line");
\`\`\`

    for (int i = 0; i < 100; ++i)  
        printf(i);
    for (int i = 0; i < 100; ++i)  
        printf(i);
    for (int i = 0; i < 100; ++i)  
        printf(i);
```

`one line`

```java
for (int i = 0; i < 100; ++i)  
    printf(i);
```

```
for (int i = 0; i < 100; ++i)  
    printf("longggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggg line");
```

    for (int i = 0; i < 100; ++i)  
        printf(i);
    for (int i = 0; i < 100; ++i)  
        printf(i);
    for (int i = 0; i < 100; ++i)  
        printf(i);

## 引用

注意每行后面都有两个空格

### 简单

```
> 这是一个有两段文字的引用,  
  无意义的占行文字1.  
  无意义的占行文字2.  
> 
  无意义的占行文字3.  
  无意义的占行文字4.  
```

> 这是一个有两段文字的引用,  
  无意义的占行文字1.  
  无意义的占行文字2.  
> 
  无意义的占行文字3.  
  无意义的占行文字4.  

### 嵌套

```
> > > 请问 Markdwon 怎么用? - 小白  
> > 
> > 自己看教程! - 愤青  
> 教程在哪? - 小白  
```

> > > 请问 Markdwon 怎么用? - 小白  
> > 
> > 自己看教程! - 愤青  
> 教程在哪? - 小白  

[更多](#blockquote)

### 引用其他要素

```
> 1.   这是第一行列表项.
> 2.   这是第二行列表项.
> 
> 给出一些例子代码:
> ```c
>    return shell_exec("echo  $input | $markdown_script");
> ```
```

> 1.   这是第一行列表项.
> 2.   这是第二行列表项.
> 
> 给出一些例子代码:
> ```c
>    return shell_exec("echo  $input | $markdown_script");
> ```

## 注脚


```
Here's a simple footnote,[^1] and here's a longer one.[^bignote]

[^1]: This is the first footnote.

[^bignote]: Here's one with multiple paragraphs and code.

    Indent paragraphs to include them in the footnote.

    `{ my code }`

    Add as many paragraphs as you like.
```

Here's a simple footnote,[^1] and here's a longer one.[^bignote]

[^1]: This is the first footnote.

[^bignote]: Here's one with multiple paragraphs and code.

    Indent paragraphs to include them in the footnote.

    `{ my code }`

    Add as many paragraphs as you like.

## 段落

### 列表缩进

` * ` 后有个空格, 最多3个, 如果每一项多个段落, 则段落前4个空格

```
*   轻轻的我走了， 正如我轻轻的来； 我轻轻的招手， 作别西天的云彩。   
那河畔的金柳， 是夕阳中的新娘； 波光里的艳影， 在我的心头荡漾。  
软泥上的青荇， 油油的在水底招摇； 在康河的柔波里， 我甘心做一条水草！  

    那榆荫下的一潭， 不是清泉， 是天上虹； 揉碎在浮藻间， 沉淀着彩虹似的梦。 
寻梦？撑一支长篙， 向青草更青处漫溯； 满载一船星辉， 在星辉斑斓里放歌。  
但我不能放歌， 悄悄是别离的笙箫； 夏虫也为我沉默， 沉默是今晚康桥！  

*   悄悄的我走了， 正如我悄悄的来； 我挥一挥衣袖， 不带走一片云彩。 
```

*   轻轻的我走了， 正如我轻轻的来； 我轻轻的招手， 作别西天的云彩。  
那河畔的金柳， 是夕阳中的新娘； 波光里的艳影， 在我的心头荡漾。  
软泥上的青荇， 油油的在水底招摇； 在康河的柔波里， 我甘心做一条水草！  

    那榆荫下的一潭， 不是清泉， 是天上虹； 揉碎在浮藻间， 沉淀着彩虹似的梦。  
寻梦？撑一支长篙， 向青草更青处漫溯； 满载一船星辉， 在星辉斑斓里放歌。  
但我不能放歌， 悄悄是别离的笙箫； 夏虫也为我沉默， 沉默是今晚的康桥！  

*   悄悄的我走了， 正如我悄悄的来； 我挥一挥衣袖， 不带走一片云彩。  


## 换行

```
longggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggg
newlongggggggggggggggggggggg  
gggggggggggggggggggggggggggg(我后面有两个空格)  
gggggggggggggggggggggggggggg

aaa<br/><br/>bbb<br/><br/>
```

longggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggggg
newlongggggggggggggggggggggg  
gggggggggggggggggggggggggggg(我后面有两个空格)  
gggggggggggggggggggggggggggg

aaa<br/><br/>bbb<br/><br/>

## 表格

### 无表头

对齐方式靠上下线与第一行第一个字母和最后一个字母有关

    -------  -------------   -------   ----
    Joan      saag paneer    medium     $11
    Sally      vindaloo        mild     $14
    Erin      lamb madras       HOT      $5
    -------  -------------   -------   ----

-------  -------------   -------   ----
Joan      saag paneer    medium     $11
Sally      vindaloo        mild     $14
Erin      lamb madras       HOT      $5
-------  -------------   -------   ----

### 有表头

```
Name | Lunch order | Spicy      | Owes
------- | :----------------: | :---------- | ---------:
Joan  | saag paneer | medium | $11
Sally  | vindaloo        | mild       | $14
Erin   | lamb madras | HOT      | $5

冒号**:**表示对齐方式, 没有默认居右
```

Name | Lunch order | Spicy      | Owes
------- | :----------------: | :---------- | ---------:
Joan  | saag paneer | medium | $11
Sally  | vindaloo        | mild       | $14
Erin   | lamb madras | HOT      | $5

冒号**:**表示对齐方式, 没有默认居右

## 其他

### 分割线

```
______
```
______

### 反斜杠

```
符号|名称
:---:|:---
\\ | 反斜线
\` | 反引号
\* | 星号
\_ | 底线
\{ |花括号
\[ |方括号
\( |括弧
\# | 井字号
\+ | 加号
\- | 减号
\. | 英文句点
\! | 惊叹号
```

符号|名称
:---:|:---
\\ | 反斜线
\` | 反引号
\* | 星号
\_ | 底线
\{ |花括号
\[ |方括号
\( |括弧
\# | 井字号
\+ | 加号
\- | 减号
\. | 英文句点
\! | 惊叹号

# 非标用法

## hexo

### blockquote

```
{% blockquote QRS, https://qrsforever.github.io https://qrsforever.github.io/2019/07/18/Tools/How/china_images/#npm "国内npm下载源镜像" %}

npm config set registry https://registry.npm.taobao.org  
npm config get registry

{% endblockquote %}
```

{% blockquote QRS, https://qrsforever.github.io https://qrsforever.github.io/2019/07/18/Tools/How/china_images/#npm "国内npm下载源镜像" %}

npm config set registry https://registry.npm.taobao.org  
npm config get registry

{% endblockquote %}

### codeblock

```
{% codeblock "codeblock test" lang:c http://www.baidu.com "link text" line_number:true highlight:true first_line:1 %}
aa = 1
bb = 2
cc = 3
for (int i = 0; i < 10; ++i) {
    print(i);
}
{% endcodeblock %}
```

{% codeblock "codeblock test" lang:c http://www.baidu.com "link text" line_number:true highlight:true first_line:1 %}
aa = 1
bb = 2
cc = 3
for (int i = 0; i < 10; ++i) {
    print(i);
}
{% endcodeblock %}

### Emoji

[官网](https://emojipedia.org/)

[查找](https://www.emojicopy.com/)

{% emoji_hj people/thinking-face %}

