# My Environment Dock

使用 Docker 搭建自己的开发环境。

## 环境准备

> [Get Docker Engine - Community for Ubuntu | docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

```bash
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

> [Install Docker Compose | docker](https://docs.docker.com/compose/install/)

```bash
$ sudo curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
```

## 下载项目

```bash
git clone https://github.com/imzyf/my-dock.git
```

## 配置项目

本地化环境设置：

```bash
cp env-example .env

# OR

cp my-env-example .env
```

启动并构建：

```bash
docker-compose up -d workspace php-worker nginx redis mysql

docker-compose up --build -d workspace php-worker nginx redis mysql
```

返回上一级目录：

```
cd ..
mkdir public
echo "<?php phpinfo(); ?>" > public/index.php
```

可访问 `http://localhost` 参看效果。

## 快捷命令

进入 workspace bash

```
./sync.sh bash
```

## issues

### 多项目间的通信

> - [Guzzle/Curl connections between multiple projects](https://github.com/laradock/laradock/issues/435)

例如有两个项目：

1. api project `api.test`
2. web project `web.test` 需要访问 `api.test`

这时候会遇到：

```log
Failed to connect to api.test port 80: Connection refused
```

解决办法：

```bash
# 查看 nginx ip
$ docker-compose exec php-fpm ping nginx
```

在 `docker-compose.yml` 中 `php-fpm` 下的 `extra_hosts` 中补充：

```env
- "api.dev:👆 得到的 nginx ip"
```

## References

- [laradock/laradock](https://github.com/laradock/laradock)
- [serafinomb/docker-compose-laravel](https://github.com/serafinomb/docker-compose-laravel)
