---

title: (原创)MediaBinder-草稿

date: 2018-03-08 16:47:05
tags: [Android]
categories: [Note]

---

[源码](https://pan.baidu.com/s/1c1mSTDe)

![MediaBinder](https://raw.githubusercontent.com/qrsforever/assets/master/Note/Android/MediaBinder.jpg)

# 总结
1. 音视频媒体播放机制主要在binder.   
2. BpBinder是个代理, 主要传输工作交给了ICPThreadState类.   
3. Interface用来制订业务, Binder用来实现通讯, 分工明确.   
4. 通讯属于C-S模式, Bnxxx是本地对象, Bpxxx是代理对象, 一端是本地服务, 另一端是远程代理.   
5. 本地<->代理 类型的转换是在kernel中binder.c文件的binder_transaction函数中实现的.  
6. binder_transaction函数会为代理生成handle, 在用户空间通过ProcessState::getStrongProxyForHandle创建BpBinder代理.  
7. Parcel类readStrongBinder和writeStrongBinder记录或还原obj的type, binder, cookie.  
6. Binder驱动层面的结构体未在图中体现. 需要单独分析.  

