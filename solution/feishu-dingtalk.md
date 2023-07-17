# 飞书和钉钉开发

#### 目的

让大家了解飞书和钉钉开发的能力，帮助大家做技术咨询。

通过飞书和钉钉的案例，帮助大家理解此类产品的设计原理，看到类似需求或技术时能触类旁通。

## 一、应用开发

### 飞书应用开发

#### 

##### 参考资料

飞书开发平台：https://open.feishu.cn/    

飞书开发文档：https://open.feishu.cn/document/

##### 应用类型

* 企业自建应用
  * 类型：小程序、网页、机器人
  * 开发/使用：企业内部人员
* 应用商店应用
  * 类型：小程序、机器人
  * 开发/使用：服务商/飞书用户

##### 应用类型选择

企业自建应用 ：公司内容人员使用

应用商店应用：飞书用户使用

飞书应用：https://open.feishu.cn/app
![](https://leetan.oss-cn-beijing.aliyuncs.com/code/feishu-app.png)

##### 企业自建应用

可以选择开启机器人、小程序、网页

###### 机器人

1、机器人可以向用户、群发送消息

* 通过API获取当前应用中所有用户
* 通过用户的ID

2、机器人可以接受私聊、群里@并作出相应操作

* 配置消息卡片请求网址
* 订阅 接收消息 事件
* 应用机器人接收私聊消息与群里中@的消息时，开放平台推送 message 消息到请求网址，请求网址做成不同响应

###### 小程序

类似微信小程序

###### 网页

配置桌面端主页、移动端主页

在飞书客户端中点击应用后会打开该主页

------



### 钉钉应用开发

![](https://leetan.oss-cn-beijing.aliyuncs.com/code/dd-app.png)

#### 企业内部应用

* 应用类型
  * 小程序
  * H5
* 案例
  * Farm

#### 第三方企业应用

* 应用类型
  * 小程序
  * H5
* 特性
  * 上架到应用商店
  * 运上部署
* 案例
  * 标准OA类产品

#### 第三方个人应用

* 工具类为主
* 不提供服务端API

#### 移动应用

* 开发平台，和微信类似

## 二、消息推送

### 飞书

* 个人推送
  * 应用机器人(聊天机器人)消息推送
* 群消息

  * 通知机器人
  * 聊天机器人
  * 飞书机器人开发文档：https://open.feishu.cn/document/uQjL04CN/uYTMuYTMuYTM

### 钉钉（企业内部/第三方企业）

1. 工作通知消息：是以企业工作通知会话中某个微应用的名义推送到员工的通知消息，例如生日祝福、入职提醒等。
2. 普通消息：是指员工个人在使用应用时，可以通过界面操作的方式往群或其他人的会话里推送消息，例如发送日志的场景。
3. 任务类通知：是指需要发送一条任务提醒给员工，比如审批任务等，这类情况下可参考[待办任务案例](https://ding-doc.dingtalk.com/doc#/serverapi3/sp7iyx)。

## 三、第三方登录

### OAuth 2.0的四种授权方式

* 授权码模式（authorization code）
* 密码模式（resource owner password credentials）
  * 使用用户名/密码作为授权方式从授权服务器上获取令牌  
  * 不推荐
* 简化模式（implicit）
  * 少了code环节
  * 需要安全性考虑
* 客户端模式（client credentials）
  * 客户端向认证服务器验证身份来获取令牌（无需用户参与）

### 授权码模式

授权码模式（authorization code）是功能最完整、流程最严密的正宗的oauth2的授权模式

1、获取预授权码 code    （一般有有效期如5 分钟的临时授权码） 客户端

2、通过预授权码 code获取access_token     服务端

一般两个小时有效期   可以通过 refresh_token 更新

3、通过access_token获取用户openid及其他用户信息    服务端

4、用户通过 access_token、openid、api请求资源
![](https://leetan.oss-cn-beijing.aliyuncs.com/code/oauth2.png)

#### 预授权码获取方式

方式一：调用开放平台提供的登录API获取预授权码 （飞书：小程序  `tt.login     钉钉：网页 dd.ready`）

方式二：通过重定向url获取预授权码   

* 重定向url需要配置在第三方服务中
* 跳转到第三方时 除了参数redirect_uri 、app_id一些必要参数   不同第三方服务还需要一些不同的额外参数

```
https://open.feishu.cn/open-apis/authen/v1/index?redirect_uri={REDIRECT_URI}&app_id={APPID}&state={STATE}
https://git.chilunyc.com/oauth/authorize?response_type=code&client_id={client_id}&redirect_uri={redirect_uri}&scope=read_user+api
```

飞书参考链接：https://open.feishu.cn/document/uYjL24iN/ukTO4UjL5kDO14SO5gTN