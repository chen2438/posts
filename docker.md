## ubuntu 20.04

进入超级用户模式

以下操作默认 root 用户进行, 或自行添加`sudo`

```bash
sudo -i
```

更新源, 升级本地文件

```bash
apt update
apt -y upgrade #可选
```

安装 docker

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

安装 docker-compose

```bash
sudo apt install docker-compose
```

新建 `zstu-api` 目录, 编写 `docker-compose.yml`

```bash
mkdir zstu-api
cd zstu-api
vim docker-compose.yml
```

粘贴以下内容

```yaml
version: '3.3'
services: 
    mongo: 
      image: mongo:latest
      container_name: "mongo"
      network_mode: "host"
      ports: 
        - "27017:27017"
      volumes: 
        - ~/data:/data/db
      restart: always

    zstu-api: 
      depends_on: 
        - mongo
      tty: true
      image: chen2438/zstu-api:1.1
      container_name: "zstu-api"
      network_mode: "host"
      ports: 
        - "80:80"
      privileged: true
```

输入 `:wq` 退出

开始后台构建

```bash
docker-compose up -d
```

构建成功如图所示

![image-20221006133659332](http://nme-200t.oss-cn-hangzhou.aliyuncs.com/notes/2022-10-06-053659.png)

确认容器状态

[待更正: 疑似PORTS配置被忽略, 由于network_mode]

```bash
docker ps
```

![image-20221006142343916](http://nme-200t.oss-cn-hangzhou.aliyuncs.com/notes/2022-10-06-062344.png)

如果 STATUS 是 Up 表示启动成功

进入 zstu-api 容器

```bash
docker exec -it zstu-api bash
```

进入 Zstu-Api 目录

```bash
cd /Zstu-Api
```

运行服务

```bash
node out/app.js
```

![image-20221006143210603](http://nme-200t.oss-cn-hangzhou.aliyuncs.com/notes/2022-10-06-063211.png)

Ctrl + P 然后 Ctrl + Q 挂起容器, 服务将持续运行

运行服务时请确保服务器 80 端口已被放行

### 待解决

- 更换linux发行版, 减小镜像大小
- 构建时更新 git 仓库
- latest 镜像版本号
