## express+mongodb+vue实现增删改查-全栈之路2.0

前后端分离实现增删改查Demo

## 效果图

### 登陆页
![登陆页](https://user-gold-cdn.xitu.io/2018/12/20/167cb14e1dd84392?w=1460&h=718&f=gif&s=268679)
### 查询
![查询](https://user-gold-cdn.xitu.io/2018/12/20/167cb15638df70eb?w=1460&h=718&f=gif&s=81164)
### 新增
![新增](https://user-gold-cdn.xitu.io/2018/12/20/167cb15e8d017438?w=1460&h=718&f=gif&s=478159)
### 修改
![修改](https://user-gold-cdn.xitu.io/2018/12/20/167cb161ae684a67?w=1460&h=718&f=gif&s=297382)
### 删除
![删除](https://user-gold-cdn.xitu.io/2018/12/20/167cb19584fc3497?w=1412&h=677&f=gif&s=347169)
### 详情页
![详情](https://user-gold-cdn.xitu.io/2018/12/21/167d0a710750ad9d?w=1413&h=677&f=gif&s=4728826)
## 技术栈
`vue` `axios` `vue-router` `express` `mongo` `element` `iconfont` `scss`

## 前言
半年前写过一个[express+mongodb+vue][1]的项目，其中大致的给大家展示了从零构建一个前后台项目所需要的技术点和思路，以及在开发过程中遇到的一些坑。
    
之后收到一些小伙伴的私信包括[github][2]上提出的**issue**。总结一下就是一下以下两点。

 1. 项目启动报错的问题
 2. 希望案例可以更加的丰富（分页、条件查询）
 
**其中项目报错404的问题，是因为该项目是一个前后端项目，不仅仅需要通过`npm run dev`启动前端，还需要通过终端`node app.js`启动后台。这里大家一定要注意！**

> 本次版本是之前版本的升级版，项目中对部分代码做了一定的优化，也增加了一些新的模块和功能点，使得项目更加完善，大致有以下模块。


## 新增模块

 - 登陆页面
 - 条件查询
 - 分页查询
 - 本地缓存
 - 图标使用
 - scss使用
 - ......

## 提示
本篇主要是围绕**本次版本新增**的一些技术点展开陈述。不会过多的给大家讲解实现整个前后端项目的思路。如果你对整个项目的搭建思路还不是很明确的话，建议您先去阅读上一个版本[express+mongodb+vue][3]。

**强烈建议去我的[github][4]上，将项目下载到本地，启动项目后，顺着本文的思路与我进行灵魂深处的探讨，如果有任何问题的话，欢迎私信我！**

# 正文

## 封装常用工具类函数

因为在真实的项目中，我们需要频繁的用到`ajax`获取数据，之前的版本，是采用`vue-resource`完成的，但是由于官方不再维护，所以本次版本中采用官方建议使用的[axios][5]。我们可以方便实现自定义`axios`实例，拦截器，请求添加字段等功能。

在`src`目录下面，有个`utils`文件夹，里面用来存放一些工具类函数，这些工具类函数应该是具有通用性的。也就是在不同的组件页面中都可以引用。实现一次定义，多次使用的目的。具体的工具类函数分类大致有以下一些点。

 - axios实例
 - 常用正则校验函数
 - cookie、sessionStorage添加、获取、删除方法
 - ......

**总之从事开发的同学们一定要有简化业务逻辑的思维，经常用到的模块，独立出来。**

>Tips:文件命名和函数命名尽量标准一点，不要想起啥就是啥，尽量用对应的英文去命名，英文不好的去百度呗，磨刀不误砍柴工！

## 登陆页面的那点事

合理的登陆逻辑应该是以下两点:

 1. 用户在查询英雄列表之前，首先需要登录，否则**重定向**到登录页面
 2. 对于已经登录的用户，可以直接访问列表页

为了实现上述逻辑，我们可以使用[vue-router][6]中提供的前置守卫导航`beforeEach`配合`路由重定向`实现。具体代码参考`permission.js`文件。

> Tips:这里要提醒以下大家路由导航中的`next`使用一定要注意，其中传参和不传参是有不同的效果的。我在开发的过程中，就因为这个，遇到了**无限循环**的坑。

## 更加丰富的图标选择

本项目使用的前端框架[element][7]中虽然为我们提供了一些常用的图标，但是在真实的开发场景中，是无法满足的。如果你还在用图片实现icon的话，我只想送你两个字——**牛逼**！

[阿里巴巴iconfont][8]图标库，可以帮助我们解决框架提供图标不完善的问题，其中使用方法有三种，它们之间的利和弊可自行前往了解。本项目中使用的是`unicode`方式。

我们在开发一个项目之前，可在`阿里巴巴iconfont`上新建一个项目，然后去图标库中查找对应的图标，添加到项目中。再下载到本地，引入项目中即可。

项目`src`目录下面的`assets`文件夹中，主要存放一些静态资源，比如`css` `image` `icon`等。

然后在`main.js`文件中引入对应图标的css文件。
    
    #main.js
    
    import "./assets/icon/iconfont.css"

    

> Tips:其中对于图标库的前缀命名和图标的命名一定要规范，否则后期可能会遇到很大的麻烦。
重要的事说三遍：
命名规范！
命名规范！
命名规范！

## scss的使用

之前的版本中，关于样式是用`css`进行命名的，这样就会出现以下这种情形...

    .container{
        width:...
        margin:...
    }
    
    .container header{
        padding:...
        border-radius:...
        box-shadow:...
    }
    .container header .title{
        background:...
        color:...
        font-size:...
    }

这种方式虽然没有问题，但是书写起来及其蛋疼，而且一旦形如这种的代码多了，代码看起来也会很不美观。

为了使性能更加好，逼格更加高，代码更加美观，所以我去学了下如何使用`scss`，大致分为以下三步骤：

    第一步：cmd终端或者**vscode**终端输入：
    npm  install sass-loader --save-dev
    npm install node-sass --sava-dev
    
    第二步：在build文件夹下的webpack.base.conf.js的rules里面添加配置
    {
        test: /\.scss$/,
        loaders: ['style', 'css', 'sass']
    }
    
    第三步： 使用scss时候在所在的style样式标签上添加lang=”scss”即可应用对应的语法，否则报错
    
再用scss语法去书写上面的css规则，则变成以下这种格式:

    .container{
        width:...
        margin:...
            header:{
                padding:...
                border-radius:...
                box-shadow:...
                    .title{
                        background:...
                        color:...
                        font-size:...
                    }
            }
    }
    
**两种风格的区别和优劣，我相信不用多说，你也应该明白了。**

> Tips:css的预处理器有less、sass、scss等，它们之间有各自的特点和风格，但是万变不离其宗，你要做的就是打好css基本功。

## sessionStorage实现本地缓存

就拿本案例的实际场景来说吧，当用户从列表页跳转到详情页面时，再返回列表页面时，列表页面的查询条件和查询结果应该还存在那里。而不是需要用户再次输入查询条件进行二次查询，这样做的好处主要有以下两点：

>更加符合实际使用场景，减少用户使用成本

毕竟前端工程师是要用自己最大的技术能力让用户体验更佳卓越！

这里我的**思路**是：

    1.用户输入查询条件后，进行列表查询
    
    2.用户点击某条数据的相信按钮，跳转到详情页面（这时我们要去保存用户的查询条件和当前的页数）
    
    3.用户从详情页返回列表页（在mounted钩子函数中，判断缓存中是否存在缓存数据，如果存在的话，则用缓存数据去进行查询）
    
    注意用户每次进行查询后，我们需要将缓存给删除，否则用户可能刷新页面后缓存仍然存在，这里我们将添加缓存的时机选在（用户点击详情按钮的那一刻）
    
**大致代码:**

    #List.vue组件中
    
    #点击详情按钮函数
    toDetail(id){
        var queryParmas = {
            ...
            ...
            ...
        };
        //在本地缓存中存储查询条件
        sessionStorage.queryParmas = JSON.stringify(queryParmas);
        
    }
    
    #查询函数
    search(){
        ...
        ...
        ...
        //每次查询数据后，删除缓存
        sessionStorage.removeItem("queryParmas");
    }
    
    #mounted钩子函数
    mounted(){
        //进入页面判断是否存在缓存，如果有缓存，直接查询
        var sessionObj = sessionStorage.getItem("queryParmas");
        if(sessionObj){
            //取出缓存数据，包括上次查询条件和上次查询页数，进行查询
        }
    }
    
> Tips:element中分页的使用中会存在一些坑，当使用上述缓存数据进行查询时，可能会出现页码的一些bug。这里我也没有细找原因，但是通过使用vue中的$nextTick方法控制分页的显隐，可以解决这个bug。具体的有兴趣可以了解下。


## 总结

本篇文章主要是围绕一些功能点和方案实现进行展开，同时也提出了一些个人建议。细心的你一定会发现，其实我提炼出的很多点，都可以围绕**前端性能优化**进行展开。其中里面还有很多好玩的，包括`http` `浏览器渲染机制` `重排、重绘` `函数节流、防抖`等需要我们去学习，这些会鞭策着我们，不断地去优化自己的程序。最终写出更加优质的代码！

## 最后的祝福

**天青色等烟雨，而我在等你！动动你们的☝️️️，点个赞再走！**

**2019即将到来，愿所有人牛逼！在这给你们🙏个早年！**

**原创不易，且👣且珍惜！**

[Github传送门][9] 

☝️☝️☝️☝️☝️

**ruiwei88888@163.com**

☝️☝️☝️☝️☝️有任何问题，欢迎邮箱私信我！



  [1]: https://juejin.im/post/5aabc2caf265da239376d5ff
  [2]: https://github.com/weirui88888
  [3]: https://juejin.im/post/5aabc2caf265da239376d5ff
  [4]: https://github.com/weirui88888
  [5]: https://www.kancloud.cn/yunye/axios/234845
  [6]: https://router.vuejs.org/zh/guide/advanced/navigation-guards.html
  [7]: http://element-cn.eleme.io/#/zh-CN/
  [8]: https://www.iconfont.cnment-cn.eleme.io/#/zh-CN/
  [9]: https://github.com/weirui88888
