# bootStrap

## bootstrap基本使用

下载后需要引入它的css和js文件，需要注意bootstrap的js文件以jquery为基础，即要先引入jquery文件。

## 容器类

### container

固定容器，宽度以阈值进行分段，在一定的分段空间内为固定，有padding，自动居中。

槽宽指的是padding，left和right 各15px。

| 屏幕                        | 容器宽              |
| --------------------------- | ------------------- |
| \>=1200   (lg 大屏pc)       | 1170px（1140+槽宽） |
| \>=992 && <1200 (md 中屏pc) | 970px（940+槽宽）   |
| \>=768 && <992  (sm 平板)   | 750px（720+槽宽）   |
| <768 (xs 移动手机)          | auto                |

### container-fluid

流体容器，宽度为auto，有padding

## 栅格布局

默认是分为12份，这里的lg是标识符代表不同的阈值区间，可以写多个阈值区间以适配不同的屏幕下不同的分布方式。

```html
<div class="row">
	<div class="col-lg-10"> col-lg-10 </div>
	<div class="col-lg-2"> col-lg-2 </div>
</div>
```

## 核心知识

Bootstrap的核心就是它的栅格响应布局，可以观看源码理解，其他的组件会用即可。