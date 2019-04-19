---
title: 使用webpack从零开始搭建react项目
catalog: true
date: 2018-12-21 14:45:17
subtitle:
header-img:
tags:
---

## 使用webpack从零开始搭建react项目

[webpack中文文档](https://webpack.docschina.org/)

![yarn语法](https://note.youdao.com/yws/res/16810/WEBRESOURCE5e9deae4ba81f2d9d1631772395a9a40)

#### webpack的安装

![webpack的安装](https://note.youdao.com/yws/res/16815/WEBRESOURCE952f785a54e37cc1cbd1c0cb168a7dd5)

```
yarn add webpack@3.10.1 --dev
```
#### 需要处理的文件类型

![文件类型](https://note.youdao.com/yws/res/16821/WEBRESOURCEe3931234abcfdb25f16f6f72a75893fb)

#### webpack常用模块

![webpack常用模块](https://note.youdao.com/yws/res/16827/WEBRESOURCE26f6eb42de497c5d062a8eca60e2a1ea)

#### webpack-dev-server

![webpack-dev-server](https://note.youdao.com/yws/res/16832/WEBRESOURCE5dcf8d2cd6c1af757623e5d31892ceac)

```
yarn add webpack-dev-server@2.9.7 --dev
```


#### webpack用法

> 创建webpack.config.js文件

```js
const path = require('path');

module.exports = {
    entry: './src/app.js', //入口文件
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'app.js'
    }
};
```

> 执行命令

```
node_module/.bin/webpack
```

##### 打包html的配置

> htmlWebpackPlugin

```
// 安装html-webpakc-plugin
yarn add html-webpack-plugin --dev
```
> 自定义html模版(title,mate等信息)

[配置链接](https://github.com/jantimon/html-webpack-plugin#options)

```
// webpack.config.js文件
plugins: [
    new HtmlWebpackPlugin({
        template: './src/index.html'
    })
]

```

##### 安装babel

[参考链接](https://webpack.docschina.org/loaders/babel-loader/#src/components/Sidebar/Sidebar.jsx)

```
// 安装
// 多个插件之间空格分隔
yarn add babel-core@6.26.0 babel-present-env@1.6.1 babel-loader@7.1.2 --dev

// webpack.config.js配置
module: {
  rules: [
    {
      test: /\.js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env'],
          plugins: [require('@babel/plugin-transform-object-rest-spread')]
        }
      }
    }
  ]
}
```

##### 安装处理React的插件

> `babel-preset-react`

```
//babel-preset-react
yarn add babel-preset-react@6.24.1 --dev
```
##### 如额使用React
```
// 安装react react-dom
yarn add react react-dom

```

##### 加载CSS

> `style-loader`与`css-loader`

```
module:{
    rules: [
        {
            test: /\.css$/,
            use: [
                'style-loader',
                'css-loader'
            ]
        }
    ]
}

```


##### 将文件提取出来

> `ExtractTextWebpackPlugin`

[ExtractTextWebpackPlugin: 将包或包中的文本提取到单独的文件中。](https://webpack.docschina.org/plugins/extract-text-webpack-plugin/#src/components/Sidebar/Sidebar.jsx)

> 配置

```
const ExtractTextPlugin = require('extract-text-webpack-plugin');
module:{
    rules: [
        {
            test: /\.css$/,
            use: ExtractTextPlugin.extract({
                  fallback: "style-loader",
                  use: "css-loader"
            })
        }
    ]
},
plugins: [
    new ExtractTextPlugin("styles.css"),
]
```

##### 处理sass

> `sass-loader`, `sass-loader`依赖`node-sass`与`webpack`

```
yarn add sass-loader node-sass
```

##### 图片资源处理 

> 用`file-loader`与`url-loader`处理图片资源，`url-loader`依赖`file-loader`

```
// 安装
yarn add url-loader file-loader --dev

// 配置

module: {
    rules: [
        {
            test: /\.(png|jpg|gif)$/,
            use: [
                {
                    loader: 'url-loader',
                    options: {
                        limit: 8192
                    }
                }
            ]
        }
    ]
}
```

##### font-awesome

```
yarn add font-awesome

// jsx中引入css
import 'font-awesome/css/font-awesome.min.css';
```

##### CommonsChunkPlugin

```
new webpack.optimize.CommonsChunkPlugin({
    name: 'common',
    filename: 'js/base.js'
})
```


#### webpack-dev-server

```
// 安装
yarn webpack-dev-server@2.9.7

// webpack.config.js中 配置

devServer: {
    contentBase: './dist'
    port: 8086
}

// 更改启动方式 package.json
"scripts": {
    "dev" : "node_modulse/.bin/webpack-dev-server",
    "dist": "node_modules/.bin/webpack -p" //添加-p为线上打包
}
```

##### resolve

> object

配置模块如何解析，例如，挡在ES2015中调用`import "loadsh"`, `resolve`选项能够对webpack查找`"lodash"`的方式去做修改。

###### resolve.alias

> object

创建`import`或`require`的别名，来确保模块引入变得更简单，例如一些位于`src/`文件夹下的常用模块

```
// webpack.config.js 配置
module.exports = {
    // ...
    resolve: {
        alias: {
            Utilities: path.resolve(__dirname, 'src/utilities/'),
            Templates: path.resolve(__dirname, 'src/templates/')
        }
    }
}
```

现在，替换【在导入时使用相对路径】这种方式，就像这样：
```
import Utility from '../../utilities/utility';
```
可以这样使用别名
```
import Utility from 'Utilities/utility';
```

###### devServer.historyApiFallback

当使用 `HTML5 History API` 时，任意的 404 响应都可能需要被替代为 `index.html`。通过传入以下启用：

```
module.exports = {
    // ...
    devServer = {
        historyApiFallback: {
            index: '/dist/index.html'
        }
    }
}
```
###### 接口Api代理`devServer.proxy`

[参考地址](https://webpack.docschina.org/configuration/dev-server/#devserver-proxy)

如果你有单独的后端开发服务器 API，并且希望在同域名下发送 API 请求 ，那么代理某些 URL 会很有用(可以避免浏览器跨域报错)。

在 localhost:3000 上有后端服务的话，你可以这样启用代理：

```
// webpack.config.js配置
module.exports = {
    // ...
    devServer: {
        proxy: {
            '/api': 'http://localhost:3000'
        }
    }
}

```
请求到 `/api/users` 现在会被代理到请求 `http://localhost:3000/api/users`。

```
module.exports = {
    // ...
    devServer: {
        proxy: {
            '/manage': {
                    target: 'http://admintest.happymmall.com',
                    changeOrigin: true
                },
                '/user/logout.do': {
                    target: 'http://admintest.happymmall.com',
                    changeOrigin: true
                }
        }
    }
}
```

