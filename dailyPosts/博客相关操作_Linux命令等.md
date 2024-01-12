## Ubuntu安装docker

1. 前置条件

查看操作系统和CPU架构：Ubuntu 20.04.1，docker是支持的。CPU是x86_64，docker也是支持的。

```bash
root@iZwz9a84py7p6c5yv6azdrZ:~# cat /etc/*release*
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=20.04
DISTRIB_CODENAME=focal
DISTRIB_DESCRIPTION="Ubuntu 20.04.1 LTS"
NAME="Ubuntu"
VERSION="20.04.1 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04.1 LTS"
VERSION_ID="20.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=focal
UBUNTU_CODENAME=focal

root@iZwz9a84py7p6c5yv6azdrZ:~# uname -a
Linux iZwz9a84py7p6c5yv6azdrZ 5.4.0-47-generic #51-Ubuntu SMP Fri Sep 4 19:50:52 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux

```

如果敲docker命令是command not found，则不需要第2、3步。

```bash
root@iZwz9a84py7p6c5yv6azdrZ:~# docker

Command 'docker' not found, but can be installed with:

apt install docker.io
```



2. 卸载旧版本

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

3. 卸载历史版本

```bash
卸载软件：
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras -y

卸载软件时，原数据还在，想卸载干净还需要删除目录：
rm -rf /var/lib/docker
rm -rf /var/lib/containerd
```



4. 配置docker下载源

```bash
# 课件流程：（不靠谱）
# 安装 curl 命令， curl相当于一个浏览器的客户端，可以从网络下载东西
sudo apt install curl -y
# 创建 gpg key 目录 ，公钥，出于安全考虑的
sudo mkdir -m 0755 -p /etc/apt/keyrings
# 下载 gpg key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor --yes -o /etc/apt/keyrings/docker.gpg

# 官网流程：
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

```

注：查看系统源：

```bash
root@iZwz9a84py7p6c5yv6azdrZ:~# vi /etc/apt/sources.list
```

配置docker源：

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

执行完上面命令其实就是写了一些内容到 /etc/apt/sources.list.d/docker.list 文件里了：

```bash
root@iZwz9a84py7p6c5yv6azdrZ:~# cat /etc/apt/sources.list.d/docker.list
deb [arch=amd64 signedby=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu focal stable
```

5. 更新一下apt源并安装：

```bash
sudo apt update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# 测试是否安装成功
sudo docker run hello-world
```





## 用docker compose 部署Halo时出错

有可能会遇到下面的情况：

```bash
root@iZwz9a84py7p6c5yv6azdrZ:~/halo# docker-compose up -d
ERROR: The Compose file './docker-compose.yaml' is invalid because:
services.halo.healthcheck value Additional properties are not allowed ('start_period' was unexpected)
services.halodb.healthcheck value Additional properties are not allowed ('start_period' was unexpected)
services.halo.depends_on contains an invalid type, it should be an array
```

说是什么语法错误，但这docker-compose.yaml是按官网抄过来的，应该没问题。

问题解决：使用 `docker compose up -d` 而不是 `docker-compose up -d`，去掉 - 

链接：

https://bbs.halo.run/d/3197-docker-compose%E9%83%A8%E7%BD%B2halomysql%E5%A4%B1%E8%B4%A5

