---
title: kaikeba_0704
date:
tags:
---
kaikeba
<!-- more -->

### no-repeat
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .nihao {
            width: 300px;
            height: 200px;
            background-color: green;
            border: 20px dashed red;

            background: url("2.jpg");
            margin-bottom: 20px;
        }

        .han {
            width: 300px;
            height: 200px;
            background-color: green;
            border: 20px dashed red;

            /* 有no-repeat，在边框的下面不会有图片 */
            background: url("2.jpg") no-repeat;

            /* 字体会变色 */
            color: blue;

            /* padding部分也显示图片， 字围效果，图片从边框坐上角显示，但是文字会在padding后的未知显示 */
            padding: 20px;

            /* 设置图片从哪里开始显示 */

            /* 图片从边框左上角的位置显示 */
            /* background-origin: border-box; */

            /* 图片从内边距的位置显示 */
            /* background-origin: padding-box; */

            /* 图片从内容的位置显示 */
            /* background-origin: content-box; */

            /* 设置图片在哪里显示 */
            /* background-clip: border-box; */
            /* background-clip: padding-box; */
            /* background-clip: content-box; */

            /* 显示完整图片，padding处也显示 */
            /* background-size: contain; */

            /* 边框填满为止 */
            background-size: conver;
        }
    </style>
</head>

<body>
    <div class="nihao">hhhh, woyou bianshuaile</div>
    <div class="han">hhhh, woyou bianshuaile</div>
</body>

</html>
```


### 伪元素、伪类
```css
: 与 ::的区别
```

### 魔方如何进行鼠标拖动旋转, (3D展示)
