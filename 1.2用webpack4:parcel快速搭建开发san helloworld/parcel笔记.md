# parcel

> Parcel是Web应用打包工具，利用多核处理提供了极快的速度，并且不需要任何配置。

## 快速开始
### 安装
* Yarn: `yarn global add parcel-bundler`
* npm: `npm install -g parcel-bundler`

### 在你正在使用的项目目录下创建一个package.json文件
* Yarn: `yarn init -y`
* npm: `npm init -y`

### 创建文件
> Parcel可以使用任何类型的文件作为入口，最好是HTML或JavaScript文件。
如果HTML文件中使用相对路径引入了主要的JavaScript文件，Parcel也会对它进行处理替换为相对于输出文件的URL地址。

* Parcel内置了改变文件时自动构建应用的服务器，支持热模块替换。只需在入口文件指出`parcel index.html`
* 浏览器默认端口为[http://localhost:1234](#)，也可以使用`-p <port number>`选择覆盖默认端口。
* 如果没有自己的服务器可以使用开发服务器，或者由客户端呈现应用
* 如果有自己的服务器，可以在watch模式下运行Parcel。
* 当文件改变它仍然会自动重新构建并支持热替换，但是不会启动web服务。
	`parcel index.html`
* 准备在生产模式下创建时，build模式会关闭监听并且只建立一次

## 资源（Assets）
> Parcel是基于资源的，资源可以代表任意文件，并且Parcel对JS, CSS, HTML文件有更多的支持。
> Parcel会自动分析文件和包中的引用依赖。相同类型的额资源会被组合到同一捆绑包中。如果导入其他类型的资源（如JS中导入CSS文件），Parcel会启动子捆绑包，并在父捆绑包中保留对它的引用

### JavaScript
* JS是最传统的web打包文件类型。
* Parcel同时支持CommonJS和ES6两种语法模块来导入文件。
* 它也支持动态的`import()`函数语法异步加载模块。

```
//使用CommonJS语法导入模块
const dep = require('./path/to/dep');

//使用ES6语法导入模块
import dep from './path/to/dep';
```
* 在JS文件中导入非JS资源时，Parcel不会像其他打包工具一样内联该文件，而是将所有依赖放在另外一个捆绑包里（例如：一个CSS文件）。
* 当使用CSS Module时，这个导出类会被放置在JS包里，其他的资源文件将以URL的形式导出到JS包中的output中，以便能在代码中引用。

```
//引入CSS文件
import './test/css';

//引入包含CSS模块的CSS文件
import classNames from './test.css';

//以URL的形式引入图片
import imageURL from './test.png';
```
* 如果想通过内联文件到 JavaScript 包取代 URL 引入文件的方式，可以使用 Node.js 的 fs.readFileSync API。
* URL 必须是静态可分析的，意味着它不能有任何变量（除了__dirname 和 __filename）。

```
import fs from 'fs';

// 以字符串的形式读取内容
const string = fs.readFileSync(__dirname + '/test.txt', 'utf8');

// 以 Buffer 的形式读取内容
const buffer = fs.readFileSync(__dirname + '/test.png');
```

### CSS
* JS文件或HTML文件都能导入CSS资源，并能通过@import语法引用依赖。
* 还能通过url()函数引入图片，字体等。
* 其他通过@import导入的CSS文件被内敛到同一个CSS包里，并将url()引用重写为其输出文件名。
* 所有文件名都应当与前CSS文件相关联。