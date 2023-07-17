# 前端代码规范

宗旨：做好代码规范、提高代码质量。增强项目代码的**可读性**、**可维护性**、**可变更性**。

HTML、CSS、JS

## 一、文件资源

### 1、结构

js、css、html、images、fonts等目录结构要清晰。

js、css与HTML分离。

### 2、命名

* 文件名不得含有空格
* 文件名建议只使用小写字母( 某些说明文件的文件名，可使用大写字母，比如README、LICENSE等 )
* 文件名语义清晰、推荐使用语义清晰的英文单词、避免使用拼音与英文混合方式、避免使用拼音首字母。
* 文件名包含多个单词时，单词之间建议使用中划线 (-) 分隔

```
规范示例：
proposals/images/status-waiting-icon.svg 等待认领
proposals/images/status-ongoing-icon.svg 进行中
proposals/images/status-bizopp-icon.svg 商机

不规范： 除了quip可读 其他很难猜出来是什么、很难复用。
proposals/images/td-i-cdjj.svg
proposals/images/td-i-ddgt.svg
proposals/images/td-i-ddrl.svg
```

## 二、HTML&CSS

### 1、HTML结构

* 基本结构

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
</html>
```

* css 引用置于头部head标签内、js引用置于底部body标签前
* 引入资源使用相对路径、忽略（Omit）协议 ( http:,https: ) 【除非必须使用某个协议】
* 标签属性推荐顺序：
    * class （class是为高可复用组件设计的）
    * id、name （id更加具体且应尽量少使用）
    * data-
    * *src、for、type、href、value
    * *placeholder、title、alt
    * *aria-*、role
    * required、readonly、 disabled


### 2、命名规范

对class的理解：class是用来定义一类**高可复用**的样式

* 定义样式时考虑复用性、有复用意识
* `class` 必须单词全字母小写，单词间以 `-` 分隔、语义明确，命名不可出现a1，b1。
* `class` 必须代表相应模块或部件的**内容或功能**，**不得以样式信息进行命名**。
*  `id` 必须保证页面唯一、`id` 建议单词全字母小写，单词间以 `-` 分隔。
* `id`、`class` 命名，在避免冲突并描述清楚的前提下尽可能短
* 不允许 `class` 只用于让 JavaScript 选择某些元素，`class` 应该具有明确的语义和样式
* 使用sass或less时，标签嵌套层数应进行控制，以免权值过高后期修改权值不够

规范命名示例：

```
//规范示例：
<section class="content">
    <p class="content-title">所有报告</p>
    <div id = "report-content">
        <div class="loading-content">
            <i class="loading-icon loading"></i>
        </div>
    </div>
</section>

//不规范： 
//以样式信息进行命名的class难以复用、容易命名泛滥、难以维护
//这种命名规则：td-width-1 - td-width-∞  便有无穷个class

.td-width-88{
    padding-left: 18px;
    width: 88px;
}
.td-paddind-22{
    padding-left: 22px;
}
.td-paddind-18{
    padding-left: 18px;
}
```

不规范命名：

```
语义不详、class难以复用、容易命名泛滥、难以维护。
其表格table更难以复用 表格列数是一个变量 
这种命名规则：lead-th-1 - lead-th-∞  便有无穷个class

表格：
全部线索、我的线索、全部需求、我的需求、全部项目
现在在采用的是统一的表格样式
可以定义一套可复用的 表格样式class表格保持统一样式
表格的内容基本包含时间、状态、靠谱度、名称、人员、任务 
可定义一类可复用的一类class
比如 table-td-time、 table-td-member、table-td-name、table-td-task、table-td-task
代表相应模块或部件的**内容或功能 具有明确的语义和样式
```

### 3、css规范

## 三、JS

### 1、命名规范

* 变量名、函数名 建议采用小驼峰式命名且要求语义明确、严禁使用拼音与英文混合，错别字英文、更不允许中文。 

```
//规范示例：
var userName='李帆平'
function commonRequest(){
}
class ProposalList = ...
//不规范：错别字英文、拼音与英文混合
var propposalList=[];
function tijiaoForm(){
}
```

* 类名 采用大驼峰式命名且要求语义明确、严禁使用拼音与英文混合，错别字英文、更不允许中文。
* `常量` 使用 `全部字母大写，单词间下划线分隔`

```
var HTML_ENTITY = {};
```

### 2、注意事项

* 谨慎使用全局变量，将变量限制于一个个模块之中，使得 JavaScript 代码变得更有条理且更便于维护。
* 必须使用英文分号 ;  结尾。
* 不使用for in使用遍历array，应用在遍历object。

```
var arr = [1,2,3,4,5];
var obj = {
						'name': 'Tom',
         		'age' : 30   
        	}
for(var i=0;i<arr.length;i++)
for(var key in obj)
```

* 代码简洁、提高代码复用性、使用三目和短路减少代码数量、注重性能。
* 方法和类添加必要的注释。

### 3、module管理

* package.json中依赖包指定**固定版本**
* package.json中清除无效的包
