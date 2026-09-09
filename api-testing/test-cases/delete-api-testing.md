## 一、 DELETE /brands/{brandId}

| 测试用例编号 | 测试用例标题 | 前置条件 | 请求方式 | 接口地址 | 请求头 | 请求体 | 预期结果 | 实际结果 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TC_BRAND_DEL_001 | 管理员携带有效Token删除存在品牌 | 1.管理员账号登录；2.获取合法admin Token；3.数据库存在brandId=10品牌，无商品关联 | DELETE | /brands/10 | Authorization: Bearer {{admin_token}}   Accept: application/json | 无 | 1.响应状态码204；2.无响应体；3.数据库中brandId=10数据已删除 | 与预期一致 |
| TC_BRAND_DEL_002 | 普通用户Token尝试删除品牌 | 1.普通客户登录；2.获取customer Token；3.存在brandId=3品牌 | DELETE | /brands/3 | Authorization: Bearer {{customer_token}}   Accept: application/json | 无 | 权限不足，返回403 | 无需管理员Token即可删除，返回204 |
| TC_BRAND_DEL_003 | 无Token访问删除品牌接口 | 无登录，未携带Authorization头 | DELETE | /brands/4 | Accept: application/json | 无 | 状态码401，响应{"message":"Unauthorized"} | 无需Token即可删除，返回204 |
| TC_BRAND_DEL_004 | 删除不存在的品牌ID | 1.管理员有效Token；2.数据库无brandId=9999 | DELETE | /brands/9999 | Authorization: Bearer {{admin_token}}   Accept: application/json | 无 | 状态码404，响应{"message":"Requested item not found"} | 返回422：{"id":["The selected id is invalid."]} |
| TC_BRAND_DEL_005 | 删除已关联商品的品牌 | 1.管理员有效Token；2.brandId=2下存在商品数据 | DELETE | /brands/2 | Authorization: Bearer {{admin_token}}   Accept: application/json | 无 | 返回409，提示实体被其他地方使用，禁止删除 | 与预期一致 |
| TC_BRAND_DEL_006 | brandId传字符串非整数 | 1.管理员有效Token | DELETE | /brands/abc | Authorization: Bearer {{admin_token}}   Accept: application/json | 无 | 返回422，服务器无法处理参数 | 与预期一致 |
| TC_BRAND_DEL_007 | 使用PUT请求访问删除品牌路由 | 1.管理员有效Token | PUT | /brands/1 | Authorization: Bearer {{admin_token}}   Accept: application/json | 无 | 状态码405，响应{"message":"Method is not allowed for the requested route"} | 返回200:{"success": true} ，成功删除该品牌|

## 二、 DELETE /products/{productId}

| 测试用例编号 | 测试用例标题 | 前置条件 | 请求方式 | 接口地址 | 请求头 | 请求体 | 预期结果 | 实际结果 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TC_PROD_DEL_001 | 管理员Token删除无订单关联商品 | 1.管理员登录获取admin Token；2.productId=2商品无发票关联 | DELETE | /products/2 | Authorization: Bearer {{admin_token}}   Accept: application/json | 无 | 1.状态码204；2.商品数据从库中删除 | 与预期一致 |
| TC_PROD_DEL_002 | 普通用户Token删除商品 | 1.普通客户登录获取customer Token；2.存在productId=4商品 | DELETE | /products/4 | Authorization: Bearer {{customer_token}}   Accept: application/json | 无 | 返回403 | 普通用户可直接删除商品，返回204 |
| TC_PROD_DEL_003 | 未携带Token删除商品 | 无登录 | DELETE | /products/6 | Accept: application/json | 无 | 状态码401，{"message":"Unauthorized"} | 可直接删除商品，返回204 |
| TC_PROD_DEL_004 | 删除不存在商品ID | 管理员有效Token，无productId=9999商品 | DELETE | /products/9999 | Authorization: Bearer {{admin_token}}   Accept: application/json | 无 | 状态码404，{"message":"Requested item not found"} | 返回422：{"id":["The selected id is invalid."]} |
| TC_PROD_DEL_005 | 删除已存在发票关联的商品 | 管理员Token，productId=5已在发票明细中使用 | DELETE | /products/5 | Authorization: Bearer {{admin_token}}   Accept: application/json | 无 | 返回409，提示实体被占用无法删除 | 与预期一致 |
| TC_PROD_DEL_006 | productId传字母非法参数 | 管理员有效Token | DELETE | /products/test | Authorization: Bearer {{admin_token}}   Accept: application/json | 无 | 返回422，参数处理失败 | 与预期一致 |
| TC_PROD_DEL_007 | POST请求访问商品删除路由 | 管理员有效Token | POST | /products/9 | Authorization: Bearer {{admin_token}}   Accept: application/json | 空json | 返回405，请求方法不允许 | 与预期一致 |