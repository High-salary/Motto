####  使用scss
下载 node-sass  sass-loader 即可


#### 使用axios


#### 关闭提示与报错
```
node_modules/dva/lib/index.js/line 22  
修改为   require("history").createHashHistory

node_modules/react-domo/cjs/react-dom.development.js/line 11494 
注释掉

```

**配置别名 @ 映射src目录**
webpack.config.js
```
alias:{
    "@":path.join(__diename,'../src')
}
```
##### 装饰器语法-- + --按需引入antd
######  配置装饰器语法之前必须配置@别名
src目录的 .babelrc文件
```
{
    "presets": [
        "react-app"
    ],
    "plugins": [
        [ //    装饰器语法
            "@babel/plugin-proposal-decorators",
            {
                "legacy": true
            },
        ],
        [ //    antd 按需引入
            "import",
            {
                "libraryName": "antd",
                "libraryDirectory": "es",
                "style": "css",
            }
        ]
    ]
}
//
```
使用 @...
```
const mapStateToProps=state=>state.login

const createFn={ name: 'normal_login' }

@connect(mapStateToProps)

@Form.create(fn)
...
class Home{
    
}
export default Home
```

---
# 具体的配置文件
---
### 基本配置
**src/index.js**
```
import dva from 'dva';
import RouterView from '@/router';
import { createModel } from '@/store';

const createBrowserHistory = require('history').createBrowserHistory;
const app = dva({
    history: createBrowserHistory()
})
createModel(app)
app.router(RouterView)
app.start('#root')

```

###  src/routes路由配置

**src/routes/routes.js**
```
import Home from '@/containers/home';
import Login from '@/containers/login';
import List from '@/containers/list';
import Detail from '@/containers/detail';

export default [
    {
        path: '/login',
        name: 'login',
        component: Login,
        children: [
            {
                path: '/login/home',
                name: 'home',
                component: Home,
            }
        ]
    },
    {
        path: '/list',
        name: 'list',
        component: List
    }, {
        path: '/detail',
        name: 'detail',
        component: Detail
    },
    {
        from: '/',
        to: "/login"
    }
]

```
**src/routes/map.js**
```
import React, { Component } from 'react';
import { Route, Switch, Redirect } from 'dva/router';

class RouterMap extends Component {
    render() {
        const { routes, history } = this.props;
        // console.log(history)
        return <Switch>
            {routes.map((item, index) =>
                item.path
                    ? <Route key={index} path={item.path} render={(props) =>
                        item.children
                            ? <item.component>
                                <RouterMap routes={item.children} history={history} {...props} />
                            </item.component>
                            : <item.component history={history} {...props} />
                    }></Route>
                    : <Redirect key={index} {...item} />
            )}
        </Switch>

    }
}

export default RouterMap;
```
**src/routes/index.js**
```
import React from 'react';
import RouterMap from '@/router/map';
import Routes from '@/router/routes';
import { BrowserRouter as Router } from 'react-router-dom'

function RouterView(props) {
    const routes = props.routes ? props.routes : Routes;
    return <Router>
        <RouterMap routes={routes} {...props} />
    </Router>
}

export default RouterView;
```
### store配置
#### 基本配置   src/store/action 文件夹
```
const context = require.context('./model', false, /\.js$/);
const getModel = context.keys().map(key => context(key));

export function createModel(app) {
    return getModel.map(key => app.model(key.default))
};
```
#### store配置  src/store/model/ login.js
可以用插件  的  admodel 快速生成文件内容   必须加上namespace字段的值
```
export default {
    namespace: 'login',
    state: {
       list: [
            {
                tit: 'asdf',
                num: 5,
                bool: true
            }
        ],
        show: []
    },
    // 同步逻辑
    reducers:{ 
        add(state, action) {
            // 深层次的数据结构需要展开
            return { ...state, list: [...action.list] }
        },
         min(state, action) {
            return { ...state, list: [...action.list] }
        },
        changeshow(state, action) {
            console.log(action, '---')
            return { ...state, show: [...action.show] }
        }
    },
    //异步逻辑
     effects: {
         *getlist({ url }, { put }) {
            console.log(url, '---------------------')
            const show = (yield Axios.get(url)).data
            // const aa = (yield Axios.get('/get')).data
            yield put({ type: "changeshow", show })
        }
     }
}
```
#### action配置  src/store/action 文件夹
配置具体的action逻辑
<!--写在组件里的 -->
```
import React, { Component } from 'react'
import { connect } from 'dva';
const mapDispatchToProps = dispatch => ({
    min: (item, list) => {
        item.num--
        return dispatch({ type: 'list/min', list })
    },
    add: (item, list) => {
        item.num++
        return dispatch({ type: 'list/add', list })
    },
    getlist: () => dispatch({ type: "list/getlist" })
})
@connect(state => ({ ...state.list }), mapDispatchToProps)
class index extends Component {
    render() {
        const { show, history } = this.props;
        const { list } = this.props;
        const List = () => list.map(item =>
            <div key={item.tit} className='flex list'>
                <input type="checkbox" />
                <p>{item.tit}</p>
                <button onClick={() => this.props.min(item, list)}>-</button>
                <span>{item.num}</span>
                <button onClick={() => this.props.add(item, list)}>+</button>
            </div>
        )
        return (
            <div className=''>
                <List />
                <button onClick={() => { history.push('/login') }}>to home</button>
                <button onClick={() => { history.push('/detail/555') }}>to detail</button>
            </div>
        )
    }
}

export default index

```




