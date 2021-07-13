# Vue
## 1.vue的数据绑定机制是如何实现的
答：
通过Object.defineProperty在属性get中收集依赖和set中派发更新实现的数据响应式绑定

依赖收集
- 渲染DOM之前调用渲染函数，渲染函数中解析字符串模版时会访问data对象中的属性，此时触发属性get方法进行依赖收集，将对应key和value以及更新DOM函数的Wacher实例保存在Dep的订阅列表中。

派发更新
- 属性值发生变化时，将会通过Dep的notify方法调用所有该属性值的Wacher实例中的update方法触发对应DOM视图的更新

加分项：

[开始学习](https://juejin.cn/post/6844903869730799629)
## 2.vue next tick实现原理
答：分别判断是否能用Promise/MutationObserver/setImmediate/setTimeout，如果能用就调用对应的方法执行微任务或宏任务，参数是缓冲在同一事件循环中发生的所有数据变更的函数。微任务中遇到微任务会加到栈尾执行，因此会在下个事件循环前执行更新
- 机制：vue会把属性所有修改操作收集起来，在下个事件循环中更新
加分项：事件循环

[开始学习](https://www.cnblogs.com/leiting/p/13174545.html)
## 3.vue的computed和watch的区别
答：

computed可以设置成带get/set属性的对象

监听对象不同:
- computed:依赖值改变才会改变
- watch:属性值改变会触发监听函数传入改变前和后的值

有无缓存
- computed:有，依赖值不变则使用缓存的结果
- watch:无缓存

适用场景
- computed:依赖其他值计算出另一个值并需要实时响应，记录缓存
- watch:根据观察的属性变化，来进行复杂业务逻辑

加分项：

[开始学习](https://www.cnblogs.com/tugenhua0707/p/11760466.html)
## 4.说下vue的keep alive
答：
- 是什么：
是一个抽象组件，他自身不渲染DOM，不显示在父组件链中，用于保留组件状态或避免重新渲染。

执行过程：

- 挂载/卸载不同: 普通组件实例化会执行$mount来创建组件vnode后挂载真实dom，销毁时会递归调用子组件的destroy移除真实DOM，而keep-alive是调用render在首次渲染时候创建第一个子组件vnode，并将其DOM挂到vnode的elm属性，并且缓存vnode，将vnode.data.keepAlive标记为true，再次渲染时会取出缓存的vnode替换组件实例，卸载时根据内部状态keepAlive是否为true，来决定是递归调用destroy，还是递归将子组件状态标记为deactivated表示停用组件。

- include: 要缓存的组件数组，render第一次通过include/exclude决定是否用缓存，第二次查看实例上根据vnode.key在cache中查找是否命中
- exclude:  不缓存的组件数组
- max: 缓存的数组最大长度

就主要的几个阶段进行分析
- createComponent：生成DOM，保存vnode.elm
- render
- 拿vnode：拿第一个子组件vnode
- 判断缓存：include/exclude判断是否缓存；$ref.cache、$ref.key判断和存取缓存
- max优化：$ref.keys保存最新缓存的key到最后端，超过max时删除最前端的key的子组件
- vnode.data.keepAlive：重新createComponent DOM时启用之前停用的组件，要销毁组件前做判断决定是销毁还是停用

加分项：缓存vnode、DOM，标记状态，LRU

[开始学习](https://juejin.cn/post/6844903950886371342)
