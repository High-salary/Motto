#### plugin.js
```
 mysql: {
    // mysql
    enable: true,
    package: 'egg-mysql'
  },
 cors: {
    // 跨域
    enable: true,
    package: 'egg-cors',
    // ignoreJSON: true
  }
```

#### config.default.js
```
multipart: {
    //  文件上传
      mode: 'file'
    },
cors: {
    //  跨域
      origin: '*', //允许所有跨域访问
      // allowMethods: 'GET,HEAD,PUT,POST,DELETE,PATCH'
    },
bodyParser :{
    //  传参的大小限制
      jsonLimit: '10mb',
      formLimit: '10mb'
    },
security : {
    //  csrf 安全权限关闭
      csrf: false
    },
mysql: {
      client: {
        host: 'localhost',
        port: '3306',
        user: 'root',
        password: '123456',
        database: 'mysql'
      },
      app: true,
      agent: false
    }
```

#### jsonp 跨域的解决方案
```
const jsonp = app.jsonp()
//  作为中间件加到 router 里面

```

