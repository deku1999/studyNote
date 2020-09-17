### nextTick原理

```javascript
<template>
  <div>
    <div>{{number}}</div>
    <div @click="handleClick">click</div>
  </div>
</template>
export default {
    data () {
        return {
            number: 0
        };
    },
    methods: {
        handleClick () {
            for(let i = 0; i < 1000; i++) {
                this.number++;
            }
        }
    }
}
```

当我们按下 click 按钮的时候，number 会被循环增加1000次。

那么按照之前的理解，每次 number 被 +1 的时候，都会触发 number 的 setter 方法，从而根据上面的流程一直跑下来最后修改真实 DOM。那么在这个过程中，DOM 会被更新 1000 次！那怎么办？

Vue.js中的 nextTick 函数，会传入一个 cb ，这个 cb 会被存储到一个队列中，在下一个 tick 时触发队列中的所有 cb 事件。

Vue.js 肯定不会以如此低效的方法来处理。Vue.js在默认情况下，每次触发某个数据的 setter 方法后，对应的 Watcher 对象其实会被 push 进一个队列 queue 中，在下一个 tick 的时候将这个队列 queue 全部拿出来 run（ Watcher 对象的一个方法，用来触发 patch 操作） 一遍。

因为目前浏览器平台并没有实现 nextTick 方法，所以 Vue.js 源码中分别用 Promise、setTimeout、setImmediate 等方式在 microtask（或是task）中创建一个事件，目的是在当前调用栈执行完毕以后（不一定立即）才会去执行这个事件。

```javascript
export function nextTick (cb?: Function, ctx?: Object) {
  let _resolve
  callbacks.push(() => {
    if (cb) {
      try {
        cb.call(ctx)
      } catch (e) {
        handleError(e, ctx, 'nextTick')
      }
    } else if (_resolve) {
      _resolve(ctx)
    }
  })
  if (!pending) {
    pending = true
    timerFunc()
  }
  // $flow-disable-line
  if (!cb && typeof Promise !== 'undefined') {
    return new Promise(resolve => {
      _resolve = resolve
    })
  }
}
```

首先定义一个 callbacks 数组用来存储 nextTick，在下一个 tick 处理这些回调函数之前，所有的 cb 都会被存在这个 callbacks 数组中。pending 是一个标记位，代表一个等待的状态。 这里用setTimeout做描述（真实源码里更复杂）： setTimeout 会在 task 中创建一个事件 flushCallbacks ，flushCallbacks 则会在执行时将 callbacks 中的所有 cb 依次执行。

## watcher

上面例子中，当我们将 number 增加 1000 次时，先将对应的 Watcher 对象给 push 进一个队列 queue 中去，等下一个 tick 的时候再去执行，这样做是对的。但是有没有发现，另一个问题出现了？

因为 number 执行 ++ 操作以后对应的 Watcher 对象都是同一个，我们并不需要在下一个 tick 的时候执行 1000 个同样的 Watcher 对象去修改界面，而是只需要执行一个 Watcher 对象，使其将界面上的 0 变成 1000 即可。

那么，我们就需要执行一个过滤的操作，同一个的 Watcher 在同一个 tick 的时候应该只被执行一次，也就是说队列 queue 中不应该出现重复的 Watcher 对象。

那么我们可以用 id 来标记每一个 Watcher 对象，让他们看起来不太一样。

我们再回过头聊一下第一个例子， number 会被不停地进行 ++ 操作，不断地触发它对应的 Dep 中的 Watcher 对象的 update 方法。然后最终 queue 中因为对相同 id 的 Watcher 对象进行了筛选，从而 queue 中实际上只会存在一个 number 对应的 Watcher 对象。在下一个 tick 的时候（此时 number 已经变成了 1000），触发 Watcher 对象的 run 方法来更新视图，将视图上的 number 从 0 直接变成 1000。