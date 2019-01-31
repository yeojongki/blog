# 微信浏览器中安卓手机采用媒体查询判断横竖屏时有误（注：移动端）

```css
//横屏时显示
@media all and (orientation: landscape) {
  #orientLayer {
    display: block;
  }
}

//竖屏时隐藏
@media all and (orientation: portrait) {
  #orientLayer {
    display: none;
  }
}
```

## 在一般浏览器下，表现正常。但在安卓手机实测中，出现了问题。

### 问题重现：

- 页面中有文本框
- 点击输入框，弹出输入法
- 网页可视区域减小，**安卓手机下会识别为横屏** `ios`下表现正常

### 解决方法

- 使用`javascript`的`orientation`和`orientationchange`事件判断。
- 当手机屏幕旋转时，会触发`orientationchange`事件。
- `orientation`为 `null||0||180` 的时候则为竖屏，为 `90||-90` 时则为竖屏。

```js
const orientation = window.orientation;
const $ = q => document.querySelector(q);
// 竖屏
if (orientation == null || orientation === 0 || orientation === 180) {
  $('#orientLayer').classList.remove('landscape');
} else if (orientation === 90 || orientation === -90) {
  // 横屏
  $('#orientLayer').classList.add('landscape');
}
```

```scss
#orientLayer {
  display: none;
  &.landscape {
    display: block;
  }
}
```
