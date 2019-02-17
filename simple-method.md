# 简单方法
将页面上所有图片的src属性设置为loading.gif，而图片的真实路径则设置在data-src属性中。
当页面滚动的时候计算图片位置和滚动的位置，当图片出现在浏览器视口内时，将图片的src属性设置为data-src的值。

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
function lazyload() {
  var images = document.getElementsByTagName('img');
  var n = 0; // 用于存储图片加载到的位置，避免每次都从第一张图片开始遍历 
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
window.addEventListener('scroll', lazyload(), false);

```


