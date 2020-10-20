![img](https:////upload-images.jianshu.io/upload_images/18029972-5dce465c9571fa15.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/800/format/webp)

bg2019083104.jpg

#### 为什么会有Hooks？

介绍Hooks之前，首先要给大家说一下React的组件创建方式，一种是***类组件\***，一种是***纯函数组件\***，并且React团队希望，组件不要变成复杂的容器，最好只是数据流的管道。开发者根据需要，组合管道即可。也就是说**组件的最佳写法应该是函数，而不是类。**。
 但是我们知道，在以往开发中*类组件*和*纯函数组件*的区别是很大的，纯函数组件有着类组件不具备的多种特点，简单列举几条

- 纯函数组件**没有状态**
- 纯函数组件**没有生命周期**
- 纯函数组件没有`this`
- 只能是纯函数

这就注定，我们所推崇的函数组件，只能做UI展示的功能，涉及到状态的管理与切换，我们不得不用类组件或者redux，但我们知道类组件的也是有缺点的，比如，遇到简单的页面，你的代码会显得很重，并且每创建一个类组件，都要去继承一个React实例，至于Redux,更不用多说，很久之前Redux的作者就说过，“能用React解决的问题就不用Redux”,等等一系列的话。关于React类组件redux的作者又有话说

> - 大型组件很难拆分和重构，也很难测试。
> - 业务逻辑分散在组件的各个方法之中，导致重复逻辑或关联逻辑。
> - 组件类引入了复杂的编程模式，比如 render props 和高阶组件。

下面我们用类组件做一个简单的计数器



```javascript
import React from 'react'
class AddCount extends React.PureComponent {
  constructor(props){
    super(props)
    this.state={
      count: 0
    }
  }
  addcount = () => {
    let newCount = this.state.count
    this.setState({
      count: newCount +=1
  })
  }
  render(){
    return (
      <>
        <p>{this.state.count}</p>
        <button onClick={this.addcount}>count++</button>
      </>
    )
  }
}
export default AddCount
```

可以看出来，上面的代码确实很重。
 为了解决这种，*类组件功能齐全却很重，纯函数很轻便却有上文几点重大限制*，React团队设计了**React Hooks**
 ***React Hooks就是加强版的函数组件，我们可以完全不使用 `class`，就能写出一个全功能的组件\***

#### 什么是Hooks?

'Hooks'的单词意思为“钩子”。
 **React Hooks 的意思是，组件尽量写成纯函数，如果需要外部功能和副作用，就用钩子把外部代码"钩"进来。**而React Hooks 就是我们所说的“钩子”。
 那么Hooks要怎么用呢？“你需要写什么功能，就用什么钩子”。对于常见的功能，React为我们提供了一些常用的钩子，当然有特殊需要，我们也可以写自己的钩子。下面是React为我们提供的默认的四种最常用钩子

> - useState()
> - userContext()
> - userReducer()
> - useEffect()

不同的钩子为函数引入不同的外部功能，我们发现上面四种钩子都带有`use`前缀，React约定，钩子*一律使用* `use`前缀命名。所以，你自己定义的钩子都要命名为useXXX。

#### React Hooks的用法

下面介绍四种默认钩子的用法

##### 一、userState():状态钩子

我们知道，纯函数组件没有状态，`useState()`用于为函数组件引入状态。
 下面我们使用Hooks重写上面的计数器。



```javascript
import React, {useState} from 'react'
const AddCount = () => {
  const [ count, setCount ] = useState(0)
  const addcount = () => {
    let newCount = count
    setCount(newCount+=1)
  } 
  return (
    <>
      <p>{count}</p>
      <button onClick={addcount}>count++</button>
    </>
  )
}
export default AddCount 
```

通过上面的代码，我们实现了一个功能完全一样的计数器，代码看起来更加的轻便简洁，没有了继承，没有了渲染逻辑，没有了生命周期等。这就是hooks存在的意义。
 在`useState()`中，它接受状态的初始值作为参数，即上例中计数的初始值，它返回一个数组，其中数组第一项为一个变量，指向状态的当前值。类似`this.state`,第二项是一个函数，用来更新状态,类似`setState`。该函数的命名，我们约定为`set`前缀加状态的变量名。

##### 二、useContext():共享状态钩子

该钩子的作用是，在组件之间共享状态。关于Context这里不再赘述，其作用就是可以做状态的分发，在React16.X以后支持，避免了react逐层通过Props传递数据。
 下面是一个例子，现在假设有A组件和B组件需要共享一个状态。



```javascript
import React,{ useContext } from 'react'
const Ceshi = () => {
  const AppContext = React.createContext({})
  const A =() => {
    const { name } = useContext(AppContext)
    return (
        <p>我是A组件的名字{name}<span>我是A的子组件{name}</span></p>
    )
}
const B =() => {
  const { name } = useContext(AppContext)
  return (
      <p>我是B组件的名字{name}</p>
  )
}
  return (
    <AppContext.Provider value={{name: 'hook测试'}}>
    <A/>
    <B/>
    </AppContext.Provider>
  )
}
export default Ceshi 
```

我们看下页面



![img](https:////upload-images.jianshu.io/upload_images/18029972-330eff02bd728b32.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/682/format/webp)

捕获.PNG



可以看到，我们可以通过hooks做状态的共享。

##### 三、useReducer():Action钩子

我们知道，在使用React的过程中，如遇到状态管理，我们一般会用到Redux,而React本身是不提供状态管理的。而`useReducer()`为我们提供了状态管理。首先，关于redux我们都知道，其原理是我们通过用户在页面中发起action,从而通过reducer方法来改变state,从而实现页面和状态的通信。而Reducer的形式是`(state, action) => newstate`。类似，我们的`useReducer()`是这样的



```cpp
const [state, dispatch] = useReducer(reducer, initialState)
```

它接受reducer函数和状态的初始值作为参数，返回一个数组，其中第一项为*当前的*状态值，第二项为发送action的dispatch函数。下面我们依然用来实现一个计数器。
 和redux一样，我们是需要通过页面组件发起action来调用reducer方法，从而改变状态，达到改变页面UI的这样一个过程。所以我们会先写一个Reducer函数，然后通过useReducer()返回给我们的state和dispatch来驱动这个数据流。思路就是这样，下面我们上代码



```javascript
import React,{useReducer} from 'react'

const AddCount = () => {
const reducer = (state, action) =>  {
 if(action.type === ''add){
  return {
  ...state,
  count: state.count +1,
  }
 }else {
   return state
  }
 }
const addcount = () => { 
  dispatch({
    type: 'add'
  })
 }
const [state, dispatch] = useReducer(reducer, {count: 0})
return (
<>
<p>{state.count}</p>
<button onClick={addcount}>count++</button>
</>
)
}
export default AddCount
```

通过代码我们看到了，我们使用`useReducer()`代替了Redux的功能，但`useReducer`无法为我们提供中间件等功能，加入你有这些需求，还是需要用到redux。

##### 四、useEffect():副作用钩子

熟悉redux-saga的同学一定对`Effect`不陌生,它可以用来更好的处理副作用，如异步请求等，我们的`useEffect()`也是为函数组件提供了处理副作用的钩子。依然我们会把请求房子`componentDidMount`里面，在函数组件中我们可以使用`useEffect()`。其具体用法如下



```javascript
useEffect(() => {},[array])
```

`useEffect()`接受两个参数，第一个参数是你要进行的异步操作，第二个参数是一个数组，用来给出Effect的依赖项。只要这个数组发生变化，`useEffect()`就会执行。当第二项省略不填时，`useEffect()`会在每次组件渲染时执行。这一点类似于类组件的`componentDidMount`。下面我们通过代码模拟一个异步加载数据。



```javascrpit
import React, { useState, useEffect } from 'react'
const AsyncPage = () => {
const [loading, setLoading] = useState(true)
  useEffect(() => {
    setTimeout(()=> {
      setLoading(false)
    },5000)
  })
return (
loading ? <p>Loading...</p>: <p>异步请求完成</p>
)
}

export default AsyncPage 
```

上面的代码实现了一个异步加载，下面我们再做一个`useEffect()`依赖第二项数组变化的例子。



```javascript
import React, { useState, useEffect } from 'react'

const AsyncPage = ({name}) => {
const [loading, setLoading] = useState(true)
const [person, setPerson] = useState({})

  useEffect(() => {
    setLoading(true)
    setTimeout(()=> {
      setLoading(false)
      setPerson({name})
    },2000)
  },[name])
  return (
    <>
      {loading?<p>Loading...</p>:<p>{person.name}</p>}
    </>
  )
}

const PersonPage = () =>{
  const [state, setState] = useState('')
  const changeName = (name) => {
    setState(name)
  }
  return (
    <>
      <AsyncPage name={state}/>
      <button onClick={() => {changeName('名字1')}}>名字1</button>
      <button onClick={() => {changeName('名字2')}}>名字2</button>
    </>
  )
}

export default PersonPage 
```

上面代码中，通过改变传给`AsyncPage`的props,从而调用`useEffect()`。

##### 五、创建自己的Hooks

以上我们介绍了四种最常用的react提供给我们的默认React Hooks,有时候我们需要创建我们自己想要的Hooks,来满足更便捷的开发，在小编看来，无非就是根据业务场景对以上四种Hooks进行组装，从而得到满足自己需求的钩子。
 比如，我们要将我们上面的代码功能封装成Hooks,代码如下



```dart
import React, { useState, useEffect } from 'react'

const usePerson = (name) => {
const [loading, setLoading] = useState(true)
const [person, setPerson] = useState({})

  useEffect(() => {
    setLoading(true)
    setTimeout(()=> {
      setLoading(false)
      setPerson({name})
    },2000)
  },[name])
  return [loading,person]
}

const AsyncPage = ({name}) => {
  const [loading, person] = usePerson(name)
    return (
      <>
        {loading?<p>Loading...</p>:<p>{person.name}</p>}
      </>
    )
  }

const PersonPage = () =>{
  const [state, setState]=useState('')
  const changeName = (name) => {
    setState(name)
  }
  return (
    <>
      <AsyncPage name={state}/>
      <button onClick={() => {changeName('名字1')}}>名字1</button>
      <button onClick={() => {changeName('名字2')}}>名字2</button>
    </>
  )
}

export default PersonPage 
```

上面代码中，我们将之前的例子封装成了自己的Hooks,便于共享。其中，我们定义`usePerson()`为我们的自定义Hooks,它接受一个字符串，返回一个数组，数组中包括两个数据的状态，之后我们在使用`usePerson()`时，会根据我们传入的参数不同而返回不同的状态，然后很简便的应用于我们的页面中。

至此，文章关于React Hooks的讲解结束，它为我们带来了React翻天覆地的变化，也让我们感受到了React的未来，不过，假如你不会Hooks也是没有关系的。根据官方文档的话来说

> - **完全可选的。** 你无需重写任何已有代码就可以在一些组件中尝试 Hook。但是如果你不想，你不必现在就去学习或使用 Hook。
> - **100% 向后兼容的。** Hook 不包含任何破坏性改动。
> - **现在可用。** Hook 已发布于 v16.8.0。
> - **没有计划从 React 中移除 class。**
> - **Hook 不会影响你对 React 概念的理解。** 恰恰相反，Hook 为已知的 React 概念提供了更直接的 API：props， state，context，refs 以及生命周期。



