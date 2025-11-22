
# Single Player Tarkov - 服务器项目(社区版)

每天午夜自动编译SPT代码，并将编译版本推送到发布页面。

你可以从[此处](https://github.com/AirryCo/spt-launcher-ci/releases)获取启动器

[![SPT-Server Build Nightly](https://github.com/xxoolm/spt-server-ci/actions/workflows/build-nightly-cron.yaml/badge.svg)](https://github.com/xxoolm/spt-server-ci/actions/workflows/build-nightly-cron.yaml)


## 使用方法

### Windows系统

1. 访问 https://github.com/AirryCo/spt-server-ci/releases  

2. 下载 `.zip` 文件

3. 使用解压缩软件解压文件

4. 然后运行 `SPT.Server.exe`

5. 运行 `SPT.Launcher` 进行连接

### Linux系统

仓库地址: ~~https://dev.sp-tarkov.com/medusa/spt-server~~   https://github.com/AirryCo/spt-server  

~~SPT 镜像仓库: https://dev.sp-tarkov.com/medusa/-/packages/container/spt-server/nightly~~  

Docker Hub: https://hub.docker.com/r/stblog/spt-server  

Github 容器注册表: https://github.com/AirryCo/spt-server-ci/pkgs/container/spt-server  

阿里云镜像仓库: registry.cn-shenzhen.aliyuncs.com/spt-server/spt-server

> [!注意]
> ***Mods 使用说明***: 在运行前，请将mod文件夹中所有"/snapshot/project"字符串替换为"/snapshot/workspace/medusa/spt-server/code/project"。(**仅限3.9版本**，3.10版本无需更改)
> 
> 你可以运行命令 `sed -i "s/\/snapshot\/workspace\/project/\/snapshot\/workspace\/medusa\/spt-server\/code\/project/g" $(grep -rl "/snapshot/workspace" .)` 进行全局替换。

### 3.10版本

1. 使用docker命令行

```bash
docker pull stblog/spt-server:nightly
docker run -d --name spt-server -v ./spt-server:/opt/spt-server -e backendIp=192.168.1.1 -e backendPort=6969 -p 6969:6969 stblog/spt-server:nightly
```

2. 或使用docker compose

```yaml
services:
  spt-server:
    image: stblog/spt-server:nightly
    container_name: spt-server
    hostname: spt-server
    restart: unless-stopped
    volumes:
      - ./spt-server:/opt/spt-server
    network_mode: host
    environment:
      - backendIp=192.168.1.1
      - backendPort=6969
```

`backendIp`(可选): 你的服务器IP，默认是容器IP如`172.17.0.2`。如果`network_mode`设置为`host`，则默认使用你的服务器IP

`backendPort`(可选): 你的服务器端口，默认是`6969`

### 3.9版本

```bash
docker run -d --name spt-server --restart always -p 6969:6969 -v ./spt-server:/opt/spt-server stblog/spt-server:3.9
```

docker compose配置
```yaml
services:
  spt-server:
    image: stblog/spt-server
    container_name: spt-server
    restart: always
    volumes:
      - './spt-server:/opt/spt-server'
    ports:
      - '6969:6969'
```

你需要在 `SPT_Data/Server/configs/http.json` 文件中将 `backendIp` 的值修改为你的服务器IP地址
