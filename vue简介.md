传统的java web开发是使用servlet + jsp的模式，出现了后端模板引擎，比如Freemarker等。

前端模板引擎(underscore.js,)

前端开发模式MVC->MVP->MVVM

MVC:以这为代表的如Backbone.js，Angular.js等。双向数据绑定。
MVP:Backbone.js
MVVM:Vue.js, React

### 基本概念
组件：组件是可复用的Vue实例。可以简单理解一下html里面的元素都可以说是documentElement的组件，组件是组成视图的元素。只不过呢，Vue里面的组件指的是用户自定义的。比如我们现在项目中用到的iview中的各种组件，Form,Table,Button等。

指令：用于组件上，协助组件完成某项功能。比如内置的指令，v-bind, v-model等。我们项目中自定义了一个指令v-auth，用来处理权限。

事件：除了一些原生事件外，还可以自定义事件。这种就是订阅/发布模式。一般用于父子组件传值。

混入：分发Vue组件可复用功能的方式。比如几个组件有共同的data，method等，可以提取出来，在用到的地方引入即可，在我们的项目中用到了两处，一个是commonMix, 另一个是downloadMix。

渲染函数：我们在单文件组件里都是用template创建自己的html，但是存在一些情况需要用js创建，比如我们用的Table组件，有些单元格的内容不是简单的字符串，是按钮。

虚拟DOM：实际上我们在单文件组件里面写的都会被编译成渲染函数，createElement(...),通过一种方式创建Node， 这个node包含Vue页面上需要渲染成什么样子，以及有些什么子node,这个的node叫作虚拟node(VNode)。这样一个一个node串起来成了Vnode树，当有数据变化时，会先比对Vnode树，判断是否需要更新视图。当然如何判断这又是一个话题，只能简单来说Vue的判断算法很精妙，所以使Vue的性能提高了不少。


### 响应式
首先理解什么是响应式。响应式即视图绑定的数据发生了变化后，视图会自动进行更新。其中视图绑定的数据即为Modal,视图View，改变这些数据的指令等即是ViweState，合起来就是MVVM的模式。

Vue的响应式实现借助了Object.defineProperty，把data全部转成getter/setter，这个就类似于java dto的getter，setter，使得数据变化时，Vue可以追踪到，并通知相应的watcher,重新计算，并更新关联的组件。

### 生命周期
这里请参考vue官网上的图。这里只需要有个大概的轮廓，后面用到的时候再反复查看。






