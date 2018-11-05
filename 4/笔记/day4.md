# React.js - day4 - 豆瓣电影



## 1. 路由

> 什么是路由：路由就是对应关系；
>
> 后端路由：URL地址到后端处理函数之间的对应关系；
>
> 前端路由：hash地址到组件之间的对应关系；  监听`window.onhashchange`事件，并拿到最新的hash值，然后对应展示不同的组件即可；

### 配置路由

> Vue中的路由怎么使用；
>
> 1. cnpm i `vue-router` -S
> 2. 创建路由的实例对象   const router = new VueRouter({ routes: [] })    【创建路由规则】
> 3. 将 new 出来的 路由实例对象，挂载到 VM 的 router 属性上；
> 4. 在 对应的组件中，使用`<router-link to="路由地址"></router-link>`创建路由连接；
> 5. 在页面上放一个 `<router-view></router-view>` 【路由组件的容器】

总结：如果在框架中，要使用路由，一定要有 【路由规则、路由链接、呈现路由组件的容器】

1. 运行`cnpm i react-router-dom -S`安装依赖项

2. 创建一个`App.jsx`根组件，并在根组件中，按需导入路由需要的三个组件：

   ```js
   // Link 是路由链接
   // Route是路由的规则，同时，也是路由的容器
   // HashRouter 表示路由的包裹容器；在一个项目中，只需要使用唯一的一次！！！
   import { HashRouter, Route, Link } from 'react-router-dom'
   ```

3. 在`App.jsx`中，render 函数中，最外层使用`HashRouter`进行包裹：

   ```jsx
   render(){
       return <HashRouter>
       	<div>
           	<h1>大标题</h1>
           </div>
       </HashRouter>
   }
   ```

4. 在 需要的地方，使用`Link`组件创建路由链接，其中，通过`to`属性指定路由地址：

   ```jsx
   <Link to="/home">首页</Link>
   ```

5. 使用`Route`组件创建路由规则，同时注意：Route组件有两重身份：1. 路由规则（path   component）；2. 占位符（用来显示匹配到的组件）

   ```js
   // 导入 Home 组件
   import Home from './components/Home.jsx'

   <Route path="/home" component={Home}></Route>
   ```

   ​

### 路由重定向

1. 需要按需导入`Redirect`组件：

   ```js
   import { HashRouter, Route, Link, Redirect } from 'react-router-dom'
   ```

2. 新建一个路由规则

   ```jsx
   <Route exact path="/" render={() => <Redirect to="/home" />}></Route>
   // 其中  exact 属性表示 精确匹配
   // path 表示 重定向之前的 路由规则
   // render 是一个函数，必须 为 render 属性绑定一个 function，因此最佳实践是提供一个 箭头函数
   //        在 提供的 箭头函数中，需要return 一个 <Redirect> 组件，其中， to 属性为 重定向的路由
   ```

   ​

### 路由嵌套

1. react中如何实现路由嵌套：直接在需要的组件页面中，创建属于当前页面的 `Link`和`Route`，那么，这些创建的 `Route`和`Link`都属于子路由；

2. 导入需要的路由组件:

   ```js
   import { Link, Route, Redirect } from 'react-router-dom'
   ```

3. 导入需要的子路由组件：

   ```js
   // 导入 子路由组件
   import Tab1 from './Tabs/Tab1'
   import Tab2 from './Tabs/Tab2'
   ```

4. 在指定页面中，创建独属于当前页面的子路由链接：

   ```jsx
   <Link to="/about/tab1">Tab1</Link>&nbsp;&nbsp;
   <Link to="/about/tab2">Tab2</Link>
   ```

5. 在指定页面中，创建独属于当前页面的子路由规则：

   ```jsx
   {/* 应该对应放置两个 Route 占位符，分别用来显示 匹配到的 路由组件 */}
   {/* 重定向的路由规则 */}
   <Route exact path="/about" render={() => <Redirect to="/about/tab1" />}></Route>
   <Route path="/about/tab1" component={Tab1}></Route>
   <Route path="/about/tab2" component={Tab2}></Route>
   ```

   ​

### 路由传参

1. 需要把路由规则中，对应参数的片段区域，使用`:`指定为参数：

   ```jsx
   <Route exact path="/movie/:type/:id" component={Movie}></Route>
   ```

2. 获取 路由规则中匹配到的参数：

   ```js
   this.props.match.params
   ```



### 编程式导航

通过 `this.props.history`对象提供的方法， 可以实现编程式导航：

1. `this.props.history.go(n)` 前进或后退N个历史记录
2. `this.props.history.goBack()`后退1个历史记录
3. `this.props.history.goForward()`前进1个历史记录
4. `this.props.history.push('url地址')`跳转到哪个路由超链接中去





## 2. 使用[AntDesign](https://ant.design/index-cn)组件库

### 完整导入和使用Ant Design的步骤

1. 安装包`cnpm i antd -S`

2. 导入完整的样式表：

   ```js
   // 导入 Antd 的 样式表
   import 'antd/dist/antd.css';
   ```

3. 按需导入需要的组件：

   ```js
   // 按需导入 DatePicker 组件
   import { DatePicker } from 'antd';
   ```

4. 把导入的组件，以标签形式引用到页面上：

   ```jsx
   <DatePicker />
   ```

5. 注意：由于 antd 有自己的css样式表，所以，大家需要包装提前在项目中，配置好了打包`.css`样式表的相关loader;



### 按需导入和使用 Ant Design 的步骤

1. 运行`cnpm i babel-plugin-import -D`安装按需导入的babel插件

2. 修改项目中的`.babelrc`文件如下,新增节点被 `+`标识：

   ```json
   {
     "presets": ["env", "stage-0", "react"],
     "plugins": [
       "transform-runtime",
   +    ["import", { "libraryName": "antd", "libraryDirectory": "es", "style": "css" }]
     ]
   }
   ```

3. 当实现了前两步以后，我们在`main.js`中，就不需要再引入样式表了；

   ​


## 3. 基于Promise规范的fetch API的使用

### fetch的使用

1. 作用：fetch 这个API，是专门用来发起Ajax请求的；

2. fetch 是由原生 JS 提供的 API ，专门用来取代 XHR 这个对象的；

   ```js
   fetch('请求的url地址').then(response => res.json()).then(data= > console.log(data))
   // 注意： 第一个.then 中获取到的不是最终的数据，而是一个中间的数据流对象；
   // 注意： 第一个 .then 中获取到的数据，是 一个 Response 类型的对象；
   // 第二个 .then 中，获取到的才是真正的 数据；
   ```

3. 发起 Get 请求：

   ```js
   // 默认 fetch('url') 的话，发起的是 Get 请求
     fetch('http://39.106.32.91:3000/api/getlunbo')
       .then(response => {
         // 这个 response 就是 服务器返回的可读数据流，内部存储的是二进制数据；
         // .json() 的作用，就是 读取 response 这个二进制数据流，并把 读取到的数据，转为 JSON 格式的 Promise对象
         return response.json()
       })
       .then(data => {
         // 这里，第二个.then 中，拿到的 data，就是最终的数据
         console.log(data)
       })
   ```

   ​

4. 发起 Post 请求：

   ```js
   // 这是 查询参数   name=zs&age=20
     var sendData = new URLSearchParams()
     sendData.append('name', 'ls')
     sendData.append('age', 30)

     fetch('http://39.106.32.91:3000/api/post', {
       method: 'POST',
       body: sendData // 要发送给服务器的数据
     })
       .then(response => response.json())
       .then(data => console.log(data))
   ```

5. 注意： fetch 无法 发起 JSONP 请求



### fetch-jsonp的使用

1. `fetch-jsonp`最基本的用法：

   ```js
   fetchJsonp('https://api.douban.com/v2/movie/in_theaters')
     .then(function (response) {
       // response.json()   当我们为 response 对象调用了它的 .json() 方法以后，返回的是新的 promise 实例对象
       return response.json()
     })
     .then(function (result) {
       console.log(result)
     })
   ```

2. 注意事项：

   1. 在调用 fetchJsonp 的时候，小括号中写的就是 你请求的 API 地址
   2. 当调用 fetchJsonp  以后，得到的是一个 Promise  实例对象，需要为 这个 Promise 实例对象，通过`.then`指定成功的回调函数，在第一个 `.then()`中无法拿到最终的数据，拿到的是一个 `Response` 类型的对象；
   3. 在 第一个 `.then`中，需要`return response.json()`从而返回一个新的Promise 实例；
   4. 为 第一个 `.then()`中返回的promise实例，再次通过.then指定成功回调，拿到的才是最终的数据；

   > 总结: 第一个.then拿到的是中间数据;  第二个.then中拿到的才是最终的数据;





## 4. 项目结构搭建和布局



## 5. this.prop和Route的关系



## 6. 项目API接口地址

> 开发环境 - 请求根地址：`http://39.106.32.91:3005`
>
> 上线环境 - 请求根地址：`https://api.douban.com`

1. 正在热映：`/v2/movie/in_theaters?start=0&count=1`
2. 即将上映：`/v2/movie/coming_soon?start=0&count=1`
3. Top250：  `/v2/movie/top250?start=0&count=1`
4. 电影详情：`/v2/movie/subject/26861685`




## 相关文章

+ [ANT DESIGN 一个 UI 设计语言](https://ant.design/index-cn)

+ [react-router-dom](https://reacttraining.com/react-router/web/guides/quick-start)

+ [豆瓣电影API地址](https://developers.douban.com/wiki/?title=api_v2)

 - [正在热映 - in_theaters](https://api.douban.com/v2/movie/in_theaters)

 - [即将上映 - coming_soon](https://api.douban.com/v2/movie/coming_soon)

 - [top250](https://api.douban.com/v2/movie/top250)

 - [电影详细信息 - subject](https://api.douban.com/v2/movie/subject/26309788)

+ [跨域资源共享 CORS 详解 - 阮一峰](http://www.ruanyifeng.com/blog/2016/04/cors.html)

+ [Request - Simplified HTTP client](https://github.com/request/request)

+ [CSS3 transform 属性](http://www.w3school.com.cn/cssref/pr_transform.asp)

+ [ES6 - Promise规范 - 阮一峰](http://es6.ruanyifeng.com/#docs/promise)

+ [Javascript 中的神器——Promise](http://www.jianshu.com/p/063f7e490e9a)

+ [MDN - Fetch API](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API)

+ [MDN - Response](https://developer.mozilla.org/zh-CN/docs/Web/API/Response)

+ [fetch-jsonp - 支持JSONP的Fetch实现](https://www.npmjs.com/package/fetch-jsonp)