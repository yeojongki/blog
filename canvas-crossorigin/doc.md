### 问题：调用`canvas`的`drawImage`方法时，因为跨域问题而无法绘制。

#### 解决方法：
- 在服务器上配置`Access-Control-Allow-Origin`
- 给`img`也就是需要绘制的图片配置`crossOrigin`属性 `img.crossOrigin = ''`，否则则会污染canvas而无法绘制。[具体可以查看MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/CORS_enabled_image)

> 若此时因为各种原因服务器不能配置该怎么办？这里举例一个遇到的场景，需要绘制微信头像，而头像是微信那边的服务器，本地无法处理。

#### 服务端解决方法：
- 第一种方法为配置反向代理
- 第二种方法是后端把图片转成`base64`再传到前端

> 上述两种方法都可以解决问题，但都需要后端配合，比较麻烦。有没办法完全在前端处理呢？

#### 终极解决方法：
大致思路是：
- 使用ajax获取到图片
- 使用 FileReader转成base64
- 再使用canvas处理base64格式的图片

#### 获取图片的base64
```js
function getBase64(imgUrl) {
  return new Promise(((resolve, reject) => {
    window.URL = window.URL || window.webkitURL;
    // 声明一个XMLHttpRequest
    const xhr = new XMLHttpRequest();
    // 获取图片
    xhr.open('get', imgUrl, true);
    xhr.responseType = 'blob';
    xhr.send();
    xhr.onload = function () {
      if (this.status === 200) {
        // 得到一个blob对象
        const blob = this.response;
        const oFileReader = new FileReader();
        oFileReader.onloadend = function (e) {
          const base64 = e.target.result;
          //拿到base64 传出结果
          resolve(base64);
        };
        oFileReader.onerror = function (e) {
          reject(e);
        };
        oFileReader.readAsDataURL(blob);
      }
    };
  }));
}
```

然后就可以拿到`base64`的图片用`drawImage`绘制了

> 文章转载于 https://alili.tech/archive/g6kghla5ugc/  有删改