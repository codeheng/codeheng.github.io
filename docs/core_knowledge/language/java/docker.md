---
comments: true
---

## 介绍

Docker是一个虚拟环境容器(Container)，可以将开发环境、代码、配置文件等一并打包到这个容器中，并发布和应用到任意平台中

Why docker ?  --> 为了解决环境配置的问题

!!! quote "[官网](https://www.docker.com/)"
    Docker helps developers build, share, run, and verify applications anywhere — without tedious environment configuration or management. 

    `$ docker info` 显示docker系统信息

安装详见[官网下载](https://www.docker.com/get-started/)

启动Docker : `sudo systemctl enable/start docker`

**镜像**：容器的模板（类比为软件安装包，而容器是安装出来的软件）

查看下载的所有镜像：`docker images`  --> 镜像仓库[DockerHub](https://hub.docker.com/)

- 从仓库下载镜像：`docker pull docker.io/library/nginx:latest`
    * docker.io仓库地址；library命名空间；latest即tag版本号
- 删除镜像: `docker rmi [imageID/Repository]` 

### docker run 

**创建并运行容器**：`docker run [Repository/ID]` (若没有指定名字，则会随机分配)

参数`-d` 即容器后台执行

!!! Note

    运行`docker ps` 查看正在运行的容器

    - 可用`$ docker ps -a` 查看所有容器(包括运行和停止的)

    ```shell
    CONTAINER ID   IMAGE     COMMAND                   CREATED         STATUS         PORTS     NAMES
    d48f4b5fdc03   nginx     "/docker-entrypoint.…"   9 seconds ago   Up 9 seconds   80/tcp    intelligent_ishizaka
    ```
也可以pull省略，直接run，若没有则会先拉取再run

参数`-p 80:80` 前面为宿主机端口，后面为容器内端口，使其进行映射

!!! example "示例"

    ```shell
    $ docker run -p 80:80 -d nginx
    1da53412d1a5bf6d868b76ce3d44fae16a29acec8dc8acf00592ac7b45d338cd

    $ docker stop 1da53412d1a5  (进行退出)
    ```

    此时就可以在本地即宿主机上访问`localhost:80`

- `-V 宿主机目录:容器内目录` 两者目录进行绑定，修改将互相影响 『绑定挂载』
    * 保证删除容器，数据并不会丢失，还在宿主机中 
- `-v 卷的名字:容器内目录` 先创建一个存储空间(命名卷)  『命名卷挂载』

!!! example "示例"

    ``` shell
    $ docker volume create nginx_html
    nginx_html
    $ docker run -p 80:80 -v nginx_html:/user/share/nginx/html nginx
    ```

    挂载卷在宿主机哪？可利用一下命令查看：

    ``` shell
    $ docker volume inspect nginx_html
    [
        {
            "CreatedAt": "2025-06-29T13:47:54Z",
            "Driver": "local",
            "Labels": null,
            "Mountpoint": "/var/lib/docker/volumes/nginx_html/_data",
            "Name": "nginx_html",
            "Options": null,
            "Scope": "local"
        }
    ]
    ```

    `$ docker volume list` 列出所有创建的挂载卷

    `$ docker volume prune -a` 删除没有任何容器使用的卷

`-e`：向Docker传递环境变量

`--name my_nginx` 给容器自定义名字（必须是唯一的），**名字和ID等价**

`-it` 让自己的控制台进入容器，进行交互  e.g. `$ docker run -it --rm alpine`

- `--rm` 退出exit容器后并同时将容器从宿主机上删除 （可临时调试某容器）`
- `$ docker exec -it ID /bin/sh`  进入正在容器内部，获得交互环境，可运行Linux命令

> Alpine Linux 是一个面向安全，轻量级的Linux 

`-- restart xxx` 配置容器在停止时的重启策略 ^^always/unless-stopped^^

!!! warning "注意"

    `$ docker run xxx` 每次都会创建一个新容器

    Q: 若要仅仅是启动/停止某容器？ -->  `$ docker start/stop ID/Name`
  
查看容器的信息：`$ docker inspect ID`


### Dockerfile

> Dockerfile 是一个文本文件，包含了构建Docker镜像的所有指令

`$ docker build -t docker_test .`  点代表在当前目录下构建


**Docker网络**：

```shell
$ docker network list
NETWORK ID     NAME      DRIVER    SCOPE
e43929feae99   bridge    bridge    local
efd12bd5e7f7   host      host      local
a9783dcc9bf1   none      null      local
```

上述三个为默认网络，无法删除

Docker Compose 定义和运行多容器，使用`yml`文件管理多个容器

- 轻量级，适合个人使用，若对于企业级服务器集群，需要大规模容器管理 --> Kubernetes