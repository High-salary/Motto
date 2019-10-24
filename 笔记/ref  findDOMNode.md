### react预留了获取DOM的方法 ref  findDOMNode

#### ref 

>ref加在react元素上时，获取的是真实的DOM节点 

ref的值有两种数据类型：

```
<h1 ref="title"></h1>

<h1 ref={(dom) =>{
    if(dom){
        console.log(dom}
    }
}></h1>

获取this.refs.title

```

>ref加在组件上时，获取的是组件实例  

ref的值有两种数据类型：

```
<Child ref="child"/>

<Child ref={(component) =>{
    if(component){
        console.log(component)
    }
}}/>

获取this.refs.child

```
状态改变时，会触发两次，第一次是null，第二次是真实的节点

需要优化一下，判断存在时，再操作

---

#### findDOMNode  

**功能：接收组件实例，返回该组件对应的DOM节点**

import {findDOMNode} from 'react-dom'  //导入该方法

componentDidMount(){
    let ruleDom = findDOMNode(this)
}

---

#### unmountComponentAtNode

**功能：卸载由React加载的所有组件，需要提供一个DOM容器**

import {unmountComponentAtNode} from 'react-dom'

unmountComponentAtNode(document.querySelector('#root')

注意：container必须是我们挂载react应用的dom节点；

---

#### children

```
<Child>
    <h1>hello</h1>
</Child>

在子组件可以通过：this.props.chidlren来获取

```













