# front end

## Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>

    <style>
        * { 
            padding:0;  /* 去除默认填充 */
            margin: 0;  /* 去除默认边距 */
        }

        #header {
            height: 50px;
            background: #E83828;
        }

        #header .head {
            width: 1005px;
            height: 50px;
            background: #D1D3D6;
            margin: auto;
        }

        #banner {
            height: 500px;
            background: slateblue;
        }

        #category {
            width: 1005px;
            height: 200px;
            margin: auto;
            background: #FF359A;
        }

        #category .item {
            width: 125px;
            height: 165px;
            padding-right: 25px;
            padding-left: 25px;
            padding-bottom: 25px;
            padding-top: 10px;
            border-right: 1px dashed black;
            float: left;  /* 浮动元素 */
        }

        #category .item.first {
            padding-left: 0;
        }

        #category .item.last {
            padding-right: 0;
            border: 0;
        }

        #case {
            height: 490px;
            background: #eeeeee;
        }

        #case .title-text {
            width: 1005px;
            margin: auto;
            padding-top: 20px;
            padding-bottom: 10px;
            font-size: 45px;
        }

        #case .item-wrapper {
            width: 1000px;
            margin: auto;
            overflow: auto;
        }

        #case .item-wrapper .item {
            width: 320px;
            height: 330px;
            background: #9ACD32;
            float: left;
        }

        #case .item-wrapper .item  .mg {
            margin-left: 20px;
            margin-right: 20px;
        }

        #case p {
            width: 1005px;
            height: 40px;
            margin-left: auto;
            margin-right: auto;
            margin-top: 15px;
            line-height: 40px;
            text-align: center;
            font-size: 30px;
            color: dimgray;
        }

    </style>

</head>
<body>
    <div id="header">
        <div class="head"></div>
    </div>

    <div id="banner"></div>

    <div id="category">
        <div class="item first"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item last"></div>
    </div>

    <div id="case">
        <div class="title-text">
            Case
        </div>
        <div class="item-wrapper">
            <div class="item"></div>
            <div class="item mg"></div>
            <div class="item"></div>
        </div>
        <p>查看更多+</p>
    </div>
</body>
</html>
```

## HTML

## CSS

### background

```css
background-color /* 背景色 */
background-image /* 背景图片 */
background-repeat /* 背景图平铺方式 */

background: gray url(xxx/xx.png) np-repeat;

background: url(xxx/xx.png) repeat-y;

background: gray;
```

### border

```css
border-width /* 边框宽度 */
border-style /* 边框样式 */
border-color /* 边框颜色 */

border: 1px solid #D3f402;

border: 3px dashed;
```

### font

```css
font-style: italic; /* 斜体 */
font-weight: bold; /* 加粗 */
font-family: arial, sans-serif; /* 字体种类 */
font-size: 20px; /* 字号大小 */
line-height: 35px; /* 行高 */

font: italic bold 20px/35px arial, sans-serif, "微软雅黑";

font: 20px "微软雅黑";
```

### margin

当前控件和父控件的边距

```css
margin-top
margin-right
margin-bottom
margin-left

margin: 10px 15px 10px 15px; /* 上 右 下 左 */
margin: 10px 15px 15px; /* 上 左右 下 */
margin: 10px 15px; /* 上下 左右 */
margin: 10px; /* 上下左右 */
```

### padding

当前控件的内边距，即控件中内容距离控件的边缘的距离

```css
padding-top
padding-right
padding-bottom
padding-left

padding: 10px 15px 10px 15px; /* 上 右 下 左 */
padding: 10px 15px 15px; /* 上 左右 下 */
padding: 10px 15px; /* 上下 左右 */
padding: 10px; /* 上下左右 */
```

### color

```css
color: DarkGoldenRod;

color: rgb(xxx, xxx, xxx)

color: #B8860B
```

## JavaScript