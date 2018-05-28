---

title: IOT之规则引擎
date: 2018-05-27 21:04:19
tags: [ IOT, DrawIt ]
categories: [ Note ]

---


<!-- vim-markdown-toc GFM -->

* [Design](#design)
    * [Framework](#framework)
    * [Class](#class)
* [Develop](#develop)
    * [Module Tasks](#module-tasks)
    * [Module Sample](#module-sample)
* [TODO](#todo)
    * [Test Supported](#test-supported)

<!-- vim-markdown-toc -->

Design
======

Framework
---------

```
v0.0.1
                             ╔═════════════════╦════════════════════════════════════════╗
   *******      |            ║                 ║                                        ║
*** Cloud ***   |            ║                 ║ Log / MQ / Json / DataChannel / Time   ║
   *******      |     Rule   ║                 ║                                        ║
      |         |  --------> ║     Rules       ╠════════════════════════════════════════╣
      |         |            ║    Assemble     ║                                        ║
      +-------> |            ║                 ║         Rule Engine Driver             ║
    json rule   |            ║                 ║                                        ║
                |            ║                 ║                                        ║
                |            ╠════════════╦════╩══════╦═╦═══════════════════════════════╣
          +-----------+      ║            ║           ║ ║                               ║
          |           |      ║   Global   ║  Devices  ║ ║      Clips C++ Interface      ║
          |  convert  |      ║            ║           ║ ║                               ║
          |           |      ╠═══════════CLP══════════╣ ╠═══════════════════════════════╣
          +-----------+      ║            ║           ║ ║                               ║
   1.json rule parse         ║    Rules   ║   Utils   ║ ║         Clips Core            ║
   2.map profile/alias       ║            ║           ║ ║                               ║
                             ╚════════════╩═══════════╩═╩═══════════════════════════════╝

```

说明:

1. 云端下发的json规则, 需要转换成Rule对象(这个转换不属于规则引擎模块, 只提供Rule类头文件), 传递给rule translate模块.
2. 云端下发的json规则本地化存储不属于规则引擎模块负责, 但传给rule translate模块翻译后的clip规则文件, 规则引擎模块负责存储该文件.

3. 触发规则引擎的事件(属性变化等)需要胡老师提供注册回调的API
4. 规则引擎触发的动作(改变属性等)需要胡老师提供相关调用的API (同步异步)


Class
-----

### Message

```
v0.0.1
            +--------------------------------------------------------------------------------------------------+
            |                                                                                                  |
            |                                                                                                  |
            |                                                                       +-----------------+        |
            |                                                                       |     Message     |        |
            |                                                                    -->|-----------------|        |
            |                                       +----------------+          /   |      what       |        |
            |                                       |  MessageQueue  |         /    |    arg[1|2|3]   | target |
            |                                 ----->|----------------|        /     |     target      |c1------+
            |                                /      |    mMessages   |-------/      |-----------------|
            |                               /       |----------------| mMessage     |     obtain      |
            |                              /        | enqueueMessage |              |    recycle      |
            v                             /         |  removeMessage |              |      next       |
 +--------------------+                  /          |   nextMessage  |              +-----------------+
 |   MessageHandler   |                 /           +----------------+
 |--------------------| mMessageQueue  /
 |   mMessageQueue    |---------------/
 |   mMessageHandler  |c2-----------------+
 |--------------------|  mMessageHandler  |
 |  dispatchMessage   |                   |                +------------+
 | sendMessage[Delay] |                   |        +-----e4|   Thread   |
 |   handleMessage    |                   |        |       |------------|
 +--------------------+                   |        |       |    PID     |
           e1                             |        |       |------------|
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
                                               e1                 +--------------------------------------+
 +-------------------+                         |
 | RuleEngineThread  |                         |
 |-------------------|-------------------------+
 |                   |
 |-------------------|
 |       run         |
 +-------------------+

```

### Log

```
v0.0.1
                                                                  +-----------------------+
        +-------------------+     +----------------+              |      RingBuffer       |
        |  MessageHandler   |     |    DataSink    |         ---->|-----------------------|
        |-------------------|     |----------------|        /     |      mBufHead         |
        |  mMessageQueue    |     |   mRingBuffer  |-------/      |      mBufLength       |
        |  mMessageHandler  |     |   m_dataSize   | mRingBuffer  |-----------------------|
        |-------------------|     |----------------|              |  get[Write/Read]Head  |
        |   handleMessage   |     |  onDataArrive  |              |  submit[Write/Read]   |
        +-------------------+     +----------------+              +-----------------------+
                e1                      e1  ^
                |                       |   |
                +-----------+-----------+   +-------------------------------------------------------------------------------+
                            |                                                  +------------+                               |
                            |                                                  |            |                               |
                   +-----------------+                                         |            |                               |
         +-------> |    LogPool      |                                         v            |                               |
         |         |-----------------|  mFilterHead                      +-----------+      |                               |
         |         |   mFilterHead   |c1-------------------------------->| LogFilter |      |                               |
         |         |-----------------|                                   |-----------|      |                               |
         |         |   attachFilter  |                                   |  m_next   |c1----+                               |
         |         |   detachFilter  |                                   |-----------|  m_next                              |
         |         |                 |                                   | pushBlock |                                      |
         |         |   onDataArrive  |                                   +-----------+                                      |
         |         |   receiveData   |                                     e1  e1  e1                                       |
         |         |                 |                                     |   |   |                                        |
         |         |  handleMessage  |                                     |   |   |                                        |
         |         +-----------------+              +----------------------+   |   +-----------------------+                |
         |                                          |                          |                           |                |
         |                                          |                          |                           |                |
         |         +-----------+           +-----------------+        +----------------+          +-----------------+       |
         |         | LogThread |           |  ConsoleFilter  |        |   FileFilter   |          |  NetworkFilter  |       |
         |         |-----------|           |-----------------|        |----------------|          |-----------------|       |
         |         |           |           |                 |        |                |          |                 |       |
         | Create  |-----------|           |    pushBlock    |        |   pushBlock    |          |    pushBlock    |       |
         +---------|   run     |           +-----------------+        +----------------+          +-----------------+       |
                   +-----------+                                                                                            |
                         |                                                                                                  |
                         |                                                                                                  |
                         |                                                                                                  |
                         e2                                                                                                 |
              +----------------------+                   +---------------+                                                  |
              | MessageHandlerThread |                   |    Logger     |                                                  |
              |----------------------|                   |---------------|    mDataSink                                     |
              |    mMessageQueue     |                   |   mDataSink   |c1------------------------------------------------+
              |----------------------|                   |---------------|
              |        run           |                   |     log       |
              +----------------------+                   +---------------+

```

### Rule Engine Driver

```
v0.0.1

            +--------------+           +---------------+
            | DeviceShadow |           |  PropertySlot |
            |--------------|     +---->|---------------|
            |   mClsName   |     |     |    mName      |
            |   mSlots     |c1---+     |    mType      |
            |--------------| mSlots    |---------------|
            |              |           |               |
            +--------------+           +---------------+
                   ^                                                        +--------------------+
                   |                                                  ----->| RuleEngineHandler  |
                   |                                                 /      |--------------------|
                   |                                                /  /--c1|       mDriver      |
                   |          +-----------------------+            /  /     |--------------------|
                   |          |   RuleEngineDriver    |           /  /      |  handleMessage     |
                   | 1:n      |-----------------------|          /  /       |                    |
                   +--------c1|       mShadows        | mHandler/  /        |  handleRuleEvent   |
               mShadows       |       mHandler        |c1------/  /         |  handlePropertySet |
                              |       mClips          |<---------/          |  handleTimerEvent  |
                              |-----------------------|         mDriver     +--------------------+
                              |  [setup/start]Clips   |                           |     |
                              |                       |                           |     |
                              |     doPropertySet     |                    -------+     +------\
                              |     doTimerEvent      |                   /                     \
                              |     doRuleEvent       |                  /                       \
                              +-----------------------+                 /                         \
                                                           +---------------------+        +----------------------+
                                                           |                     |        |                      |
                                                           |DeviceManagerWrapper |        |   CloudRuleCovert    |
                                                           |                     |        |                      |
                                                           +---------------------+        +----------------------+


```

Develop
=======

Module Tasks
------------

1. Common Modules: Message Queue and Message Handle, Log, Timer

2. C++ Clips Interface

3. Rule Engine Manage

4. CLP files: Global, Devices, Utils, Rules

5. Rules Translate

Module Sample
-------------



TODO
====

Test Supported
--------------

Items | Support | Sample
:-------- | :----------: | :------
math: =,!= | yes | not impl
math: +,-,\*,/ | yes | not impl
math: >,< | yes | not impl
logic: and,or | yes | not impl
timer | no | not impl
update trigger | yes | not impl
state trigger | no | not impl
