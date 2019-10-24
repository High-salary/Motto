#### 复习原生事件

1.DOM0级事件

    1)只支持冒泡
    
    2)兼容性好
    
    3)只能绑定一次

```
el.onclick = function(){}
```

2.DOM2级事件

    1)支持捕获和冒泡

>事件三要素：1>事件源 2>事件类型  3>事件处理器（事件监听器）

>事件流：捕获  冒泡

el.addEventListener('click',function(){},false/true)  false:冒泡  true：捕获

<html>
<!--在这里插入内容-->
<img src="https://note.youdao.com/yws/public/resource/b1e96bb2fdece2915057c3bab3664f72/xmlnote/D9333310460E44519DA57B8056AC9A06/28175" width="400"/>
</html>

#### 一、在react中使用原生事件

```
handClick(){}

在componentDidMount(){
    //添加事件
    this.refs.title.addEventListener('click',this.handClick,false)
}

在componentWillUnMount(){
    //移除事件  防止内存泄漏
    this.refs.title.removeEventListener('click',this.handClick,false)
}
```
#### 二、合成事件的绑定方式和实现机制

在成员方法内部没有this，需要修改this指向

<html>
<!--在这里插入内容-->
<img src="https://note.youdao.com/yws/public/resource/b1e96bb2fdece2915057c3bab3664f72/xmlnote/00C66649EE4C4200881CB62B1D38D5AB/28128" width="500"/>

<img src="https://note.youdao.com/yws/public/resource/b1e96bb2fdece2915057c3bab3664f72/xmlnote/D30F27BFE94D4B1BA48C8C323405F9B8/28151" width="500"/>

<img src="https://note.youdao.com/yws/public/resource/b1e96bb2fdece2915057c3bab3664f72/xmlnote/C6644631B70443308D403F8A2CF0C263/28147" width="500"/>

<img src="https://note.youdao.com/yws/public/resource/b1e96bb2fdece2915057c3bab3664f72/xmlnote/D8F0225749334AEB871B10700217189D/28130" width="500"/>
</html>

**react并没有给每个真实的DOM节点去添加事件，而是仅仅给最顶层节点（document）添加了一个事件处理器，因为事件传递机制，在任何元素上触发的事件都会触发该事件处理器的执行。**
然后在该事件处理器内我们再根据触发的实际情况去调用真正的事件的事件处理函数。
这样的话，我们只有一个事件处理器，但是却完成了所有的任务，大大的提高了性能。

**注：react合成事件系统也实现了一套捕获和冒泡的机制。**

#### 三、react合成事件和原生事件可以混用

在react合成事件内阻止事件传递时，不会影响对原生的事件产生影响。

如果在原生事件内阻止事件的传递，则不会触发document上的事件处理器则不会触发，意味着所有的react合成事件都不会触发。

#### 四、react已经帮我们处理了事件的兼容性问题。

#### 五、获取事件对象的方式

事件处理器的第一个参数即事件对象

原生的事件对象和react的事件对象不同












