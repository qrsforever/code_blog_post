---

title: Message-Handler
date: 2018-05-31 13:30:25
tags: [C++]
categories: [Design]

---

<!-- vim-markdown-toc GFM -->

* [框架图](#框架图)

<!-- vim-markdown-toc -->

<!-- more -->

## 框架图

```

            +--------------------------------------------------------------------------------------------------+
            |                                                                                                  |
            |                                                                                                  |
            |                                                                       +-----------------+        |
            |                                                                       |     Message     |        |
            |                                                                    -->|-----------------|        |
            |                                       +----------------+          /   |      what       |        |
            |                                       |  MessageQueue  |         /    |    arg[1|2|3]   | target |
            |                                 ----->|----------------|        /     |     target      |◇ ------+
            |                                /      |    mMessages   |-------/      |-----------------|
            |                               /       |----------------| mMessage     |     obtain      |
            |                              /        | enqueueMessage |              |    recycle      |
            v                             /         |  removeMessage |              |      next       |
 +--------------------+                  /          |   nextMessage  |              +-----------------+
 |   MessageHandler   |                 /           +----------------+
 |--------------------| mMessageQueue  /
 |   mMessageQueue    |---------------/
 |   mMessageHandler  |◆ -----------------+
 |--------------------|  mMessageHandler  |
 |  dispatchMessage   |                   |                +------------+
 | sendMessage[Delay] |                   |        +-----▷ |   Thread   |
 |   handleMessage    |                   |        |       |------------|
 +--------------------+                   |        |       |    PID     |
           △                              |        |       |------------|
           |                              |        |       |    run     |
           |                              |        |       +------------+
           |                              v        |
 +-------------------+              +----------------------+
 | RuleEngineHandler |              | MessageHandlerThread |
 |-------------------|              |----------------------|
 |   handleMessage   |              |    mMessageQueue     |      +--------------------------------------+
 +-------------------+              |----------------------|      |while(true)                           |
                                    |        run           |----->|   msg = mMessageQueue->nextMessage() |
                                    +----------------------+      |   msg->target->dispatchMessage()     |
                                                                  +--------------------------------------+
```
