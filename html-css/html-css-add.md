# html-css-add

## 图片介绍

如果要展示色彩丰富的图片就选jpeg（jpeg不支持透明）；一般图片就选png，无损格式且可以无损压缩并加快页面打开速度并支持透明；gif格式图像效果差，不过可以用来做动画。

## font简写

```css
font:"微软雅黑" 12px/1.5em bold;
```

- 如果要使用字体简写，至少要指定font-family和font-size属性值，其他属性没有指定会使用默认值
- font-size和line-height值之间是需要加入“/”的，初学者要注意

## 浏览器小图标

- 浏览器标题栏小图标可以通过在head标签内添加一个link标签即可，格式是.ico，可以搜索在线ico发现很多不错的在线工具

- ```html
  <link rel="shortcut icon" type="image/x-icon" href="xx.ico"/>
  ```

  前两个是固定格式，最后一个href为路径。

## 语义化

- 标题语义化

一个页面只能有一个h1，h1~h6之间不要断层，不要用h1~h6来定义样式，不要用div来代替h1~h6，h1~h4之间的内容浏览器都会赋予比较多的权重，所以不要乱写。不过浏览器只能识别文字，要想在h1中既有文字内容又有背景图片，可以设置h1的text-indent:-9999px,再设置背景图片即可

- 图片语义化

alt属性一般一定要写也会赋予权重，titlel可写可不写。可以采用

```html
<figure><img /><figcaption>图片下方文字</figcaption></figure>
```

figure元素用来包含图片和备注，figcaption表示图注文字，figure和figcaption都是block元素

- 表格语义化

主要就是table,caption,thead,tbody,tfoot这几个标签都要根据情况加上，caption的display为table-caption，这个display可以产生新的bfc（块级格式上下文）

- 表单语义化

重点是label标签要配合后面的input来使用，通过给input设置id并在label里采用for="id名”的形式构成联系。还有采用fieldset标签来给表单元素进行分组，legend标签定义某一组表单的标题。fieldset以及legend也是block元素

- 其他语义化

br标签一般是用在p标签里进行换行的不要随意使用，一些段落能用p标签就不要用div标签

## css单位

### rpx

- rpx可以根据屏幕宽度进行自适应，规定屏幕宽度为750rpx。
- 如在iphone6上，屏幕宽度为375px。所以375px = 750rpx，1px = 2rpx。不同的机型也是类似的换算规则

### vw,vh

- vw和vh是视口（viewport units）单位，何谓视口，就是根据你浏览器窗口的大小的单位，不受显示器分辨率的影响
- vw是可视窗口的宽度单位，和百分比有点一样，1vw = 可视窗口的宽度的百分之一。比如窗口宽度大小是1800px，那么1vw = 18px。和百分比不一样的是，vw始终相对于可视窗口的宽度，而百分比和其父元素的宽度有关。
- vh就是可视窗口的高度了。

### em

- em是相对长度单位。相对于当前对象内本文的字体尺寸（如果没有设置本文尺寸，那就是相对于浏览器默认的字体尺寸，也就是16px）
- 这样计算的话。如果没有设置字体尺寸就是1em = 16px。如果使用em的话，有个好的建议，就是将body的font-size设置成62.5%，也就是16px * 62.5% = 10px。这样的话1em = 10px，方便我们计算。

### rem

- rem就是相对于根元素字体大小的像素，即相对于html元素

### px

- 相对单位里px表示屏幕上一个小方块就是一个px

### %

- width，height，font-size的百分比都是相对于父元素“相同属性”的值来计算的
- line-height的百分比是相对与父元素的font-size值来计算的
- vertical-align的百分比是相对于当前元素的line-height来计算的
- em是相对于当前元素字体大小来决定的，一般浏览器的默认字体大小都是16px，一个小技巧是首行缩进用text-indent:2em

## css继承

### 文本相关属性

- font-family,font-size,font-style,font-weight,font,line-height,text-align,text-indent,word-spacing

### 列表相关属性

- list-style-image,,list-style-position,list-style-type,list-style

### 颜色相关属性

- color

## css优先级

- 引用方式冲突

行间样式>(内联=外部）

- 继承方式冲突

如果由于继承方式引起的冲突，则最近的祖先元素获胜

- 指定样式冲突

比较权重，行内样式（1000)>id(100)>属性选择器=伪类=class选择器（10）>元素选择器=伪元素（1）>通配符(0)

- 继承样式与指定样式冲突

指定样式获胜

- !important ，权重为infinite有时可以加在属性后面覆盖别的样式

## css层次选择器

- m n(后代选择器，选择的是m元素内所有的n元素）
- m>n(子代选择器，选择的是m元素内的所有儿子n元素，注意孙子及往后级别就不算了）
- m~n（兄弟选择器，顾名思义选择的是m元素下面的所有兄弟元素n）
- m+n（相邻选择器，选择的是m元素后面的第一个兄弟n元素，经典选择如li+li{border-top:1px solid #ccc}可以给除了第一个li都添上上border线）

## css规范

- 主要就是id尽量用在关键性的结构上，id及class起名要符合语义，有相同属性的最好都用一个class表示来减少代码，选择器最好不超过三层，因为浏览器解析是从右向左解析

## border,padding

- border：可以通过设置width,height都为0，然后设置border宽度，颜色，并选择性设置透明即可构成想要的三角形，利用这个特性还可以构造聊天框那种尖角三角形。border:none和border:0的区别主要是border:0要占内存
- border构造消息框三角形

一般使用两个三角形来实现，一个作为边框色，一个作为背景色，两个三角形的border-width一般相差1px。第一个三角形绝对定位到想要的位置，第二个三角形在第一个的基础上进行定位使得子三角形填充背景色，外三角形作为边框色

- border构造圆

width，height相等，border-radius四个角都设为50%即可。

- 构造椭圆

border-radius:Xpx/Ypx,X为width的一半，Y为height一半即可

- padding：补白，小技巧是可以利用padding-left配合背景图片来使用

## 外边距叠加

外边距叠加的初衷是为了文章更好的排版，如p标签默认具有上下外边距如果不叠加则中间的分隔距离明显不合适。以下三种情况会出现外边距叠加

- 同级元素

当一个元素出现在另一个元素上面的时候，第一个元素的下边距将会于下面元素的上边距进行叠加并保留最大值

- 父子元素

当一个元素包含在另外一个元素中（父子关系），假如没有padding或者边框border把外边距分隔开的华，父子元素相邻上下外边距也会发生合并并取最大值表现在父元素的margin上

- 空元素

指没有子元素或者没有文字内容的元素没有border或者padding并有上下外边距时也会发生合并

-------------------------------------------------------------------------

另外注意以下几点：

- 水平外边距永远不会叠加，指margin-left和margin-right；

- 垂直外边距只会在上面三种情况下会叠加；
- 外边距叠加之后的外边距高度等于发生叠加之前的两个外边距中的最大值；
- 外边距叠加针对的是block以及inline元素-块元素，不包括inline元素，因为inline元素的margin-top，bottom设置无效

## 负margin技术

负margin的top和left会把自身向外拉；负margin的right和bottom会把别的元素向内拉。常用技巧

- 图片与文字对齐

除了对img使用vertical-align：text-bottom外还有兼容性更好的margin：0 3px -3px 0；不用纠结为什么是3可以看成公式一样记住就行；

- 自适应两列布局

div1，div2都设为左浮动，div1宽设为100%，margin-right：-Npx，div2宽设为Npx就可以实现右边固定宽度，左边自适应；   

- 元素垂直居中

元素垂直居中，给父元素加position:relative,子元素加position:absolute,top:50%,left:50%,margin-top,margin-left设置为-（border+padding+宽/高）/2就可以了

- tab选项卡

## overflow

| 属性值  | 含义                   |
| ------- | ---------------------- |
| visible | 默认值，溢出就可见     |
| hidden  | 溢出隐藏               |
| scroll  | 直接显示滚动条         |
| auto    | 自适应，超出显示滚动条 |

另外，overflow如果不是visible这个默认值就可以构建新的bfc

## display

### 常见取值

inline(行内元素)，block(块元素)，inline-block(行内块元素)，table(以表格形式显示，类似于table元素)，table-row(以表格形式显示，类似于tr元素)，tabe-cell(以表格单元格形式显示，类似于td元素)，none(隐藏元素)

### table-cell

- 图片垂直居中元素

父元素设置display:table-cell，再加个vertical-align:middle即可让子元素内容都垂直居中

- 等高布局

div1，div2这种display都设置为table-cell，并给其各自设置宽度，这样高度会由内容撑起，两者等高且取最高的那个为height

- 自动平均划分元素并且在一行显示，给父元素display定义table，子元素设置table-cell这时再给父元素一个宽度即可让子元素的宽度根据个数平均分配

## 去除inline-block元素间的间距

也可以用在普通inline元素，只要在父元素设置font-size：0即可，如果子元素有文本记得再重新设置子文本font-size

## text-align

- text-align对文字，inline元素，以及inline-block元素都起作用，但对块元素不起作用
- 有left，right，center三种值
- 在父级元素中定义，父级元素要是块或者行级块元素

## line-height

- 即行高，指的是两行基线之间的垂直距离，通过定义height等于line-height可以实现单行文本垂直居中
- 当line-height取值为百分比时，当前元素的行高是相对于父元素的font-size值来计算的。
- 当line-height直接为数字时，该元素行高等于该数字乘以该元素的font-size值，该属性可以继承。

## vertical-align

- vertical-align属性用于定义"周围的文字，inline元素以及inline-block元素"相对于该元素基线的垂直对齐方式
  - 这里的该元素指的是被定义了vertical-align属性的元素，对inline元素，inline-block元素和table-cell元素有效，对块元素无效。有关键字，百分比，负值三种表现形式
  - 负值，例如vertical-align:-2px，表示元素相对于基线向下偏移2px，比较经典的应用就是对齐复选框或单选按钮相对于旁边文字的底部对齐
  - 百分比，例如vertical-align:50%，该50%是当前元素行高乘以50%得到的值例如20px，则这表示将当前元素相对于基线向上偏移10px
  - 关键字，top（顶部对齐），middle（中部对齐），baseline（基线对齐），bottom（底部对齐）作用就跟定义一样

---------------------------------------------------------------------------------------------------------

注意：table-cell元素跟inline及inline-block元素使用vertical-align有区别

- table-cell元素是针对自身而言，vertical-align定义的是内部子元素相对于自身的对齐方式
- 而inline和inline-block元素的vertical-align是针对周围的元素来说的，vertical-align定义的是周围的元素想对于自身的对齐方式

## 表单效果

- 深入radio和checkbox

单选框复选框与文字垂直居中对齐可以通过设置单选复选框的vertical-align为负值向下移即可，具体数值可以自己试

- 深入textarea
  - 固定大小，可以通过max-width，max-height设置textarea的最大宽度和最大高度；
  - 禁用拖动，resize：none即可
  - 在chrome，firefox，ie实现相同的外观，先设置width，height再将overflow设为auto即可

- 表单对齐
  - 每一行表单分为若干左栏加若干右栏，所有行的左栏长度相等，所有行的右栏所有盒子长度之和相等，左栏一般是一个label，右栏是若干个文本框；
  - 所有左栏盒子和右栏盒子都设为左浮动
  - 左栏text-align属性定义为right，使得文字右对齐
  - 最重要一点，每一行中左栏长度和右栏所有盒子的总长度等于行宽，这里的盒子包括width，border，padding，margin

## 浮动布局

文档流，就是指元素在页面中出现的先后顺序；正常文档流，将一个页面从上到下分为一行一行，其中块元素独占一行，相邻行元素在每一行中按照从左到右排列知道该行排满，即默认情况下的页面布局情况。如果我们想要改变正常文档流，可以使用浮动或者定位。

- 深入浮动
  - 当一个元素定义了"float:left"或"float:right"时，不管这个元素之前是inline，inline-block或者其他类型，都会变成block类型并可以对其进行块元素的一些定义，可以通过设置margin-left，margin-right来定义浮动元素与其他元素之间的间距。
  - 当一个元素定义了float:left或float:right时，这个元素会脱离文档流，后面的元素会紧跟着填上空缺的位置。

### 浮动的影响

- 对自身的影响

如果一个元素设置了浮动，则不管这个元素是什么类型都会转化为块元素

- 对父元素的影响

如果浮动元素的高度height大于父元素height或者父元素没有定义height，则父元素高度会塌陷，即包不住子元素了，不过就算高于它也不是真正的包住，需要清除浮动才能真正包住

- 对兄弟元素的影响

如果兄弟元素也是是浮动元素，同一方向的话则会从左到右从上到下一个紧挨着一个排列。方向相反则会移向两边。如果兄弟元素不是浮动元素，建议结合层叠等级来理解，inline，inline-block这种可以感知到浮动，div本身感知不到浮动不过它的内容例如文字，图片等都是可以感知到的

- 对子元素的影响

父元素是浮动元素的话可以包住不是浮动的子元素；如果父子都是浮动的话也会自适应包住 子元素

### 浮动的副作用

- 父元素高度塌陷

通过设置父元素构建新的bfc或者利用伪元素清除浮动都可以解决该问题

- 页面容易布局混乱

注：清除浮动是清除产生浮动的元素后面的浮动，所以一般是在父级元素或者浮动元素后面的元素去清，一般是用父元素用::after伪元素去清。

## 定位布局

一共有fixed（固定定位），relative（相对定位），absolute（绝对定位），static（静态定位，默认值）四种定位。

- 默认情况下，固定定位元素和绝对定位元素的位置是相对于浏览器而言，而相对定位的元素的位置是相对原始位置而言，注意是默认情况，给父元素添加position：relative即可解决该问题。
- position属性一般配合top，bottom，left，right来使用，只有元素定义了position属性（除static）之后这几个值才生效

- position:absolute,fixed会将元素转换为块元素

- 绝对定位元素是相对于外层第一个设置了"position:relative,absolute或者fixed"的祖先元素来进行定位的

- 默认情况下，元素的z-index属性处于不激-0-活状态，也就是说默认情况下，设置元素的z-index属性无效。z-index属性只有在元素定义"position:absolute,relative,fixed"时才会被激活。

## css技巧

### 水平居中

- 块级元素，在自身设置margin：0 auto即可
- 除了块级元素，在父元素中设置text-align：center即可。

### 垂直居中

- 单行文本，line-height等于height
- 多行文本，或者inline-block：给父元素设为table-cell，再加个vertical-middle即可。注意当父级设置为绝对定位时该方法失效。

- 块级元素：给父元素设为relative，子元素为absolute，top:50%,left:50%，再给margin-top，margin-left设置盒子一半的除了margin的负宽高值即可。该方法用于别的元素类型也可以，并且也可以自由实现水平或者垂直居中，不过一般是块级元素用。

## css sprite

雪碧图，即一张图片上很多小图片，通过设置背景图片的background-position的数值来选择该出现的图片。优点是减少http请求的次数，加快页面刷新速度，不过缺点也很明显就是难以维护，推荐在开发后期使用。

## 包含块

包含块：就是可以决定一个元素大小和定位的元素，通常情况下一个元素的包含块是由离它最近的"块级祖先元素"的内容边界决定的。但当元素被设置为绝对定位时，此时该元素的包含块是由离它最近的设置了position:absolute,relative的祖先元素决定。也就是说一个元素的css盒子为它的内部元素建造了包含块。

- 包含块的判定以及包含块的范围
  - 根元素：根元素 (html)元素是一个页面中最顶端的元素，它没有父元素，根元素存在的包含块被称为初始包含块。
  - 固定定位元素：如果一个元素的position值是fixed，那么它的包含块是当前可视窗口，也就是当前浏览器窗口。
  - 静态定位元素和相对定位元素：如果元素的position属性为static或者relative，那么它的包含块是它最近的块级祖先元素创建的，祖先元素必须是block,inline-block或者table-cell类型。
  - 绝对定位元素：如果元素的position属性为absolute，那么它的包含块是由离他最近的position属性不为static的祖先元素，这里的祖先元素可以是块元素，也可以是行内元素。如果绝对定位元素找不到position属性不为static的祖先元素，则它的包含块是根元素（html元素）。

## 层叠上下文

层叠上下文：层叠上下文是html中的一个三维的概念即与z-index属性相关。

`z-index`属性仅在可以定位的元素上生效（relative，absolute,fixed）。

- 层叠上下文创建条件
  - 根元素
  - z-index不为auto的定位
  - 这两个条件中的任意一个则该元素会创建一个层叠上下文。层叠上下文可以嵌套。
- 层叠等级：同一层叠上下文中，元素显示的顺序是由层叠级别决定的。层叠级别从低到高排列为
  - 层叠上下文的背景，边框
  - z-index为负值
  - 块盒子
  - 浮动盒子
  - 行内盒子
  - z-index为0
  - z-index为正值
  - 除了背景和边框这一条是当前层叠上下文外，其他的都针对的是当前层叠上下文内部的元素。

- 层叠上下文的特点
  - 同一个层叠上下文中，我们比较的是"内部元素层叠级别"。层叠级别大的元素显示在上，层叠级别小的元素显示在下。
  - 同一个层叠上下文中，如果两个元素的层叠级别相同(即z-index值相同),则后面的的元素堆叠在前面元素的上面，遵循"后来者居上"的原则。
  - 不同的层叠上下文中，我们比较的是"父级元素层叠级别"。元素显示顺序以"父级层叠上下文"的层叠级别来决定显示的先后顺序，与自身的层叠级别无关。

## BFC和IFC

在页面中任何一个元素都可以看成一个盒子，在普通文档流中盒子会参与一种格式上下文。一个盒子只能是块盒子或者行内盒子，不能同时是块盒子又是行内盒子。其中块盒子参与BFC(块级格式上下文），行内盒子参与IFC(行级格式上下文）

- 格式上下文

指的是页面中的一块渲染区域，并且这个格式上下文有一套自己的渲染规则，格式上下文决定了其内部元素将如何定位以及和其它元素之间的关系，有块级格式上下文（BFC)和行级格式上下文（IFC）两种。

- 如何创建BFC
  - 浮动元素
  - 绝对定位元素（absolute或者fixed）
  - display属性为inline-block，table-cell，table-caption
  - overflow属性不为visible的元素
  - 根元素也会创建BFC，在默认情况下一个页面中所有的元素都处于同一个BFC种，这是默认的，不需要我们去设置，不过理解这点很重要

- BFC重要结论
  - 在一个BFC内部，盒子会在垂直方向上一个接一个的排列。
  - 在一个BFC内部，相邻的margin-top和margin-bottom会叠加。
  - 在一个BFC内部，每一个元素的左外边界会紧贴着包含盒子的左边，即使存在浮动也是如此。
  - 在一个BFC内部，如果存在内部元素是一个新的BFC，并且内部元素是浮动元素，则该BFC的区域不会与float元素的区域重叠。
  - BFC就是页面上的一个隔离的盒子，该盒子内部的元素不会影响到外面的元素。
  - 计算一个BFC的高度时，其内部浮动元素的高度也会参与计算。

- BFC常见用途
  - 创建BFC来避免垂直外边距叠加。即在相邻的某一个元素外面加一个父元素，并为父元素添加overflow：hidden这种可以创建新的BFC即可。
  - 创建BFC来清除浮动，这个是通过overflow：hidden方法，不过我更倾向与伪元素。
  - 创建BFC来实现自适应布局

------------------------------------------------

例如:(div1，div2为兄弟元素)

div1定义浮动，div1定义宽度（可以是具体值也可以是百分比），高度；

div2定义高度，再给div2加个overflow：hidden即可，这样构建一个新的BFC

根据BFC特点两者区域不会重叠，且div2的宽度会自动填充屏幕剩余宽度这样即可实现自适应布局。

​        