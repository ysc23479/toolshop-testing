## 一、GET /invoices

| 测试用例编号 | 测试用例标题 | 前置条件 | 请求方式 | 接口地址 | 请求头 | 请求体 | 预期结果 | 实际结果 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TC_INV_GET_001 | 管理员Token查询全部发票 | 管理员登录获取admin Token，系统存在多条不同用户发票 | GET | /invoices | Authorization: Bearer {{admin_token}}   Accept: application/json | 无 | 1.状态码200；2.分页结构完整；3.data包含所有用户的发票数据 | 与预期一致 |
| TC_INV_GET_002 | 普通用户Token仅查询自身发票 | 普通用户A登录获取token，系统存在A、B两名用户发票 | GET | /invoices | Authorization: Bearer {{userA_token}}   Accept: application/json | 无 | 仅返回userA发票 | 返回全部用户所有发票，无数据隔离 |
| TC_INV_GET_003 | 无Token查询发票列表 | 未登录，无Authorization头 | GET | /invoices | Accept: application/json | 无 | 状态码401，{"message":"Unauthorized"} | 返回所有用户信息 |
| TC_INV_GET_004 | 使用PUT请求访问发票查询接口 | 管理员有效Token | PUT | /invoices | Authorization: Bearer {{admin_token}}   Accept: application/json | 无 | 状态码405，提示方法不允许 | 与预期一致 |

## 二、 POST /invoices

| 测试用例编号 | 测试用例标题 | 前置条件 | 请求方式 | 接口地址 | 请求头 | 请求体 | 预期结果 | 实际结果 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TC_INV_POST_001 | 合法参数创建发票 | 普通用户登录获取token，product_id=9商品存在 | POST | /invoices | Authorization: Bearer {{user_token}}   Content-Type: application/json   Accept: application/json | {"user_id":1,"billing_address":"test addr","billing_city":"wuhan","billing_country":"cn","billing_state":"hubei","billing_postcode":"430000","total":12.01,"payment_method":"Cash on Delivery","payment_account_name":"Jogn Doe","payment_account_number":"0987654345","invoice_items":[{"product_id":9,"quantity":1,"unit_price":12.01}]} | 1.状态码201；2.返回完整发票json；3.数据库新增一条发票记录 | 与预期一致 |
| TC_INV_POST_002 | 未携带Token创建发票 | 无登录凭证 | POST | /invoices | Content-Type: application/json   Accept: application/json | 标准合法请求体 | 状态码401，{"message":"Unauthorized"} | 与预期一致 |
| TC_INV_POST_003 | 请求体篡改商品单价低于成本 | 用户有效Token，商品原价12.01 | POST | /invoices | Authorization: Bearer {{user_token}}   Content-Type: application/json   Accept: application/json | invoice_items中unit_price改为1 | 校验拦截返回422 | 接口直接使用传入单价创建发票，金额可任意篡改 |
| TC_INV_POST_004 | 请求体缺失必填total字段 | 用户有效Token | POST | /invoices | Authorization: Bearer {{user_token}}   Content-Type: application/json   Accept: application/json | 删除total字段的残缺请求体 | 返回422，参数校验失败 | 与预期一致 |
| TC_INV_POST_005 | invoice_items传入不存在product_id | 用户有效Token，无productId=99商品 | POST | /invoices | Authorization: Bearer {{user_token}}   Content-Type: application/json   Accept: application/json | product_id=99的明细数据 | 返回404，提示商品资源不存在 | 与预期一致 |
| TC_INV_POST_006 | 使用PUT请求创建发票 | 用户有效Token | PUT | /invoices | Authorization: Bearer {{user_token}}   Accept: application/json | 无 | 状态码405，请求方法不允许 | 与预期一致 |
| TC_INV_POST_007 | 传入负数购买数量quantity=-2 | 用户有效Token，合法商品ID | POST | /invoices | Authorization: Bearer {{user_token}}   Content-Type: application/json   Accept: application/json | quantity=-2的明细 | 返回422，数值非法校验失败 | 返回创建好的发票 |
