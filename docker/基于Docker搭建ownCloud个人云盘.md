**[官方文档](https://doc.owncloud.com/server/10.10/admin_manual/installation/docker/)**

机翻气息贯穿全文~

## 概述

配置：

- 公开端口 8080，允许 HTTP 连接。
- 使用单独的 *MariaDB* 和 *Redis* 容器。
- 将数据和 MySQL 数据目录挂载到主机上以进行持久存储。

以下说明假定您在本地安装。对于远程访问，必须调整 `OWNCLOUD_DOMAIN` 的值。

1. 创建一个新的项目目录。
2. 创建`docker-compose.yml`，然后将示例从[此页面](https://raw.githubusercontent.com/owncloud/docs-server/master/modules/admin_manual/examples/installation/docker/docker-compose.yml)复制到其中。
3. 创建一个配置文件`.env`，其中包含所需的配置设置。

只需要几个设置，这些是：

| 设置名称           | 描述                 | 例               |
| :----------------- | :------------------- | :--------------- |
| `OWNCLOUD_VERSION` | 自己的云版本         | `latest`         |
| `OWNCLOUD_DOMAIN`  | 自有云域             | `localhost:8080` |
| `ADMIN_USERNAME`   | 管理员用户名         | `admin`          |
| `ADMIN_PASSWORD`   | 管理员用户的密码     | `admin`          |
| `HTTP_PORT`        | 要绑定到的 HTTP 端口 | `8080`           |

然后，可以使用首选的 Docker *命令行工具*启动容器。下面的示例演示如何使用 [Docker Compose](https://docs.docker.com/compose/)。

## 开始部署

创建新的项目目录

```bash
mkdir owncloud-docker-server
cd owncloud-docker-server
```

从 GitHub 存储库复制 `docker-compose.yml`

如果网络环境差，可自行创建 `docker-compose.yml` 文件并复制链接中的内容

```bash
wget https://raw.githubusercontent.com/owncloud/docs-server/master/modules/admin_manual/examples/installation/docker/docker-compose.yml
```

创建环境配置文件

```bash
cat << EOF > .env
OWNCLOUD_VERSION=10.10
OWNCLOUD_DOMAIN=localhost:8080
ADMIN_USERNAME=admin
ADMIN_PASSWORD=admin
HTTP_PORT=8080
EOF
```

构建并启动容器

```docker
docker-compose up -d
```

该过程完成后，通过运行 `docker-compose ps` 来检查所有容器是否已成功启动。如果它们都正常工作，您应该看到类似于下面的输出：

注意，必须在 `owncloud-docker-server` 目录下运行此命令

| Name             | Command                        | State        | Ports                  |
| :--------------- | :----------------------------- | :----------- | :--------------------- |
| owncloud_mariadb | docker-entrypoint.sh --max ... | Up (healthy) | 3306/tcp               |
| owncloud_redis   | docker-entrypoint.sh --dat ... | Up (healthy) | 6379/tcp               |
| owncloud_server  | /usr/bin/entrypoint /usr/b ... | Up (healthy) | 0.0.0.0:8080->8080/tcp,:::8080->8080/tcp |

尽管容器已启动并运行，但可能需要几分钟时间才能使 ownCloud 完全正常运行。 

检查日志输出：`docker-compose logs --follow owncloud`，等到输出显示 **Starting apache daemon…** 然后再访问 Web UI。本人等待了约 1 分钟（2核 4G 6M 服务器）。

## 登录

在登录前，确保服务器的 8080 端口已被防火墙放行。

浏览器访问 `http://服务器域名:8080` 

![image-20220907184239919](https://nme-200t.oss-cn-hangzhou.aliyuncs.com/template/202209071842056.png)

用户名和密码是您之前存储的凭据，默认均为 `admin`。 请注意，即使您更改 .env 中的值，已经部署的实例中的凭据也不会改变。

### 设置中文

登录后右上角设置

![image-20220907185118178](https://nme-200t.oss-cn-hangzhou.aliyuncs.com/template/202209071851209.png)



语言中选择简体中文即可

![image-20220907185242779](https://nme-200t.oss-cn-hangzhou.aliyuncs.com/template/202209071852845.png)

## 升级

请见[官方文档](https://doc.owncloud.com/server/10.10/admin_manual/installation/docker/)

