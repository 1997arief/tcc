# UTS  
## Create Nginx on CentOS 7 Docker images using Dockerfile and upload to DockerHub  
1. Create `Dockerfile`  
```
FROM centos:centos7
RUN yum install -y epel-release && \ 
    yum install -y \
        nginx && \
    yum clean all
COPY index.html /usr/share/nginx/html
ENTRYPOINT ["/usr/sbin/nginx","-g","daemon off;"]
```  
and your `index.html` that will use to testing the app
```
<h1>Sample nginx on centos 7</h1>
```  
2. Build docker images
```
$ docker build -t nginx-centos:v1 .
```
3. Add tag before upload to repo on DockerHub
```
$ docker tag nginx-centos:v1 1997arief/tcc:v1
```
4. Push to DockerHub
```
$ docker push 1997arief/tcc:v1
The push refers to repository [docker.io/1997arief/tcc]
547fc6a2638d: Preparing 
4811c13c43db: Preparing 
877b494a9f30: Preparing 
denied: requested access to the resource is denied
```  
ups, it failed 'cause must login first
```
$ docker login docker.io
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: 1997arief
Password: 
WARNING! Your password will be stored unencrypted in /home/best/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```  
then try to push it again
```
$ docker push 1997arief/tcc:v1
The push refers to repository [docker.io/1997arief/tcc]
547fc6a2638d: Pushed 
4811c13c43db: Pushed 
877b494a9f30: Mounted from library/centos 
v1: digest: sha256:c6f13bd4efa583bcfbc25776a907d97911ecd1a1d5f51bfd62840c33aca9e4fe size: 948
```
And it was successfully uploaded to DockerHub