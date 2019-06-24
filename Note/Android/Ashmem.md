---

title: (原创)匿名共享内存之文件描述符传递

date: 2016-06-24 15:52:36
tags: [Android]
categories: [Note]

---

<!-- vim-markdown-toc GFM -->

* [目录树](#目录树)
* [源码](#源码)
    * [2.1 Common.h](#21-commonh)
    * [2.2 AshmemServer.cpp](#22-ashmemservercpp)
    * [2.3 AshmemClient.cpp](#23-ashmemclientcpp)
    * [2.4  Android.mk](#24--androidmk)
* [3. 输出结果](#3-输出结果)
    * [3.1 /data/AshmemServer](#31-dataashmemserver)
    * [3.2 /data/AshmemClient](#32-dataashmemclient)
* [4. 总结](#4-总结)

<!-- vim-markdown-toc -->

<!-- more -->

[源码下载](http://pan.baidu.com/s/1skrrTpz)

## 目录树
 
```{.txt}
Ashmem/
`-- jni
    |-- Android.mk
    |-- AshmemClient.cpp
    |-- AshmemServer.cpp
    `-- Common.h
```
 
## 源码

### 2.1 Common.h

```{.cpp .numberLines startFrom="1"}
#ifndef __Common__H_
#define __Common__H_
#define ASHMEM_SIZE 1024
#define ASHEME_DEV "/dev/ashmem"
#define OPEN_FILE "/var/_cmsg_file"
#define CLIENT_SOCK_PATH "/var/_client_sock"
#define SERVER_SOCK_PATH "/var/_server_sock"
#define TEST_STRING "HelloWorld"
#endif
```
 
### 2.2 AshmemServer.cpp

```{.cpp .numberLines startFrom="1"}
#include <stdio.h>
#include <stddef.h>
#include <fcntl.h>
#include <unistd.h>
#include <sys/socket.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <sys/un.h>
#include <sys/mman.h>
#include <errno.h>
#include <sys/uio.h>
#include <linux/ashmem.h>
#include "Common.h"

int main(int argc, char const* argv[])
{
    int sockfd = -1, clientfd = -1, memfd = -1, testfd = -1;
    int len = 0, ret = 0, err = -1, readn = 0;
    char buf1[5];
    char buf2[4];
    char* mapaddr = 0;
    struct sockaddr_un un;
    struct cmsghdr* cmptr = 0;
    struct cmsghdr* pcmsg = 0;
    struct iovec iov[2];
    struct msghdr msg;
    
    sockfd = socket(PF_UNIX, SOCK_STREAM, 0);
    if (sockfd < 0)
        return err;
    unlink(SERVER_SOCK_PATH);
    memset(&un, 0, sizeof(un));
    un.sun_family = AF_UNIX;
    strcpy(un.sun_path, SERVER_SOCK_PATH);
    len = offsetof(struct sockaddr_un, sun_path) + strlen(SERVER_SOCK_PATH);
    ret = bind(sockfd, (struct sockaddr*)&un, len);
    if (ret < 0) {
        err = -2;
        goto Err;
    }
    ret = listen(sockfd, 1);
    if (ret < 0) {
        err = -3;
        goto Err;
    }
    len = sizeof(un);
    memset(&un, 0, len);
    clientfd = accept(sockfd, (struct sockaddr*)&un, &len); 
    if (clientfd < 0) {
        err = -4;
        goto Err;
    }
    printf("accept client fd = %d\n", clientfd);
    cmptr = (struct cmsghdr*)malloc(CMSG_SPACE(sizeof(int)));
    if (!cmptr) {
        err = -5;
        goto Err;
    }
    iov[0].iov_base = buf1; 
    iov[0].iov_len = sizeof(buf1);
    iov[1].iov_base = buf2; 
    iov[1].iov_len = sizeof(buf2);
    msg.msg_iov = iov;
    msg.msg_iovlen = 2;
    msg.msg_name = 0;
    msg.msg_namelen = 0;
    msg.msg_control = cmptr;
    msg.msg_controllen = CMSG_SPACE(sizeof(int));
    readn = recvmsg(clientfd, &msg, 0);
    if (readn <= 0) {
        err = -6;
        printf("Errno = %d %s\n", errno, strerror(errno));
        goto Err;
    }
    printf("buf1 = %s\nbuf2 = %s\n", buf1, buf2); 
    pcmsg = CMSG_FIRSTHDR(&msg);
    while (pcmsg) {
        if ((pcmsg->cmsg_level == SOL_SOCKET) 
            && (pcmsg->cmsg_type == SCM_RIGHTS)) {
            if (pcmsg->cmsg_len == CMSG_LEN(sizeof(int))) {
                memfd = *(int*)CMSG_DATA(pcmsg);
                break;
            }
        }
        pcmsg = CMSG_NXTHDR(&msg, pcmsg);
    }
    if (memfd > 0) {
        mapaddr = (char*)mmap(0, ASHMEM_SIZE, PROT_READ|PROT_WRITE, MAP_SHARED, memfd, 0);  
        printf("get memfd = %d mapaddr = %p\n", memfd, mapaddr);
    }
    memcpy(mapaddr, TEST_STRING, strlen(TEST_STRING)); 
    sleep(5);
    free(cmptr);
    return 0;
Err:
    printf("error = %d\n", err);
    close(sockfd);
    return err;
}
```
 
### 2.3 AshmemClient.cpp

```{.cpp .numberLines startFrom="1"}
#include <stdio.h>
#include <stddef.h>
#include <fcntl.h>
#include <unistd.h>
#include <sys/socket.h>
#include <sys/stat.h>
#include <sys/mman.h>
#include <sys/types.h>
#include <sys/un.h>
#include <sys/uio.h>
#include <linux/ashmem.h>
#include "Common.h"
int main(int argc, char const* argv[])
{
    int sockfd = -1, memfd = -1, testfd = -1, err = -1;
    int len =0, ret = 0, writen = 0;
    char buf1[5] = {"file"};
    char buf2[4] = {"map"};
    char* mapaddr = 0;
    char content[32] = { 0 };
    struct sockaddr_un un;
    struct cmsghdr* cmptr = 0;
    struct cmsghdr* pcmsg = 0;
    struct iovec iov[2];
    struct msghdr msg;
    
    sockfd = socket(PF_UNIX, SOCK_STREAM, 0);
    if (sockfd < 0)
        return err;
    unlink(CLIENT_SOCK_PATH);
    memset(&un, 0, sizeof(un));
    un.sun_family = AF_UNIX;
    strcpy(un.sun_path, CLIENT_SOCK_PATH);
    len = offsetof(struct sockaddr_un, sun_path) + strlen(CLIENT_SOCK_PATH);
    ret = bind(sockfd, (struct sockaddr*)&un, len);
    if (ret < 0) {
        err = -2;
        goto Err;
    }
    memset(&un, 0, sizeof(un));
    un.sun_family = AF_UNIX;
    strcpy(un.sun_path, SERVER_SOCK_PATH);
    len = offsetof(struct sockaddr_un, sun_path) + strlen(SERVER_SOCK_PATH);
    ret = connect(sockfd, (struct sockaddr*)&un, len);
    if (ret < 0) {
        err = -3;
        goto Err;
    }
    testfd = open(OPEN_FILE, O_CREAT|O_RDWR, 0777);
    memfd = open(ASHEME_DEV, O_RDWR);
    printf("open %s fd = %d\n", ASHEME_DEV, memfd);
    if (testfd < 0 || memfd < 0) {
        err = -4;
        goto Err;
    }
    ret = ioctl(memfd, ASHMEM_SET_SIZE, ASHMEM_SIZE);
    if (ret < 0) {
        err = -5;
        goto Err;
    }
    mapaddr = (char*)mmap(0, ASHMEM_SIZE, PROT_READ|PROT_WRITE, MAP_SHARED, memfd, 0);
    printf("mmap addr = %p\n", mapaddr);
    printf("sizeof(short) = %d\n", sizeof(short));
    printf("CMSG_ALIGN(sizeof(short)) = %d\n", CMSG_ALIGN(sizeof(short)));
    printf("CMSG_LEN(sizeof(short)) = %d\n", CMSG_LEN(sizeof(short)));
    printf("CMSG_SPACE(sizeof(short)) = %d\n", CMSG_SPACE(sizeof(short)));
    cmptr = (struct cmsghdr*)malloc(2*CMSG_SPACE(sizeof(int)));
    if (!cmptr) {
        err = -6;
        goto Err;
    }
    printf("cmptr = %p\n", cmptr);
    iov[0].iov_base = buf1; 
    iov[0].iov_len = sizeof(buf1);
    iov[1].iov_base = buf2; 
    iov[1].iov_len = sizeof(buf2);
    msg.msg_iov = iov;
    msg.msg_iovlen = 2;
    msg.msg_name = 0;
    msg.msg_namelen = 0;
    msg.msg_control = cmptr;
    msg.msg_controllen = 2*CMSG_SPACE(sizeof(int));
    pcmsg = CMSG_FIRSTHDR(&msg);
    printf("first pcmsg = %p\n", pcmsg);
    pcmsg->cmsg_level = SOL_SOCKET;
    pcmsg->cmsg_type = SCM_RIGHTS;
    pcmsg->cmsg_len = CMSG_LEN(sizeof(int));
    *(int*)CMSG_DATA(pcmsg) = memfd;
    //TODO something is wrong!
    pcmsg = CMSG_NXTHDR(&msg, pcmsg);
    printf("next pcmsg = %p\n", pcmsg);
    pcmsg->cmsg_level = SOL_SOCKET;
    pcmsg->cmsg_type = SCM_RIGHTS;
    pcmsg->cmsg_len = CMSG_LEN(sizeof(int));
    *(int*)CMSG_DATA(pcmsg) = testfd;
    writen = sendmsg(sockfd, &msg, 0);
    printf("sendmsg write n = %d (sizeof(buf1) + sizeof(buf2) = %d)\n", writen, sizeof(buf1) + sizeof(buf2));
    sleep(2);
    memcpy(content, mapaddr, strlen(TEST_STRING));
    printf("content = %s\n", content);
    close(sockfd);
    free(cmptr);
    return 0;
Err:
    printf("error : %d\n", err);
    close(sockfd);
    return -1;
}
```
 
### 2.4  Android.mk

```{.mk .numberLines startFrom="1"}
LOCAL_PATH := $(call my-dir)
JNI_RES_PATH := /opt/android/jni
include $(CLEAR_VARS)
LOCAL_MODULE_TAGS := eng
LOCAL_MODULE := AshmemServer
$(info "Build ${LOCAL_MODULE})
LOCAL_SRC_FILES     += AshmemServer.cpp
LOCAL_C_INCLUDES += ${JNI_RES_PATH}/include ${JNI_RES_PATH}/include/bionic/libc/kernel/common/
LOCAL_C_INCLUDES += ${JNI_RES_PATH}/include/bionic/libc/include/
LOCAL_CPPFLAGS   += -DDEBUG -DHAVE_ANDROID_OS  -DHAVE_SYS_UIO_H
# 编译条件以及第三方库: -lxml2
LOCAL_LDFLAGS += -L${JNI_RES_PATH}/libs -O2
# 系统库
LOCAL_LDLIBS += -lcutils -lutils
# 动态库模块, 如果列出的库不存在会去编译 eg. libxxx
LOCAL_SHARED_LIBRARIES += 
# ALL_DEFAULT_INSTALLED_MODULES += $(LOCAL_MODULE)
# 目标输出类型
include $(BUILD_EXECUTABLE)
include $(CLEAR_VARS)
LOCAL_MODULE_TAGS := eng
LOCAL_MODULE := AshmemClient
$(info "Build ${LOCAL_MODULE})
LOCAL_SRC_FILES  += AshmemClient.cpp
LOCAL_C_INCLUDES += ${JNI_RES_PATH}/include  ${JNI_RES_PATH}/include/bionic/libc/kernel/common/
LOCAL_C_INCLUDES += ${JNI_RES_PATH}/include/bionic/libc/include/
LOCAL_CPPFLAGS   += -DDEBUG -DHAVE_ANDROID_OS  -DHAVE_SYS_UIO_H
# 编译条件以及第三方库: -lxml2
LOCAL_LDFLAGS += -L${JNI_RES_PATH}/libs -O2
# 系统库
LOCAL_LDLIBS += -lcutils -lutils
# 动态库模块, 如果列出的库不存在会去编译 eg. libxxx
LOCAL_SHARED_LIBRARIES += 
# ALL_DEFAULT_INSTALLED_MODULES += $(LOCAL_MODULE)
include $(BUILD_EXECUTABLE)
```
 
## 3. 输出结果

### 3.1 /data/AshmemServer

```
accept client fd = 4
buf1 = file
buf2 = map
get memfd = 5 mapaddr = 0x76eb6000
```
 
### 3.2 /data/AshmemClient

```
open /dev/ashmem fd = 5                                                                                                                                                                        
mmap addr = 0x76e7a000                                                                                                                                                                         
sizeof(short) = 2                                                                                                                                                                              
CMSG_ALIGN(sizeof(short)) = 4                                                                                                                                                                  
CMSG_LEN(sizeof(short)) = 14                                                                                                                                                                   
CMSG_SPACE(sizeof(short)) = 16                                                                                                                                                                 
cmptr = 0xc9e0a8                                                                                                                                                                               
first pcmsg = 0xc9e0a8                                                                                                                                                                         
next pcmsg = 0xc9e0b8                                                                                                                                                                          
sendmsg write n = 9 (sizeof(buf1) + sizeof(buf2) = 9)                                                                                                                                          
content = HelloWorld  
```
 
## 4. 总结
- Android匿名共享内存多个进程实现共享, 那么每个进程都要拿到这个/dev/ashmem打开的文件描述符
- 通过msghdr这个结构体加上sendmsg和recvmsg实现文件描述符的进程间传递
- 之所以要进程间传递文件描述符,是因为"匿名"二字, 如果通讯的每个进程都打开/dev/ashmem文件是实现不了的.
- sendmsg是怎样把文件描述符传递的, 是不是有点像dup或dup2, 复制现存的文件描述符, 使他们指向同一个file结构体(本进程内)
