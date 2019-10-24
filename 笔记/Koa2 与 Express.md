
### 生猛上手Koa
```
const app = require('koa')();//实例化
const path = require('path');
const static = require('koa-static');//静态资源文件
const KoaViews = require('koa-views');//模板引擎
const router = require('koa-router')();//路由对象
const bodyParser = require('koa-bodyparser');//解析post传参
//cookie解析中间键

//加载静态资源文件
const views = path.join(__dirname, 'views');
//没有设置'/'时   会默认加载index.html 文件
app.use(static(views));

//加载解析器  解析post传参
app.use(bodyParser());

//模板引擎设置
app.use(KoaViews(views, { extension: 'ejs' }));//

//路由对象的设置
//渲染模板文件
router.get('/', async ctx => {
    await ctx.render('index');
})

router.get('/user', ctx => {
    //获取 get 传参  ctx.query
    const query= ctx.query
    //返回response对象
    ctx.body =  {
        name: 'zhangsan',
        age: 12
    }
})
router.post('/login', ctx => {
    //获取 post 传参  ctx.request.body
    const body= ctx.request.body
    //返回response对象
    ctx.body = body
})

// 在一级路由对象上挂载二级路由
//  定义二级路由
const rout= require('koa-router')();
rout.get('/search', ctx => {
    ctx.body ={ code:1 }
})
//  挂载到一级路由上  访问方式为  /login/search
router.use('/login',rout.routes(),rout.allowedMethods());


// 挂载路由对象(一级)
//挂载路由
app.use(router.routes(),router.allowedMethods());
//设置端口
const PORT = process.env.PORT || 3000;
//监听端口
app.listen(PORT,()=>{
    console.log('port is ' + PORT);
})

```


```
// 可以将同一个应用程序同时作为 HTTP 和 HTTPS 或多个地址：
const http = require('http');
const https = require('https');
const Koa = require('koa');
const app = new Koa();
http.createServer(app.callback()).listen(3000);
https.createServer(app.callback()).listen(3001);
```

### 合理的拆分文件  模块化