# 微信小程序开发规范参考

目的： 结构清晰、代码规范、提高代码质量。增强项目代码的**可读性**、**可维护性**、**可变更性**。 

## 开发

框架选择：框架只允许使用官方的Mina、 WePY以及京东的Taro

## 目录

目录结构清晰：基本文件、组件、自定义func、api统一管理、request的封装

基本结构：

```
├── app.js
├── app.json
├── app.wxss
├── pages
│   │── index
│   │   ├── index.wxml
│   │   ├── index.js
│   │   ├── index.json
│   │   └── index.wxss
│   └── logs
│       ├── logs.wxml
│       └── logs.js
└── utils
    
```

自定义目录：

组件：components

自定义func：utils

api统一管理js：APIS.js

request的封装js：request.js

配置项

README

## 配置项

建议：所有服务器、第三方等配置信息都应该独立维护一个config.js文件来管理 XXX

目的：

```
// config.js内容
module.exports = {
    API_HOST : 'https://xxxxx.xxx.dev.aks.chilunyc.com'
    STATIC_WEB_HOST : 'https://xxxxx.xxx.dev.aks.chilunyc.com'
}

// 使用配置信息的地方
var config = require('./config.js');
var apiHost = config.API_HOST;
```

## README

使用的小程序插件

​		写明使用到的小程序插件，这些插件需要从小程序管理台开启，如果未开启，执行提交代码的时候会抛出异常

构建方式

​		如果使用了第三方组件库或者第三方框架，请写明安装依赖如何构建等信息

## API路径统一管理

建议：api不要直接写在每个页面、或组件里，在一个统一的文件管理

目的：

## API Request封装

建议： 封装request：在request中请求接口 返回数据； 在页面js、html中只负责对接口返回数据做处理、展示

目的： 接口统一管理；页面js、html中只负责对接口返回的数据做处理和展示；便于后期UI改版只改页面js中数据处理部分、html展示部分

### request.js 

```
import config from '../config';
const request = function (url, params, method) {
    let user = wx.getStorageSync('user');
    let header = {};
    if (user && user.access_token) {
        header = {
            'Authorization': user ? 'Bearer ' + user.access_token : '',
            'content-type': 'application/json' // 默认值
        }
    } else {
        header = {
            'content-type': 'application/json' // 默认值
        }
    }
    return new Promise((resolve, reject) => {
        wx.request({
            url: config.API_HOST + url, //仅为示例，并非真实的接口地址
            data: params,
            method: method,
            header,
            success(res) {

                if (res.statusCode == 401) {
                    reject(res.data)
                } else if (res.statusCode == 200 || res.statusCode == 204) {
                    resolve(res.data)
                } else {
                    reject(res.data)
                }
            },
            fail(err) {
                reject(err.data)
            }
        })
    });
}
export default request;
```

### api单独目录

每个API文件可以使用对应服务为名 

例：apis/UploadRecordService.js

```
import request from '../utils/request'
//对应upload-record-service服务下的所有接口
const UploadRecordService = {
    getUploadRecords(params={},method='get') {
        let res = request('/services/upload-record-service/api/upload-records/timeline/year',params,method)
        //根据不同的method  可以做一些统一初步处理 
        return res
    }
}
export default UploadRecordService;
```

代码中使用

```
import UploadRecordService from '../../apis/UploadRecordService'
UploadRecordService.getUploadRecords({timeStr: "2020"},'get').then((res)=>{
    console.log(res);
}).catch((err)=>{
    console.log(err);
})
```

### 页面js中将API层返回的数据以数据模型的形式固定下来

## 页面注意项

js中data数据结构清晰、初始数据完整 

```
在js中setData会设置的字段；在页面会用的字段；都在页面的data中定义一下
```
