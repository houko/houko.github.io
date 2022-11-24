---
date: 2016-05-17
title: webpack进阶之loader篇
tags:
  - webpack
  - javascript
  - node
  - 工具
categories: 
  - 工具
slug: WebpackLLink
---
webpack的loaders是一大特色，也是很重要的一部分。这遍博客我将分类讲解一些常用的laoder
<!--more-->
### 一、loaders之 预处理
* css-loader 处理css中路径引用等问题
* style-loader 动态把样式写入css
* sass-loader scss编译器
* less-loader less编译器
* postcss-loader scss再处理

`npm install --save -dev css-loader style-loader sass-loader less-loader postcss-loader`  

栗子:

```
module: {
  loaders: [
    {test: /\.css$/, loader: "style!css?sourceMap!postcss"},
    {test: /\.less$/, loader: "style!css!less|postcss"},
    {test: /\.scss$/, loader: "style!css!sass|postcss"}
  ]
}
```

### 二、loaders之 js处理
* babel-loader
* jsx-loader

`npm install --save-dev babel-core babel-preset-es2015  babel-loader jsx-loader`

栗子  

新建一个名字为`.babelrc`的文件

```
{
  "presets": ["es2015","react"],
  "plugins":["antd"]
}
```

新建一个名字为`webpack.config.js`文件

```
module.exports ={
 entry: './entry.js',
 output: { path: __dirname,
 filename: 'bundle.js'
 },
 module: {
loaders: [
  {test: /\.js$/, loader: "babel", exclude: /node_modules/},
  {test: /\.jsx$/, loader: "jsx-loader"}
  {test: /.css$/, loader: 'style!css'} ]
  }
};
```

### 三、loaders之 图片处理
* url-loader

`npm install --save-dev url-loadr`

```
module: {
  loaders: [
    {test: /\.(jpg|png)$/, loader: "url?limit=8192"},
  ]
}
```

### 四、loaders之 文件处理
* file-loader

`npm install --save-dev file-loader`

```
module: {
  loaders: [
    {
      test: /\.(png|jpg|jpeg|gif|svg|woff|woff2|ttf|eot)$/,
      loader: 'file'
      },
  ]
}

```

### 五、loaders之 json处理
* json-loader

`npm install --save-dev json-loader`

```
module: {
  loaders: [
    {test: /\.json$/,loader: 'json'},
  ]
}
```

### 六、loaders之 html处理
* raw-loader

`npm install --save-dev raw-loader`

```
module: {
  loaders: [
    { test: /\.html$/,loader: 'raw'},
  ]
}
```
