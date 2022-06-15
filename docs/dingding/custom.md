## 自定义工作台



### 进入工作台

登录钉钉开发者平台，选择对应的组织架构。

选择应用开发—企业内部开发—工作台—访问老版工作台页面

 [进入工作台][!https://open-dev.dingtalk.com/v1/fe/old#/list-bench]   查看当前工作台列表



### 配置工作台

选择一个工作台后，点击操作栏的设置按钮，查看详情，跳转到查看页面。选择自动配置中，跳转到自定义设计页面。





#### 问题：

1. 工作台模板如何配置？入口在哪儿？

2. 工作台如何配置权限？或者说如何实现权限和组件的关系？

   —只在官方组件里面看到一句话：如果宫格关联的应用或二级页面用户没有权限，则只展示图片，点击不跳转？？？

   那如果是这样的话，是不是表示 无法做到动态渲染。只能根据应用的权限来控制工作台的权限？？



### 前端开发

#### 开发前的准备工作

查阅文档 ：开发文档页面  https://open.dingtalk.com/document/dashboard/create-console-template

1. 更新小程序开发工具，现在是2.0+版本
2. 全局安装ding的npm包
3. 

#### 概念

1.组件，一个组件拥有一个完整的业务功能，包含视图和逻辑。（即模块功能）

2.插件，是组件的载体。一个插件可以包含多个定制工作台组件。钉钉工作台插件是一组小程序组件的集合。用于嵌入钉钉工作台中使用。每个组件可以在工作台设计器中独立使用，也可以通过工作台小程序提供的SDK事件通道进行联动。当为多个客户设计设计钉钉工作台时，可以使用同一套插件提供的组件，无需重复开发



3.钉钉脚手架，[DingTalk Design CLI][!https://open.dingtalk.com/document/resourcedownload/introduction]





定制组件
通用组件的区别：





组件和插件的区别：



#### 自建组件

1. 创建插件
2. 开发组件并上传
3. 配置组件
4. 调试组件
5. 提交上架
6. 提交发布
7. 全量发布

开发规范：https://open.dingtalk.com/document/orgapp-client/development-process

开发流程：https://open.dingtalk.com/document/dashboard/dashboard-component-develop-overview



#### 开发组件



#### 开发插件



#### 开发应用





#### 升级组件（旧版本的应用、组件等）

文档：https://open.dingtalk.com/document/dashboard/zig1pf









### 设计规范

#### 图片大小

- 资讯组件的图片：408px * 252px
- banner图支持gif
- 

