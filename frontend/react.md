# React的简单说明

### 一、React的描述

React 起源于 Facebook的内部项目。

其设计思想独特、性能出众、代码逻辑简单，是现在主流的javaScript库，是现在Web 开发中的主流工具。

### 二、React的特点

#### 1、面向对象式编程思想

react中的元素、组件都可以看作一个对象；每个对象包含自身的属性和方法

#### 2、组件化思想

基于组件（Component）化思考，组件可组合、可复用、易维护

一个组件包含数据和方法；

**props** 由外部传入、  **state**为组件自己的状态

组件的生命周期方法、组件的自定义方法

每个组件的目的是返回一个JSX元素，对应页面上的一个DOM结构

#### 3、数据单向流动

数据单向流动 保证的代码的准确性

**state**  组件自己的状态  可用state来完成对行为的控制、数据的更新、界面的渲染 ，更改state使用方法setState

**props** 由外部传入  在组件中是**只读的** **不可被更改**

在子组件中的数据需要传递给父组件，或者在子组件中改变父组件中的数据，可通过调用父组件传进来的方法 

```
父组件中有方法
closeEditTaskModal() {
        this.setState({
            showEditTaskModal: false,
        })
    }
父组件中调用了子组件 并传入了一些数据和方法   
在子组件可以通过   this.props.XXX 获取
<EditTaskModal
    visible={this.state.EditTaskModal}
    title={"编辑任务"}
    taskData={this.state.taskData}
    closeModal={this.closeEditTaskModal.bind(this)}
/>
子组件中调用 父组件传进来的方法 可以修改父组件的数据
this.props.closeModal();   
```

#### 4、JSX的使用

JSX是JavaScript XML，是一种JavaScript的语法扩展。其编写简单快捷、执行快。

在React中使用**JSX** 来替代常规的**JavaScript，**用来声明 React 中的元素（ React 中的元素最后会被渲染为DOM）

可以任意地在 JSX 当中使用 JavaScript 表达式(在 JSX 当中的表达式要包含在大括号里)

```
class UserProfilePage extends React.Component {
    render() {
        let username = "木帆平";
        let welcomeMessages = [<h1>你好</h1>, <h2>学的不仅是技术，更是梦想！</h2>];
        let usernameSpan = (<span>{username.indexOf("木") == 0 ? username.replace('木', "李") : username}</span>);
        return (
            <div>
                {welcomeMessages}
                {usernameSpan}
            </div>
        );
    }
}
ReactDOM.render(
    <UserProfilePage/>, document.getElementById('UserProfile')
)
```

#### 5、All in JavaScript

React中没有html模版； 与**DOM**对应的**React元素**也是**一个JavaScript对象**(编译后是一段创建dom元素的js)

### 三、React的生命周期

React生命周期会经历如下三个过程：

装载过程（Mount），组件第一次在DOM树中渲染的过程；

更新过程（Update），组件被重新渲染的过程。

卸载过程（Unmount），组件从DOM中删除的过程。

#### 1、装载过程（Mount）

当组件第一次被渲染的时候，依次调用如下函数

constructor   构造函数，在创建组件的时候调用一次

componentWillMount 

render                                     react最重要的步骤，创建虚拟dom

componentDidMount         组件渲染之后调用，只执行一遍

#### 2、更新过程（Update）

当props或者state被修改的时候，就会引发组件的更新过程

componentWillReceiveProps  组件初始化时不调用，组件接受新的props时调用。

```
注意：如果组件在componentDidMount调用了接口获取数据，
而且父组件传过来的props改变会影响接口数据的话，
因为componentDidMount只执行一遍，
就需要在componentWillReceiveProps中判断this.props和nextProps是否相同，
不相同了执行接口更新方法
```

shouldComponentUpdate

componentWillUpdate

render

componentDidUpdate

注意：并不是所有的更新过程都会执行全部函数

#### 3、卸载过程（Unmount）

componentWillUnmount

### 参考文档：

react简介：

https://react.docschina.org/

https://www.jianshu.com/p/ee97db18dcf3

生命周期：

https://www.cnblogs.com/kdcg/p/9182393.html

https://www.jianshu.com/p/e7f7967f8928