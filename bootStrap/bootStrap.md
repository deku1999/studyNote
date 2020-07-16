# 布局

## 容器类

固定容器，宽度以阈值进行分段，在一定的分段空间内为固定，自动居中，左右padding各15px。

|                  | <576 | ≥576px | ≥768px | ≥992px | ≥1200px |
| ---------------- | ---- | ------ | ------ | ------ | ------- |
| .container       | auto | 540px  | 720px  | 960px  | 1140px  |
| .container-sm    | auto | 540px  | 720px  | 960px  | 1140px  |
| .container-md    | auto | auto   | 720px  | 960px  | 1140px  |
| .container-lg    | auto | auto   | auto   | 960px  | 1140px  |
| .container-xl    | auto | auto   | auto   | auto   | 1140px  |
| .container-fluid | auto | auto   | auto   | auto   | auto    |

## 栅格布局

- 栅格布局必须要写在row下，默认是分为12份。

- 这里的lg是标识符代表不同的阈值区间，可以写多个阈值区间以适配不同的屏幕下不同的分布方式。

```html
<div class="row">
	<div class="col-lg-10"> col-lg-10 </div>
	<div class="col-lg-2"> col-lg-2 </div>
</div>
```

| 阈值   | 无   | 540像素 | 720px   | 960px   | 1140px  |
| ------ | ---- | ------- | ------- | ------- | ------- |
| 类前缀 | .col | .col-sm | .col-md | .col-lg | .col-xl |

- 这里的阈值代表着大于它是能保持栅格结构的，而当小于它时列就独占一行。
- .col会始终等分。而别的没分够12份则会空出，多出则会换行。
- 栅格布局可以嵌套。

### 跨行

- 设置一个w-100类即可换行。

```html
<div class="container">
  <div class="row">
    <div class="col">col</div>
    <div class="col">col</div>
    <div class="w-100"></div>
    <div class="col">col</div>
    <div class="col">col</div>
  </div>
</div>
```

