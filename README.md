![image](https://github.com/user-attachments/assets/04226652-3881-4bbd-8177-035ef288d8f4)
![image](https://github.com/user-attachments/assets/f3bd5be2-008f-468c-8567-061076e2a43b)
统一登录地址：https://authserver.nuist.edu.cn/authserver/login



**登录流程分析**

1.前往登录页面获取，才用session获得获取验证码所需的cookie

请求为：
`https://authserver.nuist.edu.cn/authserver/login?service=https%3A%2F%2Fi.nuist.edu.cn%2Flogin%23%2F`

返回值为html源代码，从中获取execution，pwdEncryptSalt。

这里的pwdEncryptSalt是与登录密码一起加密的信息

2.获取验证码

利用上一步session构建的cookie去获取验证码，采用OCR识别验证码

3. 发起登录请求

url为
`https://authserver.nuist.edu.cn/authserver/login?service=https%3A%2F%2Fi.nuist.edu.cn%2Flogin%23%2F`

采用post请求。
请求字典如下，分别对应账号，加密后的密码，验证码

`{
    'username': '202412492138',
    'password': encrypted_password,
    'captcha': result,
    '_eventId': 'submit',
    'cllt': 'userNameLogin',
    'dllt': 'generalLogin',
    'lt':'',
    'execution': execution,
}`


|  表头   | 表头  |
|  ----  | ----  |
| 单元格  | 单元格 |
| 单元格  | 单元格 |

