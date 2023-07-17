# 常见bug

#### Ant Design React

1、Form表单的赋值

初始赋值：initialValue、initialValues

由于initialValue、initialValues只执行一次

在接口数据更新的时候 保持表单数据数据同步更新

- 方案一：使用Form的setFieldsValue方法使数据同步更新
- 方案二：Form表单的key用UUID、或随机数 每次render保证更新

2、State 与 Form表单的赋值

如果Form表单中，有某些项Form.Item的展示由State值决定

请在setState完成后的回调中再设置这些Form.Item的值