# TraceWhitelist

基于邀请制的白名单管理插件，支持白名单审计。（仅支持正版验证服务器）

## 简介

最初设计给 RIA-RED 服务器，目前由个人独立维护。这是一套支持追踪审计的白名单审核系统。邀请和审核操作都在数据库中被如实记录，无法被删除。（例如添加了一个玩家白名单又删除了它，那么会有一条添加和一条移除的记录）  
记录将会详细记录操作人，担保人，批次号和备注信息，可通过 /wlquery 命令查询。

## 指令

* /wladd <玩家ID> <担保人> <批次号> <白名单添加备注> - 管理员命令，以指定理由添加指定玩家到白名单中 (twhitelist.add)
* /wlremove <玩家ID> <删除原因> - 管理员命令，以指定理由将玩家从白名单中移除 (twhitelist.remove)
* /wlinvite/invite <玩家ID> <被邀请人简介> - 玩家命令，以指定理由邀请某个玩家 (twhitelist.invite)
* /wlquery <玩家ID> - 查询指定玩家的白名单审计记录 (twhitelist.query)

## 配置文件

```yaml
lang:
  general:
    internal-error: "<red>发生了一个内部错误：<white>{0}"
    reject-join-no-whitelist: ":( 抱歉，您似乎没有此服务器的白名单，找一个好朋友来邀请你吧！"
    player-not-exists: "<yellow>指定的玩家 {0} 不存在，请检查后再试！"
    please-wait: "<yellow>请稍等……"
  wladd:
    bad-arguments: "<red>参数错误：/wladd <玩家ID> <担保人> <批次号（担保或史前时代，请填 Trace-000）> <白名单添加备注>\n<gray>示例：/wladd Ghost_chu Shura_Dream Trace-110 2077年1月1日白名单批量发车"
    success: "<green><yellow>{0} (ID: {1})</yellow> 的白名单已成功添加"
    error-no-updates: "<red>添加失败，此玩家可能已经有白名单了，使用 /riawlquery <玩家ID> 查询白名单详情"
  wlremove:
    bad-arguments: "<red>参数错误：/wlremove <玩家ID> <删除原因>\n<gray>示例：/wlremove Ghost_chu 用于测试删除功能"
    error-no-updates: "<red>删除失败，共 0 行数据被更新，指定要删除的玩家 ID 是否本就没有白名单？"
    success: "<green><yellow>{0}</yellow> 的白名单已成功移除"
  wlinvite:
    bad-arguments: "<red>参数错误：/wlinvite <玩家ID> <被邀请人简介，不填或敷衍填写将导致白名单被删除>\n<gray>示例：/wlinvite Ghost_chu 这是我的好朋友，从2016年开始我们就一起玩MC，现在我们一起来到了这里"
    success: "<green><yellow>{0} (ID: {1})</yellow> 已被成功邀请到此服务器！\n<yellow>注意：您将对您邀请的玩家承担连带责任，请确保您已将服务器规则清楚的告知对方"
    error-no-updates: "<green><yellow>添加失败，此玩家可能已经有白名单了，使用 /wlwlquery <玩家ID> 查询白名单详情"
  wlquery:
    bad-arguments: "<red>参数错误：/wlquery <玩家ID>\n<gray>示例：/wlremove Ghost_chu"
    entry: "ID: {0} - {1} - {2}\n担保人：<gray>{3}</gray> 操作员：<gray>{4}</gray>\n批次号：<gray>{5}</gray>\n描述：<gray>{6}</gray>\n删除于：<gray>{7}</gray> 删除操作员：<gray>{8}</gray>\n删除原因：<gray>{9}</gray>"
    valid: "<green>生效中</green>"
    invalid: "<red>被移除</red>"
    no-guarantor: "<gray>无担保人</gray>"
    no-delete-at: "<gray>未删除</gray>"
    no-delete-operator: "<gray>未删除</gray>"
    no-delete-reason: "<gray>无原因</gray>"
    no-data: "<gray>没有在数据库中检索到任何有关数据</gray>"
database:
  # 使用 MySQL 存储数据，false 则直接崩溃
  mysql: true
  # 下面的配置应该就不用说了吧
  host: localhost
  port: 3306
  database: tracewhitelist
  user: root
  password: passwd
  # 表前缀
  prefix: "tracewhitelist_"
  # 使用 SSL
  usessl: false
  # 额外配置
  properties:
    connectionTimeout: 60000
    idleTimeout: 10000
    maxLifetime: 1800000
    maximumPoolSize: 4
    minimumIdle: 4
    cachePrepStmts: true
    prepStmtCacheSize: 250
    prepStmtCacheSqlLimit: 2048
    useUnicode: true
    characterEncoding: utf8
    allowPublicKeyRetrieval: true
    keepaliveTime: 60000
```
