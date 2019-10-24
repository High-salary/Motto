### 下载常用的插件
```
    @babel/plugin-proposal-decorators
    babel-plugin-import
    antd
    axios
    node-sass

    react-router-dom
    redux
    react-redux
    redux-thunk
```
### 配置别名@、按需加载antd 装饰器语法
@/.babelrc
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
```
### 改造    @/App.js
```
import React from 'react';
import RouterView from '@/routers'
import { BrowserRouter as Router } from 'react-router-dom'
import routes from '@/routers/routes'
import Store from '@/store'
import { Provider } from 'react-redux'

function App() {
  return (
    <div className="App">
      <Provider store={Store}>
        <Router>
          <RouterView routes={routes} />
        </Router>
      </Provider>
    </div>
  );
}

export default App;

```

### 改造    @/index.js
```
import '@/scss/common.scss'
import '@/scss/index.scss';
```
### 配置    @/store
@/store/index.js
```
import { createStore, applyMiddleware, combineReducers } from 'redux'
import thunk from 'redux-thunk'
import reducers from './reducer'
export default createStore(
    combineReducers({ reducers }),
    applyMiddleware(thunk)
)

```

@/store/action.js  抛出的是一个对象{} 

且异步需要手动调用dispatch方法

同步不需要dispatch提交 会自动提交

```
import axios from 'axios'

export default {
    //★ ★ ★ ★ ★ ★
    getdata: url => dispatch =>
        axios.get(url).then(res => {
            dispatch({ type: 'getdata', res: res.data })
        }),
    //★ ★ ★ ★ ★ ★
    add: num => ({ type: 'add', num: num + 1 }),
    dis: num => ({ type: 'dis', num: num - 1 })
}
```

@/store/reducer.js
```
const defaultState = {
    num: 1,
    list: []
}
export default (state = defaultState, action) => {
    console.log(state, action)
    switch (action.type) {
        case 'add':
            console.log(state, action)
            return { ...state, num: action.num }

        case 'dis':
            return { ...state, num: action.num }

        case 'getdata':
            return { ...state, list: [...action.list] }

        default:
            return state
    }
}
```

### 配置    @/routes

@/router/index.js
```
import { Switch, Redirect, Route } from 'react-router-dom'

import React from 'react'

const RouterView = ({ routes }) => {
    return <Switch>
        {
            routes.map((item, index) => {
                return item.path
                    ? <Route key={index} path={item.path} render={(props) =>
                        item.children
                            ? <item.component>
                                <RouterView routes={item.children} {...props} />
                            </item.component>
                            : <item.component {...props} />
                    } ></Route>
                    : <Redirect key={index} {...item} />
            })
        }
    </Switch>
}

export default RouterView 
```

@/router/routes.js
```
import Login from '@/containers/login'
import Home from '@/containers/home'
import LoginList from '@/containers/login/list'

export default [
    {
        path: '/login',
        component: Login,
        children: [
            {
                path: '/login/list',
                component: LoginList,
            }
        ]
    },
    {
        path: '/home',
        component: Home
    },
    {
        from: '/',
        to: '/home'
    }
]
```

