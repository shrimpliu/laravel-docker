# 万峰项目Docker

## 配置文件
```shell
cp .env.example .env
```
修改`APP_PATH`为站点存放目录。

## 查看当前用户uid和gid
修改`.env`中的WORKSPACE_PUID和WORKSPACE_PGID与当前用户相同

## 添加站点
`nginx/sites/`目录中添加站点配置文件

## 添加supervisor守护进程
`supervisor/conf.d/`目录中添加.conf配置文件

## 启动
```shell
docker-compose up -d nginx mysql redis elasticsearch supervisor
```

## 进入Shell
```shell
docker-compose exec --user=shrimp workspace bash
```

## crontab生效
首次运行时，crontab不会自动生效，需要编辑一次.
以root用户进入workspace
```shell
docker-compose exec workspace bash
```
打开一次crontab文件并保存退出
```shell
vim /etc/cron.d/shrimp
```
然后直接`:wq`保存退出，该crontab即可生效。