---
title: 鼠标无限移动 JS API Pointer Lock简介
---

#### Pointer Lock API的作用就是：Pointer Lock API可以让你的鼠标无限移动，脱离浏览器窗体的限制！

### Pointer Lock API有哪些具体API名称？

#### 目前，Pointer Lock API共支持3个属性，2个方法和2个事件，分别如下：

#### 3个属性
##### Document.pointerLockElement
##### Document.onpointerlockchange
##### Document.onpointerlockerror

#### 2个方法
##### Element.requestPointerLock()
##### Document.exitPointerLock()

#### 2个事件
##### pointerlockchange
##### pointerlockerror

#### 其中，2个事件和其中2个属性是一一对应的，因此，我们实际上需要了解的知识点是下面这些：
#### Document.pointerLockElement，以及Element.requestPointerLock()，Document.exitPointerLock()以及pointerlockchange和pointerlockerror事件。

#### 1. Document.pointerLockElement

#### 指当前页面触发鼠标无限滚动的元素，通常使用语法为：
``` bash
var element = document.pointerLockElement;
```
#### 返回的是一个DOM元素对象，如果当前页面是非鼠标锁定状态，则返回值是null。

#### 2. Element.requestPointerLock()

#### 可以让页面进入鼠标锁定状态（鼠标直接消失），鼠标无限滚动，坐标无限变化。通常使用语法为：
``` bash
var element.requestPointerLock();
```
#### 3. Document.exitPointerLock()

#### 让页面从鼠标锁定状态退出，通常使用语法为：
``` bash
document.exitPointerLock();
```
#### 浏览器默认支持按下ESC键退出鼠标锁定状态，但是用户有时候更习惯于点击取消等，此时就可以使用document.exitPointerLock()进行设置。

#### 4. pointerlockchange事件

#### 当页面鼠标锁定状态改变的时候触发。例如：
``` bash
document.addEventListener('pointerlockchange', function () {
    // ...
}, false);
```
#### 5. pointerlockerror事件

#### 当页面鼠标锁定失败的时候触发。例如当你试图同时锁定同一个页面的多个<iframe>时候，就会触发这个出错事件。

#### 点击这张图片,此时鼠标会立即消失，同时图片就会根据你的鼠标位置开始翻江倒海。而且鼠标的活动范围似乎没有边界，即使移动了一万个屏幕的宽度，我们的图片依然翻转个不停。这就是Pointer Lock API的特性表现。

###相关代码
``` bash
//html
<img id="image" src="img.jpg">

//js
var eleImage = document.getElementById('image');
if (eleImage) {
    // 起始值
    var moveX = 0, moveY = 0;
    // 图片无限变换的方法
    var rotate3D = function (event) {
        moveX = moveX + event.movementX;
        moveY = moveY + event.movementY;

        eleImage.style.transform = 'rotateX(' + moveY + 'deg) rotateY(' + moveX + 'deg)';  
    };

    // 触发鼠标锁定
    eleImage.addEventListener('click', function () {
        eleImage.requestPointerLock();
    });

    // 再次点击页面，取消鼠标锁定处理
    document.addEventListener('click', function () {
        if (document.pointerLockElement == eleImage) {
            document.exitPointerLock();
        } 
    });

    // 检测鼠标锁定状态变化
    document.addEventListener('pointerlockchange', function () {
        if (document.pointerLockElement == eleImage) {
            document.addEventListener("mousemove", rotate3D, false);
        } else {
            document.removeEventListener("mousemove", rotate3D, false);
        }
    }, false);
}
```
### 原理大致如下：
#### 点击图片，执行eleImage.requestPointerLock()让页面进入鼠标锁定状态，此时会触发'pointerlockchange'事件，于是，给页面绑定鼠标移动改变图片旋转的方法，为了避免重复绑定，我们借助document.pointerLockElement判断页面的锁定状态。最后，为了方便取消页面的锁定状态，我们在页面上绑定了点击事件，通过document.exitPointerLock()取消页面的锁定状态。

### 需要说明的：
#### event.movementX和event.movementY表示每次mousemove事件触发时候，距离上次移动的水平和垂直位置大小，而不是具体的某个坐标值。因此，需要和初始坐标不断的累加确定当前移动位置。

### Pointer Lock API的兼容性
#### 由于Pointer Lock API是与鼠标相关的API，因此所有移动端都不支持，因为没有必要支持。

### 对于桌面浏览器，Chrome，Firefox以及Edge浏览器都是支持的，并且现在使用可以不加私有前缀。IE并不支持，但这并不妨碍我们进行增强使用Pointer Lock API。