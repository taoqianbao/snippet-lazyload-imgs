# 浏览器最新接口方案 - IntersectionObserver

目前有一个新的 IntersectionObserver API，可以自动"观察"元素是否可见。
用这种方法实现代码非常简洁，但是许多浏览器不支持。

* IntersectionObserver 传入一个回调函数，当其观察到元素集合出现时候，则会执行该函数。
* io.observe 即要观察的元素，要一个个添加才可以。
* io 管理的是一个数组，当元素出现或消失的时候，数组添加或删除该元素，并且执行该回调函数。


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

function query(selector) {
  return Array.from(document.querySelectorAll(selector));
}
var io = new IntersectionObserver(function (items) {
  items.forEach(function (item) {
    var target = item.target;
    if (target.getAttribute('src') == 'loading.png') {
      target.src = target.getAttribute('data-src');
    }
  })
});
query('img').forEach(function (item) {
  io.observe(item);
```
