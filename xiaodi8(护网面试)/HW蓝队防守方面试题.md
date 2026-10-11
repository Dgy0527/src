# 1、应急响应
![alt text](../img-2/image-2.png)
## (1)基本思路流程
### 收集信息:收集客户信息和中毒主机信息,包括样本
### 获取基础数据(判断类型):判断是否是安全事件,何种安全事件(勒索、挖矿、断网、Dos等)
### 抑制范围:隔离使受害面不继续扩大,防止事件扩散
### 深入分析:日志分析、进程分析、启动项分析、样本分析,方便后期溯源
### 清理处置:杀掉进程、删除文件、打补丁、删除异常系统服务、清除后门账号,处理完后恢复生产
### 产出报告:整理并输出完整的安全事件报告
### 总结复盘

# 2、Windows入侵排查思路
![alt text](../img-2/image-3.png)
## （1）检查系统账号安全
### `netstat -ano/netstat -anob` :查看服务器是否有弱口令,远程管理端口是否对公网开放
### 使用`lusrmgr.msc`查看服务器是否存在可疑账号、新增账号
### 检查管理员群组(Administrators)里的新增账户,如果有则立即禁用或删除
### 使用D盾或者注册表查看是否存在隐藏账号、克隆账号
### 结合日志,查看管理员登陆时间、用户名是否存在异常
#### win+R -> eventvwr.msc -> Windows日志 -》 安全 -> 导出 -> Log Parser分析
## (2)检查异常端口、进程
### `netstat -ano` :检查端口连接情况
### 任务管理器查看进程 :win+R -> `taskmgr`
## (3)检查启动项(taskmgr)、计划任务(taskschd.msc)、服务(services.msc)
## (4)查看系统相关信息
### 系统版本以及补丁信息
### 查找可疑目录及文件
### 日志分析

# 3、Linux入侵排查思路
![alt text](../img-2/image-4.png)
![alt text](../img-2/image-5.png)
![alt text](../img-2/image-6.png)
![alt text](../img-2/image-7.png)
## (1)账号安全
### `who` :查看当前登录用户(tty本地登录,pts远程登录)
### `w` :查看系统信息和用户行为
### `uptime` :查看登陆时长、用户数、负载
## (2)用户信息文件
### `/etc/passwd` :任何人可访问
### `/etc/shadow` :仅root可访问
## (3)历史命令
### `history` :查看root的历史命令
### `history -c` :清除历史,需手动删除文件记录
## (4)检查异常端口
### `netstat -antlp | more`
#### a :all
#### n :numeric
#### t :tcp
#### l :listening
#### p :program
![alt text](../img-2/image-8.png)
![alt text](../img-2/image-9.png)
### `ls -l /proc/$PID/exe` :查看进程文件路径
![alt text](../img-2/image-10.png)
![alt text](../img-2/image-11.png)
### `file /proc/$PID/exe` :查看进程文件类型
## (5)`ps aux | grep pid` :查找特定进程
![alt text](../img-2/image-12.png)
![alt text](../img-2/image-13.png)
## (6)
### `/etc/rc.d/rc[0~6].d`
![alt text](../img-2/image-14.png)
##  `/etc/systemd`
![alt text](../img-2/image-15.png)
![alt text](../img-2/image-16.png)
## (7)检查定时任务
### `crontab(cron table) -l` :查看当前用户定时任务
### `crontab -e` :编辑当前用户定时任务
### `crontab -r` :删除当前用户所有定时任务(慎用)
## (8)检查服务
### `chkconfig --list`
![alt text](../img-2/image-17.png)
## (9)Linux登录日志查看
### `/var/log/wtmp` :存储登录成功的信息 
### `/var/log/btmp` :存储登录失败的信息 
### `/var/log/utmp` :存储当前正在登录的信息
### `cat /etc/passwd | grep '/bin/bash'` :查看可登录账户
### `lastlog` :查看所有用户最后的登录信息
### `last` :查看用户最近登录信息
### `who` :查看当前登录系统情况
## (10)Linux常用排查命令
### `top` :查看当前系统状态
### `uname -a` :操作系统信息
### `ps` :查看系统进程信息
### `history` :列出历史命令
### `netstat` :列出本机所有连接和监听端口
### `lsof(list open file)` :查看谁在使用某个端口
![alt text](../img-2/image-18.png)
### `lastb(last bad/last btmp)` :查看用户错误的登录信息
![alt text](../img-2/image-19.png)
![alt text](../img-2/image-20.png)
### kali要安装`util-linux`软件包 :sudo apt install util-linux
![alt text](../img-2/image-21.png)