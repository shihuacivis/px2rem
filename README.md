# px2rem

一款css的批量转换工具，可以将css中的为px单位属性批量转成rem单位。

## 使用场景

适用于不使用前端自动化工具的项目，将纯手工打造的以px来布局的css文件批量转换成rem布局的css。

## 如何转换

1. 安装node.js环境

2. 将需要转换的文件拖到`/css`目录中,如果css中包含不希望转换成rem的属性，比如`font-size`、`border`等属性，可以在对应的属性后面加上注释`/*px*/`,如下：
```css
.role {
  width: 200px;
  height: 200px;
  border: 1px solid black;/* px */
  margin: 10px;
}
```

3. 打开工具根目录下的index.js文件进行配置
```javascript
var config = {
  dir: '/css'    // 需要转换的css的目录
, unitPx: 100    // 预设的rem值 即 1rem = ? px
}
```

4. 使用系统命令行工具进入到工具的根目录，运行
```javascript
node index.js
```

5. 文件将默认输出到`/output`目录中

## 如何使用rem
rem自适应布局原理可以自行检索或参考[我的博文](http://www.shihua.im/2015/12/20/20151220_rem/)

1. 完成上述转换后，需要记住设置的`unitPx`的值（建议长期使用某个固定值），注意该值最好大于12（由于webkit内核不支持小于12px的字体）

2. 在页面加载时，我们可以根据设备实际的宽度与设计稿的比值，动态的去设置rem的值：
 ```javascript
  var pxUnit = 100;     // 在px2rem中预设rem的值 即 1rem = ? px
  var designWid = 750;  // 设计稿宽度
  var winWid = document.body.clientWidth;
  var winHei = document.body.clientHeight;
  var bl = winWid / designWid;
  document.querySelector('html').style.fontSize = (bl * pxUnit) + 'px';

```

3. 针对ios视网膜屏幕1px边框的问题（1px hairline），可以采用某宝的方案，**在设置字体大小之前**，根据屏幕像素比，更改页面的viewport：
 ```javascript
  var ua = navigator.userAgent.toLowerCase();
  if (/iphone|ipad|ipod/.test(ua)) {
    var sc = 1 / window.devicePixelRatio;
    document.getElementsByName('viewport')[0].content = 'initial-scale='+ sc +', maximum-scale='+ sc +', minimum-scale='+ sc +', user-scalable=no';
  }
 ```