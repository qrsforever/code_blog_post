---

title: (原创)Tutorial for Markdown
date: 2017-08-30 15:31:00
tags: [Markdown]
categories: [Tutorial]

---

<!-- vim-markdown-toc GFM -->

* [Markdown 基本语法](#markdown-基本语法)
    * [块注释](#块注释)
    * [斜体](#斜体)
    * [粗体](#粗体)
    * [无序列表](#无序列表)
    * [有序列表](#有序列表)
    * [链接](#链接)
    * [锚点](#锚点)
    * [图片](#图片)
    * [代码](#代码)
    * [段落](#段落)
    * [换行](#换行)
    * [表格](#表格)
    * [分割线](#分割线)
    * [反斜杠](#反斜杠)

<!-- vim-markdown-toc -->

<!-- more -->

## Markdown 基本语法

Pandoc pass attributes via {}. see [锚点](#myanchor)

### 块注释

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

### 链接

```
内联方式 [百度](http://www.baidu.com)
引用方式 [谷歌][1] [百度][2]

[1]: http://www.google.com  "谷歌"
[2]: http://www.baidu.com   "百度"

```

内联方式 [百度](http://www.baidu.com)
引用方式 [谷歌][1] [百度][2]

[1]: http://www.google.com  "谷歌"
[2]: http://www.baidu.com   "百度"

### 锚点 {#myanchor}

```

Pandoc在标题行加`{#myanchor}`, 可以实现锚点.

[AnchorText]{#mytext}

<span id="m1">锚点1：</span>
anchor1  
anchor1  
anchor1  
anchor1  
anchor1  
anchor1  
anchor1  
<span id="m2">锚点2：</span>
anchor2  
anchor2  
anchor2  
anchor2   
anchor2  
anchor2  
anchor2  

[锚点1](#m1 "anchor alt text")

[锚点2][anchor2]  

[anchor2]:#m2 "anchor alt text"

[锚点3](#mytext)
```

[AnchorText]{#mytext}

<span id="m1">锚点1：</span>
anchor1  
anchor1  
anchor1  
anchor1  
anchor1  
anchor1  
anchor1  
<span id="m2">锚点2：</span>
anchor2  
anchor2  
anchor2  
anchor2  
anchor2  
anchor2  
anchor2  

[锚点1](#m1 "anchor alt text")

[锚点2][anchor2]  

[anchor2]:#m2 "anchor alt text"

[锚点3](#mytext)

### 图片

```
<div align='center'>
内联方式：![alt text](/img/avatar.jpg "Title")
引用方式：![alt text][id] 
</div>
[id]: /img/avatar.jpg    "Title"

```

<div align='center'>
内联方式：![alt text](/img/avatar.jpg "Title")
引用方式：![alt text][id] 
</div>
[id]: /img/avatar.jpg    "Title"

### 代码

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

### 段落

```
> Hello World
111111111111
>> Hello World
222222222222
>>> Hello World
\`\`\`java
for (int i = 0; i < 100; ++i)  
    printf(i);
\`\`\`

Normal
```

> Hello World
111111111111
>> Hello World
222222222222
>>> Hello World
```java
for (int i = 0; i < 100; ++i)  
    printf(i);
```

Normal


### 换行

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

### 表格

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
