---
title: element-ui使用心得
date: 2023-10-19 11:20:41
tags:
---

### Table

#### 合并表头

##### header-cell-style

这种方式可以直接返回一个样式:`return { display: "none" }`

使用方式是在table上绑定一个方法 

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

可以在里面加一些判断语句筛选出要改变的行列去进行改变,但是当进行合并时候用`row`取到的数据直接赋值`rowspan`跟`colsapn`是不行的会在拿到的对象身上加一个同名的属性,感觉是bug,在`header-cell-class-name`中就可以.

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

**注意:**在elementUI中，row-class-name、row-style、cell-class-name等属性要想生效必须使用**全局class**才能生效(`row-style`好像不用)

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
   <el-table :data="tableData" row-class-name="rowName"> 或者 :row-class-name="rowClassName" 绑定方法
    <el-table-column prop="name" label="姓名"></el-table-column>
    <el-table-column prop="date" label="日期"></el-table-column>
     <el-table-column prop="address" label="地址" width="800"></el-table-column>
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
.rowName{
 background:pink!important;
 color:deeppink
}
</style>

```

##### cell-class-name

```html
<template>
 <div>
   <el-table :data="tableData" cell-class-name="cellName"> 或者 :cell-class-name="cellClassName"
     <el-table-column prop="name" label="姓名"></el-table-column>
     <el-table-column prop="date" label="日期"></el-table-column>
     <el-table-column prop="address" label="地址" width="800"></el-table-column>
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
.cellName{
 background:brown!important;
 color:chartreuse
}
</style>

```

### 表单验证

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











