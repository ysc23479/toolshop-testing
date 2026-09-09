# Practice Software Testing Toolshop — API 接口测试报告

---

## 文档信息

| 项目 | 内容 |
| --- | --- |
| **报告名称** | Toolshop 电商平台 API 接口测试报告 |
| **测试版本** | 线上稳定版 |
| **测试人员** | xushixian |
| **测试日期** | 2026-07-10 |
| **报告日期** | 2026-07-10 |
| **被测系统** | Practice Software Testing Toolshop (api-with-bugs.practicesoftwaretesting.com) |

---

## 一、测试概述

### 1.1 测试目的

本次 API 接口测试旨在对 Practice Software Testing Toolshop 电商平台的 RESTful API 进行系统性功能验证与安全评估，具体目标包括：

1. **验证接口功能正确性**：确保各接口在合法输入下能正确执行业务逻辑并返回符合预期的响应。
2. **评估接口异常处理能力**：测试接口在非法输入、边界条件、异常场景下的容错与错误反馈是否规范。
3. **检测接口安全漏洞**：发现并报告鉴权缺陷、权限越权、数据泄露、注入攻击等安全风险。
4. **验证 HTTP 状态码规范**：检查接口在不同场景下返回的 HTTP 状态码是否符合 RESTful 规范。
5. **为质量改进提供依据**：汇总缺陷信息，为开发团队提供可操作的修复建议。

### 1.2 测试范围

本次测试覆盖 Toolshop 平台以下 API 模块：

| 模块 | 接口 | 方法 | 测试用例数 |
| --- | --- | --- | --- |
| 登录认证 | `/users/login` | POST | 15 |
| 用户管理 | `/users` | GET | 3 |
| 用户管理 | `/users/{userId}` | GET | 9 |
| 商品管理 | `/products` | GET | 13 |
| 商品管理 | `/products/{productId}` | DELETE | 7 |
| 品牌管理 | `/brands/{brandId}` | DELETE | 7 |
| 发票管理 | `/invoices` | GET | 4 |
| 发票管理 | `/invoices` | POST | 7 |
| **合计** | **8 个接口** | **4 种方法** | **65** |

### 1.3 测试目标

- **功能测试**：验证接口在正常输入下的业务逻辑正确性。
- **参数校验测试**：验证接口对必填参数缺失、非法格式、边界值的校验能力。
- **鉴权与权限测试**：验证 Token 鉴权机制、角色权限隔离、匿名访问拦截。
- **异常场景测试**：验证接口对非法请求方法、错误 Content-Type、不存在的资源等异常场景的处理。
- **安全测试**：SQL 注入防护、越权访问（IDOR）、敏感数据泄露、请求方法绕过等。

### 1.4 背景信息

Practice Software Testing Toolshop 是一个用于软件测试实践的开源电商模拟平台。其 API 服务 (`api-with-bugs.practicesoftwaretesting.com`) 名称表明该平台**故意包含若干 Bug**，旨在为测试人员提供真实的缺陷发现练习场景。API 采用 RESTful 风格设计，使用 Bearer Token 进行身份认证，数据格式为 JSON。接口文档通过 Swagger 提供（`/api/documentation`）。

---

## 二、测试环境

| 配置项 | 详情 |
| --- | --- |
| **被测系统地址** | `https://api-with-bugs.practicesoftwaretesting.com` |
| **接口文档地址** | `https://api-with-bugs.practicesoftwaretesting.com/api/documentation/` |
| **测试工具** | Postman（桌面客户端） |
| **测试方式** | 黑盒手工测试 |
| **网络环境** | 公网访问 |
| **操作系统** | Windows 11 Home (10.0.26100) |
| **测试数据集** | Postman Collection（toolshop-postman-collection.json） |
| **测试账号** | `customer@practicesoftwaretesting.com` / `welcome01`（普通用户） |
|  | `customer2@practicesoftwaretesting.com` / `welcome01`（普通用户2） |
|  | `admin@practicesoftwaretesting.com` / `welcome01`（管理员） |
| **鉴权方式** | Bearer Token（通过 `/users/login` 获取 access_token） |
| **请求格式** | Content-Type: application/json |

---

## 三、测试方法

### 3.1 测试策略

本次测试采用**黑盒测试**方法，不依赖对系统内部实现的知识，从 API 使用者的视角验证接口行为。测试策略如下：

1. **正向功能测试**：使用合法输入验证接口核心功能。
2. **输入验证测试**：包括必填参数缺失、参数类型错误、边界值、空值、非法格式等。
3. **鉴权测试**：验证有/无 Token 场景下的访问控制。
4. **权限测试**：验证不同角色（管理员 vs 普通用户）的数据访问边界。
5. **安全测试**：SQL 注入、越权访问、敏感字段泄露、HTTP 方法绕过。
6. **异常处理测试**：非法请求方法、错误 Content-Type、不存在资源等场景。

### 3.2 测试设计技术

| 技术 | 应用场景 |
| --- | --- |
| **等价类划分** | 登录接口的邮箱格式（合法/非法）、密码（正确/错误） |
| **边界值分析** | userId/password 的空值、null 值、不存在 ID |
| **错误推测法** | 基于 RESTful API 常见缺陷模式推测异常场景 |
| **安全测试用例设计** | OWASP API Top 10 安全风险清单指导 |
| **场景测试** | 多条件组合查询、完整业务流程链路 |

### 3.3 测试执行流程

```
1. 接口文档分析 → 2. 测试用例设计 → 3. Postman 测试脚本编写
       ↓
4. 逐条执行测试 → 5. 记录实际结果 → 6. 预期与实际对比
       ↓
7. 缺陷汇总分类 → 8. 测试报告撰写
```

---

## 四、测试案例

### 4.1 登录接口 — POST /users/login

#### 4.1.1 功能测试

| 编号 | 测试标题 | 测试数据 | 预期结果 | 实际结果 | 结论 |
| --- | --- | --- | --- | --- | --- |
| TC_LOG_001 | 合法账号密码正常登录 | email: customer@..., password: welcome01 | 200, 返回 token | 与预期一致 | ✅ PASS |
| TC_LOG_002 | 邮箱格式错误登录 | email: testdemo.com (无@) | 401, 拒绝下发 token | 与预期一致 | ✅ PASS |
| TC_LOG_003 | 账号存在但密码错误 | password: 123456wrong | 401, 账号密码不匹配 | 与预期一致 | ✅ PASS |
| TC_LOG_004 | 未注册邮箱登录 | email: nouser@test.com | 401, 用户不存在 | 与预期一致 | ✅ PASS |
| TC_LOG_013 | Token 有效期合理性校验 | 正常登录查看 expires_in | expires_in 数值合理，不超长 | 与预期一致 | ✅ PASS |

#### 4.1.2 参数校验测试

| 编号 | 测试标题 | 测试数据 | 预期结果 | 实际结果 | 结论 |
| --- | --- | --- | --- | --- | --- |
| TC_LOG_005 | 邮箱为空提交 | email: "" | 400 参数缺失 | 401 Unauthorized | ❌ FAIL |
| TC_LOG_006 | 密码为空提交 | password: "" | 400 参数非法 | 401 Unauthorized | ❌ FAIL |
| TC_LOG_007 | 缺失 email 键值对 | 仅传 password | 400 email 必填缺失 | 401 Unauthorized | ❌ FAIL |
| TC_LOG_008 | 缺失 password 键值对 | 仅传 email | 400 password 必填缺失 | 401 Unauthorized | ❌ FAIL |
| TC_LOG_015 | 空对象请求体 | {} | 400 必填项缺失 | 401 Unauthorized | ❌ FAIL |

#### 4.1.3 异常处理测试

| 编号 | 测试标题 | 测试数据 | 预期结果 | 实际结果 | 结论 |
| --- | --- | --- | --- | --- | --- |
| TC_LOG_009 | GET 方式调用登录接口 | GET /users/login | 405 Method Not Allowed | 401 Unauthorized | ❌ FAIL |
| TC_LOG_010 | Content-Type: text/plain | 非 JSON 请求头 | 415 Unsupported Media Type | 401 Unauthorized | ❌ FAIL |

#### 4.1.4 安全测试

| 编号 | 测试标题 | 测试数据 | 预期结果 | 实际结果 | 结论 |
| --- | --- | --- | --- | --- | --- |
| TC_LOG_011 | 邮箱 SQL 注入 | email: admin@...' -- | 401 防注入，拒绝登录 | 500 Internal Server Error | ❌ FAIL |
| TC_LOG_012 | 密码 SQL 注入 | password: ' or 1=1 -- | 401 鉴权失败 | 与预期一致 | ✅ PASS |
| TC_LOG_014 | 普通用户访问管理员报表接口 | 普通 Token 访问 /reports/total-sales-of-years | 403 Forbidden | 200 成功返回数据 | ❌ FAIL |

---

### 4.2 用户查询接口 — GET /users

| 编号 | 测试标题 | 前置条件 | 预期结果 | 实际结果 | 结论 |
| --- | --- | --- | --- | --- | --- |
| TC_USERS_001 | 管理员 Token 查询全部用户 | 管理员登录 | 200，返回分页用户列表 | 与预期一致 | ✅ PASS |
| TC_USERS_002 | 普通用户 Token 查询全部用户 | 普通用户登录 | 403 权限不足 | 401 Unauthorized | ❌ FAIL |
| TC_USERS_003 | 无 Token 查询用户列表 | 无鉴权 | 401 Unauthorized | 与预期一致 | ✅ PASS |

---

### 4.3 用户详情接口 — GET /users/{userId}

| 编号 | 测试标题 | 前置条件 | 预期结果 | 实际结果 | 结论 |
| --- | --- | --- | --- | --- | --- |
| TC_USER_001 | 管理员查询自身详情 | admin, userId=1 | 200，返回个人信息 | 与预期一致 | ✅ PASS |
| TC_USER_002 | 普通用户查询自身详情 | customer, userId=2 | 200，返回本人信息 | 与预期一致 | ✅ PASS |
| TC_USER_003 | 普通用户查看他人信息 | customer Token, userId=3 | 403 禁止访问 | 200 返回他人完整信息 | ❌ FAIL |
| TC_USER_004 | userId 不存在 | userId=9999 | 404 资源未找到 | 与预期一致 | ✅ PASS |
| TC_USER_005 | userId 传负数 | userId=-5 | 404 资源不存在 | 与预期一致 | ✅ PASS |
| TC_USER_006 | userId 传非数字字符串 | userId=abc | 404 资源不存在 | 与预期一致 | ✅ PASS |
| TC_USER_007 | 无 Token 访问用户详情 | 无鉴权, /users/1 | 401 Unauthorized | 与预期一致 | ✅ PASS |
| TC_USER_008 | POST 方法访问 GET 路由 | POST /users/1 | 405 Method Not Allowed | 与预期一致 | ✅ PASS |
| TC_USER_009 | 递增 ID 遍历用户数据 | customer Token 遍历 1,2,3... | 403 禁止访问他人 | 200 全量返回 | ❌ FAIL |

---

### 4.4 商品查询接口 — GET /products

| 编号 | 测试标题 | 测试参数 | 预期结果 | 实际结果 | 结论 |
| --- | --- | --- | --- | --- | --- |
| TC_PROD_001 | 无参数查询全部商品 | 无 | 200，分页列表，无敏感字段 | 200，但泄露 stock 字段 | ❌ FAIL |
| TC_PROD_002 | 按品牌筛选 | ?by_brand=1 | 200，返回该品牌商品 | 与预期一致 | ✅ PASS |
| TC_PROD_003 | 品牌 ID 不存在 | ?by_brand=9999 | 200，data 为空 | 与预期一致 | ✅ PASS |
| TC_PROD_004 | 按分类筛选 | ?by_category=4 | 200，返回该分类商品 | 与预期一致 | ✅ PASS |
| TC_PROD_005 | 租赁商品筛选 | ?is_rental=true | 200，仅租赁商品 | 与预期一致 | ✅ PASS |
| TC_PROD_006 | 价格区间筛选 | ?between=price,10,50 | 200，区间内商品 | 与预期一致 | ✅ PASS |
| TC_PROD_007 | 非法区间格式 | ?between=price,abc,50 | 200，过滤失效 | 与预期一致 | ✅ PASS |
| TC_PROD_008 | 最小值大于最大值 | ?between=price,100,10 | 200，data 为空 | 与预期一致 | ✅ PASS |
| TC_PROD_009 | 名称升序排序 | ?sort=name,asc | 200，按名称升序 | 与预期一致 | ✅ PASS |
| TC_PROD_010 | 价格降序排序 | ?sort=price,desc | 200，按价格降序 | 与预期一致 | ✅ PASS |
| TC_PROD_011 | 多条件组合筛选 | by_brand + between + sort | 200，同时满足条件 | 与预期一致 | ✅ PASS |
| TC_PROD_012 | 无 Accept 头访问 | 无 Accept 头 | 200，默认 JSON | 与预期一致 | ✅ PASS |
| TC_PROD_013 | PUT 方法访问 GET 路由 | PUT /products | 405 Method Not Allowed | 与预期一致 | ✅ PASS |

---

### 4.5 品牌删除接口 — DELETE /brands/{brandId}

| 编号 | 测试标题 | 前置条件 | 预期结果 | 实际结果 | 结论 |
| --- | --- | --- | --- | --- | --- |
| TC_BRAND_DEL_001 | 管理员删除存在品牌 | admin Token, brandId=10 | 204，删除成功 | 与预期一致 | ✅ PASS |
| TC_BRAND_DEL_002 | 普通用户删除品牌 | customer Token, brandId=3 | 403 权限不足 | 204 删除成功 | ❌ FAIL |
| TC_BRAND_DEL_003 | 无 Token 删除品牌 | 无鉴权, brandId=4 | 401 Unauthorized | 204 删除成功 | ❌ FAIL |
| TC_BRAND_DEL_004 | 删除不存在品牌 ID | admin Token, brandId=9999 | 404 资源不存在 | 422 参数非法 | ❌ FAIL |
| TC_BRAND_DEL_005 | 删除已关联商品品牌 | admin Token, brandId=2 | 409 冲突，禁止删除 | 与预期一致 | ✅ PASS |
| TC_BRAND_DEL_006 | brandId 传字符串非法参数 | brandId=abc | 422 参数处理失败 | 与预期一致 | ✅ PASS |
| TC_BRAND_DEL_007 | PUT 方法访问删除路由 | PUT /brands/1 | 405 Method Not Allowed | 200 成功删除品牌 | ❌ FAIL |

---

### 4.6 商品删除接口 — DELETE /products/{productId}

| 编号 | 测试标题 | 前置条件 | 预期结果 | 实际结果 | 结论 |
| --- | --- | --- | --- | --- | --- |
| TC_PROD_DEL_001 | 管理员删除无关联商品 | admin Token, productId=2 | 204，删除成功 | 与预期一致 | ✅ PASS |
| TC_PROD_DEL_002 | 普通用户删除商品 | customer Token, productId=4 | 403 权限不足 | 204 删除成功 | ❌ FAIL |
| TC_PROD_DEL_003 | 无 Token 删除商品 | 无鉴权, productId=6 | 401 Unauthorized | 204 删除成功 | ❌ FAIL |
| TC_PROD_DEL_004 | 删除不存在商品 ID | admin Token, productId=9999 | 404 资源不存在 | 422 参数非法 | ❌ FAIL |
| TC_PROD_DEL_005 | 删除已关联发票商品 | admin Token, productId=5 | 409 冲突，禁止删除 | 与预期一致 | ✅ PASS |
| TC_PROD_DEL_006 | productId 传字母非法参数 | productId=test | 422 参数处理失败 | 与预期一致 | ✅ PASS |
| TC_PROD_DEL_007 | POST 方法访问删除路由 | POST /products/9 | 405 Method Not Allowed | 与预期一致 | ✅ PASS |

---

### 4.7 发票查询接口 — GET /invoices

| 编号 | 测试标题 | 前置条件 | 预期结果 | 实际结果 | 结论 |
| --- | --- | --- | --- | --- | --- |
| TC_INV_GET_001 | 管理员查询全部发票 | admin Token | 200，返回所有用户发票 | 与预期一致 | ✅ PASS |
| TC_INV_GET_002 | 普通用户查询自身发票 | customer Token | 200，仅返回自身发票 | 200，返回全部用户发票 | ❌ FAIL |
| TC_INV_GET_003 | 无 Token 查询发票 | 无鉴权 | 401 Unauthorized | 200，返回所有发票 | ❌ FAIL |
| TC_INV_GET_004 | PUT 方法访问查询接口 | PUT /invoices | 405 Method Not Allowed | 与预期一致 | ✅ PASS |

---

### 4.8 发票创建接口 — POST /invoices

| 编号 | 测试标题 | 前置条件 | 预期结果 | 实际结果 | 结论 |
| --- | --- | --- | --- | --- | --- |
| TC_INV_POST_001 | 合法参数创建发票 | user Token，完整请求体 | 201，发票创建成功 | 与预期一致 | ✅ PASS |
| TC_INV_POST_002 | 无 Token 创建发票 | 无鉴权 | 401 Unauthorized | 与预期一致 | ✅ PASS |
| TC_INV_POST_003 | 篡改商品单价 | unit_price: 1（原价 12.01） | 422 校验拦截 | 201 创建成功 | ❌ FAIL |
| TC_INV_POST_004 | 缺失必填 total 字段 | 删除 total 字段 | 422 参数校验失败 | 与预期一致 | ✅ PASS |
| TC_INV_POST_005 | 传入不存在 product_id | product_id=99 | 404 商品不存在 | 与预期一致 | ✅ PASS |
| TC_INV_POST_006 | PUT 方法创建发票 | PUT /invoices | 405 Method Not Allowed | 与预期一致 | ✅ PASS |
| TC_INV_POST_007 | 负数购买数量 | quantity: -2 | 422 数值非法 | 201 创建成功 | ❌ FAIL |

---

### 4.9 测试结果汇总

| 接口模块 | 用例数 | 通过 | 失败 | 通过率 |
| --- | --- | --- | --- | --- |
| POST /users/login | 15 | 7 | 8 | 46.7% |
| GET /users | 3 | 2 | 1 | 66.7% |
| GET /users/{userId} | 9 | 7 | 2 | 77.8% |
| GET /products | 13 | 12 | 1 | 92.3% |
| DELETE /brands/{brandId} | 7 | 3 | 4 | 42.9% |
| DELETE /products/{productId} | 7 | 4 | 3 | 57.1% |
| GET /invoices | 4 | 2 | 2 | 50.0% |
| POST /invoices | 7 | 5 | 2 | 71.4% |
| **总计** | **65** | **42** | **23** | **64.6%** |

---

## 五、缺陷报告

本次测试共发现 **25 个缺陷**，按功能模块分类如下：

### 5.1 缺陷统计概览

| 维度 | 分类 | 数量 |
| --- | --- | --- |
| | 最高（严重） | 9 |
| **按优先级** | 高 | 8 |
| | 中 | 8 |
| | | |
| | 严重 | 13 |
| **按严重性** | 主要 | 5 |
| | 一般 | 2 |
| | 次要 | 5 |
| | | |
| | 登录接口 | 9 |
| **按模块** | 用户接口 | 5 |
| | 商品接口 | 4 |
| | 品牌接口 | 4 |
| | 发票接口 | 5 |

> 注：部分缺陷跨模块关联，按主要影响模块归类。

### 5.2 缺陷清单

#### 5.2.1 登录接口缺陷（9 个）

| Bug 编号 | 标题 | 严重性 | 优先级 | 关联用例 |
| --- | --- | --- | --- | --- |
| BUG_LOG_001 | 邮箱为空时错误返回 401，未按规范返回 400 | 次要 | 中 | TC_LOG_005 |
| BUG_LOG_002 | 密码为空时错误返回 401，未返回 400 | 次要 | 中 | TC_LOG_006 |
| BUG_LOG_003 | 缺失 email 参数未返回 400，错误返回 401 | 次要 | 中 | TC_LOG_007 |
| BUG_LOG_004 | 缺失 password 参数未返回 400，返回 401 | 次要 | 中 | TC_LOG_008 |
| BUG_LOG_005 | GET 方式调用未返回 405，错误返回 401 | 一般 | 高 | TC_LOG_009 |
| BUG_LOG_006 | Content-Type: text/plain 未返回 415，返回 401 | 一般 | 高 | TC_LOG_010 |
| BUG_LOG_007 | 邮箱 SQL 注入触发 500 内部错误 | 严重 | 最高 | TC_LOG_011 |
| BUG_LOG_008 | 普通用户 Token 访问管理员报表接口未拦截 | 严重 | 最高 | TC_LOG_014 |
| BUG_LOG_009 | 空对象请求体未返回 400，错误返回 401 | 次要 | 中 | TC_LOG_015 |

#### 5.2.2 用户接口缺陷（5 个）

| Bug 编号 | 标题 | 严重性 | 优先级 | 关联用例 |
| --- | --- | --- | --- | --- |
| BUG_API_001 | 普通用户查询全量用户列表权限不足返回 401 而非 403 | 主要 | 高 | TC_USERS_002 |
| BUG_API_002 | 普通用户可通过 userId 路径参数访问其他用户详情（IDOR 漏洞） | 严重 | 高 | TC_USER_003 |
| BUG_API_003 | 用户 ID 为连续自增数字，可暴力枚举遍历全部用户数据 | 严重 | 高 | TC_USER_009 |
| BUG_API_004 | 商品列表接口向非管理员泄露库存 stock 敏感字段 | 次要 | 中 | TC_PROD_001 |
| BUG_API_005 | 普通用户查询自身详情泄露 enabled、failed_login_attempts 安全敏感字段 | 主要 | 高 | TC_USER_002 |

#### 5.2.3 品牌删除接口缺陷（4 个）

| Bug 编号 | 标题 | 严重性 | 优先级 | 关联用例 |
| --- | --- | --- | --- | --- |
| BUG_API_006 | 普通用户 Token 未拦截，可越权删除品牌数据 | 严重 | 最高 | TC_BRAND_DEL_002 |
| BUG_API_007 | 匿名无 Token 可删除品牌数据，鉴权完全失效 | 严重 | 最高 | TC_BRAND_DEL_003 |
| BUG_API_008 | 删除不存在品牌 ID 错误返回 422，未返回 404 | 次要 | 中 | TC_BRAND_DEL_004 |
| BUG_API_009 | PUT 请求可成功删除品牌，请求方法校验缺失 | 主要 | 高 | TC_BRAND_DEL_007 |

#### 5.2.4 商品删除接口缺陷（4 个）

| Bug 编号 | 标题 | 严重性 | 优先级 | 关联用例 |
| --- | --- | --- | --- | --- |
| BUG_API_010 | 普通用户可越权删除商品数据 | 严重 | 最高 | TC_PROD_DEL_002 |
| BUG_API_011 | 匿名无 Token 可删除商品，鉴权完全失效 | 严重 | 最高 | TC_PROD_DEL_003 |
| BUG_API_012 | 删除不存在商品 ID 错误返回 422，未返回 404 | 次要 | 中 | TC_PROD_DEL_004 |

#### 5.2.5 发票接口缺陷（5 个）

| Bug 编号 | 标题 | 严重性 | 优先级 | 关联用例 |
| --- | --- | --- | --- | --- |
| BUG_API_013 | 普通用户无数据隔离，可越权查询全站用户发票 | 严重 | 最高 | TC_INV_GET_002 |
| BUG_API_014 | 匿名可直接查询所有用户发票数据 | 严重 | 最高 | TC_INV_GET_003 |
| BUG_API_015 | 创建发票未校验商品单价，可任意篡改生成虚假发票 | 严重 | 最高 | TC_INV_POST_003 |
| BUG_API_016 | 创建发票未校验负数购买数量，可生成非法负数金额发票 | 主要 | 高 | TC_INV_POST_007 |

> 注：商品删除接口中 TC_PROD_DEL_007 (POST 方法访问删除路由) 测试通过，未发现 Bug，故未列入；BUG_API_009 涉及品牌删除 PUT 方法绕过，已列入品牌接口缺陷。

---

### 5.3 缺陷分布分析

```
缺陷严重性分布：

严重(13)  ████████████████████████████████████████████████████ 52%
主要(5)   ████████████████████ 20%
一般(2)   ████████ 8%
次要(5)   ████████████████████ 20%

缺陷模块分布：

登录接口(9)  ██████████████████████████████████████ 36%
用户接口(5)  ████████████████████ 20%
品牌接口(4)  ██████████████████ 16%
商品接口(4)  ██████████████████ 16%
发票接口(5)  ████████████████████ 20%
```

---

## 六、测试结果分析

### 6.1 核心问题分析

#### 6.1.1 鉴权与权限控制严重缺失（最高风险）

这是本次测试暴露的**最严重问题**。品牌删除 (`/brands/{brandId}`)、商品删除 (`/products/{productId}`)、发票查询 (`/invoices`) 三个接口存在**匿名无 Token 即可操作**的严重鉴权漏洞，同时存在**普通用户可执行管理员专属操作**的垂直越权漏洞。这表明服务端在中间件或拦截器层面的鉴权逻辑存在系统性缺陷，部分接口完全未接入鉴权链。

- 受影响接口：DELETE /brands, DELETE /products, GET /invoices
- 风险等级：**严重（Critical）**
- 可能导致：数据被未授权删除、全站用户隐私数据泄露

#### 6.1.2 HTTP 状态码使用不规范（高风险）

登录接口 (`/users/login`) 的参数校验逻辑**完全被鉴权逻辑覆盖**——所有参数缺失/非法的场景全部返回 401 而非 400，请求方法错误返回 401 而非 405，Content-Type 错误返回 401 而非 415。接口在处理请求时未按"参数校验 → 鉴权 → 业务逻辑"的标准分层进行处理，导致客户端无法准确判断错误原因。

- 受影响接口：POST /users/login（影响最严重）
- 风险等级：**高（High）**
- 可能导致：客户端错误处理混乱，调试困难

#### 6.1.3 数据权限隔离缺失（高风险）

发票查询接口 (`/invoices`) 和用户详情接口 (`/users/{userId}`) 未实现用户级数据隔离：普通用户可查询所有用户的发票数据，可通过路径参数 ID 遍历访问任意用户的个人信息。这属于 OWASP API Top 10 中的 **API1:2023 — 对象级授权失效 (Broken Object Level Authorization)**。

- 受影响接口：GET /invoices, GET /users/{userId}
- 风险等级：**严重（Critical）**
- 可能导致：全站用户隐私数据批量泄露

#### 6.1.4 业务参数校验缺失（高风险）

发票创建接口 (`POST /invoices`) 未校验商品单价合规性和购买数量的合法性，攻击者可任意篡改单价生成虚假财务发票，或传入负数数量制造脏数据。这属于 **OWASP API Top 10 — API3:2023 对象属性级授权失效** 与业务逻辑校验缺失的复合问题。

- 受影响接口：POST /invoices
- 风险等级：**高（High）**
- 可能导致：财务数据造假、统计报表失真

#### 6.1.5 SQL 注入安全风险（最高风险）

登录接口邮箱字段存在 SQL 注入漏洞，传入注入载荷后服务端直接抛出 500 Internal Server Error，表明后端未使用参数化查询或预编译语句，注入载荷导致 SQL 执行异常。这是 **OWASP API Top 10 — API8:2023 安全配置错误/注入** 的典型表现。

- 受影响接口：POST /users/login
- 风险等级：**严重（Critical）**
- 可能导致：数据库被脱库、数据被篡改

#### 6.1.6 敏感信息泄露（中高风险）

- 商品列表接口 (`GET /products`) 向所有用户（含匿名）暴露 `stock` 库存字段，属于管理敏感数据，普通用户和匿名用户不应可见。
- 用户详情接口 (`GET /users/{userId}`) 泄露 `enabled`（账号状态）和 `failed_login_attempts`（登录失败次数）字段，这些字段可被攻击者利用进行账号枚举和撞库攻击。

- 受影响接口：GET /products, GET /users/{userId}
- 风险等级：**中-高（Medium-High）**
- 可能导致：商业敏感数据泄露、账号安全攻击面扩大

### 6.2 接口质量评分

| 接口 | 通过率 | 鉴权 | 权限控制 | 参数校验 | 异常处理 | 安全 | 综合评分 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| POST /users/login | 46.7% | ⚠️ | ⚠️ | 🔴 | 🔴 | 🔴 | **D** |
| GET /users | 66.7% | ✅ | 🔴 | ✅ | ✅ | ⚠️ | **C** |
| GET /users/{userId} | 77.8% | ✅ | 🔴 | ✅ | ✅ | 🔴 | **C** |
| GET /products | 92.3% | ✅ | ✅ | ✅ | ✅ | ⚠️ | **B** |
| DELETE /brands/{brandId} | 42.9% | 🔴 | 🔴 | 🔴 | 🔴 | 🔴 | **F** |
| DELETE /products/{productId} | 57.1% | 🔴 | 🔴 | ⚠️ | ✅ | 🔴 | **D** |
| GET /invoices | 50.0% | 🔴 | 🔴 | ✅ | ✅ | 🔴 | **D** |
| POST /invoices | 71.4% | ✅ | ⚠️ | ⚠️ | ✅ | 🔴 | **C** |

> 图例：✅ 正常 ⚠️ 存在问题 🔴 严重缺陷

---

## 七、结论与建议

### 7.1 总体结论

本次对 Practice Software Testing Toolshop 平台的 8 个 API 接口进行了系统性测试，共执行 **65 个测试用例**，通过 **42 个**，总通过率为 **64.6%**。测试发现 **25 个缺陷**，其中严重级别 13 个、主要级别 5 个。

**平台 API 整体安全性堪忧**：鉴权机制存在系统性缺陷（多个接口完全未接入鉴权链）、权限控制大面积失效（普通用户可执行管理员操作）、存在 SQL 注入漏洞以及多处敏感数据泄露。平台名称中"with-bugs"名副其实，不建议在生产环境中以当前状态使用。

### 7.2 重点修复建议

#### 🔴 紧急修复（Critical — 建议 24 小时内修复）

1. **【BUG_API_007/011/014】修复匿名访问鉴权漏洞**
   - 在品牌删除、商品删除、发票查询接口接入统一的 Token 校验中间件
   - 确保所有需鉴权的接口在未携带 Token 时返回 401

2. **【BUG_API_006/010/013】修复垂直越权漏洞**
   - 品牌/商品删除接口增加管理员角色校验
   - 发票查询接口增加用户数据隔离逻辑（普通用户仅返回自身数据）
   - 用户详情接口增加所有权校验（仅允许查看本人或管理员查看所有人）

3. **【BUG_API_002/003】修复水平越权漏洞（IDOR）**
   - 用户详情接口校验请求者 userId 与路径参数 userId 是否一致
   - 考虑使用 UUID 替代自增整数 ID

4. **【BUG_LOG_007】修复 SQL 注入漏洞**
   - 所有数据库查询统一使用参数化查询（Prepared Statement）
   - 对用户输入进行严格的输入校验和转义

#### 🟡 高危修复（High — 建议 1 周内修复）

5. **【BUG_LOG_001-004/009】修复登录接口参数校验顺序**
   - 将参数校验逻辑前置到鉴权之前：先校验必填参数 → 再校验参数格式 → 最后鉴权
   - 参数缺失/非法返回 400，鉴权失败返回 401

6. **【BUG_LOG_005/006】修复异常处理状态码**
   - 请求方法校验返回 405 Method Not Allowed
   - Content-Type 校验返回 415 Unsupported Media Type

7. **【BUG_API_015/016】修复发票创建业务校验**
   - 增加商品单价后端校验（与数据库商品单价比对）
   - 增加购买数量合法性校验（正整数 > 0）

#### 🟢 建议优化（Medium — 建议下个迭代修复）

8. **【BUG_API_004/005】敏感字段脱敏**
   - 商品列表接口对非管理员用户隐藏 stock 字段
   - 用户详情接口对非管理员/非本人隐藏 enabled、failed_login_attempts 字段

9. **【BUG_API_008/012】HTTP 状态码规范化**
   - 资源不存在统一返回 404（而非 422）
   - 参数格式非法返回 422

10. **【BUG_API_009】请求方法校验**
    - 每个路由显式声明允许的 HTTP 方法，拒绝未声明的方法

### 7.3 回归测试建议

建议在修复后进行以下回归测试重点：

1. **鉴权全链路回归**：对所有需鉴权接口验证匿名/普通用户/管理员三级权限边界
2. **参数校验回归**：重点验证登录接口的参数校验→鉴权的分层逻辑
3. **SQL 注入回归**：对所有接受用户输入的接口进行注入测试
4. **数据隔离回归**：验证发票查询、用户详情的用户级数据隔离
5. **敏感字段回归**：确认所有接口对非管理员用户正确脱敏

### 7.4 测试改进建议

1. **引入自动化测试**：将 Postman Collection 集成到 CI/CD 流水线中，使用 Newman 进行自动化回归
2. **建立安全测试基线**：将 OWASP API Top 10 安全检查项固化为每次发版前的必测用例
3. **补充性能测试**：本次仅覆盖功能与安全测试，建议补充并发、压力测试
4. **补充接口契约测试**：基于 Swagger/OpenAPI 规范进行接口契约验证

---

> 📎 **附录文件**：
> - [登录接口测试详情](./login-api-testing.md)
> - [查询接口测试详情](./query-api-testing.md)
> - [删除接口测试详情](./delete-api-testing.md)
> - [发票接口测试详情](./invoices-api-testing.md)
> - [缺陷汇总表](./api-bug-summary.md)
> - [Postman 测试集合](./toolshop-postman-collection.json)
> - [学习记录与接口文档](./study-record.md)

