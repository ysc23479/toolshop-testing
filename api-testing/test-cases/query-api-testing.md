#

## 一、GET /users

| 测试用例编号 | 测试用例标题 | 前置条件 | 请求方式 | 接口地址 | 请求头 | 请求体 | 预期结果 | 实际结果 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TC_USERS_001 | 携带有效管理员Token查询全部用户 | 1. 管理员账号admin已登录；2. 获取合法Bearer Token | GET | /users | Authorization: Bearer {{admin_token}}   Accept: application/json | 无 | 1. 状态码200；   2. 返回分页结构current_page、data用户数组、total总条数等；   3. data内每条用户包含id、姓名、邮箱、failed_login_attempts、启用状态等完整字段 | 与预期一致 |
| TC_USERS_002 | 携带普通用户Token查询全部用户 | 1. 普通客户账号customer登录；2. 获取合法普通用户Bearer Token | GET | /users | Authorization: Bearer {{token}}   Accept: application/json | 无 | 1. 状态码200；   2. 正常返回用户分页列表；   3. 接口返回数据中泄露其他用户enabled、failed_login_attempts字段 | 角色权限不足错误返回 401 |
| TC_USERS_003 | 无Authorization头访问用户列表 | 无登录，未携带任何Token | GET | /users | Accept: application/json | 无 | 状态码401，返回{"message":"Unauthorized"} | 与预期一致 |

## 二、GET /users/{userId}

| 测试用例编号 | 测试用例标题 | 前置条件 | 请求方式 | 接口地址 | 请求头 | 请求体 | 预期结果 | 实际结果 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TC_USER_001 | 管理员Token查询自身userId详情 | 管理员admin账号登录，获取有效token，admin对应userId已知为1 | GET | /users/1 | Authorization: Bearer {{admin_token}}   Accept: application/json | 无 | 状态码200，完整返回当前管理员个人信息 | 与预期一致 |
| TC_USER_002 | 普通用户Token查询自身userId详情 | customer账号登录，持有有效token，自身userId为2 | GET | /users/2 | Authorization: Bearer {{customer_token}}   Accept: application/json | 无 | 状态码200，正常返回本人完整用户信息 | 与预期一致 |
| TC_USER_003 | 普通用户Token传入其他用户ID查看他人信息 | 1. 普通客户token有效；2. 存在另一用户userId=3 | GET | /users/3 | Authorization: Bearer {{customer_token}}   Accept: application/json | 无 | 状态码403，禁止访问其他用户信息 | 正常返回ID=3用户完整信息，存在垂直越权漏洞 |
| TC_USER_004 | 路径参数userId不存在 | 系统用户最大id为10，传入userId=9999 | GET | /users/9999 | Authorization: Bearer {{admin_token}}   Accept: application/json | 无 | 状态码404，返回{"message":"Requested item not found"} | 与预期一致 |
| TC_USER_005 | userId传负数非法数字 | 有效管理员token，路径参数userId=-5 | GET | /users/-5 | Authorization: Bearer {{admin_token}}   Accept: application/json | 无 | 状态码404，未找到资源 | 与预期一致 |
| TC_USER_006 | userId传非数字字符串 | 有效管理员token，userId=abc | GET | /users/abc | Authorization: Bearer {{admin_token}}   Accept: application/json | 无 | 状态码404，资源不存在 | 与预期一致 |
| TC_USER_007 | 无Token访问用户详情接口 | 未登录，无Authorization请求头 | GET | /users/1 | Accept: application/json | 无 | 状态码401，返回Unauthorized | 与预期一致 |
| TC_USER_008 | 使用POSTT请求访问/users/{userId}接口 | 有效管理员token，userId=1 | POST | /users/1 | Authorization: Bearer {{admin_token}}   Accept: application/json | {} | 返回405，提示Method is not allowed for the requested route | 与预期一致 |
| TC_USER_009 | 递增ID遍历用户数据 | 普通用户token，循环传入1、2、3自增userId | GET | /users/1、/users/2... | Authorization: Bearer {{customer_token}}   Accept: application/json | 无 | 普通用户仅允许查看自身 userId 信息，访问其他递增 userId 应返回 403 禁止访问 | 可依次获取全部用户详情，ID自增可猜测 |

## 三、GET /products

| 测试用例编号 | 测试用例标题 | 前置条件 | 请求方式 | 接口地址 | 请求头 | 请求体 | 预期结果 | 实际结果 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TC_PROD_001 | 无任何查询参数查询全部商品 | 无需登录鉴权，系统存在商品数据 | GET | /products | Accept: application/json | 无 | 1. 状态码200；   2. 返回分页商品列表current_page、data、total等；   3. 商品data中无暴露字段 | data中暴露库存stock字段 |
| TC_PROD_002 | 按品牌by_brand筛选商品 | 系统存在品牌ID=1，该品牌下有商品 | GET | /products?by_brand=1 | Accept: application/json | 无 | 200，仅返回该品牌对应商品分页数据 | 与预期一致 |
| TC_PROD_003 | by_brand传入不存在品牌ID筛选 | 无品牌ID=9999 | GET | /products?by_brand=9999 | Accept: application/json | 无 | 200，data数组为空，total=0 | 与预期一致 |
| TC_PROD_004 | 按分类by_category筛选有效分类商品 | 存在分类ID=4，分类下有商品 | GET | /products?by_category=4 | Accept: application/json | 无 | 200，仅返回该分类商品列表 | 与预期一致 |
| TC_PROD_005 | is_rental筛选租赁商品 | 存在租赁类型商品 | GET | /products?is_rental=true | Accept: application/json | 无 | 200，只返回租赁类商品数据 | 与预期一致 |
| TC_PROD_006 | between价格区间合法格式筛选 | 存在价格10~50区间商品 | GET | /products?between=price,10,50 | Accept: application/json | 无 | 200，仅返回价格区间内商品 | 与预期一致 |
| TC_PROD_007 | between传非法区间格式 | 参数between=price,abc,50 | GET | /products?between=price,abc,50 | Accept: application/json | 无 | 200，不生效过滤，返回全部商品，过滤失效 | 与预期一致 |
| TC_PROD_008 | 价格区间between最小值大于最大值 | between=price,100,10 | GET | /products?between=price,100,10 | Accept: application/json | 无 | 200，无匹配商品，data为空数组 | 与预期一致 |
| TC_PROD_009 | sort升序按商品名称排序 name,asc | 商品列表存在多条数据 | GET | /products?sort=name,asc | Accept: application/json | 无 | 200，商品data数组按名称升序排列 | 与预期一致 |
| TC_PROD_010 | sort降序按价格排序 price,desc | 商品存在不同价格 | GET | /products?sort=price,desc | Accept: application/json | 无 | 200，商品按价格从高到低排序 | 与预期一致 |
| TC_PROD_011 | 多条件组合筛选 | 品牌1、价格0-100、价格升序 | GET | /products?by_brand=1&between=price,0,100&sort=price,asc | Accept: application/json | 无 | 200，同时满足全部筛选条件并按规则排序 | 与预期一致 |
| TC_PROD_012 | 无Accept头访问商品接口 | 无需登录鉴权 | GET | /products | 无 | 无 | 200，默认返回json分页商品数据 | 与预期一致 |
| TC_PROD_013 | PUT方式请求/products商品接口 | 无鉴权 | PUT | /products | Accept: application/json | {} | 状态码405，提示Method is not allowed for the requested route | 与预期一致 |
