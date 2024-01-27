# 问题描述

登录linux时，用户名@后面的那一串太长了：

![](img/2024-01-13-03-48-32-image.png)

很烦，所以想改掉他。

# 方法

修改配置文件/etc/hostname的内容即可：

```bash
root@iZwz9a84py7p6c5yv6azdrZ:~# vi /etc/hostname


# 重启
root@iZwz9a84py7p6c5yv6azdrZ:~# reboot
```

成功了：

![](img/2024-01-13-03-50-06-image.png)
