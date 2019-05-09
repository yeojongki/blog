# IOS 下 new Date() 时间转换存在格式上的问题

有一个日期字符串 `2019-02-11 0:0:0`

```js
// 使用Date API 转为时间对象
let str = '2019-02-11 0:0:0';
let ret = new Date(str);
console.log(ret); // Mon Feb 11 2019 00:00:00 GMT+0800 (中国标准时间)
```

IOS 下为：`Invalid Date`

**原因：ios 下解析不了日期字符串中的连接符`-`**
解决方法：将 `-` 改成 `/`

```js
let str = '2019-02-11 0:0:0';
str = str.replace(/-/g, '/'); // -> 改成这种格式2019/02/11 0:0:0
let ret = new Date(str);
console.log(ret); // Mon Feb 11 2019 00:00:00 GMT+0800 (中国标准时间)
```

然后就可以兼容了
