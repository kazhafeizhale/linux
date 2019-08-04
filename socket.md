

[TOC]

# socket

## 命令

cat /etc/services 查看服务信息
cat /proc/sys/kernel/hostname **查看主机名字**

### 启动时间服务：time 服务的知名端口是 13

1、发现xinetd服务还未安装，可以用apt-get install xinetd安装

2、vi编辑/etc/xinetd.d/daytime文件，将disable = yes改为disable = no

3、注销系统或重启xinetd服务，用service xinetd stop然后service xinetd start

4、查看网络状态netstat -ant，即可看到

tcp        0      0 0.0.0.0:13              0.0.0.0:*               LISTEN

这是daytime服务的网络监听端口状态

------



## 基本概念

------



## 函数



### 获取主机名字：gethostbyname

```cpp
/*  As usual, make the appropriate includes and declare the variables.  */

#include <netinet/in.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <netdb.h>
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
    char *host, **names, **addrs;
    struct hostent *hostinfo;

/*  Set the host in question to the argument supplied with the getname call,
    or default to the user's machine.  */

    if(argc == 1) {
        char myname[256];
        gethostname(myname, 255);
        host = myname;
    }
    else
        host = argv[1];

/*  Make the call to gethostbyname and report an error if no information is found.  */

    hostinfo = gethostbyname(host);
    if(!hostinfo) {
        fprintf(stderr, "cannot get info for host: %s\n", host);
        exit(1);
    }

/*  Display the hostname and any aliases it may have.  */

    printf("results for host %s:\n", host);
    printf("Name: %s\n", hostinfo -> h_name);
    printf("Aliases:");
    names = hostinfo -> h_aliases;
    while(*names) {
        printf(" %s", *names);
        names++;
    }
    printf("\n");

/*  Warn and exit if the host in question isn't an IP host.  */

    if(hostinfo -> h_addrtype != AF_INET) {
        fprintf(stderr, "not an IP host!\n");
        exit(1);
    }

/*  Otherwise, display the IP address(es).  */

    addrs = hostinfo -> h_addr_list;
    while(*addrs) {
        printf(" %s", inet_ntoa(*(struct in_addr *)*addrs));
        addrs++;
    }
    printf("\n");
    exit(0);
}


```

------



## 流程图

![1564929569882](C:\Users\hetong\AppData\Roaming\Typora\typora-user-images\1564929569882.png)

------







