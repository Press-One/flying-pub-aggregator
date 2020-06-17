聚合站就是把多个[飞帖站点](https://github.com/Press-One/flying-pub)的内容聚合到一个地方。

用 RSS 做类比，飞帖站点就是一个 RSS 源，聚合站就是一个 RSS 聚合器，你可以把自己感兴趣的 RSS 源汇总到一个地方，从而掌控了自己的信息来源。

![](https://img-cdn.xue.cn/616-Xnip2020-06-16_13-33-39.jpg)

## 开始

### 环境准备

你需要安装 Docker

- 如果你使用 Mac，下载 [Docker 客户端（Mac）](https://docs.docker.com/docker-for-mac/install/)
- 如果你使用 Windows，下载 [Docker 客户端（Windows）](https://docs.docker.com/docker-for-windows/install/)
- 如果你使用 Linux，根据这个[向导](https://docs.docker.com/compose/install/)来安装 Docker

克隆这个项目： `git clone https://github.com/Press-One/flying-pub-aggregator.git`

### 配置你想聚合的站点

进入 `flying-pub-aggregator` 仓库

打开 `config/Settings.toml`，添加你想要聚合的站点（配置中已经有 3 个站点作为例子）

如果你想要添加一个站点，那么就是加一个站点配置

如果你想要移除一个站点，那么就是减掉一个站点配置

### 启动

```
./scripts/start.sh
```

（备注：首次运行需要花些时间，估计耗时 15 分钟，你可以先去干点其他事情）

### 访问数据

文章列表：`http://localhost:7070/json_posts?topic=a06d94c3c6a50c5893f366148bc863dec4d4d676&limit=10&offset=0`
作者列表：`http://localhost:7070/users?topic=a06d94c3c6a50c5893f366148bc863dec4d4d676&limit=10&offset=0`

你可以根据 `config/Settings.toml` 中 3 个站点的 topic，使用 api 查看他们的作者和文章数据

你根据这两个 api，把作者和文章的数据，灌到你聚合站点的数据库里面，然后根据你想要的方式，把数据展示出来
