# My Environment Dock

使用 Docker 搭建自己的开发环境。

## Ubuntu 安装 docker docker-compose

> [Get Docker CE for Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

```
$ sudo apt-get update

$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

$ sudo apt-key fingerprint 0EBFCD88

$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

$ sudo apt-get update

$ sudo apt-get install docker-ce
```

> [Install Docker Compose](https://docs.docker.com/compose/install/)

```
$ sudo curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
```

## 配置项目

本地化环境设置：

```
cp env-example .env
```

启动并构建：

```
docker-compose up --build -d workspace php-worker nginx redis mysql

docker-compose up -d workspace php-worker nginx redis mysql
```

在上一级目录

```
mkdir public
echo "<?php phpinfo(); ?>" > public/index.php
```

可访问 `http://localhost` 参看效果。

## 相对于原项目的修改

`php-fpm/php56.ini` 修改

```
-memory_limit = 128M
+memory_limit = 1024M

-post_max_size = 8M
+post_max_size = 128M

-upload_max_filesize = 2M
+upload_max_filesize = 56M
```

`php-worker/Dockerfile`

在使用 php-fpm 5.6 时有 bug 注释了：

```
-RUN docker-php-ext-install mysqli mbstring pdo pdo_mysql tokenizer xml
-RUN pecl channel-update pecl.php.net && pecl install memcached mcrypt-1.0.1 && docker-php-ext-enable memcached
+# RUN docker-php-ext-install mysqli mbstring pdo pdo_mysql tokenizer xml
+# RUN pecl channel-update pecl.php.net && pecl install memcached mcrypt-1.0.1 && docker-php-ext-enable memcached
```

## 多项目间的通信 issues

> - [Guzzle/Curl connections between multiple projects](https://github.com/laradock/laradock/issues/435)

例如有两个项目：

1. api project `api.test`
2. web project `web.test` 需要访问 `api.test`

这时候会遇到：

```
Failed to connect to api.test port 80: Connection refused
```

解决办法：

```
# 查看 nginx ip
$ docker-compose exec nginx ifconfig
```

在 `docker-compose.yml` 中 `php-fpm` 下的 `extra_hosts` 中补充：

```
- "api.dev:👆 得到的 nginx ip"
```

> Reference:
> - [laradock/laradock](https://github.com/laradock/laradock)
> - [serafinomb/docker-compose-laravel](https://github.com/serafinomb/docker-compose-laravel)
