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

## input标签

| type属性 | 含义与规范                                                   |
| -------- | ------------------------------------------------------------ |
| text     | 文本框                                                       |
| password | 暗文密码框                                                   |
| button   | 按钮                                                         |
| reset    | 重置按钮，需要在同一个form下                                 |
| image    | 图像形式的提交按钮                                           |
| radio    | 单选按钮，name，value必须设置，name需要相同，value为向服务器提交的选项值 |
| checkbox | 复选框                                                       |
| hidden   | 隐藏字段                                                     |
| file     | 文件上传                                                     |
| textarea | 多行文本框，rows和cols属性可以设置行宽，为闭合标签，标签间内容文默认显示内容 |

| input属性 | 含义与规范                       |
| --------- | -------------------------------- |
| value     | 文本框的默认值                   |
| size      | 定义文本框的长度，以字符为单位   |
| maxlength | 设置文本框中最多可以输入的字符数 |

- 每个checkbox都要设置不同的id，不需要设置name，checked="checked"表示默认选中该栏。复选框一般没有文本需要加入label标签指向该栏id并在其中放上内容配合使用如

  ```html
  <input type="checkbox" id="check1" checked="checked"><label for="check1">苹果</label>
  ```

- button普通按钮一般配合js使用，submit提交按钮细节暂不知，reset重置按钮点击会清除该表单中输入的信息，不过必须是该form内





