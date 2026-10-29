# ShopXO 服务端个人副本：工程基线

- 核对日期：2026-10-29。
- 基线分支：`master`；完整提交：`aad87b5874023f0b7d207e86227214e45ede7cf9`。
- 文档工作分支：`codex/openspec-docs-20261029`。
- 本文为源码现状记录，不代表线上版本、生产可用性或独立开发贡献。

## 用途与来源

本仓是 `gongfuxiang/shopxo` 的个人副本，保留 ShopXO / Devil 作者标识与 `LICENSE.txt`。本次只维护文档，不将上游电商功能或插件列表写成个人原创、已购买插件或已上线业绩。对应移动端是个人 `shopxo-uniapp` 仓库；两仓通过现有 API 契约连接。

## 源码与能力映射

| 能力 | 源码依据 | 移动端消费者 |
| --- | --- | --- |
| 商品详情 | `app/api/controller/Goods.php::Detail`、`app/service/GoodsService.php` | `pages/goods-detail/goods-detail.vue` |
| 用户购物车 | `app/api/controller/Cart.php`、`app/service/GoodsCartService.php` | `components/cart/cart.vue` |
| 购买初始化和提交入口 | `app/api/controller/Buy.php`、`app/service/BuyService.php` | `components/cart/cart.vue`、`pages/buy/buy.vue` |
| 登录守卫 | `app/api/controller/Common.php::IsLogin` | `App.vue` 的 token 参数及登录检查 |
| 安装与依赖 | `install.php`、`app/install/controller/Index.php`、`composer.json` |

跨仓核对：移动端 `master@e0a492284090cf190cb6911957c3a23fa2cf472d`。客户端路径属于对应仓，不表示本仓包含移动端文件。当前规格只覆盖商品详情和购物车入口，不代表审计全部支付或插件行为。

## 最短本地启动路径

`composer.json` 要求 PHP >=8.0.0；具体扩展、可写目录和数据库能力按安装器检查。需要独立测试 MySQL、PHP Web 环境和隔离文件存储。

1. 将本仓放入本地 PHP Web 站点，入口可使用根目录的 `install.php`（转入 `public/install.php`）。
2. 审阅 `composer.json` 和锁文件；如果需要恢复依赖，另行授权后执行 `composer install`，它带有 post-autoload-dump 脚本，不是只读操作。
3. 在本地访问 `/install.php`，使用 `<LOCAL_DB_HOST>`、`<LOCAL_DB_NAME>`、`<LOCAL_DB_USER>`、`<LOCAL_DB_PASSWORD>` 等自行填写安装参数。安装会写配置/数据库，执行前须明确授权。
4. 按安装器完成环境检查后，再使用独立演示账号和商品验证前后台；不要连接真实支付通道。

本次不读取 `config/` 的私有配置、`runtime/` 数据或 `rsakeys/` 密钥，不复制安装参数，不运行安装器。仓库携带 vendor 不代表依赖已安全审计或当前环境可启动。

## 验证与验收

未发现 composer.json 定义项目测试脚本，也未把 vendor 自带测试当成本仓业务覆盖。可作语法检查（需 PHP 环境）：`php -l app/api/controller/Goods.php`、`php -l app/api/controller/Cart.php`、`php -l app/service/GoodsCartService.php`。语法检查不验证订单、库存、支付或用户权限。

本次已完成：核对 Git 基线、阅读上述源码和命令定义；OpenSpec 1.14.2 严格校验 2/2 通过，`git diff --check` 通过，本次新增/改写文档的 11 个本地相对链接检查无缺失（保留的上游原文不计入）。
本次未运行：依赖安装、应用启动、构建、业务测试、真实平台/外部服务集成及生产部署。原因是本次范围仅限文档与流程，不写运行数据、不调用收费服务。

- [ ] 在隔离环境按上面的路径启动。
- [ ] 按每个 [核心 spec](../openspec/specs/) 验收正常、空输入/不存在和失败场景。
- [ ] 变更身份、权限或数据语义时核对所有消费者，补充原行为与失败路径验证。
- [ ] PR 记录实际命令、结果与未覆盖项，不将“可运行命令”写成“已通过”。

## 已知边界与重新评估条件

- 与移动端保留路由查询 `api.php?s=<controller>/<action>`、`token`/客户端参数、返回 `code`/`msg`/`data` 约定；不能单边重命名字段。
- 普通购物车控制器先调用 IsLogin；未登录业务码是 `-400`，不要擅自改为 HTTP 401 或与其他业务码混用。
- `GoodsCartDelete` 服务可接收无 user 的其他调用；本次 spec 仅记录 API 控制器注入当前用户的路径，不能把它扩展为所有服务调用都强制归属校验。
- 商品详情读取还会递增访问统计、写浏览记录；不能对真实站点把详情联调当成完全只读。
- 安装器、插件、库存、下单、支付回调、退款和定时任务未实测。上线或改动相关契约时，需要另立业务验收矩阵并核对全部客户端。
- 未修复或搬迁业务实现、依赖、默认配置、运行文件与密钥，避免文档任务扩大为部署整改。

## 文档维护

[README](../README.md) 提供入口，[AGENTS](../AGENTS.md) 约束开发，[OpenSpec](../openspec/README.md) 保存行为与后续变更证据。源码或依赖变化后同步复核本基线。
