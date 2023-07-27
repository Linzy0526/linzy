# 小程序中canvas放置图片

> 小程序可以将网络图片、本地图片画在canvas画布中，一般有两种方式 `context.drawImage` 和 `context.createPattern` 去实现


### 图片资源准备

两种方式的图片参数都是需要将图片地址转换为 `ImageElement` 对象

``` js
wx.createSelectorQuery();
    query
      .select("#canvas")
      .fields({ node: true, size: true })
      .exec(async (res) => {
        const canvas = res[0].node;
        // 小程序需要通过这种方式创建图片对象
        // TODO 浏览器端暂未去试用 new Image()
        const image = canvas.createImage();
        image.src = src;
        image.onload = () => {
            // 这里开始执行画图过程
        };

      })
```

### 画图片

#### context.drawImage

该方法可以方便地将图像绘制在画布上。这个方法非常灵活，可以用于绘制整个图像或者裁剪部分图像


##### 语法

``` js
context.drawImage(image, x, y);
context.drawImage(image, x, y, width, height);
context.drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);
```
根据不同传参个数，对应参数的作用不用（这个感觉不合理，顺序？）

##### 参数解释
- image: 要绘制的图像，可以是Image对象、Canvas对象或Video对象。
- x: 目标图像的起始X坐标。
- y: 目标图像的起始Y坐标。
- width: 图像在目标画布上的宽度（可选）。
- height: 图像在目标画布上的高度（可选）。
- sx: 源图像的起始X坐标（可选）。
- sy: 源图像的起始Y坐标（可选）。
- sWidth: 源图像的宽度（可选）。
- sHeight: 源图像的高度（可选）。
- dx: 目标图像的起始X坐标（可选）。
- dy: 目标图像的起始Y坐标（可选）。
- dWidth: 目标图像的宽度（可选）。
- dHeight: 目标图像的高度（可选）。

##### 缺点

裁剪尺寸和画的尺寸不一致会导致效果图变形

#### ctx.createPattern

该方法可以方便地将图像绘制在画布上。这个方法非常灵活，可以用于绘制整个图像或者裁剪部分图像，还可以进行缩放和平铺等操作


##### 语法

``` js
const pattern = context.createPattern(image, 'repeat');
context.fillStyle = pattern;
context.fillRect(0, 0, canvas.width, canvas.height);
```

它是将图片作为纹理的形式，贴在需要画的闭合空间内

##### 参数解释
- image: 要绘制的图像，可以是Image对象、Canvas对象或Video对象。
- repeat: 平铺方式repeat、no-repeat、repeat-x、repeat-y

##### 缺点

如果场景是运动的时候，这个图片效果内部会动，需要进行特殊处理