# CPU占用过高调试





## CPU高占用率代码定位



- 部署的服务器，会出现CPU占用率特别高的情况，这是不正常的



#### top

 使用```top```命令定位异常进程

![image-20200708145705369](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200708145705369.png)

- 可以从输出信息查看到，CPU和内存的占用率都非常高



#### top -H -p pid

使用 ```top -H -p pid```查看该pid进程的异常线程

![image-20200708145903367](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200708145903367.png)





#### printf "%x\n" 线程号

使用```printf "%x\n" 线程ID```	 线程号将异常线程号转化为16进制



![image-20200708150221043](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200708150221043.png)



#### jstack PID|grep 16进制异常线程号 -A90

- 使用```jstack PID|grep 16进制异常线程号 -A90``来定位异常代码的位置（最后的-A90是日志行数，也可以输出为文本文件或使用其他数字），可以看到异常代码的位置
- 检查相应代码会发现有死循环存在





![image-20200708150502743](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200708150502743.png)