---
title: jQuery实现很好的弹出层效果
---

### 先引入js
```js
<script type="text/javascript" src="../js/tinybox.js"></script>
```
### js使用插件方法弹出弹出层
```bash
//加载一个静态页面
T$('click_test1').onclick = function(){TINY.box.show('blank-for-test.html',1,300,150,1)}

//加载一张图片
var content2 = "<img width='640' height='466' src='http://image.zhangxinxu.com/image/study/s/mm10.jpg' />";
T$('click_test2').onclick = function(){TINY.box.show(content2,0,0,0,1)}

//加载flash
var content3 = "<object classid='clsid:D27CDB6E-AE6D-11cf-96B8-444553540000' width='550' height='400'><param name='movie' value='../flash/as3_clock_2.swf' /><param name='quality' value='high' /><param name='wmode' value='opaque' /><embed height='400' width='550'  src='../flash/as3_clock_2.swf' type='application/x-shockwave-flash'></embed></object>";
T$('click_test3').onclick = function(){TINY.box.show(content3,0,0,0,1)}

//弹出层3秒后消失
var content4 = "该浮动层将在3秒钟内消失。";
T$('click_test4').onclick = function(){TINY.box.show(content4,0,0,0,0,3)}
```