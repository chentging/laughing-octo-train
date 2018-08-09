# 内部 Sentry

官方引导运行您自己的 [Sentry](https://sentry.io/) 使用 [Docker](https://www.docker.com/).

## 需求

 * Docker 1.10.0+
 * Compose 1.6.0+ _(可选)_

## 启动和运行

假设您刚刚克隆了这个存储库，下面的步骤将立即启动并运行!

可能需要对包含的`docker-compose.yml`文件进行修改，以适应您的需要或环境。这些指导方针是你应该做的事情。

1. `mkdir -p data/{sentry,postgres}` - 使我们的本地数据库和哨兵配置目录。这个目录是用postgres绑定安装的，所以不会丢失状态!
2. `docker-compose build` - 构建和标记Docker服务
3. `docker-compose run --rm web config generate-secret-key` - 生成一个密钥.
    将其添加到 `docker-compose.yml` in `base` as `SENTRY_SECRET_KEY`.
4. `docker-compose run --rm web upgrade` - 建立数据库.
    使用交互式提示创建用户帐户.
5. `docker-compose up -d` - 解除所有服务(分离/后台模式)。
6. 访问您的实例 `localhost:9000`!

> 注意，只要安装了数据库绑定，就可以很好地停止和删除容器，而不用担心。

## 确保Sentry 使用 SSL/TLS

如果您想用SSL/TLS保护您的Sentry安装，那么有一些很棒的SSL/TLS代理，比如[HAProxy](http://www.haproxy.org/)和[Nginx](http://nginx.org/)。

## 升级Sentry

使用撰写来更新岗哨相对简单。只需使用以下步骤进行更新。确保在Dockerfile中设置了最新的版本。或者使用这个存储库的最新版本。

更新此存储库或Dockerfile后，使用以下步骤:
```sh
docker-compose build # 更新后重新构建服务
docker-compose run --rm web upgrade # 运行新的迁移
docker-compose up -d # 重新创建服务
```

## 资源

 * [文档](https://docs.sentry.io/server/installation/docker/)
 * [Bug追踪](https://github.com/getsentry/onpremise)
 * [论坛](https://forum.sentry.io/c/on-premise)
 * [IRC](irc://chat.freenode.net/sentry) (chat.freenode.net, #sentry)