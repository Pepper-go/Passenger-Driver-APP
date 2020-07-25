# GDB

- 是GNU开源组织发布的一个强大的UNIX下的程序调试工具

功能主要有：

- 启动程序，自定义要求运行程序
- 支持断点
- 程序停住时检查程序问题
- 动态改变程序执行环境





## 基本命令

GDB基本命令表

![image-20200708141601599](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200708141601599.png)







## 基本使用





#### 启动gdb

对于一个编译好的可执行程序，test

启动gdb:

```gdb test```

``` gdb -q test （不打印gdb版本信息）```

![image-20200708142324862](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200708142324862.png)

#### 查看源码



启动好gdb后，想要查看源码，用list（l），默认显示10行，回车继续查看

```(gdb) list 
(gdb) list
9	#define MAX_SIZE
10	
11	int main()
12	{
13	    int i,fd,size1 ,size2 ,len;
14	    char *buf = "helo!I'm liujiangyong ";
15	    char buf_r[15];
16	    len = strlen(buf);
17	    fd = open("/home/hello.txt",O_CREAT | O_TRUNC | O_RDWR,0666);
18	    if (fd<0)
(gdb) 
19	        {
20	            perror("open :");
21	            exit(1);
22	        }
23	    else
24	        {
25	        printf("open file:hello.txt %d\n",fd);
26	        }
27	    size1 = write(fd,buf,len);
28	    if (fd<0)
(gdb) 
```

#### 运行程序

run(简写 r) ：运行程序直到遇到 结束或者遇到断点等待下一个命令

![image-20200708142823201](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200708142823201.png)





#### 设置断点

break(简写 b) ：格式 b 行号，在某行设置断点；

- info breakpoints ：显示断点信息
- Num： 断点编号
- Disp：断点执行一次之后是否有效 kep：有效 dis：无效
- Enb： 当前断点是否有效 y：有效 n：无效
- Address：内存地址
- What：位置



![image-20200708142956191](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200708142956191.png)





#### 单步执行

- continue（c）：运行到下一个断点(或程序结束)

- step（s）：：单步调试如果有函数调用，则进入函数
- next（n）：单步，遇到函数调用不进入函数体而直接调用







#### 退出GDB



直接使用quit命令







## 项目中使用

- 在开发过程中，有一个印象深刻的bug，在做前端Android和应用服务器联调的时候，调试登录模块，Android用Libcurl将post数据发送给server，启动服务器后缺一直崩溃
- 查阅过许多关于Linux关于程序崩溃时的错误定位方法，比较常见的有：由于程序崩溃时，经常会看到segmentation falt类似信息（进程非法操作内存），可以通过每个进程堆栈在进程崩溃时会保存的关键信息来定位源代码的错误，Linux下backtrace可以获得当前堆栈信息，但是学艺不精无法定位，后续自会还要多补功课
- 选择GDB单步调试的方法，先在跳过模板代码直接在关键的业务代码设置断点（-b 行数），断点处停止，下一步时程序未崩溃，后续几步仍成正确，则可以设置下一个断点较长的步长，如果崩溃，则缩小断点位置，至合适范围后继续单步调试，最终定位到错误是因为，解析jason时拿到密码时，未按照协议写成passeord而是缩写成passwd，导致程序崩溃

