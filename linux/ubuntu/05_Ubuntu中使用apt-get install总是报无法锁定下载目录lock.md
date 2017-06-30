错误：
> 无法获得锁/var/cache/apt/archives/lock-open(11 Resource temporalily unavailable)

出现这种错误的原因是：

> 有另外一个apt在运行，或者是上一个apt不正常退出。  
> apt包管理系统访问软件数据库是需要上锁的~ 这就是那个锁
> 如果能确认没有另外的apt在运行，直接将其删除即可。

解决办法：

    sudo rm /var/cache/apt/archives/lock
    # 使用该命令之前先要了解/var/cache/atp/archives/目录，以及lock文件
    sudo rm /var/lib/dpkg/lock
