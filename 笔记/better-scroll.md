 
#### react 中使用better-scroll 
cdm周期
```
componentDidMount() {
    //获取子组件的元素节点
    const eleScroll = this.refs.bscroll.refs.bscroll
    //实例化
    this.state.scroll = new BScroll(eleScroll, {
        pullDownRefresh: {
            threshold: 50,
            stop: 20
        },
        pullUpLoad: {
            threshold: 50
        }
    })
    //监听下拉事件
    this.state.scroll.on('pullingDown', () => {
        //数据请求替换
        this.newpagelist()
        setTimeout(() => {
            this.state.scroll.finishPullDown()
        }, 1000);
    })
    //监听
    this.state.scroll.on('pullingUp', () => {  
        // connect 拼接数据
        this.addpagelist()
        setTimeout(() => {
            this.state.scroll.finishPullUp()
        }, 1000);
    })
}
```
cdu周期
```
componentDidUpdate(prevProps, prevState) {
    this.state.scroll.refresh()
}
```



#### Tab 切换+bscroll
需要用到：react-bscroll 插件
```
import React, { Component } from 'react';
import axios from 'axios';
import BScroll from 'react-bscroll';
export class scroll extends Component {
    state = {
        list: [], //分类数据
        detail: [],//scroll 详情数据
        actIndex: 1,
        page: 1,//数据分页
        pagesize: 3,//数据条数
        detailUid: 1001,//分类数据的检索依据
    }
    render() {
        const { list, actIndex, detail } = this.state
        // nav 组件
        const Title = () =>
            list.length && list.map((item, index) =>
                <li key={index}
                    className={actIndex === index ? 'act' : ''}
                    // tab切换
                    onClick={() => this.getTite(item, index)}>
                    {item.title}
                </li>)
        // bscroll 内容渲染
        const ScrollList = () =>
            detail.map((item, index) =>
                <li key={index} >{item.list}</li>
            )

        return (
            <>
                <div className="title">
                    <Title />
                </div>
                <div className="scroll-container">
                    {/* 父元素需设置相对自身定位以及边界尺寸 */}
                    {
                        //BScrol  自带 w100p, h100p, position:absolute, bg-white
                        <BScroll ref='scroll' style="background:red"
                            pullDownRefresh//开启下拉刷新
                            pullUpLoad //开启上拉加载
                            scrollbar={true}//显示滚动条
                            doPullDownFresh={this.newpage}
                            pullUpLoadMoreData={this.nextpage}
                        // 跟多详细配置见 https://www.npmjs.com/package/react-bscroll
                        >
                            <ScrollList />
                        </BScroll>
                    }
                </div>
            </>
        )
    }
    componentDidMount() {
        //获取分类数据
        axios.get('/exam').then(res =>
            this.setState({ list: res.data.result }, () => {
                this.detail()
            })
        )
        // 实例化scroill
        this.scrollObj = this.refs.scroll.getScrollObj()
    }
    getTite = (item, index) => {
        this.setState({
            actIndex: index,
            detailUid: item.uid,
            page: 1,
            detail: []
        }, () => {
            this.detail(item.uid)
        })
    }
    detail = (flag = false) => {
        // scroll 默认数据拼接
        let { page,
            pagesize,
            detail,
            detailUid } = this.state
        axios.get(`/exam/${detailUid}/${page}/${pagesize}`)
            .then(res => {
                if (res.data.result.length == 0)
                    return alert('没有更多数据l');
                detail = flag
                    ? [...res.data.result]
                    : [...detail, ...res.data.result]
                this.setState({ detail })
            })
    }
    //改装成promis插件的 回调需要  数据替换
    newpage = async () => {
        const { page } = this.state
        this.setState({ page: page + 1 }, () => {
            this.detail(true)//传 true 表示数据替换
        })
    }
    //改装成promis插件的 回调需要  数据拼接
    nextpage = async () => {
        const { page } = this.state
        this.setState({ page: page + 1 }, () => {
            this.detail()
        })
    }
}
export default scroll


```
