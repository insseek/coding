# 内部文档编辑器

### 一、开源编辑器的选择：Quill

#### 标准

1、开源环境良好：Github star、 Latest commit、Documentation

2、模块化架构：易于扩展自定义组件

3、DOM数据模型化：便于内容管理、易于实时协作编辑

4、风格现代化

#### Quill

官网：https://quilljs.com/

Github地址：https://github.com/quilljs/quill

英文文档：https://quilljs.com/docs/quickstart/

中文文档：Gitbook地址 ：https://bingkui.gitbooks.io/quill/content/

Quill相关一些组件库和module：https://github.com/quilljs/awesome-quill （组件可直接使用、二次开发或模仿开发新组件）

### 二、组件的自定义

模块化架构 易于扩展自定义组件：Blots、 Formats、Modules

Blots是Parchment文档的基本组成部分。提供了几个基本的实现，如：Block、Inline和Embed

Formats：格式化

Modules：组件

#### 示例：项目框架图、脑图、评论

```javascript
    //1、定义一个Blots：GearImageBox
    const Block = Quill.import("blots/block");
    const Image = Quill.import("formats/image");

    class GearImageBox extends Block {
        static create(options) {
            let node = super.create(options);
            // node.setAttribute('class', 'image-container');
            let image = document.createElement("img");
            ATTRIBUTES.forEach(function (attribute) {
                if (options[attribute]) {
                    image.setAttribute(attribute, options[attribute]);
                }
            });
            if (!image.hasAttribute('class')) {
                image.setAttribute('class', 'gear-all-image');
            }
            if (!image.hasAttribute('flag')) {
                image.setAttribute('flag', 'general');
            }
            for (let key in options) {
                if (key == 'class') {
                    options[key] = options[key] + ' normal-image'
                }
                image.setAttribute(key, options[key]);
            }
            node.appendChild(image);

            return node;
        }

        static value(node) {
            let image = node.querySelector('img')
            var values = {};
            ATTRIBUTES.forEach(function (attribute) {
                if (image.hasAttribute(attribute)) {
                    values[attribute] = image.getAttribute(attribute)
                }
            });
            return values
        }
    }
     //插入一堆
     this.quill.insertEmbed(this.quillIndex.index, 'gear-image', {
                            src: res.data.file_url,
                            class: 'gear-all-image',
                            flag: this.imgFlag,
     });
    
    //2、定义一个Formats 样式
    let Inline = Quill.import('blots/inline');
    class CommentBlot extends Inline {
        static create(options) {
            let node = super.create(options);
            node.setAttribute('class', options['class']);
            node.setAttribute('comment_uid', options['comment_uid']);
            return node;
        }
        static formats(node) {
            let data = {
                'class':node.getAttribute('class'),
                'comment_uid':node.getAttribute('comment_uid'),
            };
            return data;
        }
    }
    CommentBlot.blotName = "comment";
    CommentBlot.tagName = "annotation";
    Quill.register({
        'formats/comment': CommentBlot
    });

    //2、定义一个Modules 模块
    class InlineComment{
        //...
        
        //点击评论的事件
        commentEventHanlder() {
            let that = this;

            let range = {};
            let text = '';
            let atSignBounds = '';

            //选中文字时对选中的评论
            if(that.quill.getSelection().length>0){
                range = that.quill.getSelection();
                text = that.quill.getText(range.index, range.length);
                atSignBounds = that.quill.getBounds(range.index+range.length);
            }else{
                //对当前行评论
                // 当前行开头索引与行的长度
                let index = that.quill.getSelection().index;
                let [line, offset] = that.quill.getLine(index);
                /*// 获取行首的index  获取行的长度
                let startIndex = quill.getIndex(line);
                let length = line.length()-1;*/
                let startIndex = index - offset;
                let length = line.cache.length - 1;
                //将这行选中
                that.quill.setSelection(startIndex,length);
                range = {
                    index: startIndex,
                    length: length
                };
                text = that.quill.getText(range.index, range.length);
                atSignBounds = that.quill.getBounds(range.index+range.length);
            }
            that.range = range;
            //创建一个评论点uid
            that.getUid().then((res)=>{
                return that.getComments(res.uid); //根据uid获取评论列表
            }).then((res)=>{

                let editor_comment = {
                    class : 'annotation tag-select-yellow active',
                    comment_uid : res.uid,
                };
                that.quill.format('comment', editor_comment);
            })
        }
        
        //...
    }
   
    Quill.register('modules/inline_comment', InlineComment);
```

### 三、评论

选中文字评论、段落评论、图片评论

* 全局评论  （针对于一个文档）
* 局部评论  （针对于一个文档内区块）
    * 文本
        * 复制一个文本区块，若有评论 复制评论
    * 图片

### 三、操作记录

#### 结构化操作记录

1、在后台构造一个json数据结构

2、对日志记录类型分类、对具体操作类型分类、对特殊字段提供特殊的数据格式

3、前端根据日志分类、操作类型，以及不同字段数据自定义展示样式

操作记录的完整数据结构


```python
# 定义操作
# 日志记录的分类:增、删 改
TYPES = ["create", "delete", "update"]
# 详细操作的类型:保持、新增、删除、修改 (展示的时候可以用颜色标识：新增：绿色；删除：红色+中划线 ）
# 对于非文本字段 比如：图片、文件、表格；提供特殊的数据格式
OPS_TYPES = ["retain", "insert", "delete", "update"]
示例
create_log ={    
                "type": "create",
                "model_name": "report",
                "title": "新建报告",
                "subtitle": "ABB变频器监控平台反馈报告"
                "comment": "内容克隆自ABB变频器监控平台反馈报告(李帆平 v1.2 2019-06-04 11:31)",
                "fields": [], 
            }
update_log = {
        "type": "update",
        "model_name":"project",
        "title": "项目",
        "subtitle": "最绅士",
        "message": null,
        "fields": [
            {
                "type": "update",
                "name": "title",
                "verbose_name": "项目名称",
                "old_value": "绅绅士",
                "new_value": "最绅士",
            },
            {
                "type": "update",
                "name": "status",
                "verbose_name": "状态",
                "old_value": "开发",
                "new_value": "测试",
            },
            ]
       }
```

#### 特殊字段的特殊处理：

```python
# 报告操作记录示例
update_log = {
        "type": "update",
        "model_name":"report",
        "title": "报告",
        "subtitle": "最绅士反馈报告",
        "message": "我是备注",
        "fields": [
            {
                "type": "update",
                "name": "title",
                "verbose_name": "报告名称",
                "old_value": "绅绅士反馈报告",
                "new_value": "最绅士反馈报告",
            },
            {
                "type": "insert",
                "name": "frame_diagram",
                "verbose_name": "项目框架图",
                "value": {"src": "/media/reports/files/5PpJ3908Sa7Ok2YcqiZS35.jpg","flag":"frame_diagram"},
            },
            {
                "type": "update",
                "name": "quotation_plan",
                "verbose_name": "报价方案",
                "old_value": {"title": "第一个报价方案",
                              "price": "10-15万",
                              "period": "10-12周",
                              "projects": "微信小程序+后端管理平台+H5",
                              "services": "PRD+设计+开发+部署", },
                "new_value": {"title": "第一个报价方案",
                              "price": "20-25万",
                              "period": "15-20周",
                              "projects": "微信小程序+后端管理平台+H5+安卓",
                              "services": "PRD+设计+开发+部署", },
            },
            {
                "type": "delete",
                "name": "quotation_plan",
                "verbose_name": "报价方案",
                "value": {"title": "第二个报价方案",
                              "price": "10-15万",
                              "period": "10-12周",
                              "projects": "微信小程序+后端管理平台+H5",
                              "services": "PRD+设计+开发+部署", }
            },
            {
                "type": "inser",
                "name": "quotation_plan",
                "verbose_name": "报价方案",
                "value": {"title": "第三个报价方案",
                              "price": "10-15万",
                              "period": "10-12周",
                              "projects": "微信小程序+后端管理平台+H5",
                              "services": "PRD+设计+开发+部署", }
            },
            {
                "type": "update",
                "name": "version_content",
                "verbose_name": "版本历史",
                "old_value": [
                    {
                        "version": "v1.1",
                        "author": "马传宗",
                        "date": "2019/5/12"
                    },
                ],
                "new_value": [
                    {
                        "version": "v1.1",
                        "author": "马传宗",
                        "date": "2019/5/12"
                    },
                    {
                        "version": "v1.2",
                        "author": "马传宗",
                        "date": "2019/6/12"
                    },
                ],
            },
        ]
    },
```

#### HTML内容的diff

对比两个版本内容的变化 输出对比结果    

结果中包含：文本内容 + 媒体标签

工具：https://github.com/google/diff-match-patch

1、进行清洗  ：评论 、 媒体标签等

2、用工具对文本diff 得到

```python
[(-1, "Hell"), (1, "G"), (0, "o"), (1, "odbye"), (0, " World.")]
# 1：新增, 0：保持 ,-1:删除
```

3、清洗diff结果：html标签、空白符号

4、特殊处理媒体内容的diff：img 

### 四、历史版本记录

编辑过程中每隔一段时间自动保存一个历史版本记录、可以选择某个历史版本记录，观看或者还原

历史版本记录的保存方式：自动保存

* 每隔一段时间(五分钟)自动保存一个历史版本记录
* 换人编辑
* 离开页面

### 五、多人协作

#### 1、简单方案（当前采用）

文档加锁。一次只能有一个人编辑。

#### 2、多人同时编辑方案（探索中）

需要解决两个技术点：实时通信问题、编辑冲突问题

理论：基版  -  实时交换diff -  更新基板 - 实时交换diff

现实：通信不能保证绝对实时、diff的冲突

##### 实时通信

第三方服务pusher：https://pusher.com/

```
import Pusher from  'pusher-js';
let pusher = new Pusher(pusher_app_key, {
                    cluster: pusher_cluster,
                    encrypted: true,
                });
const channel = pusher.subscribe(channel_prefix + '-developer-notifications');
channel.bind('new-notification', function (res) {
    if (res.message && res.message.receiver === that.state.developerId) {
        let noReadNoticeNum = that.state.noReadNoticeNum + 1;
        that.setState({
            isNewNotice: true,
            noReadNoticeNum,
        })
    }
});
```

##### 编辑冲突

解决方案：编辑锁、OT、diff-patch       

###### 编辑锁

编辑锁是实现协同编辑的一种简单的方法

* 文档锁 （有人在编辑某个文档时，系统会将这个文档锁定）
    * 最简单、无冲突
    * 一次只有一个人可以编辑文档
* 区块锁 （内容划分区块 一个区块同一时间只能有一个人编辑）
    * 每一个区块可分配一个UUID
    * 一个区块有人编辑时、对其他人设置不可编辑


```
多用户同时编辑：创建新区块、删除区块、编辑区块后，如何合并多用户的操作
1、diff-patch 用户编辑-与上一版本diff-将diff结果发给其它用户更新文档
2、OT 每个用户的行为定义添加、删除、修改 
```

* 行锁 （每一行作为一个区块 ）

###### OT技术

【quill文档text-change变动时 都会生成OT的json数据(delta)，也提供了updateContents方法】

Operational Transformation技术(简称 OT）

```
将文本内容修改转成以下3种类型的操作(Operational)：
retain(n)：保持 n 个字符，也就是说这 n 个字符不变
insert(str)：插入字符 str
delete(str)：删除字符 str
两个人的编辑之间进行操作转换、应用
```

https://github.com/marcelklehr/changesets

###### 类似Git的diff-patch

每个人可并行编辑，遇到编辑同一个文件时可以自动合并

```
1、每个用户进入编辑页面通过WebSocket建立长连接，并保存当前文档副本
2、有人编辑时，如果停顿3-5秒（待定），将现有文档和之前的副本进行diff，将结果传给服务端，更新副本
3、服务端更新文档，然后通过长连接将diff结果发给同时在编辑的其它用户，
   这些用户使用patch 方法来更新文档
```

https://github.com/wickedest/Mergely

https://github.com/google/diff-match-patch

具体方案：
1、在客户端中实现diff-patch两个算法

2、在服务端中实现diff-patch两个算法

理论上，通过加锁、实时交换diff、实时更新基板，就可以实现协作编辑。

## DOM数据模型化示例

```python
 {
            "ops": [
                {
                    "insert": "修改记录"
                },
                {
                    "attributes": {
                        "header": 2
                    },
                    "insert": "\n"
                },
                {
                    "insert": "\n"
                },
                {
                    "insert": "项目需求背景"
                },
                {
                    "insert": "\n",
                    "attributes": {
                        "header": 2
                    }
                },
                {
                    "insert": "大概会有6-8台无人机在空中飞行拍摄视频，然后传到视频直播服务器，视频直播服务器发送到pad上，其中pad上可以切换不同无人机的直播画面。",
                    "attributes": {
                        "color": "#212736"
                    }
                },
                {
                    "insert": "\n",
                    "attributes": {
                        "list": "bullet"
                    }
                },
                {
                    "insert": "项目框架图"
                },
                {
                    "insert": "\n",
                    "attributes": {
                        "header": 2
                    }
                },
                {
                    "insert": "\n"
                },
                {
                    "insert": {
                        "image": {
                            "flag": "general",
                            "class": "gear-all-image normal-image",
                            "src": "/media/reports/files/7gF6W8SOlbS6Rlm0o7Rhrd.png"
                        }
                    }
                },
                {
                    "insert": "\n核心功能"
                },
                {
                    "insert": "\n",
                    "attributes": {
                        "header": 2
                    }
                },
                {
                    "insert": "\n"
                },
                {
                    "insert": {
                        "image": {
                            "file_url": "/media/reports/mind_maps/4KciBSOEAuOAgWRcAw22xn.opml",
                            "flag": "mindmap",
                            "filename": "无人机视频直播APP.opml",
                            "class": "gear-all-image normal-image",
                            "src": "/media/reports/mind_maps/4KciBSOEAuOAgWRcAw22xn.png",
                            "json_url": "/media/reports/mind_maps/4KciBSOEAuOAgWRcAw22xn.json"
                        }
                    }
                },
                {
                    "insert": "\n\n建议"
                },
                {
                    "insert": "\n",
                    "attributes": {
                        "header": 2
                    }
                },
                {
                    "insert": "因本项目主要为展示使用，其中无人机数量固定，不存在新增无人机情况。所以我方建议将无人机配置固定、登录账号固定。"
                },
                {
                    "insert": "\n",
                    "attributes": {
                        "list": "ordered"
                    }
                },
                {
                    "insert": "本次方案不需要进行管理后台管理页面的开发。"
                },
                {
                    "insert": "\n",
                    "attributes": {
                        "list": "ordered"
                    }
                },
                {
                    "insert": "本次方案设计对无人机、摄像设备的对接，我方暂认为贵方可以提供以下支持："
                },
                {
                    "insert": "\n",
                    "attributes": {
                        "list": "ordered"
                    }
                },
                {
                    "insert": "可提供无人机设备唯一标示。"
                },
                {
                    "insert": "\n",
                    "attributes": {
                        "list": "ordered",
                        "indent": 1
                    }
                },
                {
                    "insert": "可提供摄像设备视频推流地址。"
                },
                {
                    "insert": "\n",
                    "attributes": {
                        "list": "ordered",
                        "indent": 1
                    }
                },
                {
                    "insert": "可提供无人机控制接口。"
                },
                {
                    "insert": "\n",
                    "attributes": {
                        "list": "ordered",
                        "indent": 1
                    }
                },
                {
                    "insert": "材料准备"
                },
                {
                    "insert": "\n",
                    "attributes": {
                        "header": 2
                    }
                },
                {
                    "insert": "服务器、域名及ICP备案"
                },
                {
                    "insert": "\n",
                    "attributes": {
                        "list": "ordered"
                    }
                },
                {
                    "insert": "硬件对接文档"
                },
                {
                    "insert": "\n",
                    "attributes": {
                        "list": "ordered"
                    }
                },
                {
                    "insert": "其他说明"
                },
                {
                    "insert": "\n",
                    "attributes": {
                        "header": 2
                    }
                },
                {
                    "insert": "本次方案为初步预估，准确报价及周期需等需求细节及接口确认后方可评估。"
                },
                {
                    "insert": "\n",
                    "attributes": {
                        "list": "bullet"
                    }
                },
                {
                    "insert": "本次方案为我方仅进行软件部分开发，不进行摄像头与无人机的具体对接功能，我方控制无人机及获得视频资源，均需要通过硬件部分提供对应接口及文档。"
                },
                {
                    "insert": "\n",
                    "attributes": {
                        "list": "bullet"
                    }
                },
                {
                    "insert": "因目前不确定摄像头、无人机对接方式，本次方案不包含此部分内容。"
                },
                {
                    "insert": "\n",
                    "attributes": {
                        "list": "bullet"
                    }
                }
            ]
        }
```