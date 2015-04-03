第一个:

安卓4.4以上的 webview ,也就是 App 内置浏览器不支持 input[type=file] 所以大家做上传图片的时候要特别注意这点,要跟客户说清楚啊. (郁闷...)

第二个:

安卓上4.4以下 好像不支持 new CustomEvent() 因为这个也搞得我很郁闷.

第三个:

还是上传的问题, 不要直接写简化的代码, 4.4以上是识别的,但是4.4以下会有问题.

Error:
`createObjectURLfun = (window.URL || window.webkitURL || {}).createObjectURL || function() {};`

Right:
```
createObjectURLfun = function(file) {
  if (window.navigator.userAgent.indexOf("Chrome") >= 1 || window.navigator.userAgent.indexOf("Safari") >= 1) {
    return window.webkitURL.createObjectURL(file);
  } else {
    return window.URL.createObjectURL(file);
  }
};
```

自己碰得几个坑,希望大家能避免, 大家碰到坑,也请通知我,让我也躲几个.