### 无法连接远程Linux数据库解决方法

进入数据库

```
show grants;
```

提前在my.cnf注释掉 127.0.0.1。可能路径在 `/etc/mysql/mysql.conf.d`

然后变更权限
注意自己的配置
root 代表登录名（可以更改）
% 代表任意IP（不用更改）
password 密码（必须更改为自己的密码）

```
grant all privileges on *.* to 'root'@'%' identified by 'password';
```

刷新权限

```
flush privileges;
```

重启服务

```
service mysql restart
```

就可以连接了！！！
