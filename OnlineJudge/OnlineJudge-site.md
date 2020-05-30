# SYZOJ 网页端安装



注：所有命令均在 root 下面运行，~~懒得输用户密码~~,更多详情请查看 syzoj 项目的 wiki 。

## 安装 node.js

复制

```
apt install nodejs
```

使用 `node -v` 查看 node 版本号是否为 8.16.0 。

## 安装 yarn

在 Debian 或 Ubuntu (18.04以后)上，需要用我们的 Debian 包仓库来安装 Yarn。 配置仓库和安装命令如下：

复制

```
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update && sudo apt-get install yarn
```

使用 `yarn -v` 查看 yarn 版本号是否为 1.16.0 。

## 安装系统依赖项 MariaDB 10.3 与 Redis 5

复制

```
apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://mirrors.tuna.tsinghua.edu.cn/mariadb/repo/10.3/ubuntu bionic main'
add-apt-repository ppa:chris-lea/redis-server
apt update
apt install -y mariadb-server redis-server
```

ps：这里会让你输入一个密码，这个是数据库密码，后面要用到！

使用 `mysql -V` 查看版本号是否为 10.3.14 。
使用 `redis-server -v` 查看版本号是否为 5.0.4 。

## 安装 7z

复制

~~apt install p7zip~~

```
apt-get install p7zip-full
```



## 安装 pygmentize

先安装 python3 和 python3-pip

复制

```
apt install python3 python3-pip
```

安装完了 python3 和 pip3 自己就出现了？？？？

## 安装 clang-format

复制

```
apt install clang-format
```

## 下载 syzoj 源码

复制

```
mkdir -p /opt/syzoj
cd /opt/syzoj
git clone https://github.com/syzoj/syzoj
mv syzoj web
cd web
yarn
```

## 配置 syzoj

复制

```
mkdir -p /opt/syzoj/config
cp /opt/syzoj/web/config-example.json /opt/syzoj/config/web.json
ln -s ../config/web.json /opt/syzoj/web/config.json
```

修改 `/opt/syzoj/web/config.json` , ~~这里查看wiki即可。。~~

复制

```
password
session_secret
judge_token
```

以上三个必须使用随机密码来填写，空着会报错！！！

复制

```
hostname
```

这一栏暂时不要改，安装完毕后用本地的 `127.0.0.1:5283` 测试是否成功，然后在使用 nginx 代理；啊哈，如果你是云服务器，将 hostname 的内容修改为 `::`，使用 `xxx.xxx.xxx.xxx:5283` 来测试网页端是否无误！（ `xxx.xxx.xxx.xx` 代表自己的服务器公网地址）

### 配置数据存放目录

**如果不做，评测的时候会出现 No TestData ！！！**
创建独立的目录用于存放数据和临时文件，这将便于您对网站的维护：

复制

```
mv /opt/syzoj/web/uploads /opt/syzoj/data
ln -s ../data /opt/syzoj/web/uploads
mkdir /opt/syzoj/sessions
ln -s ../sessions /opt/syzoj/web/sessions
```

## 配置用户

## 配置数据库

复制

```
mysql -u root -p
```

这里使用 MariaDB 10.3 的密码。

数据库命令

复制

```
CREATE DATABASE `syzoj` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
GRANT ALL PRIVILEGES ON `syzoj`.* TO "syzoj"@"localhost" IDENTIFIED BY "password";
FLUSH PRIVILEGES;
```

这里记得使用刚才生成的随机密码来替换 password .

## 启动网页

进入 /opt/syzoj/web 目录，运行如下命令来启动syzoj

复制

```
cd /opt/syzoj/web && node app.js
```

## 设置全站管理员

请登录您的 SYZOJ 网址，注册一个用户（你自己的用户），并授予其全站管理员权限，命令如下：

进入数据库（`mysql -u root -p`），输入如下数据库代码：

复制

```
UPDATE `syzoj`.`user` SET `is_admin` = 1 WHERE `id` = 1;
```

`systemd` 和 `nginx` 请查看官网 wiki 。

redis 报错

https://blog.csdn.net/HD243608836/article/details/101451491?utm_medium=distribute.pc_relevant.none-task-blog-baidujs-1

开机启动

https://blog.csdn.net/ViJayThresh/article/details/81317171



代理加速

https://www.zhihu.com/question/276143842

```text
export ALL_PROXY=socks5://127.0.0.1:1080
```

https://cdn.jsdelivr.net/gh/syzoj/sandbox-rootfs/releases@/download/181202/sandbox-rootfs-181202.tar.gz

https://cdn.jsdelivr.net/gh/wfy-belief/atum@v1.15/cnblogLoader.js