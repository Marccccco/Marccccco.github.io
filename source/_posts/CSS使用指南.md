---
title: CSS使用指南
date: 2023-10-10 13:45:23
cover: /img/head.png
---

### CSS 使用指南

#### 引入

| 引入方式 | 内容                                       | 备注       |
| -------- | ------------------------------------------ | ---------- |
| 行内样式 | 直接在元素上,加`style`属性                 | 优先级最高 |
| 内部样式 | 使用`<style>`标签                          |            |
| 外部样式 | 用`<head>`标签内的`<link>`标签引入样式文件 |            |

#### 语法

CSS 语法是由**_选择器_**和**_声明_**组成的

| 选择器分类 |                                                                                                            内容                                                                                                            |                                备注                                |
| :--------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :----------------------------------------------------------------: |
| 全局选择器 |                                                                                                  可与任何元素匹配;初始化                                                                                                   |                             优先级最低                             |
| 基础选择器 |                                                        元素选择器<br />类选择器(对应元素的 class 属性,用.)<br />id 选择器(对应元素的 id 属性,用#)<br />通配符选择器                                                        |                                                                    |
| 关系选择器 | 后代选择器:选择所有被祖先元素包含的*后代元素*<br />子代选择器:选择所有被祖先元素包含的*直接子元素*<br />相邻兄弟选择器:选择紧跟 E 元素后的 F 元素,相邻的第一个兄弟元素<br />通用兄弟选择器:选择 E 元素之后的所有兄弟元素 F | 后代 E F{}<br />子代 E>F{}<br />相邻兄弟 E+F{}<br />通用兄弟 E~F{} |

#### 盒模型

- 盒模型可以想象为一个盒子里装了 content,这 content 跟盒子边框的距离就是 padding,盒子的厚度就是 border,盒子的外边距就是 margin.

| name                                  | 公式                          | 使用         |
| ------------------------------------- | ----------------------------- | ------------ |
| content-box 内容盒-内容就是盒子的边界 | content-box width=内容宽度    | 一般不使用   |
| border-box 边框盒-边框才是盒子的边界  | width=内容宽度+padding+border | 一般默认使用 |

- `box-sizing: content-box`

![盒模型](./CSS使用指南/盒模型.png)

#### CSS 定位

##### 相对定位

- 用作位移,占位置但是有偏移

- 用作给`position:absolute`元素做爸爸

- 配合`z-index`

  `每个元素的z-index属性默认为auto,计算出的值为0,值大的在上面,小的在下面,可以为负数`

##### 绝对定位

脱离原来的位置,另起一层.

absolute 是相对于其祖元素第一个不是`position:static`定位的(一般是子绝父相)

##### 固定定位

一般用于广告页、回到顶部按钮等

### 字体截断(过长显示为...)

```css
.text-overflow {
	white-space: nowrap; /* 不换行 */
	overflow: hidden; /* 隐藏溢出 */
	text-overflow: ellipsis; /* 超出用省略号表示 */
}
```

非常有意思的一点在于`text-overflow:ellipsis`如果没生效,可以查看一下 display 属性是否没有设置成 block.

### display 属性

一般在用`display:flex;`进行布局时,有时会发现子元素并没有按照想象的来,可以检查子元素的样式的`display`属性是否是有改动,`flex-shrink: 0;` /_ 防止高度被压缩 _/
