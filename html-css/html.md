# html

## 基础标签知识

| 标签名   | 功能                                       |
| -------- | ------------------------------------------ |
| em       | 斜体                                       |
| strong   | 加粗                                       |
| sup      | 上标标签                                   |
| sub      | 下标标签                                   |
| dl,dt,dd | dl列表名，dt列表标题，dd列表内容，结合使用 |

## table标签

- 一般表格可以有个表格标题标签caption，th是表头标签，默认就有加粗居中的样式功能,td单元格，tr行标签
- 表格语义化，建议在书写表格时加上thead,tbody,tfoot。thead标签就放表头例如th，tbody放内容，tfoot表脚放最后结果

- 合并行属性rowspan，例如

```html
						<tr>
            					<td rowspan="2">喜欢水果:</td>
            					<td>苹果</td>
        				 </tr>
        				<tr>
            					<td>香蕉</td>
        				</tr>
```

- 合并列属性，colspan，例如

```html
				<tr>
					<td colspan="2">个人信息</td>
				</tr>
				<tr>
					<td>姓名</td>
					<td>程sir</td>
				</tr>
```

两者都是在td标签中使用

## img标签

src属性为路径，title为鼠标移动上去显示的文字，alt为图片未加载时显示的文字

## a标签

### target

网页打开方式，默认为当前窗口打开，target="_blank"为在新窗口打开

### 锚点

a标签也可以用来做锚点链接链接到页面的某一部分，例如

```html
<a href="#music">推荐音乐</a>,<div id="music"> ...</div>
```

这样点击推荐音乐就会跳到id为music的区块

### 默认样式

- 默认情况，字体为蓝色，带有下划线；
- 鼠标点击时：字体为红色，带有下划线；
- 鼠标点击后：字体为紫色，带有下划线；

我们可以通过重新定义伪类来改变他

- a:link	定义a元素未访问时的样式
- a:visited	定义a元素访问后的样式

- a:hover	定义鼠标经过显示的样式
- a:active	定义鼠标单击激活时的样式

注意这四个伪类定义的时候必须按照这个顺序定义，不然浏览器可能无法正常识别，当然也可以直接设置字体颜色让其初始化，一般情况下好像就是这么做的

## form标签

- name，表单名
- action，指定提交表单数据的位置
- method，表单提交方法
- target，目标显示方式，跟a标签的一样
- enctype，编码方式，一般用默认方式即可

### form元素

- input，输入框
- textarea，文本域
- select，option；一般是配合使用，选择列表。

## input标签

| type属性     | 含义与规范                                                   |
| ------------ | ------------------------------------------------------------ |
| text         | 文本框                                                       |
| password     | 暗文密码框                                                   |
| button       | 按钮                                                         |
| reset        | 重置按钮，需要在同一个form下                                 |
| image        | 图像形式的提交按钮                                           |
| radio        | 单选按钮，name，value必须设置，name需要相同，value为向服务器提交的选项值 |
| checkbox     | 复选框                                                       |
| hidden       | 隐藏字段                                                     |
| file         | 文件上传                                                     |
| number       | number 类型用于应该包含数值的输入域                          |
| email        | email 类型用于应该包含 e-mail 地址的输入域                   |
| Date pickers | 日期选择器,具体的有type="date"，month，week，time等等，请自己查阅 |
| submit       | 当用户单击确认按钮时，表单的内容会被传送到另一个文件         |

| input属性   | 含义与规范                                         |
| ----------- | -------------------------------------------------- |
| value       | 文本框的默认值                                     |
| size        | 定义文本框的长度，以字符为单位                     |
| maxlength   | 设置文本框中最多可以输入的字符数                   |
| placeholder | 文本框的默认值，如果与value一起存在则以value值优先 |

- 每个checkbox都要设置不同的id，不需要设置name，checked="checked"表示默认选中该栏。复选框一般没有文本需要加入label标签指向该栏id并在其中放上内容配合使用如

  ```html
  <input type="checkbox" id="check1" checked="checked"><label for="check1">苹果</label>
  ```

- button普通按钮一般配合js使用，submit提交按钮细节暂不知，reset重置按钮点击会清除该表单中输入的信息，不过必须是该form内

## 原生拖放

- 默认情况下，图像，链接和文本都是可以拖动的，也就是说，不用额外编写代码，用户就可以拖动它们。当然文本要先进行选中才可以拖动。
  - html5为元素规定了一个`draggable`属性，表示元素是否可以拖动。
- 拖动某元素时，将依次触发下列事件。
  - 按下鼠标键并开始移动鼠标时，会在被拖放的元素上触发`dragstart`事件，此时光标变成不能放符号（圆环中间有一条反斜线）
  - 触发`dragstart`事件后，随即会触发`drag`事件，而且在元素被拖动期间持续触发该事件。
  - 当拖动停止时（无论是把元素放到了有效的放置目标，还是放到了无效的放置目标上），都会触发`dragend`事件。
- 都某个元素被拖动到一个有效的放置目标时，会依次发生下列事件。以下三个事件的目标都是作为放置目标的元素。
  - 只要有元素被拖动到放置目标上，就会触发`dragenter`事件。
  - 紧随其后的是`dragover`事件，而且在被拖动的元素还在放置目标的范围内移动时，就会持续触发该事件。
  - 如果元素被拖出了放置目标，dragover事件不再发生，但会触发`dragleave`事件。如果元素被放到了放置目标中，则会触发`drop`事件，而不是`dragleave`事件。
- 更多内容（P482）

## 媒体元素

- 视频标签，`<video>`。音频标签，`<audio>`

```html
// 直接引用
<video src="xx.mp4">video not available</video>
//因为并非所有浏览器都支持所有媒体格式，所以可以指定多个不同的媒体源
<video>
 <source src="xx.webm"></source>
 <source src="xx.mp4"></source>
 <source src="xx.ogv"></source>
	sorry,video is not supported
</video>
音频文件跟上述同理
```

- 媒体元素同样有很多自己的事件，还有属性和方法，比较多不多介绍。例如`play()`，和`pause()`用于播放和暂停的。跟多内容请参见（p488）。





