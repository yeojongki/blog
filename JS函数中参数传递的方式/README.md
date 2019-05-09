# JS函数中参数传递的方式
函数参数的传递方式为值传递，而这里又分原始类型和引用类型
- 原始类型：值传递。在函数体内修改参数值，不会影响到函数外部的值
- 引用类型：内存指针的值。在函数内部修改参数，将会影响到原始值

## 原始类型
```js
var num = 1
function add(num) {
  num += 1
}
add(num)
console.log(num) // 1
```
上述代码中，全局变量`num`是一个原始类型的值，传入到函数中为值传递。因此在**函数内部`num`为全局变量的复制（拷贝）**，复制后两个变量完全独立，无论怎么修改都不会影响到原始值（全局变量`num`）。

## 引用类型
```js
var obj = { name: 'obj' }
function change(obj) {
  obj.name = 'new obj'
}
change(obj)
console.log(obj) // new obj
```
上述代码中，全局变量`obj`是一个引用类型的值，传入到函数中的是原始值的地址，因此在函数中修改了`obj`，原始值也会改变。

**特别注意：** 如果函数内部修改的不是参数对象的某个属性，而是替换掉整个参数，这时不会影响到原始值。
```js
var outObj = { name: 'outObj' }
function change(obj) {
  obj = {name: 'innerObj'}
}
change(outObj)
console.log(outObj) // outObj
```
上述代码中，在函数`change`中，参数对象`obj`被整体替换成另一个值，此时不会影响到原始值。因为这个时候直接修改了`obj`的指针的值（在堆中开辟了一个新的对象），然后把这个新的地址赋给`obj`，并不会影响到全局变量`outObj`的指向。

## 参考：
- https://www.cnblogs.com/chenwenhao/p/7009606.html
- https://www.cnblogs.com/unclekeith/p/5792485.html