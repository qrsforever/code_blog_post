---

title: (原创)Tutorial for Hexo
date: 2018-12-18 21:20:26
tags: [Markdown]
categories: [Tutorial]

---

<!-- vim-markdown-toc GFM -->

* [Tag Plugins](#tag-plugins)
    * [Block Quote](#block-quote)
    * [Include Posts](#include-posts)
    * [Include Assets](#include-assets)

<!-- vim-markdown-toc -->

[原文](https://hexo.io/docs/tag-plugins.html)

# Requirements

```shell
sudo apt install pandoc
sudo apt install nbconvert
sudo apt install texlive
```

<!-- more -->

# Tag Plugins

## Block Quote

语法:

```
    {% blockquote [author[, source]] [link] [source_link_title] %}
    content
    {% endblockquote %}
```

- Case1:

```
    {% blockquote %}
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque hendrerit lacus ut purus iaculis feugiat. Sed nec tempor elit, quis aliquam neque. Curabitur sed diam eget dolor fermentum semper at eu lorem.
    {% endblockquote %}
```


{% blockquote %}
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque hendrerit lacus ut purus iaculis feugiat. Sed nec tempor elit, quis aliquam neque. Curabitur sed diam eget dolor fermentum semper at eu lorem.
{% endblockquote %}

- Case2:

```
    {% blockquote David Levithan, Wide Awake %}
    Do not just seek happiness for yourself. Seek happiness for all. Through kindness. Through mercy.
    {% endblockquote %}
```

{% blockquote David Levithan, Wide Awake %}
Do not just seek happiness for yourself. Seek happiness for all. Through kindness. Through mercy.
{% endblockquote %}

- Case3:

```
    {% blockquote @DevDocs https://twitter.com/devdocs/status/356095192085962752 %}
    NEW: DevDocs now comes with syntax highlighting. http://devdocs.io
    {% endblockquote %}
```

{% blockquote @DevDocs https://twitter.com/devdocs/status/356095192085962752 %}
NEW: DevDocs now comes with syntax highlighting. http://devdocs.io
{% endblockquote %}


- Case4:

```
    {% blockquote Seth Godin http://sethgodin.typepad.com/seths_blog/2009/07/welcome-to-island-marketing.html Welcome to Island Marketing %}
    Every interaction is both precious and an opportunity to delight.
    {% endblockquote %}
```

{% blockquote Seth Godin http://sethgodin.typepad.com/seths_blog/2009/07/welcome-to-island-marketing.html Welcome to Island Marketing %}
Every interaction is both precious and an opportunity to delight.
{% endblockquote %}

## Code Block

语法:

```
    {% codeblock [title] [lang:language] [url] [link text] %}
    code snippet
    {% endcodeblock %}
```

- Case1:

```  
    {% codeblock %}
    alert('Hello World!');
    {% endcodeblock %}
```

{% codeblock %}
alert('Hello World!');
{% endcodeblock %}

- Case2:

```
    {% codeblock lang:objc %}
    [rectangle setX: 10 y: 10 width: 20 height: 20];
    {% endcodeblock %}
```

{% codeblock lang:objc %}
[rectangle setX: 10 y: 10 width: 20 height: 20];
{% endcodeblock %}

## Include Posts

语法:

```
    {% post_path filename %}
    {% post_link filename [optional text] %}
```

- Case1:

```
    <a href="{% post_path Tutorial/Latex %}">link to Latex</a>.
```

<a href="{% post_path Tutorial/Latex %}">link to Latex</a>.

**Not** support markdown: `[]({% post_path Tutorial/Latex %})`

- Case2:

```
    {% post_link Tutorial/Markdown 'link to Markdown' %}
```

{% post_link Tutorial/Markdown 'link to Markdown' %}

## Include Assets

语法:

```
    {% asset_path slug %}
    {% asset_img slug [title] %}
    {% asset_link slug [title] %}
```

- Case1:

```
    {% asset_path test.png %}
```

{% asset_path test.png %}

- Case2:

```
    {% asset_img test.png "test" %}
```

{% asset_img test.png "test" %}

- Case3:

```
    {% asset_link test.png "link to test" %}

```

{% asset_link test.png "link to test" %}

绑定了本**POST**的ID, 只能引用自己ID下的资源
