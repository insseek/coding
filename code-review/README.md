# 代码规范与CodeReview

## 目的

保证产品代码质量

提高团队成员代码能力

统一代码规范 增进团队共识

## 期望

从发现问题、解决问题（Bug，代码规范、设计缺陷，代码质量等）到一起技术交流和进步成长

## 开展准备

1、确认代码规范

前端代码规范

后端开发接口设计规范

Python代码规范

2、确认审核checklist

前端通用CodeReview结果模板

后端通用CodeReview结果模板

3、总结优化：透明问题，持续优化

## Code Review内容

是否实现了业务需求；是否规范；是否高质量

### 1、需要修改的、 可以改进的地方

* **代码逻辑**
    * **基础逻辑**是否正确 
    * **业务需求逻辑**是否正确、严谨、完整
    * 设计有欠缺 设计不合理
    * 代码逻辑的写法 是否有明显的优化空间 有更好实现方案
        * 时间复杂度优化
        * 逻辑是否可简化
* 代码是否符合团队约定的代码风格规范
* 代码是否安全、代码性能
* 代码的质量
    * **代码可读性**
        * **命名是否规范**
        * **代码结构**是否清晰 不混乱
        * **目录结构**合理
        * 是否有必要的注释
    * 代码可维护性 可变更性
        * 代码不能写死
        * 预测可能发生的变化
        * 将某些条件设置为可配置的
        * 是否考虑有必要的可复用性

### 2、需要肯定的、可以借鉴的地方

* 代码中是否有一些高质量代码事值得大家学习借鉴的
    * **简单高效** 的代码
    * 良好的 **解决方案**
    * 良好的 **设计方案**
    * 体现可读性 可维护性 可变更性 低耦合 高内聚等原则的代码
    * 运用必要的设计模式