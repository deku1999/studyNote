# 写 React / Vue 项目时为什么要在列表组件中写 key，其作用是什么

> 当 Vue 正在更新使用 v-for 渲染的元素列表时，它默认使用“就地更新”的策略。如果数据项的顺序被改变，Vue 将不会移动 DOM。 元素来匹配数据项的顺序，而是就地更新每个元素，并且确保它们在每个索引位置正确渲染。这个类似 Vue 1.x 的 track-by="$index"。

这个默认的模式是高效的，但是只适用于不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出。

- 避免对节点「就地复用」

需要修改的节点位置没有改变，是内容更新了，这虽然提高了复用性能，但是往往在复杂的表单会导致状态出现错位。也不会产生过度效果

- key相当于每个vnode 的唯一id，我们可以依靠`key`，更快更精确的知道oldVnode中对应的vnode节点。

带key就不会使用就定复用了，在sameNode函数`a.key===b.key`对比中可以避免就地复用的情况。

我们可以利用key的唯一性来更快获取到对应节点，比遍历更快。

## 什么是diff算法？

要渲染真实的DOM的开销很大，因为改变真实dom，会当值整个dom树的重绘和回流。我们需要渲染真实dom的时候往往会把生成一个`虚拟节点` virtual DOM，当virtual dom某个节点的数据改变后生成一个新的Vnode，然后将Vnode和oldVnode对比，当然有不同的地方教就直接修改在真实DOM上，然后是oldVnode=Vnode

- 真实DOM

```
<div>
    <p>123</p>
</div>
```

- virtual DOM （虚拟DOM）

```
var Vnode={
    tag:'div',
    children:[{
        tag:'p',text:'123'
    }]
    
}
```

### diff的比较方式

在同层级进行，不会跨层级比较

oldDOM

```
<div>
    <p>123</p>
</div>
```

newDOM

```
<div>
    <span>2222</span>
</div>
```

![clipboard.png](https://segmentfault.com/img/bVbvtvU?w=404&h=225)

- 先对比DIV，发现两个DIV不对等
- 查看DIV的子元素P、SPAN，发现不对等
- 查看P、SPAN没有子元素，则移除P，增加SPAN

![clipboard.png](https://segmentfault.com/img/bVbvtwL?w=920&h=680)

现在我们来看看在进行替换

#### 对比节点函数

```
function patch(oldVnode, vnode) {
  // 对比是否相等
  if (sameVnode(oldVnode, vnode)) {
    patchVnode(oldVnode, vnode)
  } else {
    const oEl = oldVnode.el // 当前oldVnode对应的真实元素节点
    let parentEle = api.parentNode(oEl) // 父元素

    createELe(vnode) // 为vnode生成新的元素

    if (parentEle !== null) {
      api.insertBefore(parentEle, vnode.el, api.nextSibling(oEl)) // 将新的元素添加到父元素中
      api.removeChild(parentEle, oldVnode.el) // 移除以前的元素
    }
  }

  return vnode
}
```

#### 判断两者是否相同函数

```
function sameVnode(a, b) {
  return (
    a.key === b.key && // 对比key
    a.tag === b.tag && // 对比标签名
    a.isComment === b.isComment && // 是否为注释节点
    isDef(a.data) === isDef(b.data) && // 是否定义了data，或者其他属性
    sameInputType(a, b) //判断是当<Input>时 是否type相同
  )
}
```