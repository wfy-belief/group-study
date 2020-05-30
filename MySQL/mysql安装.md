完整的纯净系统

换源

```
# 首先备份源列表
cp /etc/apt/sources.list /etc/apt/sources.list_backup
```

```
# 打开sources.list文件
vim /etc/apt/sources.list
```

编辑**/etc/apt/sources.list**文件, 在文件最前面添加阿里云镜像源：

```text
#  阿里源
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

刷新

```
apt-get update
apt-get upgrade
```

出现警告

```
Reading package lists... Done
E: Could not get lock /var/lib/apt/lists/lock - open (11: Resource temporarily unavailable)
E: Unable to lock directory /var/lib/apt/lists/
```

执行下面命令关闭进程

```
rm /var/lib/apt/lists/lock
```

依次执行刷新命令 

安装

```
sudo apt-get install mysql-server -y
```



