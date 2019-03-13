vue-router是Vue.js官方指定的路由管理器，使用它开发SPA非常快捷。
vue-router有两种模式，一种是HTML历史模式，另一种是hash模式。hash模式是默认的模式，我们系统中采用的是hash模式

### hash模式
http://localhost:8090/#/manage/datapush

#后面的部分就是路由配置。变化的就是这部分内容当路由跳转的时候。

### HTML5 History模式
这种模式主要是用到了history相关的一些API，比如history.pushState,这种模式页面的表现和hash模式是一样的，只不过URL看着比较舒服，和SSR一样。只不过这种模式需要后台配合。就是说如何不到任何资源则返回index.html，否则就会404

```
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```


```
 <error-page> <!--当系统出现404错误，跳转到页面index.html-->  
        <error-code>404</error-code>  
        <location>/index.html</location>  
    </error-page>  
```
也就是说这种时候任何没有指定的请求都会返回index.html让前端路由处理。


### 路由的定义
1.定义路由组件

项目中我们使用的单文件组件，即.vue文件。这里一个路由组件对应一个view目录下的.vue文件。

2.定义路由

这里是路由的一些配置项，比如路由的路径，映射的组件等。

```
const routers = [
    {
        path: '/',
        redirect: '/login'
    },
    {
        path: '/login',
        component: (resolve) => require(['./views/login.vue'], resolve)
    },
    {
        name: 'index',
        path: '/manage',
        component: (resolve) => require(['./views/index.vue'], resolve),
        children:[
            {
                name: 'company',
                path:'company',
                component: (resolve) => require(['./views/basic/company.vue'], resolve),
                meta: {authCode: 'M_COMPANY',parentMenu: 'basic'}
            },
            ......
```
这里的component映射的组件是用promise方式加载，webpack在打包的时候看到这种模式会把相应的路由组件进行打包，并在跳转到对应的路由加载，以达到动态按需加载的目的。官方称之为懒加载。项目中的路由配置在src\router.js。

3.创建router实例

```
const router = new VueRouter({routes})
```

4.挂载根实例
在实例化vue实例时，注入路由。

```
const app = new Vue({
  router
}).$mount('#app')
```
这样在文件的

```
 <router-view></router-view>
```
这个位置加载路由组件里面的内容， 在我们项目里面是在index.vue中。这里有几个路由标签命令

<router-view>

<router-link>:渲染成a，用来做导航



### 路由的匹配

这里路由的匹配类似restful风格,匹配的原则是先定义的优先级最高，因此可以在最后配置一个捕获所有未匹配的路由，比如404。

1.路由参数，通过下面的例子可以理解一下。

匹配某个用户的某篇博文

模式 | 匹配路径 | 路由参数
---|---|--
/user/:username | /user/evan | { username: 'evan' }
/user/:username/post/:post_id	| /user/evan/post/123 | { username: 'evan', post_id: '123' }

路由参数可以通过当前路由获取，比如在我们的路由组件中this.$route.params，此处的this指的是vue的实例。

2.路由嵌套
这个很容易理解，比如一个主页可以由header,menu,content这些内容组件，然后里面又可以有其它的部分组成，这种时候就可以使用嵌套了。这种时候在父路由中定义children即可，children里面的配置项和普通的路由配置一样。比如我们的项目中大部分的路由都是/manage的子路由。

3.编程式导航
使用路由对象，其中使用到的方法有
```
router.push('home');
router.replace('home');
router.go(n);
```
与history的一些操作API非常类似，比如
pushState, replaceState等。

### 路由组件传参

前面提到通过路由传参，然后通过路由的param对象访问对应的参数，但实际上这种方式并不是最理想的，因为这样的你的代码与$route绑定在一起了。这里面提供了props模式，替换这种方式。这里借用一下官方的例子。

```
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },

    // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]
})
```
### 导航守卫

1.导航被触发。

2.在失活的组件里调用离开守卫。

3.调用全局的 beforeEach 守卫。

4.在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。

5.在路由配置里调用 beforeEnter。

6.解析异步路由组件。

7.在被激活的组件里调用 beforeRouteEnter。

8.调用全局的 beforeResolve 守卫 (2.5+)。

9.导航被确认。

10.调用全局的 afterEach 钩子。

11.触发 DOM 更新。

12.用创建好的实例调用 beforeRouteEnter 守卫中传给 next 的回调函数。

这些钩子都会传递一个方法，并且带有三个参数，to（目标路由）, form（源路由）, next（当前钩子的下步操作，放行？跳转至其它路由？中断？）.



