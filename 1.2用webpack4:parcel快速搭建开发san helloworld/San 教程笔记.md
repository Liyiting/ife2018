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