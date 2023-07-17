# 代码优化

### 一、提高可维护性

示例：页面的path、title数据统一管理、使用

```javascript
// 页面的path、title、权限等数据，统一管理 src/utils/routeConfig.js
let ROUTE_CONFIG = {
  "create_user":{
  	"path":"/users/creare/",
  	"title":"用户创建"，
  	"loginRequired": true,
    "permsRequired": true,
  	"permission": ["create_user"],
  	"permission_relation": "and",
  }
}

// 规范写法：
import {ROUTE_CONFIG,} from '@/utils/routeConfig.js';
this.props.history.push(ROUTE_CONFIG.create_user.path)
this.props.history.push(`${ROUTE_CONFIG.user_detail_edit.path}?user_id=${res.data.id}`)

// 不规范写法：将path写死在代码中
this.props.history.push(`/users/creare/`)
this.props.history.push(`/users/detail/?user_id=${item.id}`)
```

### 二、提高代码性能

代码的实现中，要通过考虑方案选择、算法时间复杂度等方面提升代码性能。

示例：频繁判断某些元素是否在一个数组中。应避免时间复杂度O(n)，采用时间复杂度O(1)。

```javascript
let member = {'id':1}
let members = [{'id':1},{'id':2},{'id':3},{'id':4},{'id':1000}];
// 遍历 时间复杂度O(n)
let inMembers1= members.some((item)=>{return item.id===member.id})

// 构建对象，查询key的存在。时间复杂度O(1)
let memberDict = {};
for(let item of members){
    memberDict[item.id]=item
};
let inMembers2= memberDict.hasOwnProperty(member.id)
```