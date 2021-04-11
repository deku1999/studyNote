## uniapp介绍

**规范**

1.页面文件遵循Vue单文件组件规范(SFC)规范

2.组件标签靠近小程序规范

3.接口能力（JS API）靠近微信小程序规范

4.数据绑定及事件处理同Vue.js规范

5.为兼容多端运行，建议使用flex布局进行开发

**特色**

1.条件编译

2.App端的Nvue开发

3.HTML5+

## uniapp使用

### 模板语法,数据绑定,条件判断,列表渲染

上述功能都与vue语法相同，类比使用即可

```vue
<template>
	<view class="">
		<view class="content" v-bind:class = "className" v-on:click = "open">
			{{title}}点我也可以切换隐藏
		</view>
		<view v-if="isShow">
				我是一个动态显示的内容
		</view>
		<view v-else>
				我跟上面的只能显示一个
		</view>
		<view v-for="(item,index) in arr">{{index}}-{{item}}</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				title: 'Hello world!',
				className: "test",
				isShow: true,
				arr: ['张三','李四','王五']
			}
		},
		onLoad() {
			console.log('页面加载')
			setTimeout(() => {
				this.title = 'hello uniapp'
			},2000)
		},
		methods: {
			open() {
				console.log('点击方法')
				this.isShow = !this.isShow
			}
		}	
	}
</script>
```

### 基础组件与自定义组件

#### **基础组件**

##### text

**text组件的属性**

|    属性    |  类型   | 默认值 | 必填 |                      说明                      |
| :--------: | :-----: | :----: | :--: | :--------------------------------------------: |
| selectable | boolean | false  |  否  |                  文本是否可选                  |
|   space    | string  |   .    |  否  | 显示连续空格，可选参数：`ensp`、`emsp`、`nbsp` |
|   decode   | boolean | false  |  否  |                    是否解码                    |

- `text` 组件相当于行内标签、在同一行显示
- 除了文本节点以外的其他节点都无法长按选中

##### view

> View 视图容器， 类似于 HTML 中的 div

**组件的属性**

| 属性                   | 类型    | 默认值 | 必填 | 说明                                                       |
| ---------------------- | ------- | ------ | ---- | ---------------------------------------------------------- |
| hover-class            | string  | none   | 否   | 指定按下去的样式类，当hover-calss='none'时，没有点击态效果 |
| hover-stop-propagation | boolean | false  | 否   | 指定是否阻止本节点的祖先节点出现点击态，即防止冒泡         |
| hover-start-time       | number  | 50     | 否   | 按住后多久出现点击态，单位毫秒                             |
| hover-stay-time        | numebr  | 400    | 否   | 手指松开后点击状态保留时间，单位毫秒                       |

##### button

**组件的属性**

|  属性名  |  类型   | 默认值  |           说明           |
| :------: | :-----: | :-----: | :----------------------: |
|   size   | String  | default |        按钮的大小        |
|   type   | String  | default |      按钮的样式类型      |
|  plain   | Boolean |  false  | 按钮是否镂空，背景色透明 |
| disabled | Boolean |  false  |         是否按钮         |
| loading  | Boolean |  false  | 名称是否带 loading t图标 |

- `button` 组件默认独占一行，设置 `size` 为 `mini` 时可以在一行显示多个

##### image

| 属性名 | 类型   | 默认值        | 说明                 | 平台差异说明 |
| ------ | ------ | ------------- | -------------------- | ------------ |
| src    | String |               | 图片资源地址         |              |
| mode   | String | 'scaleToFill' | 图片裁剪、缩放的模式 |              |

**Tips**

- `<image>` 组件默认宽度 300px、高度 225px；
- `src` 仅支持相对路径、绝对路径，支持 base64 码；
- 页面结构复杂，css样式太多的情况，使用 image 可能导致样式生效较慢，出现 “闪一下” 的情况，此时设置 `image{will-change: transform}` ,可优化此问题。

#### **自定义组件**

就是自己定义的vue文件，想要使用先引入然后注册即可。插槽用法，props传值和事件传递都和vue相同。

```javascript
import btn from '@components/btn/btn.vue'
export default {
	components: {
		btn
	}
}
```

### uniapp中的样式

1. rpx 即响应式px，一种根据屏幕宽度自适应的动态单位。以750宽的屏幕为基准，750rpx恰好为屏幕宽度。屏幕变宽，rpx 实际显示效果会等比放大。

2. 使用`@import`语句可以导入外联样式表，`@import`后跟需要导入的外联样式表的相对路径，用`;`表示语句结束

3. 支持基本常用的选择器class、id、element等

4. 在 `uni-app` 中不能使用 `*` 选择器。

5. `page` 相当于 `body` 节点

6. 定义在 App.vue 中的样式为全局样式，作用于每一个页面。在 pages 目录下 的 vue 文件中定义的样式为局部样式，只作用在对应的页面，并会覆盖 App.vue 中相同的选择器。

7. `uni-app` 支持使用字体图标，使用方式与普通 `web` 项目相同，需要注意以下几点：

   - 字体文件小于 40kb，`uni-app` 会自动将其转化为 base64 格式；
   - 字体文件大于等于 40kb， 需开发者自己转换，否则使用将不生效；
   - 字体文件的引用路径推荐使用以 ~@ 开头的绝对路径。

   ```
    @font-face {
        font-family: test1-icon;
        src: url('~@/static/iconfont.ttf');
    }
   ```

8. 如何使用scss或者less：去插件市场下载对应的插件，在组件中将style部分加上`lany = scss`（以scss为例）

```css
//uniapp中没有body标签，可以用page代替
<style>
	/*引入外部css文件*/
	@import './index.css'
	/*body*/
	page {
		background-color: #007Aff;
	}
	/*尺寸单位*/
	.content {
		/* px rpx rem vh vw*/
		font-size: 12px;
	}
</style>
```

### 生命周期

更多的生命周期可以到官网上去查看

#### 应用生命周期

只能在`app.vue`中去执行

```vue
<script>
	export default {
		// 应用初始化完成触发一次，全局只触发一次
		onLaunch: function() {
			//可以进行登录	全局变量等操作
			console.log('App Launch')
		},
		// 应用启动的时候，或者从后台进入到前台会触发
		onShow: function() {
			console.log('App Show')
		},
		// 应用从前台进入后台触发
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
</style>
```

#### 页面生命周期

一般的页面加载执行顺序就是下图写的顺序

```javascript
	export default {
		data() {
			return {
			}
		},
		// 监听页面加载
		onLoad() {
			
		},
		// 监听页面显示，即页面每次出现在屏幕上都会触发该事件
		onShow() {
			
		},
		// 页面加载完成
		onReady() {
			// 如果渲染速度快，会在页面进入动画完成前触发
		},
		// 监听页面隐藏
		onHide() {
			
		},
		// 监听页面卸载
		onUnload() {
			
		}
	}	
```

#### 组件生命周期

介绍几个常用的

```javascript
<script>
	export default {
		name:"test",
		data() {
			return {
				
			};
		},
		// 实例初始化之后，数据观测(data observer)和event/watcher事件配置之前被调用
		beforeCreate() {
		},
		// 在实例创建完成后被立即调用。挂载阶段还没开始，$el 属性目前不可见。 
		created() {
		},
		// 挂载到实例上去之后调用
		mounted() {
		},
		// Vue实例销毁后调用
		destroyed() {
		}
	}
</script>
```

#### 执行顺序总览

忘了的时候也可以自己去测试一下

```
App Launch
App Show
component beforeCreate
component created
page onload
page onShow
component mounted
page onready
```

基础API的用法

### 全局配置和页面配置

#### pages.json

##### 通过globalStyle进行全局配置

用于设置应用的状态栏、导航条、标题、窗口背景色等。[详细文档](https://uniapp.dcloud.io/collocation/pages?id=globalstyle)

| 属性                         | 类型     | 默认值  | 描述                                                         |
| ---------------------------- | -------- | ------- | ------------------------------------------------------------ |
| navigationBarBackgroundColor | HexColor | #F7F7F7 | 导航栏背景颜色（同状态栏背景色）                             |
| navigationBarTextStyle       | String   | white   | 导航栏标题颜色及状态栏前景颜色，仅支持 black/white           |
| navigationBarTitleText       | String   |         | 导航栏标题文字内容                                           |
| backgroundColor              | HexColor | #ffffff | 窗口的背景色                                                 |
| backgroundTextStyle          | String   | dark    | 下拉 loading 的样式，仅支持 dark / light                     |
| enablePullDownRefresh        | Boolean  | false   | 是否开启下拉刷新，详见[页面生命周期](https://uniapp.dcloud.io/use?id=%e9%a1%b5%e9%9d%a2%e7%94%9f%e5%91%bd%e5%91%a8%e6%9c%9f)。 |
| onReachBottomDistance        | Number   | 50      | 页面上拉触底事件触发时距页面底部距离，单位只支持px，详见[页面生命周期]( |

#### 创建新页面

**1.右键pages新建message目录**，在message目录下右键新建.vue文件,并选择基本模板

```vue
<template>
	<view>
		这是信息页面
	</view>
</template>
<script>
</script>
<style>
</style>
```

**2.通过pages.json来配置页面**

| 属性  | 类型   | 默认值 | 描述                                                         |
| ----- | ------ | ------ | ------------------------------------------------------------ |
| path  | String |        | 配置页面路径                                                 |
| style | Object |        | 配置页面窗口表现，配置项参考 [pageStyle](https://uniapp.dcloud.io/collocation/pages?id=style) |

pages数组数组中第一项表示应用启动页

```
"pages": [ 、
		{
			"path":"pages/message/message"
		},
		{
			"path": "pages/index/index",
			"style": {
				"navigationBarTitleText": "uni-app"
			}
		}
	]
```

通过style修改页面的标题和导航栏背景色，并且设置h5下拉刷新的特有样式

```javascript
"pages": [ //pages数组中第一项表示应用启动页，参考：https://uniapp.dcloud.io/collocation/pages
		{
			"path":"pages/message/message",
			"style": {
				"navigationBarBackgroundColor": "#007AFF",
				"navigationBarTextStyle": "white",
				"enablePullDownRefresh": true,
				"disableScroll": true,
				"h5": {
					"pullToRefresh": {
						"color": "#007AFF"
					}
				}
			}
		}
	]
```

#### 配置tabbar

如果应用是一个多 tab 应用，可以通过 tabBar 配置项指定 tab 栏的表现，以及 tab 切换时显示的对应页。

**Tips**

- 当设置 position 为 top 时，将不会显示 icon
- tabBar 中的 list 是一个数组，只能配置最少2个、最多5个 tab，tab 按数组的顺序排序。

**属性说明：**

| 属性            | 类型     | 必填 | 默认值 | 描述                                                 | 平台差异说明              |
| --------------- | -------- | ---- | ------ | ---------------------------------------------------- | ------------------------- |
| color           | HexColor | 是   |        | tab 上的文字默认颜色                                 |                           |
| selectedColor   | HexColor | 是   |        | tab 上的文字选中时的颜色                             |                           |
| backgroundColor | HexColor | 是   |        | tab 的背景色                                         |                           |
| borderStyle     | String   | 否   | black  | tabbar 上边框的颜色，仅支持 black/white              | App 2.3.4+ 支持其他颜色值 |
| list            | Array    | 是   |        | tab 的列表，详见 list 属性说明，最少2个、最多5个 tab |                           |
| position        | String   | 否   | bottom | 可选值 bottom、top                                   | top 值仅微信小程序支持    |

其中 list 接收一个数组，数组中的每个项都是一个对象，其属性值如下：

| 属性             | 类型   | 必填 | 说明                                                         |
| ---------------- | ------ | ---- | ------------------------------------------------------------ |
| pagePath         | String | 是   | 页面路径，必须在 pages 中先定义                              |
| text             | String | 是   | tab 上按钮文字，在 5+APP 和 H5 平台为非必填。例如中间可放一个没有文字的+号图标 |
| iconPath         | String | 否   | 图片路径，icon 大小限制为40kb，建议尺寸为 81px * 81px，当 postion 为 top 时，此参数无效，不支持网络图片，不支持字体图标 |
| selectedIconPath | String | 否   | 选中时的图片路径，icon 大小限制为40kb，建议尺寸为 81px * 81px ，当 postion 为 top 时，此参数无效 |

案例代码：pages.json

```javascript
"tabBar": {
		"list": [
			{
				"text": "首页",
				"pagePath":"pages/index/index",
				"iconPath":"static/tabs/home.png",
				"selectedIconPath":"static/tabs/home-active.png"
			},
			{
				"text": "信息",
				"pagePath":"pages/message/message",
				"iconPath":"static/tabs/message.png",
				"selectedIconPath":"static/tabs/message-active.png"
			},
			{
				"text": "我们",
				"pagePath":"pages/contact/contact",
				"iconPath":"static/tabs/contact.png",
				"selectedIconPath":"static/tabs/contact-active.png"
			}
		]
	}
```

#### condition启动模式配置

启动模式配置，仅开发期间生效，用于模拟直达页面的场景，如：小程序转发后，用户点击所打开的页面。

**属性说明：**

| 属性    | 类型   | 是否必填 | 描述                             |
| ------- | ------ | -------- | -------------------------------- |
| current | Number | 是       | 当前激活的模式，list节点的索引值 |
| list    | Array  | 是       | 启动模式列表                     |

**list说明：**

| 属性  | 类型   | 是否必填 | 描述                                                         |
| ----- | ------ | -------- | ------------------------------------------------------------ |
| name  | String | 是       | 启动模式名称                                                 |
| path  | String | 是       | 启动页面路径                                                 |
| query | String | 否       | 启动参数，可在页面的 [onLoad](https://uniapp.dcloud.io/use?id=%e9%a1%b5%e9%9d%a2%e7%94%9f%e5%91%bd%e5%91%a8%e6%9c%9f) 函数里获得 |

### 下拉刷新

#### 开启下拉刷新

在uni-app中有两种方式开启下拉刷新

+ 需要在 `pages.json` 里，找到的当前页面的pages节点，并在 `style` 选项中开启 `enablePullDownRefresh`
+ 通过调用uni.startPullDownRefresh方法来开启下拉刷新

**通过配置文件开启**

```vue
<template>
	<view>
		杭州学科
		<view v-for="(item,index) in arr" :key="index">
			{{item}}
		</view>
	</view>
</template>
<script>
	export default {
		data () {
			return {
				arr: ['前端','java','ui','大数据']
			}
		}
	}
</script>
<style>
</style>
```

通过pages.json文件中找到当前页面的pages节点，并在 `style` 选项中开启 `enablePullDownRefresh`

```json
{
  "path":"pages/list/list",
    "style":{
      "enablePullDownRefresh": true
    }
}
```

**通过API开启**

```html
uni.startPullDownRefresh()
```

#### 监听下拉刷新

通过onPullDownRefresh可以监听到下拉刷新的动作

```javascript
export default {
  data () {
    return {
      arr: ['前端','java','ui','大数据']
    }
  },
  methods: {
    startPull () {
      uni.startPullDownRefresh()
    }
  },
  onPullDownRefresh () {
    console.log('触发下拉刷新了')
  }
}
```

#### 关闭下拉刷新

uni.stopPullDownRefresh()，停止当前页面下拉刷新。

案例演示

```vue
<template>
	<view>
		<button type="primary" @click="startPull">开启下拉刷新</button>
		杭州学科
		<view v-for="(item,index) in arr" :key="index">
			{{item}}
		</view>
	</view>
</template>
<script>
	export default {
		data () {
			return {
				arr: ['前端','java','ui','大数据']
			}
		},
		methods: {
			startPull () {
				uni.startPullDownRefresh()
			}
		},
		
		onPullDownRefresh () {
			this.arr = []
			setTimeout(()=> {
				this.arr = ['前端','java','ui','大数据']
				uni.stopPullDownRefresh()
			}, 1000);
		}
	}
</script>
```

### 导航跳转

#### 利用navigator跳转

跳转到普通页面

```vue
<navigator url="/pages/about/about" hover-class="navigator-hover">
  <button type="default">跳转到关于页面</button>
</navigator>
```

跳转到tabbar页面

```vue
<navigator url="/pages/message/message" open-type="switchTab">
  <button type="default">跳转到message页面</button>
</navigator>
```

#### 利用编程式导航跳转

**利用navigateTo进行导航跳转**

保留当前页面，跳转到应用内的某个页面，使用`uni.navigateBack`可以返回到原页面。

```html
<button type="primary" @click="goAbout">跳转到关于页面</button>
```

通过navigateTo方法进行跳转到普通页面

```javascript
goAbout () {
  uni.navigateTo({
    url: '/pages/about/about',
  })
}
```

**通过switchTab跳转到tabbar页面**

跳转到tabbar页面

```html
<button type="primary" @click="goMessage">跳转到message页面</button>
```

通过switchTab方法进行跳转

```javascript
goMessage () {
  uni.switchTab({
    url: '/pages/message/message'
  })
}
```

**redirectTo进行跳转** 

关闭当前页面，跳转到应用内的某个页面。

```vue
<!-- template -->
<button type="primary" @click="goMessage">跳转到message页面</button>
<!-- js -->
goMessage () {
  uni.switchTab({
    url: '/pages/message/message'
  })
}
```

#### 导航跳转传递参数

在导航进行跳转到下一个页面的同时，可以给下一个页面传递相应的参数，接收参数的页面可以通过onLoad生命周期进行接收。

传递参数的页面

```javascript
goAbout () {
  uni.navigateTo({
    url: '/pages/about/about?id=80',
  });
}
```

接收参数的页面

```vue
<script>
	export default {
		onLoad (options) {
			console.log(options)
		}
	}
</script>
```

### 上拉加载

1. 通过页面生命周期的onReachBottom函数可以监听到触底的行为
2. 通过在pages.json文件中找到当前页面的pages节点下style中配置，onReachBottomDistance可以设置距离底部开启加载的距离，默认为50px

### 网络请求

在uni中可以调用uni.request方法进行请求网络请求

需要注意的是：在小程序中网络相关的 API 在使用前需要配置域名白名单。

**发送get请求**

```vue
<template>
	<view>
		<button @click="sendGet">发送请求</button>
	</view>
</template>
<script>
	export default {
		methods: {
			sendGet () {
				uni.request({
					url: 'http://localhost:8082/api/getlunbo',
					success(res) {
						console.log(res)
					}
				})
			}
		}
	}
</script>
```

**发送post请求**

### 数据缓存

#### **uni.setStorage**

将数据存储在本地缓存中指定的 key 中，会覆盖掉原来该 key 对应的内容，这是一个异步接口。

```vue
<template>
	<view>
		<button type="primary" @click="setStor">存储数据</button>
	</view>
</template>
<script>
	export default {
		methods: {
			setStor () {
				uni.setStorage({
				 	key: 'id',
				 	data: 100,
				 	success () {
				 		console.log('存储成功')
				 	}
				 })
			}
		}
	}
</script>
<style>
</style>
```

#### **uni.setStorageSync**

将 data 存储在本地缓存中指定的 key 中，会覆盖掉原来该 key 对应的内容，这是一个同步接口。

```vue
<template>
	<view>
		<button type="primary" @click="setStor">存储数据</button>
	</view>
</template>
<script>
	export default {
		methods: {
			setStor () {
				uni.setStorageSync('id',100)
			}
		}
	}
</script>
<style>
</style>
```

#### uni.getStorage

从本地缓存中异步获取指定 key 对应的内容。

```vue
<template>
	<view>
		<button type="primary" @click="getStorage">获取数据</button>
	</view>
</template>
<script>
	export default {
		data () {
			return {
				id: ''
			}
		},
		methods: {
			getStorage () {
				uni.getStorage({
					key: 'id',
					success:  res=>{
						this.id = res.data
					}
				})
			}
		}
	}
</script>
```

#### uni.getStorageSync

从本地缓存中同步获取指定 key 对应的内容。

```vue
<template>
	<view>
		<button type="primary" @click="getStorage">获取数据</button>
	</view>
</template>
<script>
	export default {
		methods: {
			getStorage () {
				const id = uni.getStorageSync('id')
				console.log(id)
			}
		}
	}
</script>
```

#### uni.removeStorage

从本地缓存中异步移除指定 key。

```vue
<template>
	<view>
		<button type="primary" @click="removeStorage">删除数据</button>
	</view>
</template>
<script>
	export default {
		methods: {
			removeStorage () {
				uni.removeStorage({
					key: 'id',
					success: function () {
						console.log('删除成功')
					}
				})
			}
		}
	}
</script>
```

#### uni.removeStorageSync

从本地缓存中同步移除指定 key。

```vue
<template>
	<view>
		<button type="primary" @click="removeStorage">删除数据</button>
	</view>
</template>
<script>
	export default {
		methods: {
			removeStorage () {
				uni.removeStorageSync('id')
			}
		}
	}
</script>
```

### 上传图片、预览图片

#### 上传图片

uni.chooseImage方法从本地相册选择图片或使用相机拍照。count属性限制不了h5端。

```vue
<template>
	<view>
		<button @click="chooseImg" type="primary">上传图片</button>
		<view>
			<image v-for="item in imgArr" :src="item" :key="index"></image>
		</view>
	</view>
</template>
<script>
	export default {
		data () {
			return {
				imgArr: []
			}
		},
		methods: {
			chooseImg () {
				uni.chooseImage({
					count: 9,
					success: res=>{
						this.imgArr = res.tempFilePaths
					}
				})
			}
		}
	}
</script>
```

#### 预览图片

结构

```vue
<view>
	<image v-for="item in imgArr" :src="item" @click="previewImg(item)" :key="item"></image>
</view>
```

预览图片的方法

```javascript
previewImg (current) {
  uni.previewImage({
    urls: this.imgArr,	//需要预览的图片链接列表
    current		//当前显示图片的链接/索引值
  })
}
```

### 平台配置

#### 运行平台配置

**微信小程序**

首先确保微信开发者工具服务端口打开

![image-20210323152458189](C:\Users\22140\AppData\Roaming\Typora\typora-user-images\image-20210323152458189.png)

点击功能栏的工具，选择设置

![image-20210323152534940](C:\Users\22140\AppData\Roaming\Typora\typora-user-images\image-20210323152534940.png)

**app真机、模拟器**

usb连接线与电脑连接，之后确保手机开启开发者模式。项目直接运行到安卓真机即可，注意项目启动Hbuilder调试基座后需要重启项目即可生效。

**h5**

![image-20210323153417730](C:\Users\22140\AppData\Roaming\Typora\typora-user-images\image-20210323153417730.png)

#### 条件编译

##### 基本介绍

条件编译是用特殊的注释作为标记，在编译时根据这些特殊的注释，将注释里面的代码编译到不同平台。可以让我们的代码打包更快。

**写法：**以 #ifdef 或 #ifndef 加 **%PLATFORM%** 开头，以 #endif 结尾。

- \#ifdef：if defined 仅在某平台存在
- \#ifndef：if not defined 除了某平台均存在
- **%PLATFORM%**：平台名称

| 条件编译写法                                             | 说明                                                         |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| #ifdef **APP-PLUS** 需条件编译的代码 #endif              | 仅出现在 App 平台下的代码                                    |
| #ifndef **H5** 需条件编译的代码 #endif                   | 除了 H5 平台，其它平台均存在的代码                           |
| #ifdef **H5** \|\| **MP-WEIXIN** 需条件编译的代码 #endif | 在 H5 平台或微信小程序平台存在的代码（这里只有\|\|，不可能出现&&，因为没有交集） |

**%PLATFORM%** **可取值如下：**

下面仅介绍一些非常常用的

| 值                      | 平台                                                         |
| :---------------------- | :----------------------------------------------------------- |
| APP-PLUS                | App                                                          |
| APP-PLUS-NVUE或APP-NVUE | App nvue                                                     |
| H5                      | H5                                                           |
| MP                      | 微信小程序/支付宝小程序/百度小程序/字节跳动小程序/QQ小程序/360小程序 |

##### 支持的文件

.vue，.js，.css，pages.json，各项预编译文件如：.scss，.less，.stylus，.ts，.pug

**注意：**

- 条件编译是利用注释实现的，在不同语法里注释写法不一样，js使用 `// 注释`、css 使用 `/* 注释 */`、vue/nvue 模板里使用 `<!-- 注释 -->`；
- 条件编译APP-PLUS包含APP-NVUE和APP-VUE，APP-PLUS-NVUE和APP-NVUE没什么区别，为了简写后面出了APP-NVUE ；

##### 不同文件下的条件编译

以下仅展示部分，更多的还请查看官网

**js**

```javascript
//#ifndef H5
uni.scanCode({
	success: (res) => {console.log(res.result)}
})
//#endif
```

**组件**

```vue
<template>
	<view>
    	<!-- #ifdef MP-WEIXIN -->
        	<official></official>
        <!-- #endif -->
    </view>
</template>
```

**样式**

```css
/* #ifdef MP-WEIXIN */
.wx-color {
	color: #fff000;
}
/* #endif */
```

### 基础Api的用法

更多的用法可以去官网查阅，这里仅展示一个基础示例用法

```javascript
	export default {
		data() {
			return {
			}
		},
		onLoad() {
			uni.getSystemInfo({
				success(res) {
					console.log('成功了',res)
				},
				fail(err) {
					console.log('失败了',res)
				},
				complete(res) {
					console.log('不管成功与否都会输出')
				}
			})
		}
	}
```

## 项目配置

### uni-ui的使用

这里以九宫格为例

1、进入Grid宫格组件

2、使用HBuilderX导入该组件

3、导入该组件

```html
import uniGrid from "@/components/uni-grid/uni-grid.vue"
import uniGridItem from "@/components/uni-grid-item/uni-grid-item.vue"
```

4、注册组件

```html
components: {uniGrid,uniGridItem}
```

5、使用组件

```html
<uni-grid :column="3">
  <uni-grid-item>
    <text class="text">文本</text>
  </uni-grid-item>
  <uni-grid-item>
    <text class="text">文本</text>
  </uni-grid-item>
  <uni-grid-item>
    <text class="text">文本</text>
  </uni-grid-item>
</uni-grid>
```

### 目录解读

1.components-自定义组件目录

2.pages-页面存放目录

3.static-静态文件资源目录

4.unpackage 编译后的文件存放目录

5.utils 公用的工具类

6.common 公用的文件

7.app.vue 

8.main.js 应用入口

9.manifest.json 项目配置

10.pages.json 页面配置

11. uni.scss ，uniapp自带的sass，可以直接用里面的变量名而不用导入。

### pages.json配置

配置各个页面的个性化或者公有化配置，详情参照官网。

**tabBar导航栏配置**

1.图标图片必须是本地的，建议81*81px。

2.注意如果使用了tabBar，那么页面之间进行切换是不会触发onLoad的，即不会重新加载数据，如果我们想要重新渲染只能通过生命周期函数，如

`onShow`,或者`onTabItemTap`，事件中可以默认传入event参数，可以获取相关页面的信息。

### uniapp种sass配置

在工具栏种选择插件安装，安装sass插件即可

### uniCloud

#### 基本介绍

![image-20210323163238477](C:\Users\22140\AppData\Roaming\Typora\typora-user-images\image-20210323163238477.png)

**优势**

1.非H5，免域名使用服务器

2.对于敏捷性业务，完全不需要前后端分离，即不用自己写后台了。

**组成部分**

1.云函数

给客户端返回数据

2.云数据库

在云函数中读写nosql型的数据库

3.云存储和CDN

云存储可以上传我们的一些动图，音频之类的。

4.H5域名配置

主要是为了解决云空间中的域名的跨域问题，可以在配置中新增需要跨域的域名即可。

#### 项目中配置uniCloud

1.创建项目的时候记得勾选uniCloud

2.HbuilderX必须保持登录状态

3.manifest.json中获取应用标识

![image-20210323164225919](C:\Users\22140\AppData\Roaming\Typora\typora-user-images\image-20210323164225919.png)

4.在项目的unicloudfunctions文件夹右键创建云服务空间，之后就可以右键选择自己创建的云服务空间

5.之后可以右键创建云函数，然后对云函数右键进行上传部署。

6.可以右键unicloudfunctions文件夹选择打开uniCloud的web控制台查看自己的云空间。

#### 云函数

1.云函数的格式就用创建时生成的默认的，不要动。

2.每次修改完后一定要记得重新右键上传部署。（注：有个更快捷的是上传并运行，可以直接测试云函数）

3.调用

```javascript
uniCloud.callFunction({
	name: '函数名',
	data: {要传的数据},
	//失败与成功的回调
	success(res){},
	fail() {}
})
```

```javascript
//云函数默认格式
'use strict';
//运行在云端（服务器端）的函数
exports.main = async (event, context) => {
    //event为客户端上传的参数
    //context包含了调用信息和运行状态，获取每次调用的上下文
    console.log('event:' event)
    //返回数据给客户端
    return {
        code: 200,
        msg: event.name + '的年龄是' + event.age,
        context
    }
}
```

#### 云数据库

1.获取云数据库

```javascript
'use strict';
//获取数据库的引用
const db = uniCloud.database()
//运行在云端（服务器端）的函数
exports.main = async (event, context) => {
//获取集合的引用
    const collecttion = db.collecttion('集合名')
//增加记录
    //增加单条记录
    let res = await collection.add({
        name: 'uniapp'
    })
    //增加多条记录
    let res = await collection.add([
        {name: 'vue'},{name: 'html', type: '前端'}
    ])
//删除记录
    const res = await collection.doc('数据id').remove()
//更新记录
    //set方法，与update方法的区别是update如果找不到数据会不做操作，而set会新建
    conse res = await collection.doc('id').set({
        name: 'vue-test',
        type: '前端'
    })
    //update方法
        conse res = await collection.doc('id').update({
        name: 'vue-test',
        type: '前端'
    })
//查找记录
    //id的查询
    const res = await collection.doc('id').get()
    //where查询，应用面更宽
    const res = await collection.where({
        name: 'vue-test'
    }).get()
    console.log(JSON.stringify(res))
    return {}
}
```

#### 云存储

上传与删除图片到云存储

```javascript
//上传图片
uni.chooseImage({
	count: 1,
	success(res) {
        //获取图片路径
        const tempFilePath = res.tempFilePath[0]
        //uni上传图片至云存储方法
        uniCloud.uploadFile({
			filePath: tempFilePath,
            success(res) {
                console.log(res.fileID)
            },
            fail() {
                console.log(err)
            }
        })
		console.log(res)
	},
	fail(err) {
		console.log(err)
	}
})

//删除图片
uniCloud.deleteFile({
    fileList: ['文件云存储的路径'],
    success(res) {
		console.log(res)
    },
    fail(err) {
        console.log(err)
    }
})
```

### 项目开始

1. 初始化数据库

cloudfunctions右键创建db_init.json文件来初始化数据库

2. 实现自定义导航栏

包括首页，关注，我的三个部分

首页的导航栏图标可以通过uniapp自带的插件市场中搜索icons之后导入进行使用

3. 通过swiper-item实现左右滑动（通过change事件可以监听swiper滚动，通过:current可以跳转到特定的位置）

4. 请求时的加载图标，可以通过uniapp的loadmore插件

5. 通过vuex保存搜索记录

新建store文件夹和index.js,之后在main.js中注册，用法和vue中基本一样，对vuex中数据做缓存处理。

6. gaoyia-parse，渲染富文本的组件，可以去插件市场下（8-4）

7. popup组件（评论组件），附带的有uni-transition组件