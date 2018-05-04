# My Environment Dock

使用 Docker 搭建自己的开发环境。

启动并构建：

```
docker-compose up --build -d workspace php-worker nginx redis

docker-compose up -d workspace php-worker nginx redis
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

> Reference:
> - [laradock/laradock](https://github.com/laradock/laradock)
> - [serafinomb/docker-compose-laravel](https://github.com/serafinomb/docker-compose-laravel)
