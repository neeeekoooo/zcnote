> 镜像定制需要将每一层写入到`Dockerfile`文件,每一个指令即一个层,`FROM scratch`表示空白镜像,如果不依赖其他系统环境,是可以直接用这个空白镜像的,这样体积会更小

## 一个简单例子
```bash
FROM nginx
RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
```
以nginx镜像为基础进行定制

## 指令
```bash
# FROM表示以哪个镜像为基础,是Dockerfile的开始
FROM nginx 

# RUN执行命令行命令,例如下面将hello,docker输入到index.html,多个命令用&&隔开
RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html

# COPY将上下文文件复制到目标目录,注意是上下文环境,--chown=<user>:<group>改变文件用户和用户组
COPY --chown=<user>:<group> ./package.json /usr/src/app/

# ADD只有一个应用场景,就是解压缩文件到目标目录
ADD ./xxx.tar.gz /opt/

# CMD指定默认的容器主进程的启动命令
CMD ["nginx", "-g", "daemon off;"]
# 入口点,和CMD类似,指定容器启动程序和参数,如果指定了ENTRYPOINT,那么CMD不会直接执行命令,而是作为参数,传递给ENTRYPOINT执行,CMD和ENTRYPOINT可以同时存在,但是CMD会作为参数传递给ENTRYPOINT执行

ENTRYPOINT [ "curl", "-s", "https://ip.cn" ]
# ENV设置环境变量,在Dockerfile中可以通过$ENV获取变量值,在容器运行时是存在这些的

ENV VERSION=1.0 DEBUG=on NAME="Happy Feet"
# ARG构建参数,和ENV类似,但是在容器运行时是不保留的

ARG VERSION=1.0

# VOLUME定义匿名卷,写入的数据不会写到容器,(但是我不知道保存在哪里)
VOLUME /data

# EXPOSE声明端口,只是声明而已,使用`-p <宿主端口>:<容器端口>`来映射端口
EXPOSE 80 443

# WORKDIR指定工作目录,因为每个指令即一个层,如果在一个层做了改变,在另一个层是感知不到的,所以指定WORKDIR可以改变各个层的工作目录(改变环境状态并影响以后的层)
WORKDIR /var/www

# USER指定当前用户,只是指定而已,需要自己创建这个用户,和WORKDIR一样,会影响以后的层
USER redis
```

## CMD指定默认容器主进程启动命令
例如`ubuntu` 镜像默认的 `CMD` 是 `/bin/bash`，如果我们直接 `docker run -it ubuntu` 的话，会直接进入 `bash`。我们也可以在运行时指定运行别的命令，如 `docker run -it ubuntu cat /etc/os-release`。这就是用 `cat /etc/os-release` 命令替换了默认的 `/bin/bash` 命令了，输出了系统版本信息。

`CMD ["可执行文件", "参数1", "参数2"...]`
官方推荐使用上面的格式,这是因为,举个例子,使用`CMD service nginx start`会被解析为`CMD ["sh", "-c", "service nginx start]`,此时主进程是sh,一当执行完`service nginx start`后就会自动退出了,而我们是想让它后台执行的,应该使用`CMD ["nginx", "-g", "daemon off;"]`

## 命令
### 构建镜像
```bash
# -t指定镜像的名称, .表示上下文(当前目录)
docker build -t nginx:v3 .
```

## 加速`apt-get`命令
https://learnku.com/articles/26066	
https://opsx.alibaba.com/mirror
```bash
RUN sed -i "s@http://deb.debian.org@http://mirrors.aliyun.com@g" /etc/apt/sources.list && rm -Rf /var/lib/apt/lists/* && apt-get update
```



