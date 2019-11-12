# Minggu 08  
## Python x Flask  
1. Just pull this repo
2. If using Linux, just run :  
```
$ cd FlaskApp
$ ./start.sh
```  
or you can build image manually using this Dockerfile  
```
$ cd FlaskApp
$ sudo docker build -t flask-docker .
$ sudo docker run -d -p 56733:80 \
  --name=flask-docker \
  -v $PWD:/app flask-docker
```