### bootstrap  02

##### btn 按钮类

    按钮类可用于 <a>, <button>, 或 <input> 元素上:
    btn-color           背景颜色    自动设置合适的字体颜色
    btn-outline-color   外边框颜色 
    btn-lg              大号按钮
    btn-sm              小号按钮
    btn-block           块级按钮    独占父元素宽度的大按钮
    active              可用按钮
    disabled            禁用的按钮  button按钮是属性
                                    a 标签是设置 .disabled 类
##### badge 徽章类

    就像一个不能点击的按钮类
    badge badge-color   设置背景色和对应的字体颜色
    badge badge-pill    徽章自带圆帽 类似于胶囊形状

##### progress 进度条  默认高度16px 子元素需加上高度

    一个 progress 父类代表一个进度条
    一个 progress 父类可以有多个子类  自动分为多段  
    *
    父元素加类      progress
    子元素加类      progress-bar
        并且设置子元素 style 宽度    style="width:70%"
        设置子元素的背景类    bg-color
    条纹进度条
        子元素加类  progress-bar-striped    [straɪpɪd]
    动画进度条加类  progress-bar-animated
        
    <div class="progress">
      <div class="progress-bar bg-danger progress-bar-striped" style="width:70%"></div>
    </div>

##### pagination 分页 

    默认蓝色和白色搭配
    默认可以点击 
    父类加 pagination
    子类加 page-item    配合 active 类  可以高亮显示
                        设置 disabled   之后分页不可点击
    pagination-sm  小号的分页条目
    pagination-lg  大号的分页条目

#####   面包屑导航  样式类似于多级路由接口

    父级    breadcrumb
    子集    breadcrumb-item

####  卡片

    父类    card
    子类    card-header
    子类    card-body
    子类    card-footer
###### 图片卡片

    img当作背景图时 子类加 card-img-overlay
    
