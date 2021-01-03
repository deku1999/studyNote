# React Native 基础教程

React Native 是一个使用 [JavaScript](https://www.twle.cn/l/yufei/javascript/javascript-basic-index.html) 和 [React](https://www.twle.cn/l/yufei/react/react-basic-index.html) 来编写跨终端移动应用 ( [Android](https://www.twle.cn/l/yufei/android/android-basic-index.html) 或 iOS ） 的一种解决方案

这句话什么意思呢？

1. 即使你不懂如何使用 [Java](https://www.twle.cn/l/yufei/java/java-basic-index.html) 或 `Kotlin` 开发 Android ，或者不懂如何使用 [Swift](https://www.twle.cn/l/yufei/swift/swift-basic-index.html) 或 `Objective-C` 来开发 iPad 或 iPhone 应用也不打紧，因为 React Native 几乎不需要和它们打交道。
2. 这句话的另一个意思呢，就是，如果你想同时开发 Android 和 iOS 应用，但苦于资金或者其它杂七杂八的条件，找不齐 Android 或者 iOS 的开发人员，那么也不要紧，只要你的开发人员懂前端，懂 JavaScript 和 React 就够了，也能开发移动应用
3. 当然了，这句话还意味着，只要你招了一个会 React 的前端，那么你就拥有 **网页** 、**H5 页面** 、**移动 APP** 的全栈开发能力。是不是很惊喜....



### 微软收购了NPM，Node和JavaScript的生态都会更上一层

# React Native 简介

现在绝大多数 App 都采用混合模型开发，固定的，基础的组件使用 Java 或 Swift 等原生语言开发，而偏运营的组件和页面则采用 React Native 等 H5 形式开发。

这样做的好处就是原生开发者致力于创造基础组件，H5 致力于运营体验。

现在的 iOS 审核速度已经很快了，几乎一天就有结果，但是之前，可能要审核一周，半个月，甚至还会不通过，然后又要重新开始进入审核等待，这对于大部分需要频繁更新的 App 来说是不可接受的。

在这种情况下，React Native 出现了，它的首打功能就是 **热更新技术**。

**热更新** 技术可以稍微的绕过应用商店的审核而直接更新。这样就可以达到快速上线功能的目的。

对于 React Native，官方的介绍可能更能体现出它的诞生前因后果。

1. React Native 让我们可以只使用 `JavaScript` 语言就能构建出手机 APP。

2. React Native 采用 React 作为底层框架，如果你会 React 那么就很容易上手 React Native。

3. React Native 采用声明性组件中创建丰富的移动 UI。

   使用 React Native，你不是在构建移动 Web 应用程序，也不是在构建 HTML5 应用程序，更不是在构建混合应用程序。你是在构建了一个真正的移动应用程序，与使用 Objective-C 或 Java 构建的应用程序没啥区别的。

4. React Native 使用与原生 iOS 和 Android 应用相同的基本 UI 构建块。如果你熟悉原生 iOS 或 Android 开发，那么只需要使用 JavaScript 和 React 将这些构建块放在一起。

## React Native 特性

我经常傻傻的分不清 **特性** 和 **优点** 的区别。按照我们中文的意思来讲，**特性** 不就是 **优点** 么？

算了，不纠结了， React Native 有着以下的几个特性:

1. **React**

   底层采用 Facebook 开发的 React 技术。

   React 是一个视觉框架，使用 JavaScript 来构建网页和移动网页。

2. **原生**

   React Native 内置了大量的原生组件，这比 Web APP 有着更强大的性能。

3. **平台多样性**

   React Native 开发的 App 可以运行在 iOS 平台和 Android 平台。

## React Native 优点

现在市面上类 React Native 的框架很多，也有 H5，混合 APP 等等，还有那个淘宝开发的 Weex 好像。

即便如此，我们仍然选择 React Native，为什么？

1. **JavaScript**。

   完全采用 JavaScript 语言。而不是某些不伦不类的看似 JS 又不是 JS 的语言。

   这意味着在语言层面我们根本不需要重新学习。

2. **跨平台**。

   **Write Once, Run anywhere** 变得可能，尤其是 Android 和 iOS 两端。

3. **社区给力**。

   国人的项目差距就在这里，国内的很多项目，尤其是阿里系的，看起来就是某个人的绩效。一段时间后连维护都不了。

   React Native 有着强大的社区，有着众多的开发者提供了各种类型的组件。

## React Native 的局限性

当然了，React Native 也不是万能的，它也有着自己的缺点和局限性。

React Native 的缺点有两个：

1. 复杂的状态管理，页面切换。即使你会 React ，也会觉得它的页面切换有点绕。
2. **创建新的原生组件复杂**。如果你要创建一个之前从未出现过的原生组件，难度直线上升。你不仅需要懂得 Android 开发，还需要懂得 iOS 开发。