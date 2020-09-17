# css

## 一些补充

- css中引入别的css文件

```
@import 'css文件路径'
```

- 包括边框和内边距的百分百自适应布局

```
box-sizing: border-box;
```

加入这一属性后，盒模型的`width:100%`就包括了边框和内边距外加内容，而不是之前的只包括内容区域。

## 伪类选择器

- first-child

例如p:first-child{color:red;}，这个语句选择的其实是先找到p元素，再看它的父元素的第一个子元素是不是p，如果是就渲染它。

## 属性选择器

属性选择器可以根据元素的属性及属性值来选择元素

- 简单属性选择

如果希望选择有某个属性的元素，而不论属性值是什么，可以使用简单属性选择器。例如

```css
*[title]{color:red}
a[href]{color:red}
```

这里没有指定具体属性值，不过也要有值，而不是说可以具有这个属性就行

- 具体属性选择

即指定属性值来进行选择，如

```css
a[href="#hehe"]
input[name="xx"]
```

当然可以多个属性-值选择器进行叠加来进一步缩小范围，不能包含空格那种，例如有一个元素为

```html
<div class="a b c"></div>
```

这里有三个类，如果是用类选择器随便指定一个都能选中，但是如果写成div[class="a"]这种是不行的，必须要这样写div[class="a b c"]

- 根据部分属性值来选择

面说了属性值必须要具体，不过也有不具体的写法例如上面的div写成，div[class~="a"]{color:red;}这样class属性中只要包含a就会被选中。即使用波浪号（~）在属性名与等号间。

## 字体样式

| 字体样式    | 含义和值                                                     |
| ----------- | ------------------------------------------------------------ |
| font-family | 字体类型，如微软雅黑，Arial，Times，New Roman                |
| font-size   | 字体大小                                                     |
| font-weight | 字体粗细，属性有normal（默认），lighter（较细），bold（较粗），bolder（很粗），也可以用px这种单位表示 |
| font-style  | 字体斜体样式，normal（默认值），italic（斜体，这是一个属性不是每个字体都具有），oblique（将没有italic属性的字体倾斜） |

## 段落样式

| 段落样式        | 含义和值                                                     |
| --------------- | ------------------------------------------------------------ |
| text-decoration | 下划线，删除线，顶划线。属性none（默认），underline（下划线），line-through（删除线），overline（删除线） |
| text-transform  | 文本大小写。none（默认），uppercase（转换成大写），lowercase（转换成小写），capitalize（将每个单词的首字母大写，其余无变化） |
| text-indent     | 首行缩进，缩进字体px的整数倍就是缩进整数个字                 |
| text-align      | 文字水平对齐，left（默认值，左对齐），center（居中对齐），right（右对齐），在除了inline属性的元素中定义 |
| line-height     | 主要用处就是line-height=height使单行文本垂直方向居中         |

## 边框样式

border-style

- solid，实线
- dashed，虚线

## 背景样式

| 背景样式                          | 含义和值                                                     |
| --------------------------------- | ------------------------------------------------------------ |
| background-color                  | 背景颜色                                                     |
| background-image：url（图像路径） | 背景图像                                                     |
| background-position               | 图像出现位置，可以用像素X  Y表示，x表示距离该元素左边Xpx，y表示距离该元素上面Ypx;也可以用关键字例如:top left;right center;bottom center；效果如英文所示 |
| background-repeat                 | 背景重复样式，repeat（默认值，表示在水平竖直方向都平铺），no-repeat（不平铺），repeat-x（水平方向平铺），repeat-y（竖直方向平铺） |
| background-attachment             | 背景固定样式，默认为scroll表示背景图像随对象滚动而滚动，fixed表示固定不动，（该属性在不同的浏览器中可能会有冲突） |
| background:url(..) no-repeat 0 0  | 背景图片，是否平铺，出现位置等各个属性的简写                 |

## 列表样式

ul无序列表，list-style-type:none(无符号），disc（默认值，实心圆），circle（空心圆），square（实心正方形）

## 表格样式

- 表格边框合并

border-collapse：separate（默认值，边框分开不合并），collapse（边框合并，如果相邻共用一个边框），该属性在tabel标签设置即可

- 表格边框间距

border-spacing：Xpx Ypx,x为横向间距，y为纵向间距

- 表格标题位置

caption-side：top（顶部），bottom（底部）

## 移动端适配

### 视口设置

```html
<meta name="viewport" content="width=device-width,initial-scale=1.0">
```

- 这个标签加上去之后pc端切换为移动端像素不变，将它删除掉之后会进行缩放
- 这个标签的name就是表示视口的意思，content里width表示视口的宽度，device-width加上之后会随着不同的设备改变相应的视口宽度。Initial-scale表示缩放比例，1.0表示不缩放，大于1是放大，小于1就是缩小。
- 当然这整个标签主要是在移动端中发生作用。

### rem设置

- rem就是相对于根元素字体大小的像素，即相对于html元素

- 针对不同的手机屏幕，设置不同的html的font-size字体大小。主要是通过媒体查询

```css
@media screen and (min-width:320px) {
    html{
        font-size: 9px;
}}
@media screen and (min-width:375px) {
    html{
        font-size: 10px;
}}
@media screen and (min-width:414px) {
    html{
        font-size: 11px;
}}
```

利用js动态计算，一般是拿屏幕宽度/10来确定html字体大小（最优）。

- 将所有需要适配的图片/元素/字体大小设置为以rem为单位。可以通过各种方式自行了解

## flex布局

### 基本认识

![image-20200707092030208](C:\Users\22140\Desktop\web-study\note\assets\img\image-20200707092030208.png)

### flex布局模型

![image-20200707092222659](C:\Users\22140\Desktop\web-study\note\assets\img\image-20200707092222659.png)

![image-20200707092238076](C:\Users\22140\Desktop\web-study\note\assets\img\image-20200707092238076.png)

### flex-container属性

- 一旦父元素开启了display:flex，那么它的子元素不论之前是块级元素还是行级元素现在的排布方式默认都是沿着main axis（主轴）从main start 到main end。
- flex-direction：决定主轴的方向

| 属性值         | 含义                                               |
| -------------- | -------------------------------------------------- |
| row            | 默认方式，主轴从左到右                             |
| row-reverse    | 将主轴方向反转，将会改变元素排布方式。主轴从右到左 |
| column         | 将主轴由原来的行形式改为列形式。主轴从上到下       |
| column-reverse | 列形式取反，主轴从下到上                           |

- justify-content：决定flex items主轴的对齐方式

| 属性值        | 含义                                                         |
| ------------- | ------------------------------------------------------------ |
| flex-start    | 默认值，与main start对齐                                     |
| flex-end      | 与main end对齐                                               |
| center        | 居中对齐                                                     |
| space-between | flex items之间的距离相等，与main start，main end两端对齐     |
| space-evenly  | flex items之间的距离相等，flex items与main start，main end之间的距离等于flex items之间的距离。 |
| space-around  | flex items之间的距离相等，flex items与main start，main end之间的距离是flex items之间距离的一半。 |

- flex-wrap：决定了flex-container是单行显示子元素还是多行显示

默认情况下flex items都是在一行显示，如果不够摆flex就会自动对其进行压缩。wrap-reverse基本不用。

| 属性值       | 含义                                 |
| ------------ | ------------------------------------ |
| nowrap       | 默认，单行                           |
| wrap         | 多行                                 |
| wrap-reverse | 多行，不过cross start与cross end相反 |
| flex-flow    | flex-direction\|\| flex-wrap的简写   |

- align-items：决定flex items在cross axis上的对齐方式

注意这里的center就是垂直居中的效果，baseline如果是多行文本的话以第一行为准。

![image-20200707093411374](C:\Users\22140\Desktop\web-study\note\assets\img\image-20200707093411374.png)

- align-content：决定了多行flex items在cross axis上的对齐方式，用法与justify-content类似。

![image-20200707093510157](C:\Users\22140\Desktop\web-study\note\assets\img\image-20200707093510157.png)

### flex-items属性

- order属性：改变flex items默认的排布顺序

很少用order去改变默认排布顺序

![image-20200707093613890](C:\Users\22140\Desktop\web-study\note\assets\img\image-20200707093613890.png)

- align-self：flex items在cross axis方向上的排布方式，也很少用

![image-20200707093720379](C:\Users\22140\Desktop\web-study\note\assets\img\image-20200707093720379.png)

- flex-grow：根据需要适当增大flex items在主轴方向上的宽度

![image-20200707093748459](C:\Users\22140\Desktop\web-study\note\assets\img\image-20200707093748459.png)

- flex-shrink：收缩flex items元素

<img src="C:\Users\22140\Desktop\web-study\note\assets\img\image-20200707093842975.png" alt="image-20200707093842975" style="zoom:100%;" />

- flex-basis：决定主轴上的flex items在main axis方向上的base size，也很少用

![image-20200707094028290](C:\Users\22140\Desktop\web-study\note\assets\img\image-20200707094028290.png)

- flex：是flex-grow||flex-shrink||flex-basis的缩写，可以写一个，两个或三个

![image-20200707094139400](C:\Users\22140\Desktop\web-study\note\assets\img\image-20200707094139400.png)[]()

# css3

## 属性选择器

- ```css
  E[attr^="lvye"]
  ```

  选取了元素E，其中E元素定义了属性attr，attr属性值是以lvye开头的任何字符串。

- ```css
  E[attr$="lvye"]
  ```

  选取了元素E，其中E元素定义了属性attr，attr属性值是以lvye结尾的任何字符串

- ```css
  E[attr*="lvye"]
  ```

  选取了元素E，其中E元素定义了属性attr，attr属性值任意位置是包含了lvye的任何字符串

## 结构伪类选择器

| 选择器           | 含义                                                         |
| ---------------- | ------------------------------------------------------------ |
| E:first-child    | 搜寻e元素找到其父级的第一个子元素，如果是e就渲染             |
| E:last-child     | 跟上述同理，只不过是父级的最后一个子元素                     |
| E:nth-child(n)   | n可以为数字/odd/even，odd代表偶数，even代表奇数：同理只不过是替换为第n个子元素 |
| E:only-child     | 有限制条件，e元素的父级下必须只有e元素一个子元素才会被渲染   |
| E:first-of-type  | 先找到e元素，再找到其父级下的第一个类型是e元素的元素进行渲染，注意跟类型一的区别是一个是第一个类型是e，一个是第一个子元素是e |
| E:last-of-type   | 同上述，不过是最后一个类型，在只存在一个该种类型元素时，first-of-type和last-of-type选中的其实是同一个 |
| E:nth-of-type(n) | n可以为数字/odd/even，odd代表偶数，even代表奇数：同理只不过选中的是该种元素类型的第n个 |
| E:only-of-type   | e元素的父级下e元素这种类型只有一个会将其选中并进行渲染，父元素可以有多个类型的子元素 |
| :root            | 选择文档的根元素。在HTML中，根元素永远是HTML                 |
| :not()           | 选择某个元素之外的所有元素，比较常用例如 ul li:not(.a)选中ul下的li除了class为a的那个li |
| :empty           | 用域选择一个不包含任何子元素或内容的元素，也就是选择一个内容为空白的元素。例如td:empty选中为空的那个表格td |

## ui元素状态伪类选择器

该选择器一般都是配合表单元素使用，其中::before,::after伪元素选择器要配合content来使用，默认的display为inline

| 选择器       | 作用                                  |
| ------------ | ------------------------------------- |
| E:focus      | 指定元素获得光标焦点时使用的样式      |
| E:checked    | 选择E元素中所有被选中的元素           |
| E::selection | 改变E元素中被选择的网页文本的显示效果 |
| E:enabled    | 选择E元素中所有“可用”元素             |
| E:disabled   | 选择E元素中所有“不可用”元素           |
| E:read-write | 选择E元素中所有“可读写”元素           |
| E:read-only  | 选择E元素中所有“只读”元素             |
| E::before    | 在E元素之前插入内容                   |
| E::after     | 在E元素之后插入内容                   |

## 文字

### text-shadow

- 文字阴影

```css
text-shadow:x-offset  y-offset  blur  color；
```

- x-offset，水平偏移距离
- y-offset，垂直偏移距离
- blur，（模糊距离）表示阴影的模糊程度，单位可以是px、em或者百分比等。blur值不能为负。如果值越大，则阴影越模糊；如果值越小，则阴影越清晰。当然，如果不需要阴影模糊效果，可以吧blur值设置为0；
- color，（阴影的颜色）表示阴影的颜色

### text-stroke

- 文字描边

text-stroke:宽度值 颜色值；小技巧，将字体颜色透明再设置描边可以创建镂空的文字

### text-overflow

- 文字溢出属性
- ellipsis，当对象内文本溢出时显示省略标记（…）
- clip，当对象内文本溢出时不显示省略标记（…），而是将溢出的部分裁切掉
- text-overflow:ellipsis; overflow:hidden; white-space:nowrap（强制文本在一行显示）;这三个属性必须一起使用才有效果这个属性，因为单单text-overflow属性只是表示文字溢出显示形式。

### word-wrap

- 强制换行
- normal，默认值，文本自动换行
- break-word，“长单词”或“URL地址”强制换行；汉字一般不用因为系统能识别单个汉字

### font-face

- 嵌入字体

- 使用自定义字体需要两步，一使用@font-face方法定义字体名称；二使用font-family属性引用该字体；例如

  ```css
  @font-face { font-family: myfont;  /*定义字体名称为myfont*/
      		src: url("../font/Horst-Blackletter.ttf");}
  div{font-family:myfont}，
  ```

  不过一般开发不用因为汉字自定义包太大

### font-size-adjust

- 属性值为一个aspect值
- 出现这个属性的根本原因是不同字体类型的字一般设置相同的font-size它的高度是不一样的，每种类型字体都有它的aspect值，例如某种类型字体的字母x，aspect =（x-height）/（font-size），不同类型字体的aspect值是不一样的，所以为了避免修改字体类型页面崩溃，通过计算可得（aspect1）/（aspect2）=（font-size2）/（font-size1），这样通过查询得知两种字体aspect值之后，在已知某一种字体的font-size时可以得到另一个字体font-size值且两者高度相等。

## 颜色

### opcity

- 透明度，取值范围为0.0~1.0，0.0表示完全透明，1.0表示完全不透明（默认值），该透明度会将元素整体都变透明

### rgba

- a是透明度的意思，跟opacity差不多，取值范围也是0.0~1.0，不过rgba不会影响元素中的内容以及子元素的透明度

### 线性渐变

- 指在一条直线上进行渐变，用得较多
  background:linear-gradient(方向，开始颜色，结束颜色);
- 方向取值有两种，一种是关键字，一种是角度(deg)
  属性值	    对应角度	  说明
  to top	       0deg	       从下到上
  to right	     90deg	    从左到右
  to bottom	180deg	  从上到下（默认值）
  to left	        270deg	  从右到左
  to top left		                右下角到左上角（斜对角）
  to top right	                  左下角到右上角（斜对角）
  颜色渐变可以有多个渐变，不局限两个，例如background:linear-gradient(to right, red, orange,yellow,green,blue,indigo,violet);

### 径向渐变

- 是一种从起点到终点颜色从内到外进行圆形渐变（从中间向外拉，像圆一样）

- background:radial-gradient(position ,shape size,start-color,stop-color)，position，shape size都可以省略都有默认值

- position：定义圆心位置
- shape定义形状（圆形或椭圆）
- size定义大小

## 边框

### border-radius

- 边框圆角

### border-colors

- 多色边框
- 不能使用-moz-border-bolors属性为4条边同时设定颜色，必须分别为4条边设定颜色；
  -moz-border-top-colors:颜色值;
  -moz-border-right-colors:颜色值;
  -moz-border-bottom-colors:颜色值;
  -moz-border-left-colors:颜色值;
  如果边框宽度（border-width）为n像素，则该边框可以使用n种颜色，每种颜色显示1像素的宽度。
  当然也可以实现渐变边框，要点就是多个颜色值之间不要差异太大，要给人一种渐变的感觉。

### border-image

- 边框背景，在css3中可以使用border-image为边框添加背景图片,使用该属性要记得先设置border的width和style

### box-shadow

- 边框阴影，跟文字阴影类似
- box-shadow：x-shadow  y-shadow  blur  spread  color  inset;
- x-shadow：设置水平阴影的位置（X轴），可以使用负值；
- y-shadow：设置垂直阴影的位置（y轴），可以使用负值；
- blur：设置阴影模糊半径；
- spread：扩展半径，设置阴影的尺寸；可选参数，缺省时值为0
- color：设置阴影的颜色；
- inset：这个参数默认不设置。默认情况下为外阴影，inset表示内阴影。

## 背景

### background-size

- 背景大小

- background-size取值共有2种，一种是使用长度值（如px、百分比）；另外一种是使用关键字。
- 关键字有,cover（即覆盖，将将背景图片以等比缩放来填充整个容器元素），contain（即“容纳”，将背景图片等比缩放至某一边紧贴容器边缘为止）
- 如果是px例如background-size:100px 60px，其实就是图片填充100*60的区域，默认以左上角padding开始算

### background-origin

- 背景位置
- 一般配合着background-size来进行定位的，例如先通过background-origin指定背景图片平铺的最开始位置，再利用background-position来进行定位。属性值如下：
- border-box	表示背景图片是从边框开始平铺
  padding-box	表示背景图片是从内边距开始平铺（默认值）
  content-box	表示背景图片是从内容区域开始平铺

### background-clip

- 背景剪切
- background-clip:属性值;
- border-box	默认值，表示从边框border开始剪切
  padding-box	表示从内边距padding开始剪切
  content-box	表示从内容区域content开始剪切
- 剪切的意思其实就是剪切区域外的部分都不可见，看起来就像被剪掉一样

## 变形

- 在css3中，所有变形方法都是属于transform属性，因此所有关于变形的方法前面都要加上transform，以表示变形处理

### 位移

- translate（）
- 有translateX(x)方法，translateY(y)方法，translate(x,y)方法，前两个不用多说，x为正向右反之相反，y为正向下反之相反，最后一个方法y是缺省值，如果不设置表示只沿x轴进行移动

### 缩放

- scale（）
- scaleX(x),scaleY(x),scale(x,y)跟位移同理三种表示方法，不过参数以1为界限，大于1就表示放大，小于1就表示缩小。一二分别表示沿着x轴，y轴进行缩放，第三种方法y也是缺省值可以省略，不过省略的意思是x，y缩放相同倍数

### 旋转

- rotate（），默认旋转中心是50% 50%的点
- 指的是元素相对中心原点进行旋转的度数，单位deg（就是degree的缩写），如果度数为正表示顺时针旋转，否则是逆时针旋转

### 倾斜

- skew（）
- skewX(x),表示元素在x轴倾斜的度数，单位为deg，如果单位为正则表示元素沿x轴负方向进行倾斜，反之相反
- skewY(y),表示元素在y轴倾斜的度数，单位为deg，如果单位为正则表示沿着y轴正方向进行倾斜，反之相反
- skew(x,y)，如果y缺省则只倾斜x轴方向

### 中心原点

- transform-origin
- 任何元素都有一个中心原点，默认情况下，元素的中心原点x轴和y轴的50%处，css3进行的位移，旋转，缩放，倾斜也都是以元素的中心原点进行变形
- transform-origin：取值，一般分为长度值和关键字两种，其中长度一般使用百分比作为单位，很少使用px，em作为单位
  关键字		          百分比		             说明
  top left		          0 0		                   左上
  top center		    50% 0		            靠上居中
  top right		       100% 0		            右上
  left center		    0 50%		             靠左居中
  center center	  50% 50%		          正中
  right center	     100% 50%		      靠右居中
  bottom left	      0 100%		             左下
  bottom center	50% 100%		       靠下居中
  bottom right	   100% 100%	           右下

## 过渡

- 一般的过渡都是配合:hover伪类来使用，将过渡语句写在被改变的元素标签内，当然也可以写在:hover伪类中，区别见下面。

- 将transition放在hover伪类中，鼠标进入时会有transition效果，但是移出时属于非hover，没有过渡效果。但是将transition写在整个元素中，会在元素整个移动过程中都有过渡效果。如果想要对多个属性实现平滑过渡效果，可以用逗号隔开各个属性即可。其中transition:all 1s linear;这种写法的all表示的是对所有值有变化的css属性都执行过渡效果。
  transition:属性 持续时间 过渡方法 延迟时间;可以简写也可以分开写

- 过渡属性，transition-property

过渡属性的取值是一个css属性名

- 过渡时间，transition-duration

取值是一个时间，单位为s（秒），可以为小数如0.5s

- 过渡函数，transition-timing-function

共有5钟，linear（元素从初始状态变化到终止状态为匀速）

ease（速度由快到慢，逐渐变慢）

ease-in（速度越来越快，称为渐显效果）

ease-out（速度越来越慢，称为渐隐效果）

ease-in-out（先加速后减速，称为渐显渐隐效果），

- 延迟时间，transition-delay

可以省略，默认是0s，不然就自己设置单位为s，当然也可以为小数

## 动画

- 使用动画两步：先定义动画，再调用动画

- 基本格式：

  ```css
  animation: name duration timing-function delay iteration-count direction；
  ```

  其中delay默认值为0，iteration-count默认值为1，direction默认值为noraml即正方向播放即从0%到100%。当然也可以分开单独设置属性。

- 注意：当区间超过两个时要改变的属性要分别定义不能省略，不然就会在区间内恢复初始态或者从0就开始一直向着最终态变化。

### @keyframes

- 定义动画，@keyframes 动画名{0%{}...100%{}}，0%和100%是必须的，也可以加入别的间断点例如30%，60%等。如果只有0和100这两个区间那么可以简写成
  0%对应from，100%对应to

### animation-name

- 调用动画

- 没啥特别的，直接通过名称调用即可，一般是将animation放在：hover中，不过要想打开页面就加载可以将其放在变化的元素中即可，不过放在hover中鼠标一移开事件就终止了。

### animation-duration

- 动画持续时间，跟transition中的基本一样

### animation-timing-function

- 播放方式，跟transition中的也一样，共linear,ease,ease-in,ease-in-out,ease-out这五种

### animation-delay

- 延迟时间，跟transition中也一样，缺省时默认值为0

### animation-iteration-count

- 播放次数，默认一次，可以填正整数或者infinite

### animation-direction

- 播放方向

- 默认正方向，还有reverse（反方向），alternate（播放次数为奇数时动画沿原方向播放；为偶数时沿反方向播放），所谓的正是指从0%到100%，反是指从100%到0%

### animation-play-state

- 播放状态
- 默认为runnig（播放动画），paused（暂停动画），这个属性一般配合着按钮与js进行操作

### animation-fill-mode

- 时间外属性（注意该属性指的是动画播放完毕之后的样式，所以一般来说播放次数为有限）

| 属性值    | 说明                                                         |
| --------- | ------------------------------------------------------------ |
| none      | 动画完成最后一帧时会反转到初始帧处（默认值）                 |
| forwards  | 当动画完成后，保持最后一个属性值（在最后一个关键帧中定义）   |
| backwards | 在 animation-delay 所指定的一段时间内，在动画显示之前，应用开始属性值（在第一个关键帧中定义） |
| both      | 元素动画同时具有forwards和backwards效果                      |

