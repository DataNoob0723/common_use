```shell
# 基本操作

docker version

docker container --help

docker container ls/ps -a  # also show historical containers
docker container ls -aq  # only list container ids

docker image ls

docker image rm + image_id/repository
docker image rm $(docker image ls -q)  

docker image prune -a  # 删除全部目前没有在使用的镜像

docker container run image_name
docker container run -p host_port:container_port image_name  # attach mode
docker container run -d -p host_port:container_port image_name  # detach mode
docker attach container_id  # attach
docker container run -it unbuntu sh
docker container run --rm -it ipinfo ipinfo 8.8.8.8  # --rm 结束之后直接删除容器

docker container stop id/name
docker container stop id1 id2 id3 id4 ...
docker container stop $(docker container ls -aq)

docker container start container_id

docker container rm id/name
docker container rm id1 id2 id3 id4 ...
docker container rm $(docker container ls -aq)
docker container rm id -f

docker system prune -f  # 清理全部已经被停掉的容器

docker container logs (-f) id/name  # -f 动态跟踪

docker container top container_id  # 查看正在运行的容器内的进程

docker container inspect container_id  # 可以看到health check的results

docker exec -it container_id sh

# 镜像的创建管理和发布
docker image pull image_name:version  # default version: latest

docker image ls

docker image inspect image_id

docker image history image_id/image_name

docker image build -f Dockerfile -t image_name:tag Dockerfile_path  # tag default: latest

docker image tag hello:latest xiaopeng163/hello:1.0

docker image push xiaopeng163/hello:1.0

docker container commit container_id new_tag  # 通过修改过内容的 container创建镜像

# Dockerfile
RUN  # 执行命令 每次RUN会多成生一层

COPY/ADD  # 复制文件，如果目标目录不存在，则会自动创建
# 如果复制的是压缩文件，ADD 会帮我们自动解压缩

WORKDIR  # 切换目录，如果路径不存在，则会自动创建
# 后续操作比如 COPY hello.py hello.py 会默认在WORKDIR中进行

ARG/ENV 
ENV VERSION=2.0.1   ipinfo_${VERSION}  # 会同时设置环境变量
ARG VERSION=2.0.1   ipinfo_${VERSION}
docker image build --build-arg VERSION=2.0.0 .

CMD  # 定义容器启动时默认执行的命令，如果定义了多个，则只有最后一个会被执行

ENTRYPOINT  # 其定义的命令是一定会被执行的
# 经常和CMD联合使用，CMD传递参数
ENTRYPOINT ["echo"]
CMD []

# 容器的存储
docker volume ls

docker volume inspect volume_id

docker volume prune

docker container run -v cron-data:/app my-cron  # cron-data is volume name

docker container run -d -v $(pwd):/app my-cron

# 容器的网络

# docker-compose
docker-compose build  # 创建镜像
docker-compose up -d --build (--remove-orphans) (--scale service_name=n) (--env-file env_file_path)
docker-compose up -f filename -d --build
docker-compose ps
docker-compose stop  # 停止通过compose创建的容器
docker-compose rm  # 删除已经停止的，通过compose创建的容器
docker-compose pull  # 拉取镜像
docker-compose -p myproject up -d  # -p 指定项目的名字，一旦加了，后边所有命令都要加
docker-compose restart
docker-compose config  # 验证docker-compose文件，比如环境变量

```

```yaml
version: "3.8"

services:
	flask:
		build:
			context: ./flask
			dockerfile: Dockerfile
    image: flask-demo:latest
    environment:
    	- REDIS_HOST=redis-server
    	- REDIS_PASS=${REDIS_PASSWORD}
    networks:
    	- backend
    	- frontend
  
  redis-server:
  	image: redis:latest
  	command: redis-server --requirepass ${REDIS_PASSWORD}
  	networks:
  		- backend
  
  nginx:
  	image: nginx:stable-alpine
  	ports:
  		- 8000:80
    depends_on:
    	- flask
    volumes:
    	- ./nginx/nginx.conf:/etc/nginx/conf.d/default.conf:ro
    	- ./var/log/nginx:/var/log/nginx
    networks:
    	- frontend
  
networks:
	backend:
	frontend:
```

```.env
# .env
# need to be added to .gitignore
REDIS_PASSWORD=abc123
```



