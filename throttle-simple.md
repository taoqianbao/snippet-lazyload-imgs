# 基于简单版本，经过节流防抖动的优化代码

基于简单的方法虽然没什么问题，但是性能较差，因为当页面滚动时，scroll事件会高频触发，这非常影响浏览器性能。
所以，这里要对lazyload函数进行函数节流（throttle）与函数去抖（debounce）处理。

函数节流（throttle）与函数去抖（debounce）

## html

```html
<img src="loading.png" data-src="image.png">
```

## css

```css
img { display: block; margin-bottom: 50px; }
```

## javascript

```javascript

function throttle(fn, delay, atleast) {
  var timeout = null,
    startTime = new Date();
  return function () {
    var curTime = new Date();
    clearTimeout(timeout);
    if (curTime - startTime >= atleast) {
      fn();
      startTime = curTime;
    } else {
      timeout = setTimeout(fn, delay);
    }
  }
}

function lazyload() {
  var images = document.getElementsByTagName('img'),
    n = 0;  //存储图片加载到的位置，避免每次都从第一张图片开始遍历 
  return function () {
    var seeHeight = document.documentElement.clientHeight;
    var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
    for (var i = n; i < images.length; i++) {
      if (images[i].offsetTop < seeHeight + scrollTop) {
        if (images[i].getAttribute('src') === 'loading.png') {
          images[i].src = images[i].getAttribute('data-src');
        }
        n = n + 1;
      }
    }
  }
}
lazyload(); //初始化首页的页面图片
window.addEventListener('scroll', throttle(lazyload(), 500, 1000), false);
```
