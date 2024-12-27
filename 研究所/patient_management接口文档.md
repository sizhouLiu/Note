# patient_management接口文档

## countArea接口

- 请求方法：GET
- 请求参数：无
- 功能：根据病人id，获取病人的病灶数据、预测数据和病人信息
- 返回数据：
  - 'bingzao': 病灶数据列表
  - 'yuce_list_lung': 预测数据列表（肺部）
  - 'yuce_list_fengwofuse': 预测数据列表（风沃脂肪）
  - 'yuce_list_mobolifuse': 预测数据列表（摩尔脂肪）
  - 'yuce_list_feidapaofuse': 预测数据列表（肺大泡）
  - 'yuce_list_wanggefuse': 预测数据列表（网格脂肪）
  - 'yuce_list_shibianfuse': 预测数据列表（湿病脂肪）
  - 'code': 状态码
  - 'name': 病人姓名
  - 'sex': 病人性别
  - 'phone': 病人电话
  - 'age': 病人年龄
  - 'shengao': 病人身高

## turn接口

- 请求方法：GET
- 请求参数：id（病人id）
- 功能：存储病人id到session中
- 返回数据：
  - 'status': 状态码
  - 'msg': 提示信息

## loading接口

- 请求方法：GET
- 请求参数：无
- 功能：根据病人id和session中的参数，获取病人的CT图像数据、预测数据和病人信息
- 返回数据：
  - 'status': 状态码
  - 'msg': 提示信息
  - 'bingqingjianyi': 病情分析和建议数据
  - 'patient_message': 病人信息数据
  - 'zhexian_data': 折线图数据
  - 'zhexianyijian': 折线图分析结果
  - 'tupian': 图片数据
  - 'zhibiao': 指标数据
  - 'yishengjianyi': 医生建议数据

## liebiao接口

功能：根据患者ID、时间和文件类型返回图像文件列表
输入参数：

- patient_id：患者ID（字符串）
- timess：时间（整数）
- ziwenjian：文件类型（字符串）
  输出参数：
- img_list：图像文件列表（字符串列表）

## img_liebiao接口

功能：根据患者ID、时间、文件类型和选择返回图像文件列表
输入参数：

- patient_id：患者ID（字符串）
- timess：时间（整数）
- ziwenjian：文件类型（字符串）
- choose：选择（整数，0表示选择有预测结果的图像，1表示选择没有预测结果的图像）
  输出参数：
- img_list：图像文件列表（字符串列表）

## jianyi接口

功能：根据数据列表判断趋势和波动程度
输入参数：

- shuju：数据列表（浮点数列表）
  输出参数：
- target：趋势（字符串，可能的值为"平稳"、"上升"、"下降"）
- target_1：波动程度（字符串，可能的值为"波动较小"、"波动较大"）

## bingqing接口

功能：根据肌肉、皮下脂肪、内脏脂肪和性别判断病情
输入参数：

- jirou：肌肉（浮点数）
- pixia：皮下脂肪（浮点数）
- neizang：内脏脂肪（浮点数）
- sex：性别（字符串，可能的值为"男"、"女"）
  输出参数：
- jirou_msg：肌肉状况（字符串）
- pixia_msg：皮下脂肪状况（字符串）
- neizang_msg：内脏脂肪状况（字符串）

## patient_view接口

- 请求URL：/manage/
- 请求方法：GET
- 请求参数：无
- 返回示例：
```
{
    "status": 200,
    "mess": [
        {
            "id": 1,
            "name": "John",
            "sex": "Male",
            "age": 30,
            "phone": "1234567890",
            "size": 1.88
        },
        {
            "id": 2,
            "name": "Jane",
            "sex": "Female",
            "age": 25,
            "phone": "0987654321",
            "size": 1.65
        }
    ]
}
```

## patient_view_wx接口

- 请求URL：/wx_manage/
- 请求方法：POST
- 请求参数：
```
{
    "openid": "xxxxxxxxxxxx"
}
```
- 返回示例：
```
{
    "status": 200,
    "mess": [
        {
            "id": 1,
            "name": "John",
            "sex": "Male",
            "age": 30,
            "phone": "1234567890",
            "size": 1.88
        },
        {
            "id": 2,
            "name": "Jane",
            "sex": "Female",
            "age": 25,
            "phone": "0987654321",
            "size": 1.65
        }
    ]
}
```

## add_wx_patient接口

- 请求URL：/add_wx_patient
- 请求方法：POST
- 请求参数：
```
{
    "name": "John",
    "sex": "Male",
    "age": 30,
    "phone": "1234567890",
    "size": 1.88,
    "wx_user": "xxxxxxxxxxxx"
}
```
- 返回示例：
```
{
    "status": 200,
    "msg": "manage successful"
}
```

## del_view接口

- 请求URL：/del_view
- 请求方法：GET
- 请求参数：
```
id=1
```
- 返回示例：
```
{
    "status": 200,
    "msg": "delete success"
}
```

## upd_view接口

- 请求URL：/upd_view/1
- 请求方法：GET
- 返回示例：
```
{
    "status": 200,
    "msg": "successful",
    "patient": [
        1,
        "John",
        "Male",
        30,
        "1234567890",
        1.88
    ]
}
```
- 请求URL：/upd_view/1
- 请求方法：POST
- 请求参数：
```
{
    "name": "John",
    "sex": "Male",
    "age": 30,
    "phone": "1234567890",
    "size": 1.88
}
```
- 返回示例：
```
{
    "status": 200,
    "msg": "change successful"
}
```

## patient_csv接口

- 请求URL：/csv
- 请求方法：GET
- 请求参数：无
- 返回示例：
```
{
    "status": 200,
    "msg": "successful",
    "img_src": "/media/CT/1622345678_patient.csv"
}
```

## manage_information接口

- 请求URL：/manage_information
- 请求方法：GET
- 请求参数：无
- 返回示例：
```
{
    "status": 200,
    "msg": "successful"
}
```
- 请求URL：/manage_information
- 请求方法：POST
- 请求参数：
```
{
    "name": "John",
    "sex": "Male",
    "age": 30,
    "phone": "1234567890",
    "size": 1.88
}
```
- 返回示例：
```
{
    "status": 200,
    "msg": "manage successful",
    "patient_id": 1
}
```

## update接口

- 请求URL：/jianyi_update
- 请求方法：POST
- 请求参数：
```
{
    "id": 1,
    "content": "请对病人当前身体状况进行建议"
}
```
- 返回示例：
```
{
    "code": 200,
    "msg": "提交建议成功"
}
```