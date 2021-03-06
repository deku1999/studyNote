# jquery

## 页面载入事件

js中使用$(document).ready()方法来替代JavaScript中的window.onload方法；功能相似却也有明显区。js中的必须要等到所有dom元素（包括外部链接图片）加载完毕之后才能操作dom。jquery却不需要，只需加载html元素就可以对dom进行操作。
另外，js中一般情况下window.onload()只能执行一次如果写多个则执行最后一个不过可以通过加入兼容性代码解决，不过jquery本身就可以多次执行
ready(）方法的四种写法

```javascript
$(document).ready(function(){})
```

```javascript
$(function(){})  推荐这种
```

还有两种是将$符替换为jquery

## jquery基础选择器

本质上跟css中的规则一样

### 基本选择器

1.元素选择器

$("元素名")

2.id选择器	

$("#id名")

3.class选择器

$(".类名")

4.群组选择器

即各种选择器之间用逗号隔开

5.通配符选择器

$("#list *")表示选中id为list元素下的所有子元素，规则与css选择器规则基本相同

### 层次选择器

1.子代选择器

$("M>N")只能选择M下一层级的所有N元素，只限于子级

2.兄弟选择器

$("M～N")选中M元素后面的所有N元素；不限于子级，孙级

3.相邻选择器

$("M+N")只能选择M元素后面的一个N元素

### 属性选择器

比较灵活，例如

1.选择含有class属性的div元素

$("div[class]")

2.选择type属性值为checkbox的input元素（也就是选择所有复选框元素）

$("input[type = 'checkbox']")

3.选择type属性值不是checkbox的input元素

$("input[type != 'checkbox']")

4.选择class属性包含nav的div元素（因为class属性可以包含多个值）

$("div[class *= 'nav']")

5.选择name属性以group开始的input元素

$("input[name ^= 'group']")

6.选择name属性以group结尾的input元素

$("input[name $= 'group']")

7.选择具有id属性并且class属性是以lvye开头的div元素

```
$("div[id][class^='lvye']")
```

## jquery伪类选择器

本质上也跟css中的规则一样

### 简单伪类选择器

| 伪类选择器      | 说明                                                     |
| --------------- | -------------------------------------------------------- |
| :not(selector)  | 选择除了某个选择器之外的所有元素                         |
| :first或first() | 选择某元素的第一个元素（非子元素）                       |
| :last或last()   | 选择某元素的最后一个元素（非子元素）                     |
| :odd            | 选择某元素的索引值为奇数的元素                           |
| :even           | 选择某元素的索引值为偶数的元素                           |
| :eq(index)      | 选择给定索引值的元素，索引值index是一个整数，从0开始     |
| :lt(index)      | 选择所有小于索引值的元素，索引值index是一个整数，从0开始 |
| :gt(index)      | 选择所有大于索引值的元素，索引值index是一个整数，从0开始 |
| :header         | 选择h1~h6的标题元素                                      |
| :animated       | 选择所有正在执行动画效果的元素                           |
| :root           | 选择页面的根元素                                         |
| :target         | 选择当前活动的目标元素（锚点）                           |

注：从header开始可以直接用，例如:header{..}，这样就可以直接选中h1~h6的标题元素

### 子元素伪类选择器

| 选择器          | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| :first-child    | 选择父元素的第1个子元素                                      |
| :last-child     | 选择父元素的最后1个子元素                                    |
| :nth-child(n)   | 选择父元素下的第n个元素或奇偶元素，n的值为整数/odd/even      |
| :only-child     | 选择父元素中唯一的子元素（该父元素只有一个子元素）           |
| :first-of-type  | 选择同元素类型的第1个同级兄弟元素                            |
| :last-of-type   | 选择同元素类型的最后1个同级兄弟元素                          |
| :nth-of-type(n) | 选择同元素类型的第n个同级兄弟元素，n的值可以是整数/odd/even  |
| :only-of-type   | 匹配父元素中特定类型的唯一子元素（但是父元素可以有多个子元素） |

### 可见性伪类选择器

使用较少，了解即可

| 选择器   | 说明                            |
| -------- | ------------------------------- |
| :hidden  | 选取所有不可见元素              |
| :visible | 选取所有可见元素，与:hidden相反 |

### 内容伪类选择器

| 选择器          | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| :contains(text) | 选择包含给定文本内容的元素                                   |
| :has(selector)  | 选择含有选择器所匹配元素的元素（例如div:has(span)表示选中子级中，当然孙级这种也可以，有span标签的div标签） |
| :empty          | 选择所有不包含子元素或者不包含文本的元素                     |
| :parent         | 选择含有子元素或者文本的元素（跟:empty相反）                 |

### 表单伪类选择器

| 选择器    | 说明                                         |
| --------- | -------------------------------------------- |
| :input    | 选择所有input元素                            |
| :button   | 选择所有普通按钮，即type="button"的input元素 |
| :submit   | 选择所有提交按钮，即type="submit"的input元素 |
| :reset    | 选择所有重置按钮，即type="reset"的input元素  |
| :text     | 选择所有单行文本框                           |
| :textarea | 选择所有多行文本框                           |
| :password | 选择所有密码文本框                           |
| :radio    | 选择所有单选按钮                             |
| :checkbox | 选择所有复选框                               |
| :image    | 选择所有图像域                               |
| :hidden   | 选择所有隐藏域                               |
| :file     | 选择所有文件域                               |

### 表单属性伪类选择器

| 选择器          | 说明                                                |
| --------------- | --------------------------------------------------- |
| :checked        | 选择所有被选中的表单元素，一般用于radio和checkbox   |
| option:selected | 选择所有被选中的option元素                          |
| :enabled        | 选择所有可用元素，一般用于input、select和textarea   |
| :disabled       | 选择所有不可用元素，一般用于input、select和textarea |
| :read-only      | 选择所有只读元素，一般用于input和textarea           |
| :focus          | 选择获得焦点的元素，常用于input和textarea           |

## jquery操作元素

### 属性操作

1.获取属性

$().attr("属性名")  

2.设置属性

$().attr("属性" , "属性值") 

3.删除属性

$().removeAttr("属性")

### 内容操作

1.获取html内容

$().html()     

2.设置html内容

$().html("HTML内容")

3.获取文本内容

$().text()  

4.设置文本内容

$().text("文本内容")  

5.获取表单元素值

$().val()

6.设置表单元素值

$().val("设置值")

## jquery操作样式

### css属性操作

1.获取css属性值

$().css("属性")

2.设置单个css属性值

$().css("属性","属性值")

3.设置多个css属性值

$().css({"属性1":"属性值1","属性2":"属性值2",……})

### css类名操作

1.添加类名

$().addClass("类名")

2.删除类名

$().removeClass("类名")

3.切换类名

$().toggleClass("类名")

toggleClass()方法用于检查元素是否具有某个类名。如果类名不存在，则为该元素添加类名；如果类名已存在，则为该元素删除类名。要是给一个按钮设置个点击事件为它那么点击就换添加样式再点就会取消，类似切换效果

- 使用jQuery操作元素样式时，当样式比较少，建议使用“CSS属性操作”；当样式比较多时，建议使用“CSS类名操作”

### 元素的宽度和高度

1.获取元素宽，高

$().width()

2.设置元素宽高

$().width(n) ，n是一个整数，表示npx

- width()方法和css("width")方法类似，不过width()方法获得的宽度值不带单位（仅仅是数字），而css("width")获取的宽度值带“px”作为单位

### 元素的位置

- offset()和position()返回的都是一个对象，有着left和top两个属性，都是数值。$().offset().top;$().offset().left这样即可查看元素的上位置和左位置，position()方法基本与这一样

1.offset

offet()一直是相对于浏览器返回信息

2.position

position()类似于定位如果父级设置了relative之类的就相对于父级返回，不然也是相对于当前浏览器窗口

### 滚动条的距离

在jQuery中，我们可以使用scrollTop()来获取或设置元素相对于垂直滚动条顶部的距离，可以使用scrollLeft()来获取或设置元素相对于水平滚动条左部的距离

1.获取竖直滚动距离

$().scrollTop() 

2.设置竖直滚动距离

$().scrollTop(value)  

注：scrollTop()和scrollLeft()获取的值是一个数字（不带单位），这个跟height()、width()是一样的，常见的回到顶部，下拉固定都跟这技巧有关。

## jquery操作dom

### 创建节点

使用$()方法，节点命名一般都习惯使用$开头，以表示这是一个jquery对象.例如

```jquery
$li=$("<li>呵呵</li>")
```

### 插入节点

每一个方法有两种，另外一种就是颠倒一下顺序，推荐语义化记忆

#### 内部插入

1.append()和appendTo()

```
$(A).append(B),往元素A的末尾添加B
```

2.prepend()和prependTo()

```
$(A).prepend(B),表示在A内部的“开始”插入B
```

#### 外部插入

1.before()和insertBefore()

```
$(b).insertBefore(a) 表示在A元素前面插入B
```

2.after()和insertAfter()

```
$(b).insertAfter(a) 表示在A元素后面插入B
```

### 删除节点

1.remove

该方法删除节点后还会返回该节点,利用这个特性可以实现交换两个节点的位置

```
$(A).remove(),删除A节点
```

2.detach

跟remove方法基本一样唯一的区别是detach方法会保留节点的事件,即当我们删除节点后如果希望重新使用该节点,并且希望被删除的节点在重新使用后还能保留原来绑定的事件,那么应该使用detach()而不是remove()

3.empty

清空节点,remove和detach都是针对自身进行删除,而$(A).empty()是清空A元素内部的所有节点包括元素节点和文本节点等

### 复制节点

$(A).clone(),其中clone()方法有一个布尔参数，默认值为false。$(A).clone()表示仅仅将A节点复制，但不复制A节点所绑定的事件。$(A).clone(true)表示将A节点复制，并且复制A节点所绑定的事件。返回值为一个元素节点

### 替换节点

- replaceWith()

```
$(A).replaceWith(B),表示用B来替换A
```

- replaceAll()

跟replaceWith类似,不过次序不一样,例如$(A).replaceAll(B)表示用A替换所有的B

### 包裹节点

- wrap()

```
$(A).wrap(B)表示将A元素使用B元素包裹起来
```

- wrapAll()

与wrap方法类似但是作用并不等价,wrap是将所选中元素一个个包裹,而wrapAll是将所有匹配的元素用一个包裹

- wrapInner()

```
$(A).wrapInner(B)表示将A元素“所有内部子元素”使用B元素包裹起来（不包括A本身）
```

注：wrapAll选择的元素如果中间参杂有其他元素则会先将其踢出去再打包选中元素

### 遍历节点

- $().each(callback)

参数callback是一个function函数。该函数可以接受一个形参index，此形参为遍历元素的序号（从0开始）。如果需要访问元素中的属性，可以借助形参index，然后配合this关键字来实现元素属性的获取和设置。例如遍历li节点并设置其内容

```
$("li").each(fucntion (index){
var txt = "这是第" + (index+1)+"个元素"
$(this).text(txt)
})
```

## jquery事件

### 鼠标事件

指的是操作鼠标时触发的事件，语法都是往事件方法中插入一个匿名函数function(){}，例如$("p").click(fucntion (){})

| 事件名    | 事件含义     |
| --------- | ------------ |
| click     | 鼠标单击事件 |
| dbclick   | 鼠标双击事件 |
| mouseover | 鼠标移入事件 |
| mouseout  | 鼠标移出事件 |
| mousemove | 鼠标移动事件 |
| mousedown | 鼠标按下事件 |
| mouseup   | 鼠标松开事件 |

注：提一下鼠标移入，移出还有个mouseenter和mouseleave，与mouseover和mouseout的区别是，该方法作用的区域只包含元素本身不包括它后代子元素

### 键盘事件

| 事件名   | 事件含义                         |
| -------- | -------------------------------- |
| keydown  | 按下键事件（包括数字键、功能键） |
| keypress | 按下键事件（只包含数字键）       |
| keyup    | 放开键事件（包括数字键、功能键） |

三个按键的发生顺序就是keydown->keypress->keyup，语法跟鼠标事件相同

### 表单事件

| 事件名   | 事件说明                                         | 适用元素                       |
| -------- | ------------------------------------------------ | ------------------------------ |
| focus()  | 获得焦点                                         | text，textarea，下拉列表select |
| blur()   | 失去焦点                                         | text，textarea，下拉列表select |
| change() | 文本框内字符串的改变                             | text，textarea，下拉列表select |
| select() | 用户选中单行文本框text或多行文本框textarea的文本 | text，textarea                 |

### 滚动事件

$().scroll(fn)，fn是事件处理函数，也就是function (){}，通常和scrollTop和scrollLeft方法结合使用

### 绑定事件

$().on(type , fn)，type为必选参数，表示事件类型，例如单击事件是“click”，双击事件是“dbclick”，以此类推。注意一下，这里type是一个字符串。fn为必选参数，表示事件的处理函数。在元素已经存在的情况下和基本事件相同，不过绑定事件可以为未来创建的元素绑定事件。

### 解绑事件

解除元素身上已经存在的事件，$().off(type)，type为必选参数，表示事件类型，例如单击事件是“click”，双击事件是“dbclick”，以此类推。注意一下，这里type是一个字符串。不论是适用绑定添加的事件，还是基本事件添加的事件都可以解除绑定。

### 合成事件

在jquery中有些方法可以结合两个经常一起使用的函数事件，例如$().hover(fn1,fn2),参数fn1表示“鼠标移入”时触发的事件处理函数，参数fn2表示“鼠标移出”时触发的事件处理函数。注意这里指代的是mouseenter()和mouseleave()而不是mouseover和mouseout

### 一次事件one

在jQuery中，我们可以使用one()方法为所选元素绑定一个“只触发一次”的处理函数。$().one(type , fn)，type表示事件类型，例如单击事件是“click”，双击事件是“dbclick”，以此类推。这里的type是一个字符串。fn表示事件的处理函数。

## jquery动画

本质上都是animate方法

### 显示和隐藏

- hide()
- show()
- Toggle()

这三种方法都可以添加（speed，callback）这两个参数，speed为时间单位毫秒，callback为回调函数即完成动画之后再调用的函数。显示隐藏本质上是改变了css的宽高，透明度和display

### 淡入和淡出

- fadeIn() 	淡入
- fadeOut()  淡出
- fadeToggle() 切换状态

本质上是改变了元素css的透明度和display，也有speed和callback两个参数，speed省略的情况下默认为200ms。

- fadeTo(speed,opacity,callback)

将元素透明度指定到某个值，speed默认值为500ms

### 滑上和滑下

- slideUp()	滑上
- slideDown()   滑下
- slideToggle()   切换状态

本质上是通过改变元素height和display来实现动画效果，同样具有speed和callback参数，speed默认为200ms

### 自定义动画

- 简单动画

```
$().animate(params , speed , callback)
```

params为属性：值列表例如这种{"属性1":"属性值1","属性2":"属性值2",……, "属性n":"属性值n"}

- 累加动画

即params参数里的属性值用"width":"+=30px"这种形式，每次执行都会累加一定的长度

### 队列动画

指的是元素按照一定的顺序执行多个动画效果，即有多个animate()方法在元素中执行，然后根据这些animate()方法执行的先后顺序，形成了动画队列，然后按照这个动画队列的顺序来进行显示。队列动画包括之前学的显示隐藏，淡入淡出，滑上滑下，自定义动画

```
$().animate().animte()…….animte()
```

### 动画的停止

```
$().stop(clearQueue,gotoEnd)
```

参数clearQueue和参数gotoEnd都是可选参数，取值都为布尔值。两个参数默认值都为false。参数clearQueue表示是否要清空“未执行”的“动画队列”。这里大家要看准了，清空的是整个“动画队列”，而不仅仅是某一个动画。参数gotoEnd，表示是否直接将正在执行的动画跳转到末状态。

| 参数取值         | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| stop()           | 等价于stop(false,false)，仅仅停止“当前执行”这段动画，后面的动画还可以继续执行 |
| stop(true)       | 等价于stop(true,false)，停止所有动画，包括当前执行的动画     |
| stop(true,true)  | 停止所有动画，但是允许执行当前动画                           |
| stop(false,true) | 停止“当前执行”这段动画，然后调到最后一个动画，并且执行最后一个动画 |

一般只会用到前两个，其中stop（）经常被用来防止动画队列堆积

### 动画的延迟

$().delay(speed)，参数speed，表示延迟的时间，单位为毫秒，例如$(this).animate({ "width": "200px" }, 1000).delay(1000).animate({ "height": "200px" }, 1000)

### 判断动画状态

在jQuery中我们还可以使用is()方法来判断所选元素是否正处于动画状态，如果元素不处于动画状态，则添加新的动画；如果元素处于动画状态，则不添加新的动画。好像实际开发中这个更常用

```
if(!$().is(":animated"))
{
    //如果当前没有进行动画，则添加新动画
}
```

## jquery过滤方法

### 类过滤hasClass()

$().hasClass("类名"),hasClass()方法往往用于执行判断操作，判断当前jQuery对象中的某个元素是否包含了指定类名。如果包含，则返回true；如果不包含，则返回false

### 下标过滤eq()

$().eq(n)，在jQuery中，n是一个正整数，从0开始计算，表示用来选取“元素集合”中下标为n的的某一个元素。我们使用eq()方法来实现下标过滤。其实跟前面学到的伪类:eq()选择器差不多。

### 判断过滤is()

判断过滤，指的是根据某些条件进行判断来选取符合条件的元素。在jQuery中，我们使用is()方法来实现判断过滤。

$().is(expression)，参数expression是一个jQuery选择器表达式。所谓的jQuery选择器表达式，就是我们在前几章所学到的选择器。is()方法用于判断当前选择的元素集合中，是否含有符合条件的元素。如果含有，则返回true；如果不含有，则返回false。例如

- 判断元素是否隐藏

```
$().is(":visible")
```

- 判断元素是否处在动画中

```
$().is(":animated")
```

- 判断复选框是否被选中

```
$("input[type=checkbox]").is(":checked")
```

- 判断是否第1个子元素

```
$(this).is(":first-child")
```

- 判断文本中是否包含helicopter这个词

```
$().is(":contains('helicopter')")
```

- 判断是否包含某些类名，这个跟hasClass很像，hasClass要更高效，因为封装的东西少

```
$().is(".red,.blue")
```

### 反向过滤not()

在jQuery中，我们可以使用not()方法来过滤jQuery对象中“不符合条件”的元素，并且返回剩下的元素。$().not(expression)，参数expression是一个jQuery选择器表达式。就是前几章所学到的选择器。

### 表达式过滤flter,has

```
$("ul li").filter("#red,#yellow").click(function () {
                $(this).css("color", "red");
                })
```

```
$("li").has("span")
```

类似于前面学过的伪类选择器中的:has

## jquery查找方法

expression为过滤条件可省略

### 查找祖先元素

- parent(expression)

只查找父级元素

- parents(expression)

可以查找父级，爷级等，反正就是所有祖辈元素

- parentUntil(expression)

限制区间，范围为初始的到表达式所限制的

### 查找后代元素

- children(expression)

只能查找当前元素的所有子元素或部分子元素。不能查找其他后代元素

- contents(expression)

也是用来查找子内容的，不过它不仅能获取子元素，还可以获取文本节点，注释节点等，相当于js的childNodes

- find(expression)

查找所选元素的后代元素，不过find()方法能够查找所有后代元素，而children仅能查找子元素

### 向前查找兄弟元素

- prev(expression)

只能查找前一个

- prevAll(expression)

查找前面的所有兄弟元素

- prevUntil(expression)

限制区间

### 向后查找兄弟元素

- next()

查找后一个

- nextAll()

查找后面所有的兄弟元素

- nextUtil()

限制区间

### 查找所有兄弟元素

- siblings(expression)

不分前后查找所有兄弟元素，可以通过expression加限制条件。