# 商品详情 API

## Purpose
记录手机端商品详情查询的输入、数据结构和错误结果。依据：`app/api/controller/Goods.php::Detail`；对应移动端 `pages/goods-detail/goods-detail.vue` 调用 goods/detail。

## Requirements

### Requirement: 返回商品详情结果
商品详情接口 SHALL 按 id 或 goods_id 查询未删除商品，并返回既有业务结果结构。

#### Scenario: 商品存在
- **WHEN** 请求解析得到非零商品 ID 且查询到未删除商品
- **THEN** 返回包含 goods、cart_total、buy_left_nav、buy_button、middle_tabs_nav、nav_more_list、guess_you_like 的结果，再经现有响应服务封装

#### Scenario: 缺少商品 ID
- **WHEN** id 和 goods_id 解析后均无法提供非零 ID
- **THEN** 返回参数错误，业务 code 为 -1

#### Scenario: 商品不存在或已删除
- **WHEN** 查询没有返回可用商品
- **THEN** 返回商品不存在或已删除错误，业务 code 为 -1
