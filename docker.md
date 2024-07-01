# docker 

## 概念理解

容器是一个允许我们在资源隔离的过程中，运行应用程序和其依赖项的 、轻量的 、操作系统级别的虚拟化技术， 运行应用程序所需的所有必要组件都打包为单个镜像，这个镜像是可以重复使用的。当镜像运行时，它是运行在独立的环境中，并不会和其他应用共享主机操作系统的内存、CPU或磁盘。这保证了容器内的进程不会影响到容器外的任何进程。

仓库： 别人做好的现成的镜像，都放在仓库里
镜像： 自己要用哪个镜像，就先拉到本地来。镜像就相当于还没激活的容器。
容器： 容器就是跑起来的镜像，就是一个完整的工作环境

```cmd
docker run -dit --privileged -p21:21 -p80:80 -p8080:8080 -p30000-30010:30000-30010 --name how2jtmall how2j/tmall:latest /usr/sbin/init
"""
docker run # 表示运行一个镜像
-dit       # 是 -d -i -t 的缩写。 -d ，表示 detach，即在后台运行。 -i 表示提供交互接口，这样才 				可以通过 docker 和 跑起来的操作系统交互。 -t 表示提供一个 tty (伪终端)，与 -i 配合			  就可以通过 ssh 工具连接到 这个容器里面去了
--privileged# 启动容器的时候，把权限带进去。 这样才可以在容器里进行完整的操作
-p21:21    # 第一个21，表示在CentOS 上开放21端口。 第二个21 表示在容器里开放21端口。 这样当访问	           	  CentOS 的21端口的时候，就会间接地访问到容器里了
-p80:80    	# 和 21一个道理
-p8080:8080 # 和21 一个道理，在本例里，访问的地址是 http://192.168.84.128:8080/tmall/， 这个 				192.168.84.128 是CentOS 的ip地址，8080是 CentOS 的端口，但是通过-p8080:8080 				这么一映射，就访问到容器里的8080端口上的 tomcat了
-p30000-30010 		# 和21也是一个道理，这个是ftp用来传输数据的
--name how2jtmall 	# 给容器取了个名字，叫做 how2jtmall，方便后续管理
how2j/tmall:latest how2j/tmall# 就是镜像的名称,latest是版本号，即最新版本
/usr/sbin/init: 	# 表示启动后运行的程序，即通过这个命令做初始化

"""

docker pull how2j/tmall # 表示从镜像仓库拉取镜像

docker exec -it how2jtmall /bin/bash # 进入容器，进去之后，就可以像操作一个普通linux那样操作

systemctl start docker.service 	# 启动docker服务

systemctl status docker.service # 查看docker启动状态

systemctl enable docker.service #设置docker开机自动启动
```

## 镜像管理

镜像管理常见的有这么些：

1. `search` 查看仓库里有些什么镜像
2. `pull` 拉取镜像
3. `images` 查看本地有些什么镜像
4. `rmi` 删除本地镜像
5. 修改本地镜像名称
6. `push` , 把镜像提交到仓库

```cmd
docker search tomcat # 仓库里通过关键字 找到一个tomcat的镜像。当然我们也可以找其他常见的，如 	                       mysql,nginx 等等。
                     # 注： 镜像名称前面会默认加上 docker.io/
```

从官网（`https://hub.docker.com/_/tomcat?tab=tags`）下载镜像的时候有很多版本，我们可以去官网来去指定的版本,比如我要用 8.0 的话，那么就执行如下命令`docker pull tomcat:8.0`

```cmd
docker run -it --rm -p 8888:8080 tomcat:8.0 # 这个--rm表示如果容器已经存在了，自动删除容器
docker images # 查看所有的本地镜像
docker rmi docker.io/tomcat:8.0 # 用于删除镜像
docker rmi $(docker images -q) # 删除所有的镜像
docker bulid # 本地构建
docker save -o 文件名.tar 镜像名称 # 导出镜像
docker load -i 文件名.tar # 加载镜像
docker load -i -q 文件名.tar （不输出日志） # 加载镜像
```

## 容器管理

容器管理的相关命令

 1. 运行 run
  2. 进入 exec attach
  3. 生命周期管理， 暂停，恢复，停止，启动 `pause, unpause, stop, start`
  4.  ps 查看所有的容器
  5. 检查某个具体的容器
  6. rm 删除容器
  7. commit，对容器做了修改后，把改动后的容器，再次转换为镜像

```cmd
 docker exec -it how2jtmall /bin/bash  # 进入容器里面
```

commit 就是把一个活生生的容器，再转换为镜像。

```cmd
docker commit how2jtmall how2j/tmall:now
```

 `pause, unpause, stop, start`这几个的作用就是对容器的生命周期进行管理，可以对容器进行暂停，启动等操作;

需要注意的是， stop 之后再 start, 容器需要启动，tomcat也需要启动，里面的`mysql `也需要启动，都很花时间，所以要等待十几秒再访问，才能看到结果，否则会误以启动失败了

`ps` 命令一般两种用法

```cmd
 docker ps -a # 查询所有的容器 
 docker ps    # 查看正在运行的容器
 docker inspect how2jtmall # 检查这个容器里的各项信息
 docker rm how2jtmall # 删除容器，在运行中的 容器是不能删除的，要先 stop ,然后再删除。
 docker rm `docker ps -a -q` -f # 有时候为了调试，需要不厌其烦地删除容器，下面这句话就会自动删                                	除所有容器啦
 docker update --restart=always 容器名 # 容器设置开机自启
 docker logs 容器名 	# 查看容器日志。
 docker logs -f 容器名 # :持续查看容器日志。
 docker run --name redis1 -p 6379:6379 -d redis:6.2.8 redis-server --appendonly yes
 				# redis-server 启动redis服务
 				# –appendonly yes 开启持久化
```

## 提交镜像

在 [commit 提交容器](https://how2j.cn/k/docker/docker-container/2011.html#step9121) 中，我们把一个容器转换为了镜像，接下来我们要把这个镜像提交到仓库里

