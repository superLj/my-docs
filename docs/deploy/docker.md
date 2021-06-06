```
# docker ps
# docker images
# docker rmi imageID
# docker stop containerID
# docker start containerID
# docker restart  containerID
# docker rm containerID
```

> docker 配置 Nginx

```
# docker container run -d -p 4030:80 --rm --name mynginx nginx
# docker container stop mynginx

docker run \--name myNginx \-d -p 80:80 \-v /usr/docker/myNginx/html:/usr/share/nginx/html \-v /etc/docker/myNginx/nginx.conf:/etc/nginx/nginx.conf:ro \-v /etc/docker/myNginx/conf.d:/etc/nginx/conf.d \nginx
```

- -d：在后台运行
- -p ：容器的 80 端口映射到 127.0.0.1:4030
- --rm：容器停止运行后，自动删除容器文件
- --name：容器的名字为 mynginx
- 末尾的 nginx：表示根据 nginx 镜像运行容器

- 第一个“-v”, 是项目位置，把项目放到挂载到的目录下即可
- 第二个“-v”, 是挂载的主配置文件"nginx.conf"
- 第三个“-v”, 把 docker 内子配置文件的路径也挂载了出来

```
# docker exec -it nginx /bin/bash
```

- -it：表示分配一个伪终端。
- nginx：表示容器的名称，这里也可以使用容器 ID
- /bin/bash：表示对容器执行 bash 操作

```
# cat /etc/nginx/nginx.conf
# vim /etc/nginx/conf.d/default.conf

# exit
```
