# phpdocker #
包含:
  * memcached:alpine
  * mailhog/mailhog:latest
  * redis:alpine
  * mysql:5.7
  * nginx:alpine
  * php-fpm:7.4

# 添加到您的项目 #

只需将文件解压缩到您的项目中， 根目录下有 `docker-compose.yml` 文件和一个 `phpdocker` 目录包含 nginx and php-fpm 两个目录.

为确保 `phpdocker/nginx/nginx.conf` 能正确运行. 需要你在根目录下创建 `public/index.php`， 代码根目录即 `public`.
 
# 怎么运行 #

依赖:

  * Docker引擎v1.13或更高版本。您的操作系统提供的软件包可能有点旧，如果遇到问题，请进行升级。 参考 [https://docs.docker.com/engine/installation](https://docs.docker.com/engine/installation)
  * Docker compose v1.12 或更高版本。 参考 [docs.docker.com/compose/install](https://docs.docker.com/compose/install/)

完成后，只需cd到项目根目录项目即可docker-compose up -d。这将初始化并启动所有容器，然后使它们在后台运行。

## 暴露的端口 ##

容器运行之后，可以通过 **`localhost`** 来访问你的应用。

Service|Address outside containers
------|---------|-----------
Webserver|[localhost:8090](http://localhost:8090)
Mailhog web interface|[localhost:8091](http://localhost:8091)
MySQL|**host:** `localhost`; **port:** `8092`

## 应用中暴露的端口 ##

端口配置如下：

服务|主机名|端口号
------|---------|-----------
php-fpm|php-fpm|9000
MySQL|mysql|3306 (default)
Memcached|memcached|11211 (default)
Redis|redis|6379 (default)
SMTP (Mailhog)|mailhog|1025 (default)

# Docker compose #

**注意:** 需要先将cd转到docker-compose.yml文件所在的位置

  * 后台启动容器: `docker-compose up -d`
  * 前台启动容器: `docker-compose up`. 将看到每个正在运行的容器的日志流.
  * 停止容器: `docker-compose stop`
  * 杀死容器: `docker-compose kill`
  * 查看容器日志: `docker-compose logs`
  * 在容器内执行命令: `docker-compose exec SERVICE_NAME COMMAND` 其中 `COMMAND` 是你想执行的linux命令. 例如:
        * 进入PHP容器, `docker-compose exec php-fpm bash`
        * 打开mysql命令行, `docker-compose exec mysql mysql -uroot -pCHOSEN_ROOT_PASSWORD`

执行命令：docker exec -it -u $(id -u):$(id -g) CONTAINER_NAME COMMAND，将进入任意一个docker容器。