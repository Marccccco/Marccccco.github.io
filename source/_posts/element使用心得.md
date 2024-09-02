---
title: element使用心得
date: 2023-10-19 11:20:41
tags:
---

[toc]

## Element-ui

### Table

#### 合并表头

##### header-cell-style

这种方式可以直接返回一个样式:`return { display: "none" }`

使用方式是在 table 上绑定一个方法

```JavaScript
  <el-table
    :data="data.tableData"
    :header-cell-style="headFirst"
  >
    ......
  </el-table>
<script setup>
  const headFirst = ({ row, column, rowIndex, columnIndex })=>{

  }
</script>
```

绑定的方法会出入一个对象,有四个属性`row`是当前行的集合,`column`是当前行的当前列,`rowIndex`是当前行,`columnIndex`是当前列下标.

可以在里面加一些判断语句筛选出要改变的行列去进行改变,但是当进行合并时候用`row`取到的数据直接赋值`rowspan`跟`colsapn`是不行的会在拿到的对象身上加一个同名的属性,感觉是 bug,在`header-cell-class-name`中就可以.

##### header-cell-class-name

返回一个`class`

```javascript
  <el-table
    :data="data.tableData"
    :header-cell-class-name="headFirst"
  >
    ......
  </el-table>
<script setup>
  const headFirst = ({ row, column, rowIndex, columnIndex })=>{
    if (rowIndex === 0 && columnIndex === 0) {
      console.log(row[0]);
      row[0].rowSpan = 2;
    }
    if (rowIndex === 1) {
      if (columnIndex === 0 || columnIndex === 1) {
        return "ceshi";
      }
    }
  }
</script>
<style>
.ceshi {
  display: none;
}
</style>
```

这个是可以修改`row`的`rowsapan`和`colsapn`属性的,这俩属性主要是用于行列合并的.

##### span-method

这是表格内容合并行列的方法

```JavaScript
  <el-table
    :data="data.tableData"
    :span-method="arraySpanMethod"
  >
    ......
  </el-table>
<script setup>
  const arraySpanMethod = ({ row, column, rowIndex, columnIndex })=>{
    if (rowIndex % 2 === 0) {
      if (columnIndex === 0) {
        return [1, 2]
      } else if (columnIndex === 1) {
        return [0, 0]	// {rowspan:0,colspan:1,}
      }
    }
  }
</script>
```

返回一个数组或者对象,如果是数组那么第一个代表着占用的行数,第二个占用的列数,在使用过程中,假设你把第一列的第二行跟第三行合并了,要把第一列的第三行的`rowspan`设成`0`,要不它还是会占位值的.

#### 表格改变单独行样式

有三种方式:`row-class-name`、`row-style`、`cell-class-name`

**注意:**在 elementUI 中，row-class-name、row-style、cell-class-name 等属性要想生效必须使用**全局 class**才能生效(`row-style`好像不用)

##### row-style

```html
<template>
  <el-table stripe
    :data="tableData"
    style="width: 100%"
    :row-style="tableRowClassName">
    <el-table-column prop="date" label="日期" width="180"></el-table-column>
</template>
<script>
export default = {
 methods:{
   tableRowClassName({row, rowIndex}) {
     let styleRes = {
       "background": "#0181C2 !important"
     }
     if(this.multipleSelection.findIndex(item => item.id=== row.id) !== -1) {
       return styleRes
     }
   }
 }
}
</script>


```

##### row-class-name

```html
<template>
	<div>
		<el-table :data="tableData" row-class-name="rowName">
			或者 :row-class-name="rowClassName" 绑定方法
			<el-table-column prop="name" label="姓名"></el-table-column>
			<el-table-column prop="date" label="日期"></el-table-column>
			<el-table-column
				prop="address"
				label="地址"
				width="800"
			></el-table-column>
		</el-table>
	</div>
</template>
<script>
	export default = {
	 methods:{
	   rowClassName({row,columnIndex}){
	     if(row.date=="2022-05-01"){
	       return "rowName"
	     }
	   }
	 }
	}
</script>
<style>
	.rowName {
		background: pink !important;
		color: deeppink;
	}
</style>
```

##### cell-class-name

```html
<template>
	<div>
		<el-table :data="tableData" cell-class-name="cellName">
			或者 :cell-class-name="cellClassName"
			<el-table-column prop="name" label="姓名"></el-table-column>
			<el-table-column prop="date" label="日期"></el-table-column>
			<el-table-column
				prop="address"
				label="地址"
				width="800"
			></el-table-column>
		</el-table>
	</div>
</template>
<script>
	export default = {
	  methods:{
	   cellClassName({row,column,rowIndex,columnIndex}){
	     if(row.date!="2022-05-01" && column.label=="日期"){
	       return "cellName"
	     }
	   }
	 }
	}
</script>
<style>
	.cellName {
		background: brown !important;
		color: chartreuse;
	}
</style>
```

### 表单验证

1、form 组件上必须要有 ref
2、form-item 上必须要有 prop 属性
3、如果有 rule 属性,那么 prop 属性的值要跟 rule 属性的 key 值一样

```js
	openDialog() {
			this.dialogVisible = true;
			if(this.$refs['ruleFormRef']){
				this.$refs['ruleFormRef'].resetFields()	// 打开的时候清一波,关闭的时候清如果后续要使用表单的信息会为空
			}
		},
		Submit() {
			this.$refs['ruleFormRef'].validate(valid => {	// 关闭或者提交的时候验证一下
					if(valid){
						// doSomething
						this.dialogVisible = false;
					}
				}
			)
		},
```

#### 表单验证清空无效

准确的来说是赋值后不能重置表单项,`resetFields()`方法是重置为初始值,并移除校验结果,所以如果说在表单 mount 之前就赋值了，那么初始值就是赋值后的值,所以需要用到`nextTick`方法.
还有一种情况是如果表单项有绑定`v-if`属性,并且`v-if`为`false`的时候,那么表单清空也会失效,因为压根没有渲染,所以可以使用`v-show`代替`v-if`

## Element-plus

### 自定义组件表单模板

```vue
<template>
	<el-table :data="tableData" v-bind="$props">
		<!-- 定义一个动态footer列(插槽) -->
		<template v-if="$slots.header">
			<el-table-column v-bind="headerColumnProps" v-if="$slots.header">
				<template #default="scope">
					<!-- 跟#default插槽一个用法 -->
					<slot name="header" :scope="scope"></slot>
				</template>
			</el-table-column>
		</template>
		<el-table-column
			v-for="(item, index) in tableColum"
			:key="index"
			v-bind="item"
		>
		</el-table-column>
		<!-- 定义一个动态footer列(插槽) -->
		<template v-if="$slots.footer">
			<el-table-column v-bind="footerColumnProps" v-if="$slots.footer">
				<template #default="scope">
					<!-- 跟#default插槽一个用法 -->
					<slot name="footer" :scope="scope"></slot>
				</template>
			</el-table-column>
		</template>
	</el-table>
</template>

<script setup>
defineProps({
	tableData: Array,
	tableColum: Array,
	footerColumnProps: Object, // footer插槽的属性,例如 { label: '操作', width: '200px' }
	headerColumnProps: Object, // header插槽的属性,例如 { label: '操作', width: '200px' }
	// 添加一个接受任意属性的对象
	...Object,
});
</script>

<style lang="scss" scoped></style>
```

#### 表格模板如何展示一个 element 的元素

使用 formatter 属性跟 [h 函数](https://vue3js.cn/global/h.html) 相结合(vue2 中是`createElement`)

> h 函数定义: 返回一个“虚拟节点” ，通常缩写为 VNode: 一个普通对象，其中包含向 Vue 描述它应该在页面上呈现哪种节点的信息，包括对任何子节点的描述。用于手动编写 render

`h函数`接受三个参数：

- type 元素的类型
- propsOrChildren 数据对象, 这里主要表示(props, attrs, dom props, class 和 style)
- children 子节点
  一般要用`render`函数来使用,但是 el-table 中的`formatter`属性是可以直接解析的

例如:

```js
const App = {
	render() {
		return Vue.h("h1", {}, "Hello Vue3js.cn");
	},
};
Vue.createApp(App).mount("#app");
```

表格模板中展示一个 element 的元素:

```js
import { ElSwitch } from "element-plus";
tableColum:[
    ...
    {
        prop: "menuStatus",
			label: "状态",
			align: "center",
			formatter: (row) => {
				return showMenuStatus(row);
			},
    }
    ...
]
function showMenuStatus(row) {
	return h(ElSwitch, {
		modelValue: row.menuStatus, // v-model 绑定的数据
		"onUpdate:modelValue": (newStatus) => {
			// 监听 modelValue 的更新
			row.menuStatus = newStatus; // 更新数据
			changeStatus(row, newStatus); // 调用 changeStatus 方法
		},
		activeValue: 0,
		inactiveValue: 1,
		activeText: "正常",
		inactiveText: "停用",
	});
}
const changeStatus = async (row, newStatus) => {
    // do something
}
```
