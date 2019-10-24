#### react生命周期

参考链接：https://segmentfault.com/a/1190000016617400

**描述组件一系列成长过程就叫生命周期**。

当我们把组件挂载到页面的时候，那么该组件的生命周期正式开始，直到该组件被卸载，它的生命周期才结束。在这个生命周期里，我们可以调用一些钩子函数。

#### 生命周期分三个阶段：

1>创建阶段（出生阶段）

    1)constructor(){}         初始化函数，接收props，定义默认的状态    触发一次
    
    2)componentWillMount(){}  组件将被渲染          触发一次    **将废弃**
    
    3)render(){}              组件正在渲染          触发多次
    
        在render内会进行diff算法，比较最小化差异，更新DOM节点
    
    4)componentDidMount(){}   组件初始化渲染完成    触发一次  在这发ajax请求

2>变化阶段（成长阶段） 

    1)state发生变化
    
        shouldComponentUpdate(nextProps,nextState){}  组件应该被更新吗？
        
        必须要有返回值，true/false，true：触发组件更新  false:终止组件更新
        
        用途：一般用来做优化，如果两次的state或props一样，不需要进行diff算法
    
        componentWillUpdate(nextProps,nextState){}   组件将要更新前   **将废弃**
        
        render(){}
        
        componentDidUpdate(prevProps,prevState){}    组件更新完成
        
    2)props发生变化
    
        componentWillReceiveProps(nextProps){}  组件将要接收最新的props  **将废弃**
        
        shouldComponentUpdate(nextProps,nextState){}  组件应该被更新吗？
        
        必须要有返回值，true/false，true：触发组件更新  false:终止组件更新
        
        用途：一般用来做优化，如果两次的state或props一样，不需要进行diff算法
    
        componentWillUpdate(nextProps,nextState){}   组件将要更新前   **将废弃**
        
        render(){}
        
        componentDidUpdate(prevProps,prevState){}    组件更新完成    
    

3>卸载阶段（死亡阶段）

    componentWillUnMount(){}  卸载组件   清除定时器和添加方法
    
#### 即将废弃的生命周期

componentWillMount  componentWillUpdate  componentWillReceiveProps

<html>
<img src="https://note.youdao.com/yws/public/resource/b1e96bb2fdece2915057c3bab3664f72/xmlnote/D9A3DF7FE6914A588DB6754477AA27BF/28520" width="700"/>
</html>



#### 新旧生命周期不能同时使用

新生命周期：

创建阶段：

constructor(){}

static getDerivedStateFromProps(){}

render(){}

componentDidMount(){}

成长阶段：

**static getDerivedStateFromProps(){}  //componentWillMount **

    更新state状态，监听props的变化
    
    该函数的返回值为setState的第一个参数，或者是null（不会更新state）

shouldComponentUpdate(){}

render(){}

**getSnapshotBeforeUpdate(){}  ** 组件更新完成前执行，必须要有返回值，其为componentDidUpdate的第三个参数

<!--第一次渲染的过程，preProps可能为null， 所以要对preProps做 if-not-null判断-->
<!--不需要保存preProps为react未来版本降低内存做好准备-->

注：必须配合componentDidUpdate使用

componentDidUpdate(){}

卸载阶段：

