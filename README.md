#centos为基础镜像
FROM centos
#把dockers host主机上的nginx压缩包复制到容器的根目录并解压
#安装nginx依赖
RUN yum -y install gcc pcre pcre-devel zlib zlib-devel openssl openssl-devel
ADD nginx-1.14.0.tar.gz /
#切换到nginx-1.14.0目录下
WORKDIR nginx-1.14.0
#创建nginx用户,没有宿主目录
RUN useradd -M -s /sbin/nologin nginx
RUN ./configure --prefix=/usr/local/nginx --user=nginx --group=nginx
RUN make && make install
#优化nginx
RUN ln -s /usr/local/nginx/sbin/* /usr/local/sbin/
#进入到nginx网页的根目录
WORKDIR /usr/local/nginx/html/
#修改index.html文件
RUN echo "test file in container" > index.html
#关闭nginx的守护进程
RUN echo "daemon off;" >> /usr/local/nginx/conf/nginx.conf
CMD ["nginx"]

