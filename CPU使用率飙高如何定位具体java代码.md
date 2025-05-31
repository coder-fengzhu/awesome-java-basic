## 方式一：
1. jps列出所有java进程id
2. top -p [PID] -H 拿到threadId转化成十六进制
3. jstack PID |grep '线程ID' -A100



## 方式二：
1. 直接用arthas attach
2. thread 3命令

