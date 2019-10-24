## vue3.0   配置别名
根目录  vue.config.js

```
const path = require('path');
function resolve(dir) {
    return path.join(__dirname, dir)
}
module.exports = {
    lintOnSave: true,
    chainWebpack: (config) => {
        config.resolve.alias
            .set('@', resolve('src'))
            .set('assets', resolve('src/assets'))
            .set('components', resolve('src/components'))
            .set('layout', resolve('src/layout'))
            .set('base', resolve('src/base'))
            .set('static', resolve('src/static'))
            .set('views', resolve('src/views'))
    }
}
```


#### 路由配置
```
mode: 'history',
//  代码分割与按需加载
component: () => import(/* webpackChunkName: "about" */ './views/About.vue'),




//  路由出口
<router-view />
```


## vuex 使用
```
//  辅助函数会将这些方法绑定到this上
computed: {
    ...mapState(["list"])
},
methods: {
    ...mapActions(["getdata"]),
    ...mapMutations(["add", "dis"]),
}
//  调用时将会如下  

//  可以不用 this.$store.commit("mutationName", payload)

//  改用  this.mutationName(payload) 的方式
methods:{
    diss(i, list) {
        list[i]--;
        console.log("dis");
        // this.$store.commit("dis", list);
        this.dis(list);
    },
    addd(i, list) {
          list[i]++;
          console.log("add");
          this.add(list);
          // this.$store.commit("add", list);
    }
}

```
定义mutations和actions
```
mutations: {
    add(state, payload) {
        console.log(state, payload)
        state.list = [...payload]
    },
    dis(state, payload) {
        console.log(state, payload)
        state.list = [...payload]
    },
    changelist(state, payload) {
        state.list = payload
    }
},
actions: {
    getdata({ commit }) {
          setTimeout(() => {
                commit('changelist', [1, 99, 999])
          }, 1000);
    }
}
getters:{
    id: state => id => state.list.find(item=>item.id>id),
    done: state => state.list.filter(item=>item.bool)
}
```