# ShopXO 服务端个人副本：开发约定

## 范围与事实来源

先核对分支、工作区和 [工程基线](docs/engineering-baseline.md)。阅读调用入口、服务及消费者后再修改。保留上游署名、许可与有效说明；快照不用于证明个人原创或历史贡献。本次文档整理不授权改变业务行为。

重点保持与 shopxo-uniapp 的路由、鉴权参数、业务错误码和字段语义。修改订单、购物车、支付、会员或插件钩子前必须盘点全部前台/后台/API/定时任务调用，确认行为矩阵。vendor、runtime、rsakeys 和生产配置不是文档调查对象。

## 变更与安全边界

- 登录、权限、删除、数据结构、支付或公开接口变化，先列出需改变和需保留的行为；调查所有调用方，确认业务取舍再实现。
- 不读取或提交真实凭据、私有环境配置、上传资源、运行数据库或日志；文档示例用 `<YOUR_VALUE>` 占位。
- 不自动运行迁移、种子初始化、部署、收费模型调用或破坏性操作。未授权时不提交、推送、合并。
- 外部服务、文件、平台能力及存储失败需有明确结果；不能把 mock、代码阅读或格式校验当成业务验收。

## 验证

未发现 composer.json 定义项目测试脚本，也未把 vendor 自带测试当成本仓业务覆盖。可作语法检查（需 PHP 环境）：`php -l app/api/controller/Goods.php`、`php -l app/api/controller/Cart.php`、`php -l app/service/GoodsCartService.php`。语法检查不验证订单、库存、支付或用户权限。

文档修改执行 `git diff --check` 和 `OPENSPEC_TELEMETRY=0 openspec validate --all --strict --no-interactive`。业务检查应在隔离环境、依赖准备完成后执行，并报告未运行项。现有测试不等于完整覆盖。

## OpenSpec

按 [OpenSpec 流程](openspec/README.md) 从已实现基线维护 `specs/`；行为变更先写 `changes/<id>/proposal.md`、增量 specs、design 和 tasks。记录验证证据再归档，不把未来计划写成已实现功能。
