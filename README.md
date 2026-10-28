# toolshop‑testing
> Practice Software Testing — Toolshop(Sprint5‑with‑bugs) 手工+接口测试实战仓库
本仓库为软件测试练习项目，针对预埋90+ intentional bugs 的 Toolshop 电商实训系统，完成**手工黑盒功能测试**与**Postman API接口功能**，沉淀完整测试资产：测试计划、测试用例、缺陷报告、测试报告、Postman集合。

## 项目背景
Toolshop 是专为软件测试练习打造的开源电商实训系统，Sprint5 with‑bugs版本内置大量UI、接口、安全、无障碍缺陷，用于练习测试用例设计、缺陷挖掘、bug报告编写、接口安全测试。

- 前端站点：[https://with‑bugs.practicesoftwaretesting.com](https://with‑bugs.practicesoftwaretesting.com)
- API服务：[https://api‑with‑bugs.practicesoftwaretesting.com](https://api‑with‑bugs.practicesoftwaretesting.com)
- Swagger接口文档：[https://api‑with‑bugs.practicesoftwaretesting.com/api/documentation](https://api‑with‑bugs.practicesoftwaretesting.com/api/documentation)
- 技术栈：Laravel12(PHP8.3) / Angular20 / MariaDB / Docker

**预置测试账号**
|角色|邮箱|密码|
|---|---|---|
|管理员|`admin@practicesoftwaretesting.com`|`welcome01`|
|普通用户A|`customer@practicesoftwaretesting.com`|`welcome01`|
|普通用户B|`customer2@practicesoftwaretesting.com`|`welcome01`|
|普通用户C|`customer3@practicesoftwaretesting.com`|`pass123`|

## 仓库目录结构
```
toolshop-testing
│   .gitignore
│   README.md
│   
├───api-testing                # API接口测试相关资产
│   │   study-record.md        # 接口测试学习笔记
│   │   test-report.md         # API接口完整测试报告
│   │   
│   ├───bug-reports
│   │       api-bug-summary.md # 接口缺陷完整汇总表
│   │       
│   ├───postman
│   │       toolshop-postman-collection.json  # Postman测试集合
│   │       
│   └───test-cases             # 分模块接口测试用例
│           delete-api-testing.md
│           invoices-api-testing.md
│           login-api-testing.md
│           query-api-testing.md
│           
├───docs
│       practice-soft-testing-withbugs.md  # 项目官方文档、已知bug清单
│       
└───manual-testing             # 手工功能测试资产
    │   study-record.md        # 手工测试学习记录
    │   test-plan.md           # 手工测试计划
    │   test-report.md         # 手工测试完整测试报告
    │   
    ├───bug-reports
    │   │   bug-list.md        # UI手工测试缺陷完整汇总
    │   │   
    │   └───images             # bug复现截图png
    │           
    └───test-cases             # 手工模块测试用例
            TC_home.md
            TC_login.md
            TC_order-flow.md
            TC_register.md
```

## 测试工作概述
### 手工功能测试（manual‑testing）
**测试版本**：Sprint5 with Bugs
**测试类型**：系统级黑盒手工功能测试
**浏览器环境**：Chrome（主）、Edge
**测试设计方法**：等价类划分、边界值分析、场景法、错误推测法、抓包辅助验证
- 测试覆盖模块：登录、注册、首页&筛选、商品详情、购物车、账单地址、端到端下单结账流程
- 产出指标：**73条手工测试用例，发现30个UI缺陷**
  - 致命缺陷：2个；严重缺陷：6个；较严重：3个；一般：12个；轻微：7个
  - 用例通过率(不含阻塞)：**53.5%**
- 版本结论：**该版本不满足上线标准**，存在阻塞完整下单业务的致命缺陷，必须修复致命或严重缺陷后回归验证。

> 关键阻塞问题：支付网关缺失导致完整购物流程无法闭环；价格筛选接口传参缺失筛选完全失效。

### API接口测试（api‑testing）
**测试工具**：Postman
**测试内容**：接口功能校验、参数合法性校验、鉴权权限控制、OWASP API Top10安全类测试检查、异常HTTP状态码校验
- 覆盖接口：登录认证、用户查询、商品查询、品牌删除、商品删除、发票新增/查询，合计8个接口
- 产出指标：**65条接口测试用例，发现25个接口缺陷**，总通过率64.6%
- 高危风险：
  1. 部分接口鉴权完全失效，匿名用户可以执行删除数据操作；
  2. 垂直&水平越权漏洞，普通用户可访问他人隐私数据；
  3. 登录接口存在SQL注入风险；
  4. 业务参数校验缺失，可篡改发票单价、生成负数数量脏数据；
  5. HTTP状态码不规范，参数错误和鉴权错误返回码混淆。

## 📄 核心文档快速跳转
### 手工测试
- [测试计划](./manual-testing/test-plan.md)：测试目标、范围、环境、策略、准入准出、风险
- [测试报告](./manual-testing/test-report.md)：用例执行统计、缺陷分析、版本质量结论
- [缺陷汇总清单](./manual-testing/bug-reports/bug-list.md)：30个UI缺陷完整记录（含复现步骤、截图链接、优先级严重性）
- 测试用例
  - [登录模块用例](./manual-testing/test-cases/TC_login.md)
  - [注册模块用例](./manual-testing/test-cases/TC_register.md)
  - [首页&筛选用例](./manual-testing/test-cases/TC_home.md)
  - [订单全流程用例](./manual-testing/test-cases/TC_order-flow.md)

### 接口测试
- [API测试报告](./api-testing/test-report.md)：接口用例统计、漏洞分析、修复建议
- [接口缺陷汇总](./api-testing/bug-reports/api-bug-summary.md)：25个接口bug完整复现记录
- Postman集合文件：[toolshop‑postman‑collection.json](./api-testing/postman/toolshop-postman-collection.json)
- 分模块接口用例：`api-testing/test-cases/`目录

### 项目参考资料
- [项目官方说明 & 全部已知bug清单](./docs/practice-soft-testing-withbugs.md)

## 🚀 Postman集合导入使用
1. 打开Postman客户端 → `Import`
2. 选择文件：`api‑testing/postman/toolshop‑postman‑collection.json`
3. 导入完成后，集合内已经预置各个接口请求；
4. 使用登录接口获取Bearer Token，配置环境变量`access_token`供其他鉴权接口使用；
5. 可结合Newman实现批量接口回归执行。

## 关键测试统计摘要
### 手工测试
|统计项|数值|
|---|---|
|总用例数|73|
|通过|38|
|失败|32|
|阻塞|2|
|通过率(排除阻塞)|53.5%|
|发现Bug总数|30|
|致命与严重缺陷|8|

### API接口测试
|统计项|数值|
|---|---|
|总接口用例数|65|
|通过|42|
|失败|23|
|接口总通过率|64.6%|
|接口缺陷总数|25|
|严重级别缺陷数量|13|

## 版本核心问题摘要
1. **手工UI侧**
    - 致命：支付网关缺失，端到端下单流程完全阻塞；价格筛选maxPrice参数未传给后端筛选失效；
    - 大量导航路由错误、排序逻辑颠倒、表单校验失效；
    - 多处UI文案拼写错误、跨浏览器兼容性问题。
2. **API接口侧**
    - 高危越权&鉴权失效，匿名可删除品牌/商品，普通用户读取全站发票、用户隐私；
    - SQL注入风险；发票业务校验缺失，可构造虚假财务数据；
    - HTTP状态码使用不规范，不利于客户端错误处理。

## 改进与学习建议
1. **冒烟测试套件**：从现有用例中抽取核心P0用例组成BVT冒烟集合，每次版本更新优先执行；
2. **接口自动化**：将Postman集合结合Newman接入CI，完成接口回归自动化；
3. **安全基线**：把OWASP API Top10检查固化为每次迭代必测项；
4. **补充测试维度**：本仓库未覆盖性能、无障碍、移动端响应式测试，后续可以扩展练习；
5. **缺陷管理**：实际项目中建议将用例与缺陷导入Jira/禅道进行管理。

## 学习记录
- 手工测试学习笔记：[manual‑testing/study‑record.md](./manual-testing/study-record.md)
- 接口测试学习笔记：[api‑testing/study‑record.md](./api-testing/study-record.md)

## 📜 License
本仓库仅用于个人软件测试学习练习，被测Toolshop系统参考官方开源项目。

---