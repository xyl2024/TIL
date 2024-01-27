# 问题描述

![](img/2024-01-13-01-43-14-image.png)

本地挂了VPN，网页端可以访问github，但是git bash命令行依然没法正常访问github。

# 原因

git bash在拉取仓库时，没有使用vpn进行代理。

# 解决

```bash
git config --global http.proxy 127.0.0.1:7890
git config --global https.proxy 127.0.0.1:7890
```

![](img/2024-01-13-01-47-37-image.png)

# 注意

修改代理之后，下次再git clone、push、pull的时候，可能也需要开着vpn，除非把代理关掉。

比如我之后想克隆gitee上某个仓库时遇到的问题：# OpenSSL SSL_connect: Connection was reset in connection to gitee.com:443

可能就是因为之前设置了代理而当时没开vpn就clone了。

[【已解决】OpenSSL SSL_connect: Connection was reset in connection to github.com:443-CSDN博客](https://blog.csdn.net/qq_37555071/article/details/114260533)

**取消代理命令**：

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

# References

https://blog.csdn.net/zpf1813763637/article/details/128340109