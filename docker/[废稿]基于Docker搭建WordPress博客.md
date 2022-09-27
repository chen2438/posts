## 部署 MySQL

```bash
sudo docker run -itd  -p 3306:3306 \
 --restart=always  --user=root --privileged=true \
 -e MYSQL_ROOT_PASSWORD=123456  -e MYSQL_DATABASE=wordpress\
 -v /opt/docker/mysql:/var/lib/mysql \
--name=mysql mysql:latest
```

**参数介绍**：

- -p 本地 3306 端口映射到 mysql 3306 端口

- --restart=always 在容器退出时总是重启容器
- --user=root 容器中的进程以 root 用户权限运行
- --privileged=true 获取完整权限（Full container capabilities）

- -e MYSQL_ROOT_PASSWORD=123456 ：设置数据库的 root 用户的密码
- -e MYSQL_DATABASE=wordpress ：初始化新建一个名为 wordpress 的数据库
- -v /opt/docker/mysql:/var/lib/mysql ：将 mysql 数据映射到本地

## 部署 WordPress

```bash
sudo docker run -itd  -p 80:80 -p 443:443 \
  --restart=always  --user=root --privileged=true \
  --link mysql:mysql \
  -e WORDPRESS_DB_HOST=mysql:3306 -e WORDPRESS_DB_USER=root \
  -e WORDPRESS_DB_PASSWORD=123456 -e WORDPRESS_DB_NAME=wordpress \
  -v  /opt/docker/wordpress:/var/www/html \
  --name wordpress wordpress
```

**参数介绍**：

- –link mysql:mysql ：将上文部署的 mysql 链接到本容器内，用于 WordPress 访问数据库
- -e WORDPRESS_DB_HOST=mysql:3306 ：指定数据库地址
- -e WORDPRESS_DB_USER=root ：指定数据库登录用户名
- -e WORDPRESS_DB_PASSWORD=123456 ：指定数据库登录密码
- -e WORDPRESS_DB_NAME=wordpress ：指定数据库名称
- -v /opt/docker/wordpress:/var/www/html ：将 WordPress 相关文件映射到本地

## 登录 WordPress

服务器防火墙放行 80 端口，在浏览器输入 http://域名 即可

![image-20220907214404763](https://nme-200t.oss-cn-hangzhou.aliyuncs.com/template/202209072144847.png)