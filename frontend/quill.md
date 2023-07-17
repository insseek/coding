# Quill编辑器的使用说明

Quill is a free, open source WYSIWYG editor built for the modern web. Completely customize it for any need with its modular architecture and expressive API.

Quill 使用方便、开源，模块化架构、易于扩展自定义组件；DOM数据模型化，易于实时协作编辑。

#### Quill的一些文档及模块资源

官网：https://quilljs.com/

Github地址：https://github.com/quilljs/quill

代码下载地址:https://github.com/quilljs/quill/releases   （深度定制可下载查看源码）

英文文档：https://quilljs.com/docs/quickstart/

中文文档：

Gitbook地址 ：https://bingkui.gitbooks.io/quill/content/

Github地址：https://github.com/BingKui/QuillChineseDoc

Quill相关一些组件库和module：https://github.com/quilljs/awesome-quill （直接使用/二次开发/模仿开发）

#### 引入

```
1、npm install
2、下载到本地 https://github.com/quilljs/quill/releases 
3、线上资源：
<!-- Main Quill library -->
<script src="//cdn.quilljs.com/1.3.6/quill.js"></script>
<script src="//cdn.quilljs.com/1.3.6/quill.min.js"></script>

<!-- Theme included stylesheets -->
<link href="//cdn.quilljs.com/1.3.6/quill.snow.css" rel="stylesheet">
<link href="//cdn.quilljs.com/1.3.6/quill.bubble.css" rel="stylesheet">

<!-- Core build with no theme, formatting, non-essential modules -->
<link href="//cdn.quilljs.com/1.3.6/quill.core.css" rel="stylesheet">
<script src="//cdn.quilljs.com/1.3.6/quill.core.js"></script>
```

### 一、Toolbar工具栏

#### 1、使用方式

##### 方式一  Options数组

优势：方便 、快捷

自动生成 class name为的ql-${format}元素，可以复写样式

```
/* 你可以通过修改toolbarOptions参数来修改工具栏的配置 */
var toolbarOptions = [
  ['bold', 'italic', 'underline', 'strike'],        // 字体控制按钮  加粗，斜体，下划线，删除线
  ['blockquote', 'code-block'],                     // 引用块， 代码块
  [{ 'list': 'ordered'}, { 'list': 'bullet' }],     // 顺序列表， 无序列表
  [{ 'script': 'sub'}, { 'script': 'super' }],      // 上标和下标
  [{ 'indent': '-1'}, { 'indent': '+1' }],          // 缩进加减
  [{ 'direction': 'rtl' }],                         // 文字方向
  [{ 'size': ['small', false, 'large', 'huge'] }],  // 字号
  [{ 'header': [1, 2, 3, 4, 5, 6, false] }],        // 标题1， 标题2
  [{ 'color': [] }, { 'background': [] }],          // 字体颜色和背景色
  [{ 'font': [] }],                                 // 字体
  [{ 'align': [] }],                                //对齐
  ['link', 'image', 'video'],                       // 超链接，图片和视频
  ['clean']                                         // 清除格式
];
var quill = new Quill('#editor', {
  modules: {
    toolbar: {
      container: toolbarOptions, 
  },
  theme: 'snow'
});
```

##### 方式二 HTML

优势：更灵活地进行改写，修改样式、绑定事件

带来的问题：某些组件默认的方式是在toolbarOptions中添加；使用组件时必须自己去html中添加相应元素

```
<!-- Create toolbar container -->
<div id="toolbar">
  <!-- Add font size dropdown -->
  <select class="ql-size">
    <option value="small"></option>
    <!-- Note a missing, thus falsy value, is used to reset to default -->
    <option selected></option>
    <option value="large"></option>
    <option value="huge"></option>
  </select>
  <!-- Add a bold button -->
  <button class="ql-bold"></button>
  <!-- Add subscript and superscript buttons -->
  <button class="ql-script" value="sub"></button>
  <button class="ql-script" value="super"></button>
</div>
<div id="editor"></div>

<!-- Initialize editor with toolbar -->
<script>
  var quill = new Quill('#editor', {
    modules: {
      toolbar: { 
        container: '#toolbar' 
      }
    }
  });
</script>
```

#### 2、自定义工具栏

quil的模块化架构、允许复写原工具的处理程序及输出格式、也可以添加自定义的工具栏工具

默认的处理程序handlers：formula、image、video、link（定义在源码themes中） clean、direction、indent、link、list（定义在modules的toolbar中）

工具栏对应输出格式format：（定义在源码formats模块）

![quill-formats](https://leetan.oss-cn-beijing.aliyuncs.com/code/quill-formats.png)

```
 [{ 'align': [] }],                                //对齐
  ['bold', 'italic', 'underline', 'strike'],        // 字体控制按钮  加粗，斜体，下划线，删除线
  ['blockquote', 'code-block'],                     // 引用块， 代码块
  [{ 'list': 'ordered'}, { 'list': 'bullet' }],     // 顺序列表， 无序列表
  [{ 'script': 'sub'}, { 'script': 'super' }],      // 上标和下标
  [{ 'indent': '-1'}, { 'indent': '+1' }],          // 缩进加减
  [{ 'direction': 'rtl' }],                         // 文字方向
  [{ 'size': ['small', false, 'large', 'huge'] }],  // 字号
  [{ 'header': [1, 2, 3, 4, 5, 6, false] }],        // 标题1， 标题2
  [{ 'color': [] }, { 'background': [] }],          // 字体颜色和背景色
  [{ 'font': [] }],                                 // 字体
  [{ 'align': [] }],                                //对齐
  ['link', 'image', 'video'],                       // 超链接，图片和视频
```

##### 覆盖原工具的处理程序及输出格式

###### 覆盖handler

```
var toolbarOptions = {
  handlers: {
    // handlers将与默认handlers合并 覆盖link默认的****handler****
    'link': function(value) {
      if (value) {
        var href = prompt('Enter the URL');
        this.quill.format('link', href);
      } else {
        this.quill.format('link', false);
      }
    }
  }
}
var quill = new Quill('#editor', {
  modules: {
    toolbar: toolbarOptions
  }
});
// 在初始化后添加 为image添加****handler****
var toolbar = quill.getModule('toolbar');
toolbar.addHandler('image', showImageUI);
```

###### 覆盖输出格式

```
 // 覆盖原**video**默认的format
 var Embed = Quill.import('blots/block/embed');
  class Video extends Embed {
    static create(value) {
        let node = super.create(value);
        node.setAttribute('src', value);
        node.setAttribute('type', "video/quicktime");
        node.setAttribute('controls', 'controls');
        return node;
    }
}
Video.blotName = 'video'; //now you can use .ql-hr classname in your toolbar
Video.className = 'ql-video';
Video.tagName = 'video';

Quill.register({
    'formats/video': Video
});
```

示例：富文本组建复写图片、Vedio的handler（created by Chaoneng）
https://git.chilunyc.com/components/rich-text-editor/blob/master/quill-jquery-example/quill-extensions/gear-image-and-video-uploader.js

##### 自定义新工具

###### 方式一 Options数组

```
/* 你可以通过修改toolbarOptions参数来修改工具栏的配置 */
var toolbarOptions = [
  ['bold', 'italic', 'underline', 'strike'],        // 字体控制按钮  加粗，斜体，下划线，删除线
  ['blockquote', 'code-block'],                     // 引用块， 代码块
  [{ 'list': 'ordered'}, { 'list': 'bullet' }],     // 顺序列表， 无序列表
  [{ 'header': [1, 2, 3, 4, 5, 6, false] }],        // 标题1， 标题2
  // 自定义两个按钮撤消和重做
  ['undo', 'redo'], 
];
var quill = new Quill('#editor', {
  modules: {
    toolbar: {
      container: toolbarOptions, 
      handlers: {
        undo: function(value) {
          this.quill.history.undo();
        },
        redo: function(value) {
          this.quill.history.redo();
        }
      } ,
      history: {
      delay: 1000,//多少毫秒数内发生的更改合并为单个更改
      maxStack: 100,//撤销/重做堆栈的最大大小
      userOnly: true //只有用户更改会被撤销或者重做
    }
  },
  theme: 'snow',
});
```

###### 方式二 HTML

```
div id="toolbar">
  <!-- 默认的 -->
  <button class="ql-bold"></button>
  <button class="ql-italic"></button>
  <!-- 自定义的按钮 -->
  <button id="custom-button" class="ql-my-button"></button>
  <button id="custom-button" class="ql-my-button"></button>
</div>
<div id="editor"></div>

<script>
var quill = new Quill('#editor', {
  modules: {
    toolbar: '#toolbar'
  }
});
var customButton = document.querySelector('#custom-button');
customButton.addEventListener('click', function() {
  console.log('Clicked!');
});
</script>
```

### 二、模块的使用及自定义模块

Quill的自带模块：[Toolbar](https://quilljs.com/docs/modules/toolbar/) [Clipboard](https://quilljs.com/docs/modules/clipboard/), [Keyboard](https://quilljs.com/docs/modules/keyboard/),  [History](https://quilljs.com/docs/modules/history/) and [Syntax Highlighter](https://quilljs.com/docs/modules/syntax/)    

工具栏、剪贴板、键盘、历史记录、语法高亮

#### 第三方模块

https://github.com/quilljs/awesome-quill 

#### 自定义模块

```
// 注册模块
const QuillModule = Quill.import("core/module");
const Image = Quill.import("formats/image");
//定义并注册了框架图的format(样式) 命名为ireframe-image
class WireframeImage extends Image {
    static create(value) {
        let node = super.create(value);
        node.setAttribute('class', 'wireframe-image');
        if (typeof value === 'string') {
            node.setAttribute('src', this.sanitize(value));
        }
        return node;
    }
}
WireframeImage.blotName = 'wireframe-image';
WireframeImage.tagName = 'IMG';
Quill.register({
    'formats/wireframe-image': WireframeImage
}, true);

// 创建一个框架图的模块并注册 模块命名为select_wireframes
class SelectWireframeImage extends QuillModule {
    constructor(quill) {
        super(quill);
        this.quill = quill;
        this.toolbar = quill.getModule('toolbar');
        //给format：ireframe-image添加一个处理器
        if (typeof this.toolbar != 'undefined'){
        this.toolbar.addHandler('wireframe-image', this.selectImageHanlder);
        }
        var selectImageBtns = document.getElementsByClassName('ql-wireframe-image');
        if (selectImageBtns) {
            [].slice.call(selectImageBtns).forEach(function (selectImageBtn) {
                selectImageBtn.innerHTML = '<svg viewBox="0 0 18 18"> <rect class="ql-stroke" height="10" width="12" x="3" y="4"></rect> <circle class="ql-fill" cx="6" cy="7" r="1"></circle> <polyline class="ql-even ql-fill" points="5 12 5 11 7 9 8 10 11 7 13 9 13 12 5 12"></polyline> </svg>'
            });
        }
    }

    //点击插入框架图的处理函数
    selectImageHanlder() {
        let quill = this.quill;
        // 这里写处理逻辑 
        // 最后选中一张图后 插入框架图到编辑器中insertWireframeToEditor(quill, url)
    }
}
function insertWireframeToEditor(editor, url) {
    // push image url to rich editor.
    const range = editor.getSelection();
    editor.insertEmbed(range.index, 'wireframe-image', url);
}
// 注册模块 
Quill.register('modules/select_wireframes', SelectWireframeImage);
// 添加模块
var quill = new Quill('#editor', {
  modules: {
    select_wireframes: true
  }
```

### 三、Parchment 定制/创建Quill识别的内容和格式(DOM)

为了提供一致的编辑体验，采用Parchment文档模型提供数据一致性和可预测性。Quill不能支持任意的DOM树和HTML更改。

但我们可基于Block、Inline和Embed扩展自己需要的内容和格式

#### 自定义内容格式Blot及使用

```
const Block = Quill.import('blots/block')
const Inline = Quill.import('blots/inline')
const Embed = Quill.import('blots/embed')

const ATTRIBUTES = [
  'href',
  'class',
  'style'
];
//自定义一个LinkBlot  blotName为'link'
class LinkBlot extends Inline {
  //创建一个node
  static create(value) {  
    let node = super.create();
    node.setAttribute('href', value.href)
    node.setAttribute('class', value.class:value.class:'my-class')
    node.setAttribute('target', '_blank')
    return node;
  }
  static formats(domNode) {
      return ATTRIBUTES.reduce(function(formats, attribute) {
      if (domNode.hasAttribute(attribute)) {
        formats[attribute] = domNode.getAttribute(attribute);
      }
      return formats;
    }, {});
  }
}
LinkBlot.blotName = 'link';
LinkBlot.tagName = 'a';
Quill..register({
    'formats/link': LinkBlot
}, true);

//在编辑器中插入自定义Blot的方法 index位置 、type类型  value传入的值 source默认为api
insertEmbed(index: Number, type: String, value: any, source: String = 'api')
示例：
const range = editor.getSelection(); //
editor.insertEmbed(range.index, 'link', {"href":"https://quilljs.com/images/cloud.pn", "class":"my-class"});
```

### 四、数据模型Delta

https://quilljs.com/docs/delta/

OT Operational Transform

Realtime Collaboration
