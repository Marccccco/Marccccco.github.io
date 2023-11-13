---
title: Vue3中的列表拖拽排序
date: 2023-11-02 14:40:24
tags:
---

# [Vue3中的列表拖拽排序](https://juejin.cn/post/7231519213897875512#heading-0)

## 拖拽原理

### 属性和事件

`draggable`属性是HTML5新增的可拖拽属性.

 在HTML中,除了图像、链接和选择的文本默认可拖拽外，其它元素默认是不可拖拽的。如果想让其它元素变成可拖拽的，首先需要把`draggable`属性设置为`true`.

```html
<p draggable="true"> 可拖拽 </p>
```

设置完成之后,还需结合一些事件才能完成拖拽:

* 拖拽元素的事件

| 事件      | 触发时机           |
| --------- | ------------------ |
| dragstart | 开始拖拽时执行1次  |
| drag      | 拖拽开始后触发多次 |
| dragend   | 拖拽结束后触发1次  |

* 可释放目标的事件

| 事件      | 触发时机                                                     |
| --------- | ------------------------------------------------------------ |
| dragenter | 拖拽元素进入可释放目标时执行1次                              |
| dragover  | 拖拽元素进入可释放目标时触发多次(100毫秒一次)                |
| drop      | 拖拽元素进入可释放目标内释放时(设置了dragover此事件才会生效) |



#### 可放置目标

这是个啥呢,就是当你添加了`draggable="true"`属性时,你是可以拖动了,但你光拖动没用啊,你得拖完放到某个地方,那么哪里可以放置拖拽的元素呢,那就需要给它们设置对应的属性.

`dragenter`或`dragover`事件可==用于表示有效的放置目标==.

设置允许被被放置还需要阻止事件的默认处理

```js
<div ondragenter="event.preventDefaule()">
```

#### DataTransfer对象

`DataTransfer`对象用于保存拖拽过程中的数据、设置拖拽类型等。在所有拖动事件`event`的`dataTransfer`属性上都能获取到`DataTransfer`对象.

* `dataTransfer.dropEffect`设置拖拽的操作类型.值必须是`none`,`copy`,`link`或`move`.

  ```js
  function dragover(e){
    e.preventDefault()
    e.dataTransfer.dropEffect = 'move'
  }
  ```

* 传递数据(不建议使用这种方法,可能有兼容问题)只能在`drop`事件中接收(也有可能测试的不对)

  ```js
  function dragstart(e, index) {
  	e.stopPropagation()
  	e.dataTransfer.dropEffect = 'move'
          // 传数据
  	e.dataTransfer.setData('text/plain', '111111111')
  }
  function drop(e) {
          // 接收数据
  	console.log(e.dataTransfer.getData('text/plain'));
  }
  ```

  

































