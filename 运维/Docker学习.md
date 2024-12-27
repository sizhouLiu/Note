# Docker学习

Docker 是一个应用打包、分发、部署的工具
你也可以把它理解为一个轻量的虚拟机，它只虚拟你软件需要的运行环境，多余的一点都不要，
而普通虚拟机则是一个完整而庞大的系统，包含各种不管你要不要的软件。

### 打包、分发、部署

**打包**：就是把你软件运行所需的依赖、第三方库、软件打包到一起，变成一个安装包
**分发**：你可以把你打包好的“安装包”上传到一个镜像仓库，其他人可以非常方便的获取和安装
**部署**：拿着“安装包”就可以一个命令运行起来你的应用，自动模拟出一摸一样的运行环境，不管是在 Windows/Mac/Linux。<a >

<img src="https://cos.easydoc.net/46901064/files/kv7rlicu.png"><a>

### 重要概念：镜像、容器

**镜像**：可以理解为软件安装包，可以方便的进行传播和安装。
**容器**：软件安装后的状态，每个软件运行环境都是独立的、隔离的，称之为容器。

## Docker 命令

### 镜像

```shell
docker images  #查看镜像


#从服务器拉取镜像拉取镜像
docker pull 镜像名       #拉取最新版本的镜像
docker pull 镜像名:tag   #拉取镜像，指定版本
#推送镜像到服务
docker push 镜像名
docker push 镜像名:tag


docker save -o 保存的目标文件名称 镜像名 #保存镜像为一个压缩包
docker load -i 文件名    #加载压缩包为镜像

#从Docker Hub查找/搜索镜像
docker search [options] TERM      
docker search -f STARS=9000 mysql  #搜索stars收藏数不小于10以上的mysql镜像

#删除镜像。当前镜像没有被任何容器使用 才可以删除
docker rmi 镜像名/镜像ID     #删除镜像 
docker rmi -f 镜像名/镜像ID  #强制删除
docker rmi -f 镜像名 镜像名 镜像名     #删除多个 其镜像ID或镜像用用空格隔开即可 
docker rmi -f $(docker images -aq)  #删除全部镜像，-a 意思为显示全部, -q 意思为只显示ID

docker image rm 镜像名称/镜像ID  #强制删除镜像


#给镜像打标签【有时候根据业务需求 需要对一个镜像进行分类或版本迭代操作，此时就需要给镜像打上标签】
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

### 容器操作

容器保护三个状态：

- 运行：进程正常运行
- 暂停：进程暂停，CPU不再运行，并不释放内存
- 停止：进程终止，回收进程占用的内存、CPU等资源

其中：

- docker run：创建并运行一个容器，处于运行状态
- docker pause name：让一个运行的容器暂停
- docker unpause name：让一个容器从暂停状态恢复运行
- docker stop name：停止一个运行的容器（杀死进程、回收内存，仅剩文件系统）
- docker start name：让一个停止的容器再次运行
- docker restart name：重启容器
- docker rm：删除一个容器（进程、内存回收、文件系统彻底干掉）

```shell
docker ps      #显示正在运行的容器
docker ps -a   #-a,--all  显示全部容器，包括已停止的（默认只显示运行中的容器）

#容器怎么来？ docker run 创建并运行一个容器，处于运行状态。
#--name 给要运行的容器起的名字；   -p 将宿主机端口与容器端口映射，冒号左侧是宿主机端口，右侧是容器端口；   -d 表示可后台运行容器 （守护式运行）。具体样例见下
docker run --name containerName -p 80:80 -d nginx   

docker pause 容器名/容器ID    #让一个运行的容器暂停
docker unpause name  #让一个容器从暂停状态恢复运行
docker stop name     #停止一个运行的容器（杀死进程、回收内存，仅剩文件系统）
docker start name    #让一个停止的容器再次运行
docker restart name  #重启容器
#docker stop与docker kill的区别：都可以终止运行中的docker容器。类似于linux中的kill和kill -9这两个命令，docker stop与kill相似，docker kill与kill -9类似
docker kill 容器名    #杀掉一个运行中的容器
docker rename 容器名 新容器名  #更换容器名

#删除容器
docker rm 容器名/容器ID            #删除容器  
docker rm -f CONTAINER           #强制删除
docker rm -f 容器名 容器名 容器名   #删除多个容器 空格隔开要删除的容器名或容器ID
docker rm -f $(docker ps -aq)    #删除全部容器


docker logs 容器名        #查看容器运行日志         
docker logs -f 容器名     #持续跟踪日志
docker logs -f --tail=20 容器名  #查看末尾多少行


#进入容器执行命令，两种方式 docker exec 和 docker attach，推荐docker exec
#方式一 docker exec。
docker exec -it 容器名/容器ID bash
#方式二 docker attach，推荐使用docker exec
docker attach 容器名/容器ID

#从容器退到自己服务器中（不能用ctrl+C）
exit      #直接退出。未添加-d(持久化运行容器)时，执行此参数 容器会被关闭
ctrl+p+q  #优雅退出。无论是否添加-d参数，执行此命令容器都不会被关闭
```

```shell
#设置容器开机自启动
#法一 创建容器、使用docker run命令时，添加参数--restart=always，表示该容器随docker服务启动而自动启动
docker run --name mysqlLatest -p 3307:3306 --restart=always -d mysql

#若容器已启动，希望设置开机自启动
docker update 容器名/容器ID --restart=always
```

#### 创建并运行一个容器

```shell
docker run --name containerName -p 80:80 -d nginx
```

- docker run ：创建并运行一个容器
- --name : 给容器起一个名字，比如叫做mn
- -p ：将宿主机端口与容器端口映射，冒号左侧是宿主机端口，右侧是容器端口。宿主机端口可以任意，只要没有被占用，容器内端口取决于应用本身
- -d：后台运行容器，一般都会加
- nginx：镜像名称，例如nginx，没写标签tag 默认最新版本

这里的`-p`参数，是将容器端口映射到宿主机端口。

默认情况下，容器是隔离环境，我们直接访问宿主机的80端口，肯定访问不到容器中的nginx。

现在，将容器的80与宿主机的80关联起来，当我们访问宿主机的80端口时，就会被映射到容器的80，这样就能访问到nginx了：