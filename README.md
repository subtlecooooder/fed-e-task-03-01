# fed-e-task-03-01
## 简单题
### 1. 当我们点击按钮的时候动态给 data 增加的成员是否是响应式数据，如果不是的话，如何把新增成员设置成响应式数据，它的内部原理是什么。
```js
let vm = new Vue({
 el: '#el'
 data: {
  o: 'object',
  dog: {}
 },
 method: {
  clickHandler () {
   // 该 name 属性是否是响应式的
   this.dog.name = 'Trump'
  }
 }
})
```
- 解答：不是响应式数据，可以通过 `Vue.set( target, key, value )`或者`this.$set( target, key, value )` 把新增成员设置成响应式数据。
- 内部原理：通过查看源码可知
  - 如果是在开发环境，且target未定义（为null、undefined）或target为基础数据类型（string、boolean、number、symbol）时，抛出告警；
  - 如果target为数组且key为有效的数组key时，将数组的长度设置为target.length和key中的最大的那一个，然后调用数组的splice方法（vue中重写的splice方法）添加元素；
  - 如果属性key存在于target对象中且key不是Object.prototype上的属性时，表明这是在修改target对象属性key的值（不管target对象是否是响应式的，只要key存在于target对象中，就执行这一步逻辑），此时就直接将value直接赋值给target[key]；
  - 判断target，当target为vue实例或根数据data对象时，在开发环境下抛错；
  - 当一个数据为响应式时，vue会给该数据添加一个__ob__属性，因此可以通过判断target对象是否存在__ob__属性来判断target是否是响应式数据，当target是非响应式数据时，我们就按照普通对象添加属性的方式来处理；当target对象是响应式数据时，我们将target的属性key也设置为响应式并手动触发通知其属性值的更新；

### 2. 请简述 Diff 算法的执行过程
- patch(container, vnode) ，首次渲染，将 container 转为 vnode，并对比新旧 VNode 是否相同节点然后更新DOM
- patch(vnode, newVnode) ，数据改变二次渲染，对比新旧 VNode 是否相同节点然后更新DOM
- createElm(vnode, insertedVnodeQueue)，先执行用户的 init 钩子函数，然后把 vnode 转换成真实 DOM（此时没有渲染到页面），最后返回新创建的 DOM
- updateChildren(elm, oldCh, ch, insertedVnodeQueue), 如果 VNode 有子节点，并且与旧VNode子节点不相同则执行 updateChildren()，比较子节点的差异并更新到DOM

## 编程题
### 1. 模拟 VueRouter 的 hash 模式的实现，实现思路和 History 模式类似，把 URL 中的 # 后面的内容作为路由的地址，可以通过 hashchange 事件监听路由地址的变化。

- 代码路径：code - vue-router-hash - index.js

### 2. 在模拟 Vue.js 响应式源码的基础上实现 v-html 指令，以及 v-on 指令。

- 代码路径：code - vue-html-on - compiler.js

### 3. 参考 Snabbdom 提供的电影列表的示例，利用Snabbdom 实现类似的效果，如图：
![效果图](https://s0.lgstatic.com/i/image/M00/26/F2/Ciqc1F7zUZ-AWP5NAAN0Z_t_hDY449.png)

- 代码路径：code - snabbdom-movie