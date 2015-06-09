Docker 学习笔记.
---

docker 安装
---

用的 centos7 所以直接 `yum install docker`


docker Nodejs
---
```
> docker run -i -t node node -version
```

安装官方 node 包,700+m

1. -i 表示同步container的stdin
2. -t 表示同步container的输出
3. -d 表示deamon
4. --rm=true 表示执行后删除
5. --name name 表示 container 的名称
6. -v 将目录挂载到 container
7. --privileged=true 防止没有权限访问挂载的目录
8. -p 9998:80 指定端口映射
9. --link name:container 与其他 container 链接.

```
> docker images
```
查看 当前运行的 images

```
> docker ps -a -q
```

查看 当前运行的 container, -a 所有的 container 默认只显示运行中, -q 返回 id

```
> docker rmi #删除 images
> docker rm #删除 container
```

删除 images 或 container

```
> docker commit id name
```

将 container(id) 提交到 image(name)

docker 上传
---
```
> docker tag id docker.io/{username}/{images_name}
> docker push docker.io/{username}/{images_name}
```

有时候不加 docker.io 会有问题.