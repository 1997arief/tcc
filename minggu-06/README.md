# Minggu 06  
## Deploying First Container  
1. Search docker image contains `redis`  
```
$ docker search redis
NAME                             DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
redis                            Redis is an open source key-value store that…   7412                [OK]
bitnami/redis                    Bitnami Redis Docker Image                      129                                     [OK]
sameersbn/redis                                                                  77                                      [OK]
grokzen/redis-cluster            Redis cluster 3.0, 3.2, 4.0 & 5.0               61
rediscommander/redis-commander   Alpine image for redis-commander - Redis man…   31                                      [OK]
kubeguide/redis-master           redis-master with "Hello World!"                30
redislabs/redis                  Clustered in-memory database engine compatib…   23
oliver006/redis_exporter          Prometheus Exporter for Redis Metrics. Supp…   18
arm32v7/redis                    Redis is an open source key-value store that…   17
redislabs/redisearch             Redis With the RedisSearch module pre-loaded…   17
webhippie/redis                  Docker images for Redis                         10                                      [OK]
s7anley/redis-sentinel-docker    Redis Sentinel                                  9                                       [OK]
bitnami/redis-sentinel           Bitnami Docker Image for Redis Sentinel         8                                       [OK]
insready/redis-stat              Docker image for the real-time Redis monitor…   8                                       [OK]
redislabs/redisgraph             A graph database module for Redis               8                                       [OK]
arm64v8/redis                    Redis is an open source key-value store that…   6
centos/redis-32-centos7          Redis in-memory data structure store, used a…   4
redislabs/redismod               An automated build of redismod - latest Redi…   4                                       [OK]
circleci/redis                   CircleCI images for Redis                       2                                       [OK]
frodenas/redis                   A Docker Image for Redis                        2                                       [OK]
tiredofit/redis                  Redis Server w/ Zabbix monitoring and S6 Ove…   1                                       [OK]
runnable/redis-stunnel           stunnel to redis provided by linking contain…   1                                       [OK]
wodby/redis                      Redis container image with orchestration        1                                       [OK]
xetamus/redis-resource           forked redis-resource                           0                                       [OK]
cflondonservices/redis           Docker image for running redis                  0
```  
Create Redis Container 
```
$ docker run -d redis
a50fb3db839222625a58be9a66c136019c29864afe2f43733f05d4271e297e52
```  
2. Check Docker Running Process  
```
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTSNAMES
a50fb3db8392        redis               "docker-entrypoint.s…"   About a minute ago   Up About a minute   6379/tcpgoofy_goldberg
```  
3. Running Redis on background process on port `6379` using name `redisHostPort`
```
$ docker run -d --name redisHostPort -p 6379:6379 redis:latest
ae1764426acdeb09183d503d01588c5b84d299917e887728142301af1a5e9fba
$ 
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS    NAMES
ae1764426acd        redis:latest        "docker-entrypoint.s…"   2 minutes ago       Up 2 minutes        0.0.0.0:6379->6379/tcp   redisHostPort
a50fb3db8392        redis               "docker-entrypoint.s…"   6 minutes ago       Up 6 minutes        6379/tcp    goofy_goldberg
```  
4. Add more Redis image using randomly available port using name `redisDynamic`  
```
$ docker run -d --name redisDynamic -p 6379 redis:latest
a846d3d3c741dc987d7ba6a529b3a6bbc8075768751f967d71e47b626f63efca
$ docker port redisDynamic 6379
0.0.0.0:32768
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS     NAMES
a846d3d3c741        redis:latest        "docker-entrypoint.s…"   12 seconds ago      Up 11 seconds       0.0.0.0:32768->6379/tcp   redisDynamic
ae1764426acd        redis:latest        "docker-entrypoint.s…"   7 minutes ago       Up 7 minutes        0.0.0.0:6379->6379/tcp    redisHostPort
a50fb3db8392        redis               "docker-entrypoint.s…"   11 minutes ago      Up 11 minutes       6379/tcp     goofy_goldberg
```  
5. Persisting Data  
```
$ docker run -d --name redisMapped -v /opt/docker/data/redis:/data redis
5a51c682ba3282d6dcb08d8566a3ebdeb7a8ade027ece4e7967f752bbe1a8c12
```  
6. Running Container on Foreground  
Launch Ubuntu Container  
```
$ docker run ubuntu ps
  PID TTY          TIME CMD
    1 ?        00:00:00 ps
$ docker run -it ubuntu bash
root@c78b2e48aa64:/# pwd
/
root@c78b2e48aa64:/# ls -l
total 64
drwxr-xr-x  2 root root 4096 Feb 28  2018 bin
drwxr-xr-x  2 root root 4096 Apr 12  2016 boot
drwxr-xr-x  5 root root  360 Oct 15 13:42 dev
drwxr-xr-x  1 root root 4096 Oct 15 13:42 etc
drwxr-xr-x  2 root root 4096 Apr 12  2016 home
drwxr-xr-x  8 root root 4096 Sep 13  2015 lib
drwxr-xr-x  2 root root 4096 Feb 28  2018 lib64
drwxr-xr-x  2 root root 4096 Feb 28  2018 media
drwxr-xr-x  2 root root 4096 Feb 28  2018 mnt
drwxr-xr-x  2 root root 4096 Feb 28  2018 opt
dr-xr-xr-x 81 root root    0 Oct 15 13:42 proc
drwx------  2 root root 4096 Feb 28  2018 root
drwxr-xr-x  5 root root 4096 Feb 28  2018 run
drwxr-xr-x  2 root root 4096 Mar  6  2018 sbin
drwxr-xr-x  2 root root 4096 Feb 28  2018 srv
dr-xr-xr-x 13 root root    0 Oct 15 13:42 sys
drwxrwxrwt  2 root root 4096 Feb 28  2018 tmp
drwxr-xr-x 10 root root 4096 Feb 28  2018 usr
drwxr-xr-x 11 root root 4096 Feb 28  2018 var
```  
---
## Deploy Static HTML Website as Container  
1. Create dockerfile  
```
FROM nginx:alpine
COPY . /usr/share/nginx/html
```  
2. Build Docker Image  
```
$ docker build -t webserver-image:v1 .
Sending build context to Docker daemon  3.072kB
Step 1/2 : FROM nginx:alpine
 ---> 4d3c246dfef2
Step 2/2 : COPY . /usr/share/nginx/html
 ---> 078e46eaa0d9
Successfully built 078e46eaa0d9
Successfully tagged webserver-image:v1
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
webserver-image     v1                  078e46eaa0d9        6 seconds ago       21.2MB
nginx               alpine              4d3c246dfef2        2 weeks ago         21.2MB
redis               latest              4760dc956b2d        19 months ago       107MB
ubuntu              latest              f975c5035748        19 months ago       112MB
alpine              latest              3fd9065eaf02        21 months ago       4.14MB
```  
3. Run web server image  
```
$ docker run -d -p 80:80 webserver-image:v1
1bda956def40337283925872705fbe5697088fdd9ae7bf6672c3c8d691a6c49d
$ curl docker
<h1>Hello World</h1>
```  
---  
## Building Container Images  
1. Create docker file  
```
FROM nginx:1.11-alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```  
2. Built and run docker image  
```
$ docker build -t arief-nginx:latest .
Sending build context to Docker daemon  3.072kB
Step 1/4 : FROM nginx:1.11-alpine
 ---> bedece1f06cc
Step 2/4 : COPY index.html /usr/share/nginx/html/index.html
 ---> 33ad0d85af0c
Step 3/4 : EXPOSE 80
 ---> Running in 7b0d485f8f51
Removing intermediate container 7b0d485f8f51
 ---> 46eeb3b058e3
Step 4/4 : CMD ["nginx", "-g", "daemon off;"]
 ---> Running in e09d5c948237
Removing intermediate container e09d5c948237
 ---> d59413cac79d
Successfully built d59413cac79d
Successfully tagged arief-nginx:latest
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
arief-nginx         latest              d59413cac79d        6 seconds ago        54.3MB
ubuntu              latest              16508e5c265d        13 months ago        84.1MB
redis               latest              4e8db158f18d        14 months ago        83.4MB
weaveworks/scope    1.9.1               4b07159e407b        14 months ago        68MB
alpine              latest              11cd0b38bc3c        15 months ago        4.41MB
nginx               1.11-alpine         bedece1f06cc        2 years ago          54.3MB
$ docker run -d -p 80:80 arief-nginx:latest
5b8480e4e46782cb5295904506dbc44f459e54d642a22a588173a03d90f64ccc
```  
