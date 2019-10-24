###     小工具
####    边框

    border  类          添加边框
    border-0            没边框
    border-top-0        没有上/右/下/左 边框
    border-color        边框颜色
    **
    rounded             倒圆角
    rounded-0           不倒圆角
    rounded-circle      正圆或者椭圆
    rounded-top         单边倒圆角
    
#####   居中(块元素)  mx-auto

##### padding类
##### margin类

    配合row类
    mr-auto     当前的元素和下一个元素的margin 的距离最大   margin-right
    ml-auto     当前的元素和上一个元素的margin 的距离最大   margin-left


#### 弹性盒布局

    父类    d-flex      d-inline-flex
            flex-row    水平从左到右排列
            flex-row-reverse   水平从右到左   类似于float的效果
            flex-column     垂直排列
            flex-column-reverse     翻转垂直排列的顺序   
            justify-content-start   水平方向的
                            end
                            center
                            between
                            around
            align-content-start
                            end
                            center
                            between
                            around
                            stretch
                            
            flex-nowrap     强制在一行显示(默认)
            flex-wrap
            flex-wrap-reverse
            
            
    子类    flex-fill   强制设置每个子元素的宽度都相同
            flex-grow-1 子元素使用剩下的空间  撑开剩余的空间
            order-number    order-12~order-1  排列顺序      越大越靠前
            
            
                            
            