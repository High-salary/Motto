### 组件的通讯
>1 父 ==> 子  props
```
let {Component}=React
class Son extends Component{
    constructor(props){
        super(props)
       // this.change=this.change.bind(this)
    }
    render(){
        this.props.参数

        return <div onClick={
           // this.change.bind(this,...arguments)
           // ()=>{ this.change('123')  //用箭头函数传参}
           }></div>
    }
    change=()=>{

    }
}
```
- state
- 



插槽  
    组件的插槽里的内容显示在{this.props.children}中








