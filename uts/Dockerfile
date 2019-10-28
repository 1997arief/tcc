FROM centos:centos7
RUN yum install -y epel-release && \ 
    yum install -y \
        nginx && \
    yum clean all
COPY index.html /usr/share/nginx/html
ENTRYPOINT ["/usr/sbin/nginx","-g","daemon off;"]