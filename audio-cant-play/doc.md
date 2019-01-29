# IOS 下音频不能播放

**已知：微信下触发 `WeixinJSBridgeReady` 事件可以播放`audio`**

```js
/**
 * 微信自动播放背景音乐
 * @param {HTMLAudioElement} audio
 */
export const wxAutoPlay = audio => {
  try {
    audio.play();
    document.addEventListener(
      'WeixinJSBridgeReady',
      function() {
        audio.play();
      },
      { passive: false }
    );
  } catch (error) {
    console.warn(error);
  }
};
```

**于是有个业务场景为摇一摇，然后触发事件的时候并不能正常的播放音频。**

查阅 https://www.jb51.net/article/140649.htm ，文中说到:

> 在 IOS 中第一次调用 play 方法播放音频会被阻止，必须得等用户有交互动作，比如 touchstart，click 后才能正常调用，所以可以在摇一摇之前提醒用户点击一下开始游戏的按钮或者给用户一个弹窗，用户点击的时候播放一个超级短的无声音文件，之后替换 src，这样再调用 play 方法就可以了。

于是在页面加载的时候播放一个无声的音频，然后在摇一摇的时候替换到相应的音频`src`然后`play`。就可以正常的播放了。

```js
  // vuejs中
  mounted() {
    wxAutoPlay(this.audios['empty']); // 触发 `WeixinJSBridgeReady`事件播放无声音频
    // 摇一摇实例
    new DeviceShake(this.handleShake);
  },

  methods:{
    // 摇一摇触发处理
    handleShake() {
      // 播放摇一摇音乐
      this.playAudio('shake');
    },
    // 播放音乐
    playAudio(name) {
      this.audios['empty'].src = require(`@/assets/audio/${name}.mp3`);
      this.audios['empty'].play();
    }
  }

```

**2019/1/29 发现：元素 display:none 的时候，音频不能播放**

```js
// 此时调用 `wxAutoPlay`不能正常播放
<div class="wrap" style="display: none">
  <audio src="..."></audio>
<div>
```
