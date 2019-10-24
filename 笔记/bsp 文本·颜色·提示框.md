##### 引用
    脚手架里面  主入口js文件里面引入

##### 盒模型
    父元素  row类
    子元素  col类   col-n   n/12
##### 文本设置
    .text-color
    .small  指定更小文本 (为父元素的 85% )
    
###### 文本属性

    .text-left
    .text-center
    .text-right
    .text-justify
    .text-nowrap
###### 文本颜色

    .text-muted                         柔和的文本
    .text-primary       蓝色的文本      重要的文本
    .text-success       绿色的文本      成功的文本
    .text-info                          提示信息
    .text-warning       橘黄色文本      警告文本
    .text-danger        红色文本        危险操作文本
    .text-secondary     中灰色
    .text-dark          深灰色文本      
    .text-light         浅灰色文本
    .text-white         白色文本

###### 背景颜色

    bg-primary          重要的背景颜色      蓝色
    bg-success          执行成功背景颜色    绿色
    bg-info             信息提示背景颜色
    bg-warning          警告背景颜色        橘黄色
    bg-danger           危险背景颜色        红色
    bg-secondary        副标题背景颜色
    bg-dark             深灰背景颜色
    bg-light            浅灰背景颜色

##### 图片设置
    圆角(四角)      rounded 
    椭圆图片        rounded-circle
    带间距的边框    img-thumbnail
    自适应图片      img-fluid  
                    === max-width:100%;height:auto
                    
##### 提示框

###### 提示框可以使用 .alert 类, 后面加上--类来实现:
    
    .alert-success, 
    .alert-info, 
    .alert-warning, 
    .alert-danger, 
    .alert-primary, 
    .alert-secondary, 
    .alert-light 
    .alert-dark 
###### 关闭提示框
    我们可以在提示框中的 div 中添加 .alert-dismissible 类，然后在关闭按钮的链接上添加 class="close" 和 data-dismiss="alert" 类来设置提示框的关闭操作。
###### 提示框动画
    .fade 和 .show 类用于设置提示框在关闭时的淡出和淡入效果