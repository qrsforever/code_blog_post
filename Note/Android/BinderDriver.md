---

title: (原创)Binder简图-草稿

date: 2016-03-24 15:50:13
tags: [Android]
categories: [Note]

---

[高清图](https://pan.baidu.com/s/1o7bNB6M)

![BinderDriver](https://raw.githubusercontent.com/qrsforever/assets/master/Note/Android/Binder-Driver.jpg)

# 总结:
1. Java层的Binder是通过在底层创建对应的本地Binder, 然后就和C++层一样了. Java层保存本地的一个引用mObject即
   可.  
2. 代理服务是通过mRemote(BpBinder)中mHander找到对应的本地服务.  
3. mHandler的由来: SMgrProxy.addService("Tn", new Tnatvie()),这个过程就在Binder驱动中创建了Binder实体, 而
   且SMgr本地服务需要管理存储这个Tnative服务, 驱动同时就创建了Binder引用对象, 这个过程就产生了handler值(加
   1操作), Smgr本地服务也是保存该引用.  
4. IPCThreadState是唯一能和Binder驱动对话的类, 所以对上下文数据的转换(为Binder驱动认识的数量类型), 就是在
   该类中完成.  
