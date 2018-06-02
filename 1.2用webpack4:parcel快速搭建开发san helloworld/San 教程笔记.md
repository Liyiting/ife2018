# San 教程笔记
> 文档: [https://baidu.github.io/san/tutorial/start/](https://baidu.github.io/san/tutorial/start/)

## 安装

### 下载
 * 直接下载
 * CDN
 * NPM

### 使用
* 在页面上通过script引用
* 通过AMD方式引用src目录下的main.js

	```
	require.config({
    packages: [
        {
            name: 'san',
            location: 'san-path/dist/san'
        }
    ]
});
	```

* 在支持ESNext的环境中直接引用： `import san from 'san'`

### 开发版本 VS 生产版本
* 开发板中的`san.dev.js`中有数据校验等辅助开发功能，出于性能考虑生产环境上需要移除这些功能
* 生产版中提供`san.js`

```
///开始开发：
npm run dev

//开始构建：
npm run build
```

## 背景

### 概念
* MVVM模式：操作数据，就是操作视图，就是操作DOM。
* MVVM最标志性的特性就是**数据绑定**
* MVVM的核心理念就是通过**声明式的数据绑定**来实现View层和其它层的分离。
* MVVM是一种分层框架
	* Model: 域模型
	* View: 作为视图模板存在
	* ViewModel: 作为视图的模型，为视图服务

#### Model层
对应数据层的域模型，主要做**域模型的同步**。通过Ajax/fetch等API完成客户端和服务端业务Model的同步。

在层间关系里，它主要用于抽象出ViewModel中视图的Model。

#### View层
作为视图模板存在，在MVVM里，整个View是一个动态模板。除了定义结构、布局外，它展示的是ViewModel层的数据和状态。

View层不负责处理状态，View层做的是**数据绑定的声明、指令的声明、事件绑定的声明。**

#### ViewModel层
ViewModel层把View需要的层数据暴露，并对View层的**数据绑定声明、指令声明、时间绑定声明**负责，也就是处理View层的具体业务逻辑。

ViewModel底层会做好绑定属性的监听。当ViewModel中数据变化，View层会得到更新。而当View中声明了数据的双向绑定（通常是表单元素），框架也会坚挺View层（表单）值的变化。一单值变化，View层绑定的ViewModel中的数据也会得到自动更新。

#### 图示
![](https://raw.githubusercontent.com/X-Jray/blog/master/assets/mvvm.png)

* 在前端MVVM框架中，往往没有清晰、独立的Model层。
* 在实际业务开发中，我们通常按**Web Component**规范来组件化的开发应用，Model层的域模型往往分散在一个或几个Component的ViewModel层，而ViewModel层也会引入一些View层相关的中间状态，目的就是为了更好地为View层服务。
* 开发者在View层的视图模板中声明**数据绑定、事件绑定**后，在ViewModel中进行业务逻辑的**数据**处理。事件触发后，ViewModel中**数据**变更，View层自动更新。
* 因为MVVM框架的引入，开发者只需关注业务逻辑、完成数据抽象、聚焦数据，MVVM的视图引擎会帮你搞定View。
* 因为数据驱动，一切变得更加简单。

### MVVM框架的工作
* 视图引擎：
	* 为View层作为视图模板提供强力支持，
	* 开发者不需要操作DOM，视图引擎来做。
* 数据存取器：
	* 可以通过`Object.defineProperty()`API轻松定义，或通过自行封装存取函数的方式曲线完成。
	* 它内部往往封装了**发布/订阅模式**，以此来完成对数据的监听、数据变更时通知更新。它是实现**数据绑定**的基础。
* 组件机制：
	* 有追求的开发者往往希望按照面向未来的组件标准 - **Web Components**的方式开发。
	* MVVM框架提供组件的定义、继承、生命周期、组件间通信机制，为开发者面向未来开发点亮明灯。


## 开始