# 为什么['1', '2', '3'].map(parseInt)会返回[1, NaN, NaN]

```js
['1', '2', '3'].map(parseInt);
```

乍一看，感觉应该会输出`[1, 2, 3]`, 实际上却是`[1, NaN, NaN]`。为什么会这样呢？首先看下 `MDN` 上 `parseInt` 的函数。

## parseInt

### 语法

> parseInt(string, radix);

`string`为要被解析的值，如果不是字符串，则调用`tostring`转化为字符串。字符串开头的空白字符将会被忽略。

`radix`一个介于 `2` 和 `36` 之间的整数，表示上述字符串的基数。比如参数`"10"`表示使用我们通常使用的十进制数值系统。当未指定基数时,通常将值默认为 `10`。

`返回值`: 返回解析后的整数值。 如果被解析参数的第一个字符无法被转化成数值类型，则返回 `NaN`。

**radix 不同返回的值会不同，举个 🌰**：

```js
parseInt('11'); // 返回11 -> radix没有传值，默认为10进制，十进制的11等于11
parseInt('11', 2); // 返回3 -> radix为2，二进制的11等于3
parseInt('11', 10); // 返回11 -> radix为10，同第一种情况
```

**特别注意返回值为 NaN 的情况**

- 第一个参数不能转换成数字。

- 第二个参数不在 2 到 36 之间。

题目中我们传入的第一个参数均能转成数字, 所以出现`NaN`的原因为第二种情况

## Array.prototype.map

### 语法

```js
array.map(function callback(currentValue,index,arr), thisValue)
```

[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 上描述：`map` 方法会给原数组中的每个元素都按顺序调用一次 `callback` 函数。**callback 函数会被自动传入三个参数：数组元素，元素索引，原数组本身。**

再仔细一看，原本我们以为对`parseInt` 的三次调用是这样：

```js
parseInt('1');
parseInt('2');
parseInt('3');
```

然而实际上是这样调用的：

```js
// 这里的arr指的是原数组['1','2','3']
// callback 中自动传入了三个参数
parseInt('1', 0, arr);
parseInt('2', 1, arr);
parseInt('3', 2, arr);
```

答案应该很明显了~

## 题目推导

```js
['1', '2', '3'].map(parseInt);
```

相当于

```js
[0] = parseInt('1', 0) // 十进制的1 -> 1
[1] = parseInt('2', 1) // radix不在2-36范围内 -> NaN
[2] = parseInt('3', 2) // 二进制数没有等于3的 -> NaN
```

## 如何获取预期的值[1, 2, 3]

```js
['1', '2', '3'].map(value => {
  return parseInt(value);
});
// or
['1', '2', '3'].map(Number);
```

### 参考文章

- https://blog.csdn.net/u010703975/article/details/50261441
- https://blog.csdn.net/lxcao/article/details/52781504
