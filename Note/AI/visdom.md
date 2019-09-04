---

title: Visdom原理

date: 2019-09-03 13:20:08
tags: [AI]
categories: [Note]

---


<!-- vim-markdown-toc GFM -->

* [Visdom](#visdom)
* [How to Talk with](#how-to-talk-with)
* [DrawIt](#drawit)
* [Conclution](#conclution)

<!-- vim-markdown-toc -->

<!-- more -->

# Visdom

Visdom aims to facilitate visualization of (remote) data with an emphasis on supporting scientific experimentation.

基本使用介绍可以到[官网][visdom]查看, 本文主要重点放在解决以下疑惑:

- Backend产生的数据(json)如何提供给Frontend ?

- 如何支持多个Backend和多个Frontend之间的交互?

- Frontend收到的数据(json)如何以**窗格**(pane)形式展示出?

**Frontend**: 前端UI对数据展示, **Backend**: 生产数据的一端, 比如机器学习过程中产生的数据, loss, acc等.

# How to Talk with

下载[Visdom源码][visdom]之后, 粗略看了一下文件, Visdom完成可视化的方式由三部分组成, 分别为:

1. js: 前端app客户端, `visdom/js`目录下(注意这些文件最后安装时会meld到一个main.js里)

    ```
    visdom/js/
    ├── EmbeddingsPane.js
    ├── EventSystem.js
    ├── ImagePane.js
    ├── lasso.js
    ├── main.js
    ├── Pane.js
    ├── PlotPane.js
    ├── PropertiesPane.js
    ├── TextPane.js
    └── Width.js
    ```

Pane翻译为"窗格", UI展示都是一个一个小方格形式, 内容都在方格内体现, 从文件名可以大概知道支持
embeddings(词向量), image, plot, text等组件类型.

每个pane类型都是使用[react][react]组件化渲染, 关于react可到[官网][react]了解更多, 我们暂且简单认为, 每个组
件都有`rander()`方法用来接受输入的数据和返回需要展示的内容, 例如`main.js`:

```{.js .numberLines startFrom="1"}

const PANES = {
  image: ImagePane,
  image_history: ImagePane,
  plot: PlotPane,
  text: TextPane,
  properties: PropertiesPane,
  embeddings: EmbeddingsPane,
};

class App extends React.Component {
  ...
  ...
  render() {
    let panes = Object.keys(this.state.panes).map(id => {
      let pane = this.state.panes[id];

      ...

        let Comp = PANES[pane.type];

        ...

        return (
          <div key={pane.id} className={isVisible ? '' : 'hidden-window'}>
            <ReactResizeDetector handleWidth handleHeight>
              <Comp
                {...pane}
                key={pane.id}
                ...
              />
            </ReactResizeDetector>
          </div>
        );

        ...
}
```

上面的代码意思是从state.panes中获取到一个pane, 根据pane类型选择对应的pane组件, 且把`pane`参数传入组件.

2. visdom: 后端visdom客户端, `visdom/py/visdom/__init__.py`提供各种绘制API以及与`visdom.server`连接建立等.

把visdom作为client类,在backend端使用, 与`visdom.server`建立连接, visdom实例通过绘制api将raw数据(json)封装
打包发到`visdom.server`中, 再由`visdom.server`进行转发.

列出一些方法:

```
   +text : function
   +matplot : function
   +embeddings : function
   +image : function
   +audio : function
   +video : function
   +scatter : function
   +line : function
   +heatmap : function
   +bar : function
   +histogram : function
   +boxplot : function
   +pie : function
   +mesh : function
```

这些方法封装了json原始数据结构,对外屏蔽细节, 这些json数据转发到js端, 需要被js认得, 其实js使用到了
[plot.ly][plot.ly].

3. visdom.server: 中转服务端, `visdom/py/visdom/server.py`, 接收并转发数据, js客户端和visdom客户端的转接桥梁.

基于[tornadoweb][tornadoweb]web框架, 维护所有client端的状态信息, 服务初始化的过程中注册了与client端交互
(uri)的所有handlers, 这些handlers分为两大类,一类给visdom client端使用, 一类给js client端使用, 并且所有
visdom client的连接记录在`Application.sources`, 所有js client的连接记录在`Application.subs`, 有了这两个记
录数据从哪到哪转发就容易做到了.

列出handlers:
```{.py .numberLines startFrom="1"}

class Application(tornado.web.Application):

        ...
        self.state = self.load_state()
        self.layouts = self.load_layouts()
        self.subs = {}
        self.sources = {}
        self.port = port
        ...
        handlers = [
            (r"%s/events" % self.base_url, PostHandler, {'app': self}),
            (r"%s/update" % self.base_url, UpdateHandler, {'app': self}),
            (r"%s/close" % self.base_url, CloseHandler, {'app': self}),
            (r"%s/socket" % self.base_url, SocketHandler, {'app': self}),
            (r"%s/socket_wrap" % self.base_url, SocketWrap, {'app': self}),
            (r"%s/vis_socket" % self.base_url,
                VisSocketHandler, {'app': self}),
            (r"%s/vis_socket_wrap" % self.base_url,
                VisSocketWrap, {'app': self}),
            (r"%s/env/(.*)" % self.base_url, EnvHandler, {'app': self}),
            (r"%s/compare/(.*)" % self.base_url,
                CompareHandler, {'app': self}),
            (r"%s/save" % self.base_url, SaveHandler, {'app': self}),
            (r"%s/error/(.*)" % self.base_url, ErrorHandler, {'app': self}),
            (r"%s/win_exists" % self.base_url, ExistsHandler, {'app': self}),
            (r"%s/win_data" % self.base_url, DataHandler, {'app': self}),
            (r"%s/delete_env" % self.base_url,
                DeleteEnvHandler, {'app': self}),
            (r"%s/win_hash" % self.base_url, HashHandler, {'app': self}),
            (r"%s/env_state" % self.base_url, EnvStateHandler, {'app': self}),
            (r"%s/fork_env" % self.base_url, ForkEnvHandler, {'app': self}),
            (r"%s(.*)" % self.base_url, IndexHandler, {'app': self}),
        ]
```

# DrawIt

```
               server端: souces(数据源)记录所有visdom clients, subs(订阅者)记录所有js clients
              +------------------------------------------------------------------------------+
              |                                                                              |
          处  |                                    state                                     |  处
              |                        |          layouts            |                       |
          理  |                        |             |               |    SocketHandler      |  理
              |      VisSocketHandler  |             |               |    SocketWrap         |
      Backend |      VisSocketWrap     | <---- visdom.server  ---->  |    SaveHandler        | Frontend
              |      PostHandler       |          /     \            |    DataHandler        |
              |      UpdateHandler     |         /       \           |    EnvStateHandler    |
              |                                 /         \               IndexHandler       |
              |                    sources  <---           ---> subs                         |
              |                                                                              |
              |                                     port                                     |
              +------------------------------------------------------------------------------+
                            ^                                               ^
                           /                                                 \
                          /                                                   \
                         /                                                     \
                        /  /vis_socket[_wrap]               /socket[_wrap]      \
                       /                                                         \    /update
          /events     /                                                           \
                     /                                                             \
                    /                                                               \
                   /    /update                                        /env_state    \

       +--------------------+                                                   +-------------------+
       |   visdom client    |                                                   |   js client       |
       |--------------------|                                                   | ------------------|
       |      text          |                                                   |    TextPane       |
       |      image         |                                                   |    ImagePane      |
       |      matplot       |                                                   |    PlotPane       |
       |      scatter       |                                                   |    EmbeddingsPane |
       +--------------------+                                                   +-------------------+
```

# Conclution

1. Frontend, Backend, 以及visdom.server之间通过socket通讯, visdom.server作为他们的中转管理者.

2. Backend的json数据到了Frontend能够以panes展示出来是靠[plot.ly][plot.ly]库, json数据结构很重要.

3. 最新[visdom][visdom]源码加了polling机制.

[visdom]: https://github.com/facebookresearch/visdom
[tutorial]: https://github.com/noagarcia/visdom-tutorial
[plot.ly]: https://plot.ly/javascript
[react]: https://react.docschina.org/
[tornadoweb]: http://www.tornadoweb.org/en/stable/
