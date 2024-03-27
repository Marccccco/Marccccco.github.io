---
title: vue2兼容ie11
---
### vue2兼容ie11
`注:@babel/polyfill从 Babel 7.4.0 开始，这个包已经被弃用,取而代之的是直接包含 [ore-js/stable](https://babel.nodejs.cn/docs/babel-polyfill/),下面的方法是 Babel 7.4.0 之后的版本解决方法`
1. 使用vue cli创建项目时选择上`babel`
2. 第1步-配置[browserslistrc](https://cli.vuejs.org/zh/guide/browser-compatibility.html),在package.json中添加以下代码
```json
> 1%
last 2 versions
not dead
not ie <=11 //加上此行配置即可
```
3. 在`babel.config.js`中添加以下代码
```js
module.exports = {
  presets: [
    [
      '@vue/app',   //需要改成这种配置方式，因为main.js 引入了脚手架自带的一些插件
      {
        targets: { ie: '11' },   //目标浏览器和版本
        useBuiltIns: 'entry'   //vuecli有具体说明-参考官方浏览器兼容Polyfill配置项的第三点
      }
    ]
  ],
};
```
4. 在`main.js`中添加以下代码
```js
//建议加在文件的最顶部，否则可能无效     
import 'core-js/stable'; 
import 'regenerator-runtime/runtime'
```

参考文档:
[vue-cli官网](https://cli.vuejs.org/zh/guide/browser-compatibility.html)
[@babel/polyfill中文文档](https://babel.nodejs.cn/docs/babel-polyfill/)
