# forEach 中使用 await

## 场景：循环 ajax 获取数据

期待结果：start 1 2 3 end

```js
// simulate ajax
function ajax(num) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(num)
    }, 1000)
  })
}

function loop(arr) {
  console.log('start')
  arr.forEach(async item => {
    let result = await ajax(item)
    console.log(result)
  })
  console.log('end')
}

loop([1, 2, 3]) // start end 1 2 3
```

## 为什么不是期望结果

> 参考 [MDN 上 forEach 的 polyfill](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach#Polyfill)

```js
// ... 省略
while (k < len) {
  // ... 省略
  callback.call(T, kValue, k, O)
}
// ... 省略
```

可以看到 forEach 只是执行了回调函数，并不会处理异步的情况

## 解决方法

### for

```js
// 修改 loop 函数
async function loop(arr) {
  console.log('start')
  for (let i = 0; i < arr.length; i++) {
    let item = arr[i]
    let result = await ajax(item)
    console.log(result)
  }
  console.log('end')
}
loop([1, 2, 3]) // start 1 2 3 end
```

### for...of

```js
// 修改 loop 函数
async function loop(arr) {
  console.log('start')
  for (const item of arr) {
    let result = await ajax(item)
    console.log(result)
  }
  console.log('end')
}
loop([1, 2, 3]) // start 1 2 3 end
```

> 参考： 
> https://juejin.im/post/5cb5a669e51d456e845b41f6#heading-8
> https://juejin.im/post/5cf7042df265da1ba647d9d1