| 测试用例编号 | 测试用例标题 | 前置条件 | 请求方式 | 接口地址 | 请求头 |请求体| 预期结果 | 实际结果 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TC_LOG_001 | 正常登录 | 账号已注册激活，状态正常未锁定 | POST | /users/login | Content-Type: application/json | {"email":"<customer@practicesoftwaretesting.com>","password":"welcome01"} | 返回200 OK，正常返回access_token、token_type、expires_in | 与预期一致 |
| TC_LOG_002 | 邮箱格式错误登录 | 无登录状态 | POST | /users/login | Content-Type: application/json | {"email":"testdemo.com","password":"welcome01"} | 返回401，提示邮箱格式非法，拒绝下发token | 与预期一致 |
| TC_LOG_003 | 账号存在但密码错误登录 | 账号注册正常，密码填写错误 | POST | /users/login | Content-Type: application/json | {"email":"<customer@practicesoftwaretesting.com>","password":"123456wrong"} | 返回401，账号密码不匹配，登录失败 | 与预期一致 |
| TC_LOG_004 | 未注册邮箱尝试登录 | 邮箱不存在于系统用户库 | POST | /users/login | Content-Type: application/json | {"email":"<nouser@test.com>","password":"welcome01"} | 返回401，用户不存在，鉴权失败 | 与预期一致 |
| TC_LOG_005 | 邮箱为空提交登录 | 匿名请求，email传空字符串 | POST | /users/login | Content-Type: application/json | {"email":"","password":"welcome01"} | 400必填参数缺失校验失败，禁止登录 | 返回401，拒绝下发token |
| TC_LOG_006 | 密码为空提交登录 | 匿名请求，password传空字符串 | POST | /users/login | Content-Type: application/json | {"email":"<customer@practicesoftwaretesting.com>","password":""} | 400参数非法，登录拒绝 | 返回401，拒绝下发token |
| TC_LOG_007 | 请求体缺少email键值对 | 仅传password，缺失邮箱参数 | POST | /users/login | Content-Type: application/json | {"password":"welcome01"} | 400提示必填参数email缺失 | 返回401，拒绝下发token |
| TC_LOG_008 | 请求体缺少password键值对 | 仅传email，缺失密码参数 | POST | /users/login | Content-Type: application/json | {"email":"<customer@practicesoftwaretesting.com>"} | 400提示必填参数password缺失 | 返回401，拒绝下发token |
| TC_LOG_009 | 非法请求方式GET调用登录接口 | 匿名发起GET请求，携带合法参数 | GET | /users/login | Content-Type: application/json | {"email":"<customer@practicesoftwaretesting.com>","password":"welcome01"} | 返回405 Method Not Allowed，仅支持POST | 返回401，拒绝下发token |
| TC_LOG_010 | 请求头Content-Type错误（text/plain） | 更换非JSON解析头部 | POST | /users/login | Content-Type: text/plain | {"email":"<customer@practicesoftwaretesting.com>","password":"welcome01"} | 返回415 Unsupported Media Type，无法解析请求体 | 返回401，拒绝下发token |
| TC_LOG_011 | 邮箱字段SQL注入测试 | 未登录匿名访问 | POST | /users/login | Content-Type: application/json | {"email":"<admin@practicesoftwaretesting.com>' --","password":"123wrong"} | 后端参数预编译防注入，返回401，无法无密码登录管理员账号 | 返回500，拒绝下发token |
| TC_LOG_012 | 密码字段SQL注入安全测试 | 未登录匿名访问 | POST | /users/login | Content-Type: application/json | {"email":"<customer@practicesoftwaretesting.com>","password":"' or 1=1 --"} | 注入载荷被过滤，鉴权失败401，无法绕过密码校验 | 与预期一致 |
| TC_LOG_013 | 登录后校验Token超长有效期 | 合法账号登录成功 | POST | /users/login | Content-Type: application/json | {"email":"<customer2@practicesoftwaretesting.com>","password":"welcome01"} | expires_in数值合理，access_token不能为260000min，到期自动失效 | 与预期一致 |
| TC_LOG_014 | 普通账号登录后访问管理员接口校验状态码 | 普通用户登录获取普通权限token | GET | /reports/total-sales-of-years | Content-Type: application/json   Authorization: Bearer Token 普通用户token | 无请求体 | 权限不足返回403 Forbidden，禁止返回401，区分未认证与权限不足 | 成功返回数据 |
| TC_LOG_015 | 空请求体直接提交登录 | 请求体为{}空对象 | POST | /users/login | Content-Type: application/json | {} | 400参数不全，必填项缺失拒绝登录 | 返回401，拒绝下发token |