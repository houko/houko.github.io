---
date: 2020-11-08
title: nuxt.js使用介绍
featured_image: cover.png
tags:
  - nuxt
categories: 
  - web
slug: vueSSRNuxt
---
vue全家桶还算是比较全面的，从构建工具vue-cli、vite，到vue-router、vuex、element-ui，vue-dev-tools，最后服务器渲染方案nuxt.js等等构成了一个完整的开发生态，用着还是比较省心的。总结一下nux.js用法以供参考。

<!-- more -->

# 什么是SSR

服务端渲染（Server Side Render），即：网页是通过服务端渲染生成后输出给客户端。

在 SPA(Single Page Application，即单页面应用) 之前的时代，我们的Web架构大都是 SSR，如：Wordpress（PHP）、JSP技术、JavaWeb...或者 DEDE CMS、Discuz! 等这些程序都是传统典型的 SSR 架构， 即：服务端取出数据和模板组合生成 html 输出给前端，前端发生请求时，重新向服务端请求 html 资源，路由也由服务端来控制。

其次，有个概念叫预渲染（Prerendering）。

如果你只是用服务端渲染来改善一个少数的营销页面（如 首页，关于，联系 等等）的 SEO，那你可以用预渲染来实现。 预渲染不像服务器渲染那样即时编译 HTML，它只在构建时为了特定的路由生成特定的几个静态页面，等于我们可以通过 Webpack 插件将一些特定页面组件 build 时就编译为 html 文件，直接以静态资源的形式输出给搜索引擎。

但实际的商业应用中，大部分时候我们需要的是即时渲染，这也是我们今天讨论的主题。



# 为什么需要SSR

1.  为了兼容性      

    虽然现在大部分的浏览器对单页面应用支持优化，但还有一少部分浏览器比较古老。对于世界上的一些地区人，可能只能用1998年产的电脑访问互联网的方式使用计算机。 而 Vue 只能运行在 IE9 以上的浏览器，你可能也想为那些老式浏览器提供基础内容 - 或者是在命令行中使用 Lynx 的时髦的黑客。

2.   为了SEO     

    SEO是流量是变现的快车道，SEO 是低成本获取流量的最佳方法。

    目前大部分的搜索引擎仅能抓取URI直接输出的数据资源，对于 Ajax 类的异步请求的数据无法抓取；Google 除外，Google 有自己的[Google’s Webmaster AJAX Crawling Guidelines.](https://developers.google.com/webmasters/ajax-crawling/)技术支持。在大部分的商业应用中，我们有 SEO 的需求，我们需要搜索引擎更多地抓取到我们的内容，更详细地认识到我们的网页结构，而不是仅对首页或特定静态页进行索引，这是 SSR 最重要的意义。

    简单说就是，我们需要搜素引擎看到这样的代码：

    ![image-20201106154440746](https://image.xiaomo.info//blog/image-20201106154440746.png)

    

    而不是这样的代码：

    ![image-20201106154506477](https://image.xiaomo.info//blog/image-20201106154506477.png)

    

    3.  为了数据安全      

    现在基本上B/S架构的应用都是前后端分离方式开发的，即前端使用XHR异步获取数据并渲染到页面上，如果我们不使用SSR的话，用户可以直接在调试控制台拿到我们的接口数据。但如果是我们使用的是SSR渲染的话，浏览器收到的就是一个填充好数据的HTML，如果别有用心的人想拿我们的数据。要么人肉复制，要么使用jsonp等技术定位我们的元素。当我们发现有spider在偷我们的数据的时候，稍微换下html的dom结构，偷数据的人就得吭赤吭赤的更新他们的爬虫代码。

    

    此外，我们还需要在 SSR 的基础上实现 SPA，即：**首屏渲染**。

基本流程是：

在浏览器第一次访问某个 URI 资源的时候（首屏），Web 服务器根据路由拿到对应数据渲染并输出，且输出的数据中包含两部分：

路由页对应的页面及已渲染好的数据

完整的SPA程序代码



在客户端首屏渲染完成之后，此时我们看到的其实已经是一个和之前的 SPA 相差无几的应用程序了，接下来我们进行的任何操作都只是客户端的应用进行交互， 页面/组件由Web端渲染，路由也由浏览器控制，用户只需要和当前浏览器内的应用打交道就可以了。

之前在各大 SPA 框架还未正式官方支持 SSR 时，有一些第三方的解决方案，如：[prerender.io](https://prerender.io/)， 它们做的事情就是建立HTTP一个中间层，在判断到访问来源是蜘蛛时，输出已缓存好的html数据，此数据若不存在，则调用第三方服务对 html 进行缓存，往复进行。

另一方法是自行构建蜘蛛渲染逻辑，当识别 UA 为搜索引擎时，拿服务端已准备好的模板和数据进行渲染输出 html 数据，反之，则输出 SPA 应用代码；

我当时也考虑过此方法，但有很多弊端，如：

需要针对蜘蛛编写一套独立的渲染模板，因为大部分情况下 SPA 的代码是没法直接在服务端使用的

搜索引擎若检测到蜘蛛抓取数据和真实访问数据不一致，会做降权惩罚，也就意味着渲染模板还必须和SPA预期输出一模一样



所以，最好的方法是 SPA 能和服务端使用同一套模板，且使用同一个服务端逻辑分支，再简单说：**最好 Vue、Ng2... 能直接在服务端跑起来**。

于是，陆续诞生了基于 React 的[Next.js](https://github.com/zeit/next.js/)、基于 Vue 的[Nuxt.js](https://cn.nuxtjs.org/)、Ng2 诞生之日便支持。

# VUE的SSR方案(nuxt)

Nuxt.js是使用 Webpack 和 Node.js 进行封装的基于Vue的SSR框架，使用它，你可以不需要自己搭建一套 SSR 程序，而是通过其约定好的文件结构和API就可以实现一个首屏渲染的 Web 应用。之所以叫 Nuxt.js 也是因为受到了 Next.js 的启发。作者是法国的兄弟俩，EvenYou 在微博多次提到，也在欧洲见过哥俩。

在此之前，国内有一些对 Vue SSR 的整合尝试，但都没有成功，主要在于 Webpack 和 Node 的结合上没有实践出最佳方案， 当我看到 Nuxt.js 以约束文件夹和配置文件`nuxt.config.js`的方式来管理多个程序组件之间的关系时，就觉得，很酷！

Nuxt.js 是一个 Node 程序，就像上面说的，我们是要把 Vue 跑在服务端，所以必须使用 Node 环境。我们对 Nuxt.js 应用的访问，实际上是在访问这个 Node.js 程序的路由，程序输出首屏渲染内容 + 用以重新渲染的 SPA 的脚本代码，而路由是由 Nuxt.js 约定好的 pages 文件夹生成的。

所以，整体上，Nuxt.js 通过各个文件夹和配置文件的约束来管理我们的程序，而又不失扩展性，其有自己的[插件机制](https://nuxtjs.org/guide/plugins/)。

# Nuxt项目的创建

1.   npx

    ```sh
    npx create-nuxt-app <project-name>
    ```

2.  yarn

    ```bash
    yarn create nuxt-app <project-name>
    ```

3.  npm

    ```bash
    npm init nuxt-app <project-name>
    ```



# 启动

1.  yarn

    ```bash
    cd <project-name>
    yarn dev
    ```

2.  npm 

    ```bash
    cd <project-name>
    npm run dev
    ```

# nuxt项目结构介绍

按照目前的版本，Nuxt.js 的程序的文件结构大概分为以下部分：

**pages**：各页面组件，用于生成对应路由，支持嵌套，支持动态路由

**components**：各组件，用于你自己管理公共组件或非公共组件

**layouts**：宿主布局页面模板组件，用于你可以把不同的页面指定使用不同的布局

**assets**：用于 Webpack 编译的各类资源，通常是一些小的资源，如代替雪碧图之类的图片等东西

**middleware**：中间件，首屏渲染和路由跳转前均执行对应中间件，可以返回promise或直接next（像是一个网关，很实用！）

**plugins**：插件，SPA中用的各类第三方组件和一些node模块都可以在这引入，甚至可以引入自己编写的第三方库

**store**：内置了vuex，可以直接返回数据模块或返回一个自建vuex根对象，具体要翻文档

**其他**：你可以自定义文件夹和别名映射，文档都有提及，这里有[配置代码](https://github.com/surmon-china/surmon.me/blob/master/nuxt.config.js#L18)



# nuxt.js 配置文件介绍

```js
export default {
  // Disable server-side rendering (https://go.nuxtjs.dev/ssr-mode)
  ssr: true,

  // Global page headers (https://go.nuxtjs.dev/config-head)
  head: {
    title: 'hello-nuxt',
    meta: [
      { charset: 'utf-8' },
      { name: 'viewport', content: 'width=device-width, initial-scale=1' },
      { hid: 'description', name: 'description', content: '' },
    ],
    link: [{ rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' }],
  },

  // Global CSS (https://go.nuxtjs.dev/config-css)
  css: ['element-ui/lib/theme-chalk/index.css'],

  // Plugins to run before rendering page (https://go.nuxtjs.dev/config-plugins)
  plugins: ['@/plugins/element-ui'],

  // Auto import components (https://go.nuxtjs.dev/config-components)
  components: true,

  // Modules for dev and build (recommended) (https://go.nuxtjs.dev/config-modules)
  buildModules: [
    // https://go.nuxtjs.dev/typescript
    '@nuxt/typescript-build',
  ],

  // Modules (https://go.nuxtjs.dev/config-modules)
  modules: [
    // https://go.nuxtjs.dev/axios
    '@nuxtjs/axios',
    // https://go.nuxtjs.dev/pwa
    '@nuxtjs/pwa',
  ],

  // Axios module configuration (https://go.nuxtjs.dev/config-axios)
  axios: {},

  // Build Configuration (https://go.nuxtjs.dev/config-build)
  build: {
    transpile: [/^element-ui/],
  },
}
```



[`nuxt.config.js`](https://zh.nuxtjs.org/docs/2.x/directory-structure/nuxt-config/)对程序的扩展管理可大概分为以下类：

**build**：主要对应 Webpack 中的各配置项，可以对默认的 Webpack 配置进行扩展，如[这里代码](https://github.com/surmon-china/surmon.me/blob/master/nuxt.config.js#L17)

**cache**：主要对应内置的组件缓存模块`lru-cache`的配置对象，有默认值，可选关闭

**css**：对应我们在SPA随处引用样式文件的`require`语句

**dev**：用于自定义配置环境变量，对应之前`webpack.config.js`相关文件中的变量语句

**env**：同上息息相关

**generate**：对`generate`命令执行时的行为做一些定制

**head**：对应`vue-meta`插件的全局配置，`vue-meta`用于VUE/SSR程序的文档元信息的管理

**loading**：用于定制化Nuxt.js内置的进度条组件

**performance**：用于配置Node.js服务器性能上的配置

**plugins**：用于管理和应用对应`plugins`文件夹中的插件

**rootdir**：用于设置 Nuxt.js 应用的根目录（这俩api有很大合并的意义）

**srcdir**：用于设置 Nuxt.js 应用的源码目录（这俩api有很大合并的意义）

**router**：用于对`vue-router`的扩展和定制，其中还包括了中间件的配置，但并不完美（后面说）

**transition**：用于定制Nuxt.js内置的页面切换过渡效果的默认属性值

**watchers**：用于定制Nuxt.js内置的文件监听模块`chokidar`和 Webpack 的相关配置项



# 路由

nuxt的没有固定配置路由的文件，它是根据约定自动生成的路由，所有的页面都在pages目录下。

1.  首页 （url:port）

    对应 pages/index.vue

2.  订单列表( url:port/order)

    对应 pages/order/index.vue

使用时

`<nuxt-link to="/order">订单</nuxt-link>`



# [动态路由](https://www.nuxtjs.cn/guide/routing)

在 Nuxt.js 里面定义带参数的动态路由，需要创建对应的**以下划线作为前缀**的 Vue 文件 或 目录。

在 Nuxt.js 里面定义带参数的动态路由，需要创建对应的**以下划线作为前缀**的 Vue 文件 或 目录。

以下目录结构：

```bash
pages/
--| _slug/
-----| comments.vue
-----| index.vue
--| users/
-----| _id.vue
--| index.vue
```

Nuxt.js 生成对应的路由配置表为：

```js
router: {
  routes: [
    {
      name: 'index',
      path: '/',
      component: 'pages/index.vue'
    },
    {
      name: 'users-id',
      path: '/users/:id?',
      component: 'pages/users/_id.vue'
    },
    {
      name: 'slug',
      path: '/:slug',
      component: 'pages/_slug/index.vue'
    },
    {
      name: 'slug-comments',
      path: '/:slug/comments',
      component: 'pages/_slug/comments.vue'
    }
  ]
}
```

你会发现名称为 `users-id` 的路由路径带有 `:id?` 参数，表示该路由是可选的。如果你想将它设置为必选的路由，需要在 `users/_id` 目录内创建一个 `index.vue` 文件。



举例：



```vue
// layouts/default.vue 配置菜单
<template>
  <div>
    <nuxt-link to="/">home</nuxt-link>
    <nuxt-link to="/order">order</nuxt-link>
    <Nuxt />
  </div>
</template>
```





```vue
// pages/order/index.vue  对应/order

<template>
  <div>
    <ul>
      <li v-for="phone in phones" :key="phone">
        <nuxt-link :to="'/detail/' + phone">{{ phone }}</nuxt-link>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
export default {
  data() {
    return {
      phones: ['锤子', 'iphone', 'google'],
    }
  },
}
</script>

```



```vue
// pages/detail/_id.vue   对应 /detail/:id   动态路由

<template>
  <div>
    <h2>详情</h2>
    <!-- 因为文件名是/detail/_id.vue，所以这里的参数是id -->
    {{ $route.params.id }}
  </div>
</template>

<script>
export default {}
</script>

```



# 嵌套路由

创建内嵌子路由，你需要添加一个 Vue 文件，同时添加一个**与该文件同名**的目录用来存放子视图组件。别忘了在父组件(`.vue`文件) 内增加 `<nuxt-child/>` 用于显示子视图内容。



父路由

```vue
// pages/users.vue 这是父路由，还需要创建一个同名的users文件夹。子路由/users和/users/profile2，别忘了<nuxt-child/>
<template>
  <div>
    <h2>parent users</h2>
    <nuxt-link to="/users">user</nuxt-link>
    <nuxt-link to="/users/profile">user detail</nuxt-link>
    <nuxt-child />
  </div>
</template>

<script>
export default {}
</script>
```



2个子路由

```vue
// pages/users/index.vue

<template>
  <div>用户列表</div>
</template>

<script>
export default {}
</script>

<style></style>

```

```3vue
// pages/users/profile.vue
<template>
  <div>用户详情</div>
</template>

<script>
export default {}
</script>

<style></style>

```



# 404 页面

`_.vue`意思是无限次嵌套，因此在pages下创建如下文件

```vue
// pages/_.vue
<template>
  <div>404</div>
</template>

<script>
export default {}
</script>

<style></style>

```



# 中间件

使用中间件做权限认证

```js
// middleware/auth.js
export default function (context) {
  context.userAgent = process.server
    ? context.req.headers['user-agent']
    : navigator.userAgent
}

```


```js
// nutx.config.js 配置
export default {  
  router: {
      middleware: 'auth',   // 对应middleware/auth.js
    },
}
```

# 使用插件

```bash
yarn add vue-notifications
yarn add mini-toastr
```

```js
import Vue from 'vue'
import VueNotification from 'vue-notifications'
import miniToastr from 'mini-toastr'

const toastTypes = {
  success: 'success',
  error: 'error',
  info: 'info',
  warn: 'warn',
}

miniToastr.init({ types: toastTypes })

function toast({ title, message, type, timeout, cb }) {
  return miniToastr[type](message, title, timeout, cb)
}

const options = {
  success: toast,
  error: toast,
  info: toast,
  warn: toast,
}
Vue.use(VueNotifications, options)

```





```js
// nutx.config.js 配置
export default {
    // Build Configuration (https://go.nuxtjs.dev/config-build)
  	// 如果插件位于node_modules并导出模块，需要将其添加到transpile构建选项：
    build: {
      transpile: [/^element-ui/, 'vue-notifications'],
    },
	  // Plugins to run before rendering page (https://go.nuxtjs.dev/config-plugins)
  	plugins: [
    { src: '@/plugins/element-ui', ssr: false },
    { src: '@/plugins/vue-notifications', ssr: false },   // 对应/plugins/vue-notifications.js
  ],
}
```



# ts扩展

```sh
yarn add --dev @nuxt/typescript-build @nuxt/types
# 或
npm install --save-dev @nuxt/typescript-build @nuxt/types
```

```ts
// /vue-shim.d.ts
declare module "*.vue" {
  import Vue from 'vue'
  export default Vue
}
```

```js
// nuxt.config.js

export default {
    buildModules: [
    // https://go.nuxtjs.dev/typescript
    '@nuxt/typescript-build',
  ],
}
```

```json
// tsconfig.json
{
  "compilerOptions": {
    "target": "ES2018",
    "module": "ESNext",
    "moduleResolution": "Node",
    "lib": [
      "ESNext",
      "ESNext.AsyncIterable",
      "DOM"
    ],
    "esModuleInterop": true,
    "allowJs": true,
    "sourceMap": true,
    "strict": true,
  "noEmit": true,
    "experimentalDecorators": true,
    "baseUrl": ".",
    "paths": {
      "~/*": [
        "./*"
      ],
      "@/*": [
        "./*"
      ]
    },
    "types": [
      "@types/node",
      "@nuxt/types"
    ]
  },
  "exclude": [
    "node_modules",
    ".nuxt",
    "dist"
  ]
}

```



# 修改端口号

```js
// nuxt.config.js
export default {
  server: {
    port: 3030, // default: 3000
    host: '0.0.0.0', // default: localhost
  }
}
```



# 异步加载数据的hook(nuxt自动调用)

`yarn add @nuxt/http`



需要添加nuxt的http模块

````js
// nuxt.config.js

export default {
	modules: [
    // https://go.nuxtjs.dev/axios
    '@nuxtjs/axios',
    // https://go.nuxtjs.dev/pwa
    '@nuxtjs/pwa',
    '@nuxt/http',
  ],

}

````





```vue
<template>
  <div>
    <h1>Data fetched using asyncData</h1>
    <ul>
      <li v-for="mountain in mountains" :key="mountain.title">
        {{ mountain.title }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  async asyncData({ $http }) {
    const mountains = await $http.$get('https://api.nuxtjs.dev/mountains')
    return { mountains }
  },
}
</script>
```

![image-20201124160222403](https://image.xiaomo.info//blog/image-20201124160222403.png)

