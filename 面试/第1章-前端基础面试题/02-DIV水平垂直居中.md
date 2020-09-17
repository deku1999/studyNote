# div水平垂直居中的方法

在平时，我们经常会碰到让一个div框针对某个模块上下左右都居中（水平垂直居中），其实针对这种情况，我们有多种方法实现。

**方法一:**

　　绝对定位方法：不确定当前div的宽度和高度，采用 transform: translate(-50%,-50%); 当前div的父级添加相对定位（position: relative;）

　　图片展示：

　　![img](https://images2018.cnblogs.com/blog/1337908/201805/1337908-20180510132530310-507426098.png)

　　代码如下：

```
div{
    background:red;
    position: absolute;
    left:50%;
    top:50%;
    transform: translate(-50%, -50%);
}
```

　

**方法二:**

　　绝对定位方法：确定了当前div的宽度，margin值为当前div宽度一半的负值

　　图片展示： 如方法一的图片展示

　　代码如下：

```
div{
    width:600px;
    height: 600px;
    background:red;
    position: absolute;
    left:50%;
    top:50%;
    margin-left:-300px;
    margin-top:-300px;
}
```

　　

**方法三：**

　　绝对定位方法：绝对定位下top left right bottom 都设置0

　　图片展示： 如方法一的图片展示

　　代码如下：

```
div.child{
    width: 600px;
    height: 600px;
    background: red;
    position:absolute;
    left:0;
    top: 0;
    bottom: 0;
    right: 0;
    margin: auto;
}
```

　　

**方法四:**

　　flex布局方法：当前div的父级添加flex css样式

   展示图如下：

　　![img](https://images2018.cnblogs.com/blog/1337908/201805/1337908-20180510131947523-2121210494.png)

　　代码如下：

```
.box{
    height:800px;
    -webkit-display:flex;
    display:flex;
    -webkit-align-items:center;
    align-items:center;
    -webkit-justify-content:center;
    justify-content:center;
    border:1px solid #ccc;
}
div.child{
    width:600px;
    height:600px;
    background-color:red;
}
```

　　

**方法五:**

　　table-cell实现水平垂直居中: table-cell middle center组合使用

   展示图如下：

  ![img](https://images2018.cnblogs.com/blog/1337908/201805/1337908-20180510131450441-915853082.png)

   代码如下：

```
.table-cell {
    display: table-cell;
    vertical-align: middle;
    text-align: center;
    width: 240px;
    height: 180px;
    border:1px solid #666;
}
```

　　

**方法六:**

　　绝对定位：calc() 函数动态计算实现水平垂直居中

　　展示图如下：

![img](https://images2018.cnblogs.com/blog/1337908/201805/1337908-20180510131414396-1900857513.png)

　　代码如下：

```
.calc{
  position: relative;
  border: 1px solid #ccc;
  width: 400px;
  height: 160px;
}
.calc .child{
  position: absolute;
  width: 200px;
  height: 50px;
  left:-webkit-calc((400px - 200px)/2);
  top:-webkit-calc((160px - 50px)/2);
  left:-moz-calc((400px - 200px)/2);
  top:-moz-calc((160px - 50px)/2);
  left:calc((400px - 200px)/2);
  top:calc((160px - 50px)/2);
}　　
```