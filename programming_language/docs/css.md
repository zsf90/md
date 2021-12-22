# CSS 语言编程文档

## 垂直布局

### flex 布局
我们可以使用 flex 来实现垂直居中布局。
flex 布局青蛙游戏：[https://flexboxfroggy.com/](https://flexboxfroggy.com/ "青蛙游戏")
在线例子：[https://codepen.io/polk6/pen/aGwwMQ](https://codepen.io/polk6/pen/aGwwMQ)
css flex 布局教程 [https://www.cnblogs.com/polk6/p/css-flex.html](https://www.cnblogs.com/polk6/p/css-flex.html)

#### 父容器的属性：

- **display**: [`flex`|`inline-flex`]
- **align-items**: [`stretch`|`center`|`start`|`end`|`baseline`]
- **justify-items**: [`flex-start`|`flex-end`|`center`|`space-between`|`space-around`|`space-evenly`]
- **align-content**: [`stretch`|`start`|`end`|`center`|`space-between`|`space-around`] 针对对行。
- **flex-direction**: [`row`|`column`|`row-reverse`|`column-reverse`]
- **flex-wrap**: [`wrap`|`nowrap`|`wrap-reverse`]


#### 子元素的属性
- order: 子元素在父元素中显示的位置,默认值为 0,因此大于0的元素会显示在默认元素的后面，-1 可以让元素显示在最前面。
- flex : flex 是 flex-grow|flex-shrink|flex-basis三个的合体。

#### flex-flow 缩写
该缩写是 `flex-direction`和`flex-wrap`的缩写。
```css
div {
	flex-direction: row;
	flex-wrap: wrap;
}
```
替换为
```css
div {
	flex-flow: row wrap;
}
```

#### justify-content 属性
flex-direction: row; 时改属性控制水平对齐，如：左对齐（flex-start）|居中对齐（center）|右对齐（flex-end）|靠边居中对齐（space-between）| 居中平分对齐（space-around）。


## 媒体查询 @media
> CSS2 中引入了 @media 规则，它让为不同媒体类型定义不同样式规则成为可能。 <br>例如：您可能有一组用于计算机屏幕的样式规则、一组用于打印机、一组用于手持设备，甚至还有一组用于电视，等等。

> CSS3 中的媒体查询扩展了 CSS2 媒体类型的概念：它们并不查找设备类型，而是关注设备的能力。<br>媒体查询可用于检查许多事情，例如：

- 视口的宽度和高度
- 设备的宽度和高度
- 方向（平板电脑/手机处于横向还是纵向模式）
- 分辨率

### 媒体插叙语法
```css
@media not|only mediatype and (expressions) {
  CSS-Code;
}
```

### CSS3 媒体类型
|    值    |     描述    |
|-        -|-           -|
|all       |用于所有媒体类型设备|
|print|用于打印机|
|screen|用于屏幕设备|
|speech|用于大声“读出”页面的屏幕阅读器|

### 例子
下面的例子在视口宽度为 480 像素或更宽时将背景颜色更改为浅绿色（如果视口小于 480 像素，则背景颜色会是粉色）：
```css
@media screen and (min-width: 480px) {
  body {
    background-color: lightgreen;
  }
}
```

## Grid 网格布局

CSS 的网格布局重要用于大区域的布局。

### display: grid | inline-grid

使用 display: grid; 使一个元素成为网格布局。



### grid-template-columns 控制`网格列`的维度

```css
rid-template-columns: 60px 60px; /* 固定值 */
rid-template-columns: 1fr 60px; /* 一个自适应单位+一个固定值 */
rid-template-columns: 1fr 2fr; /* 两个都是自适应 */

```

语法：
```css
/* Keyword value */
grid-template-columns: none;

/* <track-list> values */
grid-template-columns: 100px 1fr;
grid-template-columns: [linename] 100px;
grid-template-columns: [linename1] 100px [linename2 linename3];
grid-template-columns: minmax(100px, 1fr);
grid-template-columns: fit-content(40%);
grid-template-columns: repeat(3, 200px);

/* <auto-track-list> values */
grid-template-columns: 200px repeat(auto-fill, 100px) 300px;
grid-template-columns: minmax(100px, max-content)
                       repeat(auto-fill, 200px) 20%;
grid-template-columns: [linename1] 100px [linename2]
                       repeat(auto-fit, [linename3 linename4] 300px)
                       100px;
grid-template-columns: [linename1 linename2] 100px
                       repeat(auto-fit, [linename1] 300px) [linename3];

/* Global values */
grid-template-columns: inherit;
grid-template-columns: initial;
grid-template-columns: unset;
```

