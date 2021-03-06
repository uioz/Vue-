# 创建一个 Vue 实例

> 每个 Vue 应用都是通过用 Vue 函数创建一个新的 Vue 实例开始的：
```javascript
var vm = new Vue({
  // 选项
})
```
> 当创建一个 Vue 实例时，你可以传入一个选项对象。这篇教程主要描述的就是如何使用这些选项来创建你想要的行为。

> 作为参考，你也可以在[API文档](https://cn.vuejs.org/v2/api/#%E9%80%89%E9%A1%B9-%E6%95%B0%E6%8D%AE)中浏览完整的选项列表。

## 数据与方法

> 当一个 Vue 实例被创建时，它向 Vue 的**响应式系统**中加入了其 data 对象中能找到的所有的属性。当这些属性的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。

```javascript
// 我们的数据对象
var data = { a: 1 }

// 该对象被加入到一个 Vue 实例中
var vm = new Vue({
  data: data
})

// 获得这个实例上的属性
// 返回源数据中对应的字段
vm.a == data.a // => true

// 设置属性也会影响到原始数据
vm.a = 2
data.a // => 2

// ……反之亦然
data.a = 3
vm.a // => 3
```

> 当这些数据改变时，视图会进行重渲染`(1)`。值得注意的是只有当实例被创建时 data 中存在的属性才是响应式的。也就是说如果你添加一个新的属性，比如：

 - `(1)` 渲染的概念简单理解就是将数据处理后输出的过程

```javascript
vm.b = 'hi'
```

> 那么对 b 的改动将不会触发任何视图的更新。如果你知道你会在晚些时候需要一个属性，但是一开始它为空或不存在，那么你仅需要设置一些初始值。比如：

```javascript
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```

> 除了数据属性，Vue 实例还暴露了一些有用的实例属性与方法。它们都有前缀 $，以便与用户定义的属性区分开来。例如：
```javascript
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```

> 以后你可以在 [API 参考](https://cn.vuejs.org/v2/api/#%E5%AE%9E%E4%BE%8B%E5%B1%9E%E6%80%A7)中查阅到完整的实例属性和方法的列表。

## 实例生命周期钩子`(1)`

 - `(1)` 所谓的钩子指的就是回调函数,简单理解就是一个函数.

> 每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做**生命周期钩子**的函数，这给了用户在不同阶段添加自己的代码的机会。

> 比如 created 钩子可以用来在一个实例被创建之后执行代码：

```javascript
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 Vue 实例
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```

> 下面展示了实例的生命周期。你不需要立马弄明白所有的东西，不过随着你的不断学习和使用，它的参考价值会越来越高。

常见生命周期钩子函数一览:
 - beforeCreate 在实例初始化之后，数据观测 `(data observer)` 和 `event/watcher` 事件配置之前被调用。
 - create 在实例创建完成后被立即调用,数据观测和事件配置完成.
 - beforeMount 在挂载(到HTML中)开始之前被调用：相关的`render`(渲染)函数首次被调用。
 - mounted HTML部分替换完成后触发.
 - beforeUpdate 数据更新时调用，发生在虚拟 DOM 打补丁之前。
 - updated 由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。
 - beforeDestroy 实例销毁之前调用。在这一步，实例仍然完全可用。
 - destroyed Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。
 - 