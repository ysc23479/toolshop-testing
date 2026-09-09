| 测试用例编号 | 测试描述 | 前置条件 | 测试步骤 | 预期结果 | 实际结果 |
| --- | --- | --- | --- | --- | --- |
| TC_LOGIN_001 | 验证登录页面全部UI元素完整展示功能 | 成功进入系统登录页面 | 1.访问页面加载登录弹窗 | 居中展示Login标题、邮箱输入框、密码输入框、密码显隐图标、Login按钮、注册链接、忘记密码链接；邮箱、密码框带*必填标识，布局排版正常 | 默认占位文字Your E-mail *内边距显示错误 BUG_001 |
| TC_LOGIN_002 | 验证邮箱输入框占位文本展示正确性 | 登录页面加载完毕 | 1.查看未输入状态下邮箱框提示文字 | 邮箱框默认占位文字为Your E-mail *，必填符号正常显示 | 与预期一致 |
| TC_LOGIN_003 | 验证密码输入框占位文本展示正确性 | 登录页面加载完毕 | 1.查看未输入状态下密码框提示文字 | 密码框默认占位文字为Your password *，必填符号正常显示 | 与预期一致 |
| TC_LOGIN_004 | 验证密码输入内容默认密文显示功能 | 登录页面加载完毕 | 1.在密码框输入任意字符 | 输入内容自动隐藏为密文，不会明文展示 | 与预期一致 |
| TC_LOGIN_005 | 验证密码显隐图标切换明文展示功能 | 登录页面加载完毕，密码框已输入内容 | 1.密码框输入内容；2.点击密码框右侧眼睛图标 | 密码切换为明文可见状态 | 与预期一致 |
| TC_LOGIN_006 | 验证密码显隐图标二次点击恢复密文功能 | 密码处于明文展示状态 | 1.再次点击密码显隐眼睛图标 | 密码重新切换为密文隐藏状态 | 与预期一致 |
| TC_LOGIN_007 | 验证合法账号密码登录鉴权及首页跳转功能 | 账号customer@practicesoftwaretesting.com状态正常、未锁定 | 1.打开登录页面；2.邮箱输入customer@practicesoftwaretesting.com；3.密码输入welcome01；4.点击Login按钮 | 登录鉴权成功，页面跳转至系统首页 | 与预期一致 |
| TC_LOGIN_008 | 验证邮箱密码均为空时登录必填校验功能 | 登录页面加载完成，输入框无内容 | 1.邮箱、密码均不填写；2.直接点击Login按钮 | 系统提示"Invalid email or password" | 与预期一致 |
| TC_LOGIN_009 | 验证仅邮箱填写、密码为空时登录校验功能 | 登录页面加载完成 | 1.邮箱输入有效账号customer@practicesoftwaretesting.com；2.密码框留空；3.点击Login按钮 | 系统提示"Invalid email or password" | 与预期一致 |
| TC_LOGIN_010 | 验证仅密码填写、邮箱为空时登录校验功能 | 登录页面加载完成 | 1.密码输入正确密码welcome01；2.邮箱框留空；3.点击Login按钮 | 系统提示"Invalid email or password" | 与预期一致 |
| TC_LOGIN_011 | 验证错误密码登录鉴权拦截功能 | 有效测试账号状态正常 | 1.输入正确邮箱customer@practicesoftwaretesting.com；2.输入错误密码welcome02；3.点击Login按钮 | 登录失败，提示"Invalid email or password"，停留在登录页 | 与预期一致 |
| TC_LOGIN_012 | 验证非法格式邮箱输入格式校验功能 | 登录页面加载完成 | 1.邮箱输入非法邮箱格式customer；2.填写正确密码welcome01；3.点击Login按钮 | 系统提示"Invalid email or password"，拒绝登录提交 | 与预期一致 |
| TC_LOGIN_013 | 验证未注册邮箱登录拦截功能 | 登录页面加载完成 | 1.输入未注册邮箱2163396595@qq.com；2.输入任意密码；3.点击Login按钮 | 提示"Invalid email or password"，登录请求被拒绝 | 与预期一致 |
| TC_LOGIN_014 | 验证账号锁定机制功能 | 邮箱：customer3@practicesoftwaretesting.com 密码：pass123 | 1.输入邮箱customer3@practicesoftwaretesting.com；2.输入错误密码pass1234；3.点击Login按钮 4.记录登录尝试次数 5.用正确密码验证是否真的被锁| 第3次错误后被锁 | 密码输错1次就被锁定，BUG_002 |
| TC_LOGIN_015 | 验证Register your account注册链接跳转功能 | 登录页面加载完成 | 1.点击Register your your account蓝色超链接 | 页面正常跳转至用户注册页面 | 与预期一致 |
| TC_LOGIN_016 | 验证Forgot your Password?找回密码链接跳转功能 | 登录页面加载完成 | 1.点击Forgot your Password?文字链接 | 页面正常跳转至密码找回页面 | 与预期一致 |
| TC_LOGIN_017 | 验证回车快捷键提交登录功能 | 已填写正确账号密码 | 1.输入完成后按下键盘回车键 | 等效点击Login按钮，触发登录提交逻辑 | 与预期一致 |