# API的Response响应码规范

## 请求体

### 响应体

响应参数

| 参数名称    | 是否必选 | 参数类型            | 默认值 | 说明                                           |
| ----------- | -------- | ------------------- | ------ | ---------------------------------------------- |
| result      | 是       | Boolean(true/false) |        | true或false 一般在响应码200 为true             |
| result_code | 是       | String(1-32)        | 无     | 请求返回的结果码。                             |
| result_desc | 是       | String(1-128)       | 无     | 请求返回的结果描述。                           |
| data        | 否       | Object              |        | 响应码200 result为 true时 有效的数据放在data中 |

### 结果码

```
RESPONSE_STATUS = [200, 400, 401, 403, 404, 500]
RESULT_CODE_DICT = {
    200: {
        '0': '成功',   
         },    
    400: {
        '10000': '请求参数无效',        
        '10001': '资源存在，无需重复创建',        
        '10002': '参数缺失',    
        },    
    401: {
        '10100': '用户需要登录：Token无效或缺失',        
        '10101': '用户冻结',        
        '10102': 'Token无效',        
        '10103': 'Token缺失',        
        '10104': 'Authentication Key无效',        
        '10105': 'Token过期'    
        },    
    403: {
        '10300': '没有权限访问',        
        '10301': '权限密钥无效',    
        },    
    404: {
        '10400': '所请求的资源不存在'    
        },    
    500: {
        '10500': '系统错误'    
        }
}
```

### 接口示例

```
Example Request
POST /api/tracker/apps?search_value=Farm HTTP/1.1 
Date:Wed, 28 Dec 2020 01:56:46 GMT 
Host:service.example.com 
Accept:*/* 
Content-Type:application/json; charset=UTF-8 

Example Response
200 － OK
{   "result": true,
    "result_code": "0",
    "result_desc": "成功",
    "data": [
        {
            "id": 1,
            "user": {
                "phone_number": null,
                "company": null,
                "id": 1,
                "username": "lifanping"
            },
            "key": "gtNone4u1iy3Af5oOl1usBndtQbp",
            "secret": "1bd7a8d55fe745f2b9c58f66c90004a7",
            "name": "Farm测试开发",
            "created_at": "2019-02-22 11:22"
        }
    ]
}
```

代码示意

```
# Response Example (来自python后端)
response = Response(
        {
            'result': True, 
            'result_code': '0', 
            'result_text ': message, 
            'data': data
        }, 
        status=HTTP_200_OK
     )
```