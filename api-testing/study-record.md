# 学习记录

## 1.学习 Postman 基础

1. [Postman 官方文档](https://learning.postman.com/docs/getting-started/introduction/)

## 2.接口测试

查看Swagger接口文档：[接口文档](https://api-with-bugs.practicesoftwaretesting.com/api/documentation/)

### 2.1 登录接口

登录接口信息如下：

|项目|内容|
|---|--|
|请求方式|POST|
|接口地址|`https://api-with-bugs.practicesoftwaretesting.com/users/login`|
|请求体参数|email: 登录邮箱, password: 登录密码|
|响应参数|响应状态码: 200 OK,access_token: 登录成功后的token,token_type: 登录成功后的token类型,expires_in: 登录成功后的token过期时间|
|Content-Type|application/json|

安全测试：SQL注入漏洞、Token有效期
等价类划分：邮箱、密码
边界值测试：邮箱、密码

### 2.2 查询接口

#### 2.2.1 GET /users

|项目|内容|
|---|--|
|作用|检索所有用户|
|请求方式|GET|
|请求地址|`https://api-with-bugs.practicesoftwaretesting.com/users`|
|鉴权方式|Bearer Token，请求头携带 Authorization: Bearer {{token}}|
|请求参数|无请求参数|
|请求头可选参数|Accept: application/json（指定返回数据格式为 JSON）|
|可能的响应结果1|响应状态码: 200 响应数据格式：application/json 返回示例：current_page: 当前页码,data: 用户数据数组,from: 本页起始数据序号,last_page: 最后一页页码,per_page: 每页数据条数,to: 数据列表结束页码,total: 数据总条数|
|可能得响应结果2|响应状态码: 400 描述：错误请求 返回示例：Bad Request|
|可能得响应结果3|响应状态码: 401 描述：未登录/身份认证失败 响应数据格式：application/json 返回示例：{"message": "Unauthorized"}|

#### 2.2.2 GET /users{userId}

|项目|内容|
|---|--|
|作用|根据路径参数userId检索指定单个用户信息|
|请求方式|GET|
|请求地址|`https://api-with-bugs.practicesoftwaretesting.com/users/{userId}`|
|鉴权方式|Bearer Token，请求头携带 Authorization: Bearer {{token}}|
|请求参数|路径必填参数userId：整数类型，代表用户唯一标识，示例值：1|
|请求头可选参数|Accept: application/json（指定返回数据格式为 JSON）|
|可能的响应结果1|响应状态码: 200 响应数据格式：application/json 返回示例：{"first_name":"John","last_name":"Doe","address":"Street 1","city":"City","state":"State","country":"Country","postcode":"1234AA","phone":"0987654321","dob":"1970-01-01","email":"<john@doe.example>","id":0}|
|可能的响应结果2|响应状态码: 401 描述：未登录/身份认证失败 响应数据格式：application/json 返回示例：{"message": "Unauthorized"}|
|可能的响应结果3|响应状态码: 404 描述：未找到对应资源 响应数据格式：application/json 返回示例：{"message": "Requested item not found"}|
|可能的响应结果4|响应状态码: 405 描述：当前请求路由不允许使用该请求方法 响应数据格式：application/json 返回示例：{"message": "Method is not allowed for the requested route"}|

#### 2.2.3 GET /products

|项目|内容|
|---|--|
|作用|检索所有商品，支持多条件筛选、价格区间过滤、排序分页查询|
|请求方式|GET|
|请求地址|`https://api-with-bugs.practicesoftwaretesting.com/products`|
|鉴权方式|无|
|请求参数|查询参数：1.by_brand（整数，品牌ID）；2.by_category（整数，分类ID）；3.is_rental（字符串，是否租赁商品标识）；4.between（字符串，价格区间，示例price,10,30）；5.sort（字符串，排序规则，示例name,asc / price,desc）|
|请求头可选参数|Accept: application/json（指定返回数据格式为 JSON）|
|可能的响应结果1|响应状态码: 200 响应数据格式：application/json 返回示例：current_page: 当前页码,data: 商品数据数组,next_page_url: 下一页链接,path: 请求路径,per_page: 每页条数,prev_page_url: 上一页链接,to: 本页结束序号,total: 商品总数量|
|可能的响应结果2|响应状态码: 404 描述：未找到对应资源 响应数据格式：application/json 返回示例：{"message": "Requested item not found"}|
|可能的响应结果3|响应状态码: 405 描述：当前请求路由不允许使用该请求方法 响应数据格式：application/json 返回示例：{"message": "Method is not allowed for the requested route"}|

## 2.3 DELETE

### 2.3.1 DELETE /brands/{brandId}

| 项目 | 内容 |
| --- | --- |
| 作用 | 根据路径参数brandId删除指定品牌，仅管理员角色允许执行删除操作 |
| 请求方式 | DELETE |
| 请求地址 | `https://api-with-bugs.practicesoftwaretesting.com/brands/{brandId}` |
| 鉴权方式 | Bearer Token，请求头携带 Authorization: Bearer {{admin_token}}，校验用户是否为管理员角色 |
| 请求参数 | 路径必填参数brandId：整数类型，品牌唯一标识，示例值：1 |
| 请求头可选参数 | Accept: application/json（指定返回数据格式为 JSON） |
| 可能的响应结果1 | 响应状态码: 204 描述：删除成功，无返回响应体 |
| 可能的响应结果2 | 响应状态码: 401 描述：未登录/身份认证失败 响应数据格式：application/json 返回示例：{"message": "Unauthorized"} |
| 可能的响应结果3 | 响应状态码: 404 描述：不存在对应ID的品牌资源 响应数据格式：application/json 返回示例：{"message": "Requested item not found"} |
| 可能的响应结果4 | 响应状态码: 405 描述：当前路由不允许DELETE请求 响应数据格式：application/json 返回示例：{"message": "Method is not allowed for the requested route"} |
| 可能的响应结果5 | 响应状态码: 409 描述：该品牌已被商品关联使用，无法删除 |
| 可能的响应结果6 | 响应状态码: 422 描述：服务器无法处理请求内容（如brandId传非整数格式） |

### 2.3.2 DELETE /products/{productId}

| 项目 | 内容 |
| --- | --- |
| 作用 | 根据路径参数productId删除指定商品，仅管理员角色允许执行删除操作 |
| 请求方式 | DELETE |
| 请求地址 | `https://api-with-bugs.practicesoftwaretesting.com/products/{productId}` |
| 鉴权方式 | Bearer Token，请求头携带 Authorization: Bearer {{admin_token}}，校验用户是否为管理员角色 |
| 请求参数 | 路径必填参数productId：整数类型，商品唯一标识 |
| 请求头可选参数 | Accept: application/json（指定返回数据格式为 JSON） |
| 可能的响应结果1 | 响应状态码: 204 描述：删除成功，无返回响应体 |
| 可能的响应结果2 | 响应状态码: 401 描述：未登录/身份认证失败 响应数据格式：application/json 返回示例：{"message": "Unauthorized"} |
| 可能的响应结果3 | 响应状态码: 404 描述：不存在对应ID的商品资源 响应数据格式：application/json 返回示例：{"message": "Requested item not found"} |
| 可能的响应结果4 | 响应状态码: 405 描述：当前路由不允许DELETE请求 响应数据格式：application/json 返回示例：{"message": "Method is not allowed for the requested route"} |
| 可能的响应结果5 | 响应状态码: 409 描述：该商品已被订单/发票关联使用，无法删除 |
| 可能的响应结果6 | 响应状态码: 422 描述：服务器无法处理请求内容（如productId传非整数格式） |

## 2.4 invoices

### 2.4.1 GET /invoices

| 项目 | 内容 |
| --- | --- |
| 作用 | 查询全部发票；管理员可查看所有用户发票，普通用户仅能查看自身关联发票 |
| 请求方式 | GET |
| 请求地址 | `https://api-with-bugs.practicesoftwaretesting.com/invoices` |
| 鉴权方式 | Bearer Token，请求头携带 Authorization: Bearer {{admin_token/token}}，区分管理员/普通用户数据权限 |
| 请求参数 | 无查询参数 |
| 请求头可选参数 | Accept: application/json（指定返回数据格式为 JSON） |
| 可能的响应结果1 | 响应状态码: 200 响应数据格式：application/json 返回示例：current_page当前页码、data发票数组、next_page_url下一页链接、path请求路径、per_page每页条数、prev_page_url上一页链接、to本页结束序号、total发票总数量；单条发票包含id、开票日期、发票编号、账单地址、总金额、支付方式、状态、invoice_items商品明细、商品及品牌分类完整信息 |
| 可能的响应结果2 | 响应状态码: 401 描述：未登录/身份认证失败 响应数据格式：application/json 返回示例：{"message": "Unauthorized"} |
| 可能的响应结果3 | 响应状态码: 404 描述：未查询到发票资源 响应数据格式：application/json 返回示例：{"message": "Requested item not found"} |
| 可能的响应结果4 | 响应状态码: 405 描述：当前路由不允许GET请求 响应数据格式：application/json 返回示例：{"message": "Method is not allowed for the requested route"} |

### 2.4.2 POST /invoices

| 项目 | 内容 |
| --- | --- |
| 作用 | 提交请求体创建新发票，包含账单信息、支付信息、商品明细列表 |
| 请求方式 | POST |
| 请求地址 | `https://api-with-bugs.practicesoftwaretesting.com/invoices` |
| 鉴权方式 | Bearer Token，请求头携带 Authorization: Bearer {{token}} |
| 请求参数 | 无路径/查询参数，请求体为application/json格式必填 |
| 请求体结构 | user_id用户ID、账单地址/城市/国家/州/邮编、total总金额、payment_method支付方式、付款账户姓名、付款账号；invoice_items数组（product_id商品ID、quantity购买数量、unit_price单价） |
| 请求头可选参数 | Accept: application/json、Content-Type: application/json |
| 可能的响应结果1 | 响应状态码: 200 描述：创建发票成功；返回完整发票JSON结构，包含发票ID、编号、明细、商品品牌分类图片等全量字段 |
| 可能的响应结果2 | 响应状态码: 401 描述：未登录/身份认证失败 响应数据格式：application/json 返回示例：{"message": "Unauthorized"} |
| 可能的响应结果3 | 响应状态码: 404 描述：关联商品/用户不存在 响应数据格式：application/json 返回示例：{"message": "Requested item not found"} |
| 可能的响应结果4 | 响应状态码: 405 描述：当前路由不允许POST请求 响应数据格式：application/json 返回示例：{"message": "Method is not allowed for the requested route"} |
| 可能的响应结果5 | 响应状态码: 422 描述：请求体参数校验失败（缺失必填字段、数值非法等） |
