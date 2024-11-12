![image](https://github.com/user-attachments/assets/04226652-3881-4bbd-8177-035ef288d8f4)
统一登录地址：https://i.nuist.edu.cn/



**登录流程分析**
=================

**1.前往登录页面获取，才用session获得获取验证码所需的cookie**

请求 URL:`https://authserver.nuist.edu.cn/authserver/login?service=https%3A%2F%2Fi.nuist.edu.cn%2Flogin%23%2F`

请求方法:`GET`

返回值为html源文件

![img_2](https://github.com/user-attachments/assets/b13beb0d-c16b-41f7-849c-0b3c7063f63d)


从中获取两个隐藏信息execution，pwdEncryptSalt


这里的pwdEncryptSalt是与输入的密码一起加密，获得登录的加密密码，execution是登录请求字典中所携带





**2.获取验证码**

利用上一步session构建的cookie去获取验证码，采用OCR识别验证码，验证码的请求中包含一个时间戳
![img_3](https://github.com/user-attachments/assets/f9e882b5-98d2-4ab9-9bad-cdb122106664)


**3. 发起登录请求**

通过断点发现，密码saltPassword和前面的pwdEncryptSalt一起被传入到encryptPassword函数进行加密

`$("#saltPassword").val(encryptPassword($(LOGIN_PASSWORD_ID).val(), $("#pwdEncryptSalt").val()));`

![img_4](https://github.com/user-attachments/assets/80a4cf09-1b42-4591-a2d6-62fb3012f736)

![img_5](https://github.com/user-attachments/assets/69b4937b-8cae-4a1b-9726-be5fcb5fd1a9)


直接把整个js文件复制下来，进行密码的加密


请求 URL:`https://authserver.nuist.edu.cn/authserver/login?service=https%3A%2F%2Fi.nuist.edu.cn%2Flogin%23%2F`
请求方法:`POST`

请求字典如下，分别对应账号，加密后的密码，验证码

`{
    'username': '2024XXXXXXXXX',
    'password': encrypted_password,
    'captcha': result,
    '_eventId': 'submit',
    'cllt': 'userNameLogin',
    'dllt': 'generalLogin',
    'lt':'',
    'execution': execution,
}`

| 关键字       | 信息          |
|-----------|-------------|
| username  | 学号          |
| password  | 加密后的密码      |
| captcha   | 验证码         |
| execution | 首次访问获取的加密信息 |






