## ubuntu 20.04

进入超级用户模式

以下操作默认 root 用户进行, 或自行添加`sudo`

```bash
sudo -i
cd /root
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
apt install docker-compose
```

### 运行 api

新建 `zstu-api` 目录并进入

```bash
mkdir zstu-api
cd zstu-api
```

#### 自动部署

**先决条件：** 服务器可以正常访问 github.com

编写 `docker-compose.yml`

```bash
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
      # ports: 
      #   - "27017:27017"
      volumes: 
        - /root/zstu-api/data:/data/db
      restart: always

    zstu-api: 
      depends_on: 
        - mongo
      tty: true
      image: chen2438/zstu-api:latest
      container_name: "zstu-api"
      network_mode: "host"
      # ports: 
      #   - "80:80"
      command:
        - sh
        - -c 
        - |
          rm -rf /Zstu-Api
          git clone https://github.com/Neutron-Bomb/Zstu-Api
          cd Zstu-Api && npm install && tsc
          node out/app.js
```

输入 `:wq` 退出

开始后台启动并运行

```bash
docker-compose up -d
```

启动成功如图所示

![image-20221006133659332](http://nme-200t.oss-cn-hangzhou.aliyuncs.com/notes/2022-10-06-053659.png)

确认容器状态

```bash
docker ps
```

![image-20221006174610261](https://picgo-1303840613.cos.ap-shanghai.myqcloud.com/image-20221006174610261.png)

如果 STATUS 是 Up 表示启动成功

启动成功后，在可以访问 github 的情况下，等待约 45 秒即完成部署

运行服务时请确保服务器 80 端口已被放行

#### 手动部署

**注意：**Zstu-Api 版本可能过时，请参考自动部署的 `command` 命令自行升级

编写 `docker-compose.yml`

```bash
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
      # ports: 
      #   - "27017:27017"
      volumes: 
        - /root/zstu-api/data:/data/db
      restart: always

    zstu-api: 
      depends_on: 
        - mongo
      tty: true
      image: chen2438/zstu-api:latest
      container_name: "zstu-api"
      network_mode: "host"
      # ports: 
      #   - "80:80"
```

输入 `:wq` 退出

开始后台启动并运行

```bash
docker-compose up -d
```

启动成功如图所示

![image-20221006133659332](http://nme-200t.oss-cn-hangzhou.aliyuncs.com/notes/2022-10-06-053659.png)

确认容器状态

```bash
docker ps
```

![image-20221006174610261](https://picgo-1303840613.cos.ap-shanghai.myqcloud.com/image-20221006174610261.png)

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

- [not ok]更换linux发行版, 减小镜像大小
- [ok]构建时更新 git 仓库
- [ok]latest 镜像版本号
- [not ok] [安全性]取消 zstu-api 特权模式
- [not ok] [安全性]使用自定义网桥, 取消共享宿主机网络
