title: react+webpack+es6基础配置
date: 2016-02-17 16:00:20
tags: React

---

今天折腾react,发现很多react+webpack的入门文章推荐里的webpack.config.js配置基本如下：
```javascript
module.exports = {
  entry: "./entry.js",
  output: {
    path: __dirname + '/dist',
    filename: "bundle.js"
  },
  resolve: {
    extensions: ['', '.js', '.jsx']
  },
  module: {
    loaders: [{
      test: /\.js|jsx$/,
      loader:'babel',
      exclude: /node_modules/,     
    }]
  }
}
```

兴致勃勃地写了个hello world,结果webpack编译时一直报错:

```
ERROR in ./entry.js
Module build failed: SyntaxError: E:/myCode/React/entry.js: Unexpected token (4:7)
  2 | import {render} from 'react-dom';
  3 | import Hello from './components/Hello';
 4 | render(<Hello name="yunl" />,document.getElementById("test"));
    |        ^
  5 |
  6 |
    at Parser.pp.raise (E:\myCode\React\node_modules\babel-core\node_modules\babylon\index.js:1425:13)
    at Parser.pp.unexpected (E:\myCode\React\node_modules\babel-core\node_modules\babylon\index.js:2905:8)
    at Parser.pp.parseExprAtom (E:\myCode\React\node_modules\babel-core\node_modules\babylon\index.js:754:12)
    at Parser.pp.parseExprSubscripts (E:\myCode\React\node_modules\babel-core\node_modules\babylon\index.js:509:19)
```

逛了很多社区，发现给出的方案都不能解决问题，心里有成千上万的马奔腾而过啊！
最后折腾了很久发现，需要在webpack.config.js里添加：

`query: {presets: ['react', 'es2015']}`

==！
然后，分别install 一下：
``` 
npm install babel-preset-react --save-dev 

mpm install babel-preset-es2015 --save-dev

```
最终的webpack.config.js配置如下：
```
module.exports = {
  entry: "./entry.js",
  output: {
    path: __dirname + '/dist',
    filename: "bundle.js"
  },
  resolve: {
    extensions: ['', '.js', '.jsx']
  },
  module: {
    loaders: [{
      test: /\.js|jsx$/,
      loader:'babel',
      exclude: /node_modules/,
      query: {
        presets: ['react', 'es2015']
      }
    }]
  }
}
```


最有还要注意一点的是 ** React 0.14版本后的React拆分成了react和react-dom **。
> 其中 react package 中包含 React.createElement、.createClass、.Component，.PropTypes，.Children这些API，而react-dom package中包含ReactDOM.render、.unmountComponentAtNode、.findDOMNode。

所以之前的`React.render`要改成`ReactDOM.render`:
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import Hello from './components/Hello';
ReactDOM.render(<Hello name="yunl" />,document.getElementById("test"));
```
希望此文能帮助初学者少踩两个坑!

