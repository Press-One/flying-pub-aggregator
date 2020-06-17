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

站点字段解释：

- **name**: 站点名称
- **topic**: 站点的 topic
- **encryption_key**: 飞帖站点加密用的 encryption_key（这个 key 需要飞帖站点的开发者提供给你），站点内容都是加密的，聚合之前需要解密，然后再把原文存到聚合数据库
- **iv_prefix**: 飞帖站点加密用的 iv_prefix（这个 key 需要飞帖站点的开发者提供给你），站点内容都是加密的，聚合之前需要解密，然后再把原文存到聚合数据库

如果你想要添加一个站点，那么就是加一个站点配置

如果你想要移除一个站点，那么就是减掉一个站点配置

### 启动

```
./scripts/start.sh
```

（备注：首次运行需要花些时间，估计耗时 15 分钟，你可以先去干点其他事情）

### 访问数据

作者列表
http://localhost:7070/users?topic=a06d94c3c6a50c5893f366148bc863dec4d4d676&offset=0&limit=10

文章列表
http://localhost:7070/json_posts?topic=a06d94c3c6a50c5893f366148bc863dec4d4d676&offset=0&limit=10

你可以根据 `config/Settings.toml` 中 3 个站点的 topic，使用 api 查看他们的作者和文章数据

你根据这两个 api，把作者和文章的数据，灌到你聚合站点的数据库里面，然后根据你想要的方式，把数据展示出来

### 作者 API 字段解释

- **user_address**: 用户地址
- **status**: 用户状态，如果是 `allow`，说明这个用户可以显示，Ta 的文章也可以显示，如果是 `deny`，说明这个用户被禁止显示，Ta 的文章也不应该显示出来
- **tx_id**: 用户状态所属的区块 id
- **updated_at**: 更新时间
- **topic**: 用户所属的 topic

### 文章 API 字段解释

- **publish_tx_id**: 这篇文章的区块 id
- **file_hash**: 文章 hash
- **topic**: 文章所属的 topic
- **updated_tx_id**: 区块链不支持删除，只允许覆盖更新，如果这个字段不为空，说明这篇文章覆盖了另外一篇旧文章，这个 id 就是指向那篇旧文章的区块 id
- **updated_at**: 更新时间
- **deleted**: 是否被删除了，如果一篇文章被更新了，那么它就是被另外一篇文章覆盖了，这里会标记为已删除，就不要显示出来了
- **content**: 文章内容
