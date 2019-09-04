---

title: 改进visdom来适应项目

date: 2019-09-04 20:48:29
tags: [Cauchy]
categories: [Company]

---


<!-- vim-markdown-toc GFM -->

* [当前现状](#当前现状)
* [改进方案](#改进方案)
* [代码](#代码)
    * [Visdom Client](#visdom-client)
    * [WebSocket Client](#websocket-client)
    * [Visdom Server](#visdom-server)
* [测试](#测试)

<!-- vim-markdown-toc -->

<!-- more -->

# 当前现状

当前Web和Cauchy框架的交互逻辑为:


```


                     Web              PHP             Framework

                      |                |                  |
  parameters -------->|                |                  |
                      |                |                  |
       train -------->|                |                  |
                      |   freeport     |   (8140-8199)    |
                      |--------------->|                  |
                      |                |    free_port     |
                      |                |----------------->| start visdom.server process
                      |                | vis.port vis.pid |
                      |                |<-----------------| (每一个user会对应一个visdom进程)
                      |                |                  |
                      | start_monitor  , connnect visdom  |
                      |---------------------------------->|
                      |                |                  |
                      |   starttrain   |                  |
                      |--------------->|                  |
                      |                |      train       |
                      |                |----------------->| start train task process
                      |                |                  |
                      |                |                  |
                      |                |                  |
        stop -------->|                |                  |
                      |   stroptrain   |                  |
                      |--------------->|                  |
                      |                |    stop_train    |
                      |                |----------------->| kill train task and visdom.server
                      |                |                  |

```

1. 每个user对应一个visdom.server进程

2. 唯一停止train进程的只有页面上的**停止按钮**

3. 页面刷新或者退出重新登陆, 虽然后台框架仍在训练, 但页面没有记忆, 回复如初.


# 改进方案

启动一个visdom.server进程来管理多个后端(框架训练任务产出)和多个前端(数据传到UI显示), 即多用户共享一个
visdom.server进程, 假设端口为固定**8186**.

框架服务对多用户的任务进程进行维护, 当然每个用户实际上也可以启动多个训练任务, 所以框架管理进程最小单位不是
针对一个用户, 而是**任务**(包括测试/评估任务), 框架同时需要提供获取每个用户下的仍在继续训练的任务信息
**taskinfo**, 例如: `project_id`, `task_pid`, `task_id`等等

Web通过获取到当前用户下仍在训练任务信息**taskinfo**. 可以方便展示出来, 及时提醒用户, 比如, 用户刷新了页面
或者重新登陆了, 立刻提示仍在训练的任务, 并可以直接跳转到这些任务的训练情况页面, 跳转到这个页面后, 直接去连
接**visdom.server:8186**, 因为训练还在进行, 一旦连接上, 就会有数据继续显示.


改进后的交互逻辑

```
                                          +-------------+
                                          |visdom.server|
                                          |             |
             Web           PHP            |  Framework  |            PHP             Web
                                          +-------------+
login         |             |                    |                    |               |
refresh  ---->|             |                    |                    |               |  (USER-2)
              | gettaskinfo |                    |                    |               |
  (USER-1)    |------------>|                    |                    |  gettaskinfo  |<-------login
              |             |   get_task_info    |                    |<--------------|
              |             |------------------->|     get_task_info  |               |
              |             |                    |<-------------------|               |
   display    |             |<-------------------|                    |               |
     taskinfo |<------------|                    |------------------->|               |    display
              |             |                    |                    |-------------->| taskinfo
              |             |                    |                    |               |
    goto ---->|             |                    |                    |               |(没有运行的任务)
     tabpage  |     connect to visdom.server     |                    |               |
              |--------------------------------->|         connect to visdom.server   |<-----train
              |             |                    |<-----------------------------------|
              |      the newer trainning data    |                    |   starttrain  |
    monitor   |<-------------------------------- |      train         |<--------------|
    trainning |             |                    |<-------------------|               |
              |             |                    |                    |               |   monitor
              |             |                    |         the newer trainning data   |   monitor
              |             |                    |----------------------------------->| trainning
              |             |                    |                    |               |
              |             |                    |                    |               |
```


# 代码

每个任务分配一个`task_id`, 唯一的标示任务, 在页面上**我的项目**里面可以创建多个项目每个项目对应一个
`project_id`, 简单设计的话, `task_id`等于`project_id`, 意味着同一个项目不能同时启动两个不同的任务, 即不能
一边训练,一边测试/评估, 保险一些的话, 让`task_id = user_id + project_id`, 保证全局(all users)唯一.

用户项目创建之后, `task_id`唯一定了, 在启动任务连接**visdom.server**时, 创建socket就将这个`task_id`传递到
**visdom.server**中, 取代原有的sid.

同时, `task_id`也会下发到**framework**中, **framework**启动train, 建立visdom客户端, 将`task_id`作为参数去
构造vimsdom, 那么这个`task_id`就是纽带一样将**后端**(visdom客户端)和**前端**(websocket客户端)联系在一起.

## Visdom Client

`py/visdom/__init__.py`:

```{.py .numberLines startFrom="1"}

class Visdom(object):

    def __init__(
        self,
        server='http://localhost',
        endpoint='events',
        port=8097,
        base_url='/',
        ipv6=True,
        http_proxy_host=None,
        http_proxy_port=None,
        env='main',
        send=True,
        raise_exceptions=None,
        use_incoming_socket=True,
        log_to_filename=None,
        username=None,
        password=None,
        proxies=None,
        task_id=None,    # qrs: 增加一个task_id参数
    ):

    ...  # Utils
    def _send(self, msg, endpoint='events', quiet=False, from_log=False):
        ...
        # qrs: 框架使用所有的绘制api最终都这, 所以消息中加个task_id(传到visdom.server)
        if self.task_id is not None:
            msg['task_id'] = self.task_id

        try:
            r = self.session.post(
                "{0}:{1}{2}/{3}".format(self.server, self.port, self.base_url, endpoint),
                data=json.dumps(msg),
            )

            ...

            return r.text
        except requests.RequestException:
            ...

```

## WebSocket Client

`basic/views/cauchy/train.php`:

```{.py .numberLines startFrom="1"}

   function initsocket(type,ip,port,task_id){
      try {
          # qrs: 与visdom.server建立连接时将task_id传递到visdom.server中
          socket = new WebSocket('ws://' + ip + ':'+port+'/socket?task_id='+task_id);
          ...
          socket.onopen = function(evt) {
          };
          socket.onerror = function(evt){
          };
          socket.onmessage = function(evt){
          };
     }
     ...
```

## Visdom Server

`py/visdom/server.py`:

```{.py .numberLines startFrom="1"}

# Websocket客户端connect visdom.server
class SocketHandler(BaseWebSocketHandler):
   ...

   def open(self):
      if self.login_enabled and not self.current_user:
          print("AUTH Failed in SocketHandler")
          self.close()
          return
      # qrs: Web连接时, 传递`task_id`作为sid, 最终放到全局subs列表中
      sid_ = self.get_arguments('task_id')
      if sid_ is None:
          self.sid = str(hex(int(time.time() * 10000000))[2:])
      else:
          self.sid = sid_[0]
      if self not in list(self.subs.values()):
          self.eid = 'main'
          self.subs[self.sid] = self
      logging.info(
          'Opened new socket from ip: {}'.format(self.request.remote_ip))
       ...


# visdom客户端post训练数据时到visdom.server
class PostHandler(BaseHandler):
class UpdateHandler(BaseHandler):
    ...
    @check_auth
    def post(self):
       ...
       args = tornado.escape.json_decode(
           tornado.escape.to_basestring(self.request.body)
       )

       # qrs: /events,/update的消息
       self.task_id = req.get('task_id')

       ...

# 广播数据到WebSocket客户端, Cauchy是一对一没必要广播, 转发到对应的task_id端
def broadcast(self, msg, eid):
    # qrs: self是PostHandler,UpdateHandler, 判断是否含有task_id
    task_id = getattr(self, 'task_id', None)
    if task_id is None:
        for s in self.subs:
            if type(self.subs[s].eid) is list:
                if eid in self.subs[s].eid:
                    self.subs[s].write_message(msg)
            else:
                if self.subs[s].eid == eid:
                    self.subs[s].write_message(msg)
    else:
        # qrs: 如果有task_id, 将数据转发到web端.
        if task_id in self.subs:
            if type(self.subs[task_id].eid) is list:
                if eid in self.subs[task_id].eid:
                    self.subs[task_id].write_message(msg)
            else:
                if self.subs[task_id].eid == eid:
                    self.subs[task_id].write_message(msg)
```

# 测试

在测试服务器上测试, 方案可行.
