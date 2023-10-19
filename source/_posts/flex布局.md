---
title: flex布局
---



[toc]

### flex布局

#### flex-direction

`flex-direction`设置主轴和交叉轴,`row`(起始线为左终止线在右)、`row-reverse`(起右终左)、`column`(起上终下)、`column-reverse`(起下终上)

#### flex-wrap

`flex-wrap`是设置项目太大无法全部显示在一行中时的应对

`nowarp`:这是默认值,它们将会缩小以适应容器,如果子元素无法缩小,则会导致溢出.

`wrap`:如果项目太大无法在一行显示,那么会换行显示.

#### flex-flow

这是`flex-direction`和`flex-wrap`的简写模式,用法为第一个值为`flex-direction`,第二值为`flex-wrap`

#### Flex属性简写

##### flex-basis

`flex-basis`定义了该元素的空间大小,默认属性是`auto`,如果元素设置了宽度,那么就是值就是该元素设置的宽度.

##### flex-grow

`flex-grow`若被赋值为一个正整数,flex元素会以`flex-basis`为基础,沿主轴方向增长尺寸.值越大,分的`可用空间`就越多.

##### flex-shrink

`flex-shrink`跟`flex-grow`相反,处理的收缩问题,值越大,收缩的越多.

##### 简写形式

`Flex` 简写形式允许你把三个数值按这个顺序书写 — `flex-grow`，`flex-shrink`，`flex-basis`

有几个预定义的值:

`flex:initial`相当于`flex:0 1 auto`会收缩但不会拉伸.

`flex:auto`等同于`flex:1 1 auto`既可以拉伸也可以收缩.

`flex:none`等同于`flex:0 0 auto`既不能拉伸也不能收缩,但如果元素没设定`width`那么`flex-basis:auto`就可以进行布局.

`flex:1`或者`flex:2`相当于`flex:1 1 0`或者`flex:2 2 0`.

#### align-items

`align-items`属性可以使元素在交叉轴方向对齐.
`stretch`:初始值,flex元素会默认被拉伸到最高元素的高度(在元素没有设置width的情况下).
`flex-start`:使flex元素按flex容器的顶部对齐.
`flex-end`:使它们按flex容器的下部对齐.
`center`:使它们居中对齐.

##### align-self

跟`align-item`属性一样,一个是控制全部元素,一个是控制单个元素的.

#### justify-content

`justify-content`属性是用来使元素在主轴方向上对齐的.

```
justify-content: flex-start\start; /* 初始值,从容器起始线排列. */
justify-content: flex-end\end; /* 从容器终止线排列. */
justify-content: center; /* 中间排列. */
justify-content: space-between; /* 均匀排列每个元素
                                   首个元素放置于起点，末尾元素放置于终点 */
justify-content: space-around; /* 均匀排列每个元素
                                   每个元素周围分配相同的空间 */
justify-content: space-evenly; /* 均匀排列每个元素
                                   每个元素之间的间隔相等 */
justify-content: stretch; /* 均匀排列每个元素
                                   'auto'-sized 的元素会被拉伸以适应容器的大小 */
```

#### gap

`gap`是`row-gap`和`column-gap`的简写

#### align-content

如果有一个折行的多条flex项目的flex容器适合用这个.

````
align-content: flex-start	
align-content: flex-end
align-content: center
align-content: space-between	/* 平分可用空间 */
align-content: space-around	/* 每行的元素上下/左右间隔同等距离(两行之间的距离是*2的) */
align-content: stretch
align-content: space-evenly /* 每行上下/左右间隔一样
````

#### 弹性盒子

可以给flex单个元素设置margin值为auto,以此来实现左右/上下分割的效果











