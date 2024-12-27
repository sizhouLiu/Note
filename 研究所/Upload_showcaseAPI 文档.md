## Upload_showcaseAPI 文档



### 上传数据

#### 请求接口:

#### **POST** 

Upload/upload/

#### 描述:

此端点允许用户上传数据，包括用户信息、文本内容和图像。

通过session获取patient_phone来上传数据和获取数据

#### 请求参数:

- `name` (字符串): 用户姓名。
- `content` (字符串): 文本内容。
- `Upload_file` (文件): 要上传的图像文件。

#### 请求示例:

```json
jsonCopy code{
    "name": "John Doe",
    "content": "示例内容文本"
    // 图像文件以表单数据的形式包含
}
```

#### 响应格式:

- 成功 (HTTP 200):

  ```
  jsonCopy code{
      "file": "image_filename.jpg",
      "code": 200,
      "msg": "添加成功"
  }
  ```

- 失败 (HTTP 201):

  ```
  jsonCopy code{
      "file": "",
      "code": 201,
      "msg": "添加失败"
  }
  ```

### 获取用户数据

#### 接口:

POST

/Upload/get_data/

#### 描述:

此端点检索特定用户的用户数据，包括用户信息、文本内容和图像。

#### 请求参数:

无

#### 响应格式:

- 成功 (HTTP 200):

  ```
  jsonCopy code{
      "data": [
          {
              "userid": "user123",
              "name": "John Doe",
              "content": "示例内容文本",
              "image": "base64编码的图像数据"
          },
          // 如果有的话，还会有同一用户的其他数据条目
      ]
  }
  ```

#### 示例响应:

```
jsonCopy code{
    "data": [
        {
            "userid": "user123",
            "name": "John Doe",
            "content": "示例内容文本",
            "image": "base64编码的图像数据"
        },
        {
            "userid": "user123",
            "name": "John Doe",
            "content": "另一个内容条目",
            "image": "base64编码的图像数据"
        }
    ]
}
```

