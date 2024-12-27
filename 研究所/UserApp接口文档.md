# UserApp接口文档

## **login接口**

- URL：/login
- 请求方法：GET、POST
- 功能：用户登录验证
- 请求参数：
  - GET请求：无
  - POST请求：
    - role: 用户角色（管理员/医生）
    - username: 用户名
    - password: 密码
- 响应数据：
  - GET请求：
    - status: 状态码（200）
    - msg: 消息（ok）
  - POST请求：
    - msg: 错误消息（如果有错误）
    - status: 状态码（200，登录成功；其他，登录失败）
    - uid: 用户ID（如果登录成功）
- 示例：
  - GET请求：/login
  - POST请求：/login
    - role: 管理员
    - username: admin
    - password: 123456

## **administrator接口**

- URL：/administrator
- 请求方法：GET
- 功能：获取医生信息列表
- 请求参数：无
- 响应数据：
  - doctors: 医生信息（JSON数组）
  - status: 状态码（200）
- 示例：
  - GET请求：/administrator

## **logout接口**

- URL：/logout
- 请求方法：GET
- 功能：用户退出登录
- 请求参数：无
- 响应数据：
  - status: 状态码（200）
  - msg: 消息（logout successful）
- 示例：
  - GET请求：/logout

## **add_doctor接口**

- URL：/add_doctor
- 请求方法：GET、POST
- 功能：添加医生信息
- 请求参数：
  - GET请求：无
  - POST请求：
    - name: 医生姓名
    - number: 医生编号
    - password: 医生密码
    - phone: 医生电话
    - sex: 医生性别
    - age: 医生年龄
- 响应数据：
  - GET请求：
    - status: 状态码（200）
    - msg: 消息（successful）
  - POST请求：
    - status: 状态码（200）
    - msg: 消息（successful）
    - doctorid: 医生ID
- 示例：
  - GET请求：/add_doctor
  - POST请求：/add_doctor
    - name: 张三
    - number: 001
    - password: 123456
    - phone: 123456789
    - sex: 男
    - age: 30

## **change接口**

- URL：/change
- 请求方法：GET、POST
- 功能：修改医生信息
- 请求参数：
  - GET请求：
    - phone: 医生电话
  - POST请求：
    - name: 医生姓名
    - number: 医生编号
    - password: 医生密码
    - phone: 医生电话
    - sex: 医生性别
    - age: 医生年龄
- 响应数据：
  - GET请求：
    - status: 状态码（200）
    - msg: 消息（successful/error）
    - doctor: 医生信息（姓名、电话、编号、密码）
  - POST请求：
    - status: 状态码（200）
    - msg: 消息（successful）
- 示例：
  - GET请求：/change?phone=123456789
  - POST请求：/change
    - name: 张三
    - number: 001
    - password: 123456
    - phone: 123456789
    - sex: 男
    - age: 30

## **delete接口**

- URL：/delete
- 请求方法：POST
- 功能：删除医生信息
- 请求参数：
  - number: 医生编号
- 响应数据：
  - status: 状态码（200）
  - msg: 消息（delete successful/error）
- 示例：
  - POST请求：/delete
    - number: 001

## **openid接口**

- URL：/openid
- 请求方法：POST
- 功能：微信登录获取openid和session_key
- 请求参数：
  - code: 微信登录凭证
- 响应数据：
  - code: 状态码（200）
  - openid: 用户唯一标识
  - session_key: 会话密钥
  - meg: 消息（successful）
- 示例：
  - POST请求：/openid
    - code: wxlogin123456

## **token接口**

- URL：/token
- 请求方法：POST
- 功能：发放token
- 请求参数：
  - openid: 用户唯一标识
  - nickname: 用户昵称
  - avatar_url: 用户头像URL
  - gender: 用户性别
- 响应数据：
  - status: 状态（success）
  - nickname: 用户昵称
  - user_id: 用户ID
  - token: 访问令牌
- 示例：
  - POST请求：/token
    - openid: wxuser123456
    - nickname: 张三
    - avatar_url: https://example.com/avatar.jpg
    - gender: 男

## **make_token函数**

- 功能：创建访问令牌
- 参数：
  - nikname: 用户昵称
  - expire: 令牌过期时间（默认为3600 * 24秒）
- 返回值：访问令牌
- 示例：
  - make_token('张三')