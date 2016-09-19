---
title: Vuex 初步印象
date: 2016-09-09 23:13:36
tags: [javascript, vue, vuex, 前端开发]
---
刚刚把 Vue.js 中用于状态管理架构的 Vuex 的文档啃了一遍。

Vue 组件化之后各个组件都拥有自己的状态，这让状态在组件中的共享变得十分困难，通常用事件的方式把状态推送到其他组件中去，当组件树变得非常大的时候，事件流会变得非常复杂，很难维护，会 Out of Control。

Vuex 就是为此而诞生的，用 Vuex 来维护一个应用级的状态，它不属于任何特定的组件，组件之间可以共享这个状态。

## Vuex 数据流

首先来看一下 Vuex 数据流的一张图，这张图画的十分清晰。

![](http://vuex.vuejs.org/zh-cn/vuex.png)

从组件开始看，当用户对 Vue 的组件进行操作的的时候，组件会调用 *Actions*，*Actions* 顾名思义是一系列的动作，由它来决定对 *Store* 做什么操作，但是它不能直接操作 *State*，也就是上面所说的状态。按照我的理解，*Action* 作为一个通知者，把所要做的操作分发到 *Mutation*，*Mutation* 会去修改 *State*，当 *State* 发生变化会由 *Getter* 来从 *State* 里获取 Vue 组件中所需要的数据，从而更新界面。

看上去很麻烦的样子啊，但是这一切都是建立在我们要构建的是一个大型应用的前提之下的。目的是提高可维护性，降低调试与长期维护的难度。

接下来就来看一下 Vuex 中几个核心的内容：

## 核心内容

### State

*State* 对象包含了全部的应用级状态，是一个唯一数据源。一个应用只有一个 `store` 实例。

不过，它是可以分模块的，就像这样：

``` javascript
import module1 from './modules/module1'
import module2 from './modules/module2'

export default new Vuex.Store({
  modules: {
    module1,
    module2
  }
});
```

这里面 `module1` 和 `module2` 各自有的 *State*，子模块的初始状态是作为底层 `state` 的子树的。在所有子模块上定义的 *Mutation* 都只能改变当前相关子模块上的 `state` 子树。

### Getters

*Getters* 是用来获取 `state` 中的数据的。好像就这么简单……

为什么要有它？共享！如果多个组件都会使用同一个 `getter`，是很高效的，因为它会被缓存，而且当它所依赖的状态发生改变的时候，相应的 `getter` 只会被计算一遍。

### Mutation

它就是一个事件系统，按照惯例 `mutation` 用全大写命名。它不能直接被调用，而需要用 `dispatch` 的方式来触发：

``` javascript
// ...
  mutations: {
    MUTATION (state, para) {
      // ...
    }
  }
}
store.dispatch('MUTATION')
```
所有的 *Mutation* 的第一个参数永远为所属 `store` 的整个 `state` 对象。

注意：*Mutation* 必须是同步函数！如果在它里面进行异步操作，对于状态的控制将会空前混乱。

### Actions

那么想要进行异步操作的话，应该要把异步的行为放在 *Actions* 里面。*actions* 就是用于分发 *Mutation* 的。

在组件中，需要将 *Actions* 暴露到组件方法中去，让组件觉得个它自己本身的 *methods* 没有差别。

## 一些规则

1. 应用 `state` 存在于单个对象中；
2. 只有 `mutation handlers` 可以改变 `state`；
3. *Mutations* 必须是同步的，它们做的应该仅仅是改变 `state`；
4. 所有类似数据获取的异步操作细节都应封装在 `actions` 里面。
5. 组件通过 `getters` 从 `store` 中获取 `state`，并通过调用 `actions` 来改变 `state`。

Vuex 的主要内容基本上就这些，关于细节方面不多说了。