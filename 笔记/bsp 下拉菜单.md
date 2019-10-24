##### 折叠实现手风琴
##### 导航实现 tab 切换  动态(淡入淡出)选项卡
    
    fixed-top       固定在头部
    fixed-bottom    固定在底部


#### dropdown下拉菜单  
##### 点击展开 菜单项可以点击  但是不可以选中
##### 类似于一个展示菜单

    dropdown 类用来指定一个下拉菜单。
    我们可以使用一个按钮或链接来打开下拉菜单， 按钮或链接需要添加 .dropdown-toggle 和 data-toggle="dropdown" 属性。
    <div> 元素上添加 .dropdown-menu 类来设置实际下拉菜单，然后在下拉菜单的选项中添加 .dropdown-item 类。
###

    <div class="dropdown">
        <button type="button" class="btn btn-primary dropdown-toggle" data-toggle="dropdown">
            Dropdown button
        </button>
        <div class="dropdown-menu">
            <a class="dropdown-item" href="#">Link 1</a>
            <a class="dropdown-item" href="#">Link 2</a>
            <a class="dropdown-item" href="#">Link 3</a>
        </div>
    </div>
    

### 折叠 展开 收起  callapse类

    .show   默认显示
    data-parent 属性可以很好的实现手风琴效果(点击展开收起的切换)
    
### 导航

    父类    nav
    子类    nav-item
    字字类  nav-link    实现上下居中  也可以加到子类上 效果一样
    默认从左到右
    nav-justified   导航等宽显示    