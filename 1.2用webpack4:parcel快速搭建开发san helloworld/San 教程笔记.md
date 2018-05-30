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
