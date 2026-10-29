# 用户购物车 API

## Purpose
记录普通购物车控制器的身份注入、汇总响应和删除边界，保持与 uni-app 的契约。依据：`app/api/controller/Cart.php`、`app/api/controller/Common.php::IsLogin`、`app/service/GoodsCartService.php`。

## Requirements

### Requirement: 购物车接口要求用户身份
Cart 控制器 SHALL 在进入业务方法前调用 IsLogin。

#### Scenario: 用户缺失
- **WHEN** 当前请求没有解析到用户
- **THEN** 返回登录失败消息及业务 code=-400，不执行购物车方法

#### Scenario: 加载购物车
- **WHEN** 已登录用户调用 Index
- **THEN** 以当前用户读取汇总与列表，并将列表置于汇总的 data 字段后返回

### Requirement: API 删除限定当前用户
Cart Delete SHALL 将当前用户注入服务参数，让该路径的删除条件包含 user_id。

#### Scenario: 已登录用户删除
- **WHEN** 已登录用户提交有效的 id 或 ids
- **THEN** 删除查询同时约束提交的购物车 ID 和当前用户 ID；仅在数据库报告删除记录时返回成功与新汇总

#### Scenario: 缺少删除 ID
- **WHEN** id 和 ids 均为空
- **THEN** 服务返回数据 ID 错误，业务 code=-1
