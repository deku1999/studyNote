## 最简单的案例

相比于`react`，多导入了`react-native`的东西，这里选用了函数式组件作为参考

```react
import React from 'react'
import {StyleSheet,View,Text} from 'react-native'
const App = () => {
    return (
        <View style = {styles.view}>
            <Text>hello world!</Text>
        </View>
    )
}
const styles = StyleSheet.create({
    view: {
        backgroundColor: 'rgba(200,200,200,0.5)'
    }
})
export default App
```

## 样式

react-native中元素的样式是通过`style`属性来设置的

### 特点

- 样式属性命名必须采用驼峰命名
- 样式不会继承，每一个组件必须单独设置样式，不过文本组件可以继承父文本组件的
- 如果有多个相同属性的样式作用于同一元素，那么以最右边的为准
- React Native 所有的外观都是没有单位的。或者说你可以理解它们的默认单位都是 **像素**。

### 设置方法

1. 通过`StyleSheet`创造的样式对象，然后令元素的style等于对象的属性即可，例如`style = {styles.container}`
2. 直接书写，不利用`StyleSheet`，`style = {{marginTop: 8, padding: 8}}`，注意这里不是双括号语法，而是单括号里嵌了一个括号
3. 将样式对象与直接书写结合，需要写成数组的形式，重复的就以最右边的为准，`style = {[ styles.container, {padding: 8}]}`

## FlexBox布局与Text

### FlexBox布局

我们在 React Native 中使用 flexbox 规则来指定某个组件的子元素的布局。Flexbox 可以在不同屏幕尺寸上提供一致的布局结构。一般来说，使用`flexDirection`、`alignItems`和 `justifyContent`三个样式属性就已经能满足大多数布局需求

**不同点**：

1.`flexDirection`的默认值是`column`而不是`row`

2.`flex`只能指定一个数字值，即不支持多参数

3.View组件设置`flex: 1`会在竖直方向直接铺满屏幕，因为默认的主轴是竖直方向。

4.align-items默认是`stretch`，而webcss是`flex-start`

5.不支持属性:`align-content`，`flex-basis`，`order`，`flex-flow`，`flex-shrink`

### 文本组件Text

文本组件可以嵌套另一个组件，被嵌套的组件会继承父级的文本组件的样式和属性

```react
<Text
   color="#333333"
   lineHeight="12"
   fontSize="12" >简单教程
      <Text
         fontSize="11"
         >简单编程</Text>
</Text>
```

#### 基本介绍

在 React Native 中如果要显示一段文本，可以使用 React Native 内置的文本组件 `Text`。

文本组件 `Text` 只能用来显示文本，如果要显示网页，可以使用网页组件 `WebView`。

虽然文本组件可能将部分文本显示为电话号码或者网址等可以点击的样子，但毕竟有限。没有 WebView 来的强大。

#### 使用语法

文本组件可以直接写样式属性

```react
<Text
   color="#333333"
   lineHeight="12"
   fontSize="12" >简单教程</Text>
```

#### 属性说明

| 属性               | 类型   | 是否必填 | 说明                                                         |
| ------------------ | ------ | -------- | ------------------------------------------------------------ |
| selectable         | bool   | false    | 是否可选中，`true` 为真，`false` 为否                        |
| numberOfLines      | number | false    | 用于在计算文本布局（包括换行）后使用省略号截断文本，使得总行数不超过此数字 |
| ellipsizeMode      | string | false    | 如果设置了 `numberOfLines`，那么该属性用于设置文本如何被截断 |
| dataDetectorType   | string | false    | 用于设置如何转换文本中的某些子文本                           |
| color              | color  | 否       | 用于设置文本的颜色                                           |
| fontFamily         | string | 否       | 用于设置文本的字体                                           |
| fontSize           | number | 否       | 用于设置文字的大小                                           |
| fontStyle          | string | 否       | 用于设置文字是否倾斜，`normal` 正常，`italic` 倾斜，默认为 `normal` |
| fontWeight         | string | 否       | 文字的粗细，可以设置的值有: 'normal', 'bold', '100', '200', '300', '400', '500', '600', '700', '800', '900' |
| lineHeight         | number | 否       | 用于设置文本的行高                                           |
| textAlign          | string | 否       | 用于设置文本的对其方式，可选的值有 'auto', 'left', 'right', 'center', 'justify'。Android 下只有 `left` 即使设置了其它值扔就是 `left` |
| textDecorationLine | string | 否       | 用于设置文本的下划线类型，可选的值有 'none', 'underline', 'line-through', 'underline line-through' |
| textShadowColor    | color  | 否       | 用于设置文本的阴影色                                         |
| textShadowOffset   | object | 否       | 用于设置阴影的偏移量，格式为 `{width: number,height: number}` |
| textShadowRadius   | number | 否       | 用于设置阴影的圆角度。                                       |
| letterSpacing      | number | 否       | 用于设置字与字之间的距离                                     |
| textTransform      | string | 否       | 用于设置文本转换格式，可选的值有 'none', 'uppercase', 'lowercase', 'capitalize' |

## 组件状态state

组件状态 state 是一个`js`对象或字典 `{}`。

### 初始化state

在 ES6 时代，组件状态就是组件内部的一个变量。

初始化的方式有两种：

1.类的实例属性

```react
class App extends React.Component {
    state = {
        name: '老陈打码',
        site: 'www.cpengx.cn'
    }
}
```

2.在类的构造函数中初始化

```react
class App extends React.Component {
    constructor()
    {
        super()
        this.state = {
            name: '简单教程',
            site: 'https://www.twle.cn'
        }
    }
}
```

### 使用

直接`this.state.属性名`即可，不过推荐使用对象解构`const {name, site} = this.state`

### 更新state

通过`setState`方法

```react
this.setState({
	name: 'new Name'
})
```

## 组件属性props

组件的调用者可以通过 **属性** 将数据传递给组件，然后组件内部可以通过 **组件属性 props** 来获取调用者传递的数据。例如父组件传值给子组件。

```react
import React, { Component } from 'react'
import { Text, View, StyleSheet,Alert} from 'react-native'
//子组件
class SiteNameComponent extends React.Component {
    constructor(props) {
        super(props)
        this.state = { name: props.name }
    }
    updateState = () => {
        const name = this.state.name == '简单教程' ? '简单教程，简单编程' : '简单教程'
        this.setState({name:name})
    }
    render() {
        const { name } = this.state
        return (
        <View>
            <Text onPress={this.updateState}>{name}</Text>
        </View>
        )
    }
}
//父组件
export default class App extends React.Component {

    render() {
        return (
            <View style={styles.container}>
                <SiteNameComponent name={'简单教程'} />
            </View>
        )
    }
}

const styles = StyleSheet.create ({
   container: {
      margin:10
   },
})

```

## 输入组件TextInput

输入组件 TextInput 是一个可视组件，使用语法如下

```react
<TextInput 
   style = {styles}
   underlineColorAndroid = "{transparent|"
   placeholder = "Email"
   placeholderTextColor = "{#9a73ef}"
   numberOfLines={1}
   editable={true|false}

   keyboardType={"default"|"number-pad"|"decimal-pad"|
      "numeric"|"email-address"|"phone-pad"}

   secureTextEntry={true|false}
   multiline={true|false}
   returnKeyType = {"done"|"go"|"next"|"search"|"send"}
   autoCapitalize = "none"
   onChangeText = {function(text){}}/>
```



| 属性                  | 类型                                                         | 说明                                                         |
| --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| style                 | [style](https://www.twle.cn/c/yufei/reactnative/reactnative-basic-styling.html) | 用于定制组件的样式                                           |
| underlineColorAndroid | color                                                        | Android 中下划线的颜色，透明则为 `transparent`               |
| placeholder           | string                                                       | 占位符                                                       |
| placeholderTextColor  | color                                                        | 占位符的颜色                                                 |
| multiline             | bool                                                         | 是否多行，默认为单行                                         |
| numberOfLines         | number                                                       | 设置了 `multiline` 后要设置的行数                            |
| editable              | bool                                                         | 是否可编辑                                                   |
| keyboardType          | string                                                       | 键盘类型，可选的值有 "default","number-pad","decimal-pad", "numeric","email-address","phone-pad" |
| secureTextEntry       | bool                                                         | 是否属于密码框类型                                           |
| returnKeyType         | string                                                       | 键盘上的返回键类型，可选的值有 "done","go","next","search","send" |
| autoCapitalize        | string                                                       | 字母大写模式，可选的值有:'none', 'sentences', 'words', 'characters' |
| onChangeText          | function                                                     | 文本变更后的回调函数，参数为输入框里的文本                   |

## 图片组件Image

React Native 内建了图片组件 Image 来显示图片，这个组件既可以显示本地图片，也可以显示网络图片，还可以显示 base64 格式的图片。

### 使用组件

Image 组件的基本使用语法如下

```react
<Image 
   style  = {image_style}
   source = {image_url} 
   resizeMode = {"cover"|"contain"|"stretch"|"repeat"|"center"}
/>
```

Image 组件既可以显示本地图片也可以显示网络图片，但它们的语法格式有点不同。

显示本地图片的语法格式为

```react
<Image 
   style  = {image_style}
   source = {require('./image_path')} 
   resizeMode = {"cover"|"contain"|"stretch"|"repeat"|"center"}
/>
```

显示网络图片的语法格式为

```react
<Image 
   style  = {image_style}
   source={{uri: 'image_url'}
   resizeMode = {"cover"|"contain"|"stretch"|"repeat"|"center"}
/>
```

显示 base64 格式的图片的语法格式为

```react
<Image 
   style  = {image_style}
   source={{uri: 'uri: data:image/png;base64,[image_base64_data]'}
   resizeMode = {"cover"|"contain"|"stretch"|"repeat"|"center"}
/>
```

> 注意： 显示网络图片和显示 base64 格式的图片，`style` 样式中一定要包含 `width` 和 `height` 属性。

### 属性

1. `style` 属性。

| 属性                    | 类型   | 说明                                                         |
| ----------------------- | ------ | ------------------------------------------------------------ |
| borderTopRightRadius    | number | 设置右上角的圆角度数，默认值为 0                             |
| borderBottomLeftRadius  | number | 设置左下角的圆角度数，默认值为 0                             |
| borderBottomRightRadius | number | 设置右下角的圆角度数，默认值为 0                             |
| borderTopLeftRadius     | number | 设置左上角的圆角度数，默认值为 0                             |
| borderRadius            | number | 统一设置四个角的圆角度数，默认值为 0                         |
| borderColor             | color  | 设置边框的颜色                                               |
| borderWidth             | number | 设置边框的宽度，默认值为 0                                   |
| backgroundColor         | color  | 设置图片组件的背景色                                         |
| opacity                 | number | 设置图片组件的透明度                                         |
| overflow                | string | 当组件超出屏幕宽高时是否可见， 'visible' 显示, 'hidden' 隐藏 |
| backfaceVisibility      | string | 定义当组件不面向屏幕时是否可见， 'visible' 显示, 'hidden' 隐藏 |
| tintColor               | color  | 将所有非透明的图片像素改为此颜色                             |
| resizeMode              | string | 设置图片如何适应图片容器，可选的值有： 'cover', 'contain', 'stretch', 'repeat', 'center' |

2.`source` 属性。

`source` 属性用于设置图片的地址，图片地址可以是本地图片，网络图片和 base64 格式的图片。使用方式我们上面已经介绍过了。

3.`loadingIndicatorSource` 属性。

`loadingIndicatorSource` 属性用于加载网络图片时的 placeholder 图片。也可以说是图片加载指示器。它的使用格式和 `source` 属性一样，但不能是网络图片，只能是本地图片或 base64 格式图片。

4.`resizeMethod` 属性。

`resizeMethod` 属性用于设置图片如何适配图片组件。默认值为 `auto`。可选的值有： 'cover', 'contain', 'stretch', 'repeat', 'center'。

| 值     | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| auto   | 由系统自己在 `resize` 或 `scale` 之间选择                    |
| resize | 显示之前先进行重新调整大小，当图片超出组件太多的时间建议使用此值 |
| scale  | 缩放图片，当地图片比组件小或者图片和组件差不多大小时使用此值 |

## 活动指示器(loading)组件ActivityIndicator

### 介绍

React Native 中的活动指示器组件 ActivityIndicator 就长下面这样。

> 嗯，不是全部，只是其中一个转圈圈的。

![React Native 活动指示器组件 ActivityIndicator](https://www.twle.cn/static/upload/img/2019/07/14/20190714090724_4.gif)

有一些比较耗时的操作，可能需要用户等待，那么就可以用 **活动指示器组件 ActivityIndicator** 告诉用户你需要等待。

其实，任何从用户点击开始，不能立刻给予用户反馈的操作，都需要使用 **活动指示器组件 ActivityIndicator** 告诉用户等待片刻。

### 使用语法

**活动指示器组件 ActivityIndicator** 的使用语法和其它大部分组件一样。只不过它是一个自闭合组件，没有任何子元素。

```react
<ActivityIndicator
  animating = {true|false}
  color = {'[color]'}
  size = {"large"| "small"} />
```

#### 属性说明

| 属性      | 类型    | 是否必须 | 说明                                                        |
| --------- | ------- | -------- | ----------------------------------------------------------- |
| animating | boolean | 否       | 是否显示活动指示器。默认为 `true`，`false` 则表示隐藏       |
| color     | color   | 否       | ⭕️ 的颜色，默认情况下，`iOS` 为灰色，`Android` 为 **深青色** |
| size      | string  | 否       | 只有两个选项 `large` 和 `small`，默认是 `small`             |

## 弹出框Alert

弹出框 是浮于当前界面之上的，用于阻止用户的下一步操作，直到用户点击了弹出框上的任意按钮为止。弹出框 一般用于弹出 **提示**、**弹出警告**、**弹出确认** 等需要用户注意和确认的动作。

### 弹出提示

弹出提示框一般只有一个 **确认** 按钮，用户点击 **确认** 就是 **我知道了的** 意思。

![React Native Alert](https://www.twle.cn/static/upload/img/2019/07/10/20190710071357_4.png)

### 弹出警告

弹出警告框一般有两个按钮 **确认** 和 **取消**， **取消** 按钮在右边，方便用户点击。

![React Native Alert](https://www.twle.cn/static/upload/img/2019/07/10/20190710071725_4.png)

### 弹出确认

弹出确认框一般有两个按钮 **确认** 和 **取消**， **确认** 按钮在右边，方便用户点击。

![React Native Alert](https://www.twle.cn/static/upload/img/2019/07/10/20190710071505_4.png)

### 使用方法

**弹出提示**

```react
const showAlert = () =>{
        Alert.alert('发送数据成功')
    }
```

**弹出警告**

```react
const showTip = () => {
        Alert.alert('删除数据成功')
    }
const showAlert = () =>{
        Alert.alert(
            '警告',
            '确认删除？',
            [
                {text: '确认', onPress: () => showTip()},
                {text: '取消', style: 'cancel'}, 
            ],
            {cancelable: false}
        )
    }
```

**弹出确认**

```react
const showTip = () => {
        Alert.alert('修改数据成功')
    }
const showAlert = () =>{
    Alert.alert(
        '确认',
        '是否确认修改？',
        [
            {text: '取消', style: 'cancel'},
            {text: '确认', onPress: () => showTip()},
        ],
        {cancelable: false}
    )
}
```

## 存储数据组件AsyncStorage

需要先安装组件

```
npm i @react-native-community/async-storage
```

**引入组件**

```
import AsyncStorage from '@react-native-community/async-storage';
```

**对外提供的方法**

| 方法          | 说明                                              |
| ------------- | ------------------------------------------------- |
| getItem()     | 根据给定的 key 来读取数据                         |
| setItem()     | 将一个键值对添加到系统中，如果已经存在 key 则覆盖 |
| removeItem()  | 根据给定的 key 删除指定的键值对                   |
| getAllKeys()  | 返回数据库中所有的 **键**                         |
| multiGet()    | 根据给定的 key 列表获取多个键值对                 |
| multiSet()    | 将多个键值对存储到系统中                          |
| multiRemove() | 根据多个 key 删除多个键值对                       |
| clear()       | 清空整个数据库系统                                |

**使用示例**

- 数据可能不存在，推荐在 `constructor()` 构造函数中先初始化一个默认值
- 推荐把读取数据的逻辑放到 `componentDidMount()` 中。

下面的代码演示了如何在存储数据组件 AsyncStorage 中存储和读取数据。

```react
import React, { Component } from 'react'
import { Text, View, Alert,TextInput, StyleSheet,TouchableHighlight } from 'react-native'
import AsyncStorage from '@react-native-community/async-storage';

export default class App extends Component {
   state = {
      'name': '你好 www.twle.cn',
      'inputText':'你好，简单教程',
   }

   async readName() {
        try {
          const value = await AsyncStorage.getItem('name')
          if(value !== null) {
              this.setState({ 'name': value })
          }
          Alert.alert("读取数据成功")
        } catch(e) {
          console.log(e);
          Alert.alert("读取数据失败!")
        }
   }

   setName = () => {
      AsyncStorage.setItem('name', this.state.inputText);
      Alert.alert("保存成功!")
   }
}
```

## 动画组件Animated

### 基本介绍

React Native 动画组件 `Animated` 是对 Android 和 iOS 动画的封装，以统一的接口的提供了为 React Native 提供了动画功能。

动画组件 Animated 提供的是一种值动画，也就是属性改变动画。也就是通过动态的不断的改变控件的某个属性的值来达到动画的目的。

当我们需要创建一个动画时，我们必须先初始化一个值。React Native Animated 组件提供了两种值类型

| 值类型             | 说明       |
| ------------------ | ---------- |
| Animated.Value()   | 单个值变化 |
| Animated.ValueXY() | 两个值变化 |

Animated 组件提供了三种类型来控制动画的缓动过程。

| 函数              | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| Animated.decay()  | 以摩擦力模型来控制动画的缓动，简单的说就是以初始速度开始并逐渐减速到停止 |
| Animated.spring() | 使用弹簧物理模型来控制动画的缓动                             |
| Animated.timing() | 使用时间来控制动画的缓动                                     |

默认情况下， React Native 只能对以下组件提供动画功能

```
Animated.Image
Animated.ScrollView
Animated.Text
Animated.View
Animated.FlatList
Animated.SectionList
```

如果其它组件也需要动画动能，那么需要使用 `createAnimatedComponent()` 函数来开启动画功能。总之，React Native 动画组件 Animated 有点复杂，详细功能可以直接参考文档。

### 创建过程

1.首先，一般给要创建动画的组件设置一个初始的样式，这个通过 `style` 属性来解决。

例如下面的代码为某个 `box` 组件设置了初始化的 **背景色**、**长** 和 **框**。

```react
const styles = StyleSheet.create({
    box: {
        backgroundColor: 'blue',
        width: 50,
        height: 100
    }
})
```

2.其次，在组件即将加载的生命周期函数 `componentWillMount()` 中初始化动画。现在已经是`UNSAFE_componentWillMount`

```react
componentWillMount = () => {
    this.animatedWidth = new Animated.Value(50)
    this.animatedHeight = new Animated.Value(100)
}
```

这是什么意思呢？ 比如我们要实现动画：**长从 50 变化到 100**。 那么动画初始化的时候就需要把值 `50` 传递给 `Animated.Value(50)`

3.使用动画类型来定义动画滑动的过程。这个一般包装在某个函数里面，`.start()` 方法用于开始一个动画。

```react
animatedBox = () => {
    Animated.timing(this.animatedWidth, {
        toValue: 200,
        duration: 1000
    }).start()
    Animated.timing(this.animatedHeight, {
        toValue: 500,
        duration: 500
    }).start()
}
```

`Animated.timing()` 用于定义随时间变化的函数。它的函数原型如下

```
static timing(value, config)
```

各个参数说明如下

| 参数   | 说明                                           |
| ------ | ---------------------------------------------- |
| value  | 要实现缓动的值。也就是我们第一步中初始化的动画 |
| config | 配置动画缓动的各种参数                         |

`config` 可配置的参数如下

| 参数            | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| toValue         | 用于设置动画结束的值                                         |
| duration        | 动画时长，单位为 **毫秒**，默认值是 `500`。                  |
| easing          | 时间缓动曲线函数。默认值为渐入渐出 `Easing.inOut` 别名 `Easing.ease` |
| delay           | 延迟多少毫秒才开始动画，默认值是 `0`                         |
| isInteraction   | 此动画是否在 InteractionManager 上创建 "交互句柄"。默认为 `true` |
| useNativeDriver | 是否使用原生动画来实现，默认值是 `false`。                   |

4.将初始化的动画和属性包装成一个样式。我们后面会用这个样式去覆盖动画组件的默认样式。

```react
const animatedStyle = { width: this.animatedWidth, height: this.animatedHeight }
```

5.这里用`TouchableOpacity`组件定义一个自定义动画组件，将它包裹一个`Animated.View`组件

```react
<TouchableOpacity style = {styles.container} onPress = {this.animatedBox}>
    <Animated.View style = {[styles.box, animatedStyle]}/>
</TouchableOpacity>
```

### 范例

```react
import React, { Component } from 'react'
import { View, StyleSheet, Animated, TouchableOpacity } from 'react-native'

class App extends Component {
   UNSAFE_componentWillMount = () => {
      this.animatedWidth = new Animated.Value(50)
      this.animatedHeight = new Animated.Value(100)
   }
   animatedBox = () => {
      Animated.timing(this.animatedWidth, {
         toValue: 200,
         duration: 1000
      }).start()
      Animated.timing(this.animatedHeight, {
         toValue: 500,
         duration: 500
      }).start()
   }
   render() {
      const animatedStyle = {
            width: this.animatedWidth, 
            height: this.animatedHeight }
      return (
         <TouchableOpacity 
            style = {styles.container} 
            onPress = {this.animatedBox}>
            <Animated.View style = {[styles.box, animatedStyle]}/>
         </TouchableOpacity>
      )
   }
}
export default App

const styles = StyleSheet.create({
   container: {
      justifyContent: 'center',
      alignItems: 'center'
   },
   box: {
      backgroundColor: 'blue',
      width: 50,
      height: 100
   }
})
```

## 开关组件Switch

### 使用语法

```react
<Switch
   onValueChange = {function(value){}}
   thumbColor    = {color}
   trackColor    = {{false:color,true:color}}
   onChange      = {function(event){}}
   value         = {true|false}/>
```

Switch 只有两个值 `true` 和 `false`，都是布尔类型。

- `true` 表示开关的 **开**状态。
- `false` 表示开关的 **关** 状态，默认值。

这两个值是固定的，我们不能变更。

如果我们要改变开关的初始状态，可以使用 `value` 属性来设置初始值，不过只能设置为 `true` 或 `false`。

> 注意：`value` 是必填属性，如果不设置，开关的状态看起来用于处于 **关** 状态。

Switch 还有两个事件回调函数 `onValueChange` 和 `onChange`。前者当开关的值发生改变时触发，参数是 **开关变更后的新值**。 后者当用户尝试改变开关状态时触发，参数是 **事件**。

开关的外观基本是固定的，我们不能改变，唯一能做的就是改变颜色。这里有三个颜色可以改变，一个是导轨的颜色，分为 **开** 状态下导轨的颜色和 **关** 状态下导轨的颜色。还有一个是 **滑块** 的颜色。

### 使用实例

可以改变状态的开关。

```react
import React, { Component } from 'react'
import { View, Text, Switch, StyleSheet } from 'react-native'

export default class App  extends Component {
    constructor() {
        super();
        this.label = {false:'关',true:'开'}
        this.state = {
            switch1Value: true,
        }
    }
    toggleSwitch = (value) => {
        this.setState({switch1Value: value})
    }
    render() {
        return (
            <View style = {styles.container}>
                <Switch 
                    onValueChange = {this.toggleSwitch} 
                    value= {this.state.switch1Value} 
                />
                <View><Text>Switch 当前的状态是：{this.label[this.state.switch1Value]}</Text></View>
            </View>
        )
    }
}
const styles = StyleSheet.create ({
    container: {
        flex: 1,
        alignItems: 'center',
        marginTop: 100
   }
})
```

## 状态栏组件StatusBar

**状态栏 StatusBar** 就是手机屏幕最顶上一个区域，包含 **运营商名称**、**网络情况**、**电池情况那一条**。

在 React Native 中我们可以定制 **状态栏** StatusBar 。当然了，说是定制，无非以下几点

1. 显示或隐藏状态栏。
2. 设置主题色：亮色系还是暗色系。
3. 设置显示或隐藏时是否启用动画。

暗色系

![React Native  状态栏  StatusBar](https://www.twle.cn/static/upload/img/2019/07/11/20190711080554_4.png)

亮色系

![React Native  状态栏  StatusBar](https://www.twle.cn/static/upload/img/2019/07/11/20190711080704_4.png)

**使用语法**

```react
<StatusBar 
    barStyle    = "dark-content|light-content" 
    hidden      = {true|false}
    animated    = {true|false}
/>
```

