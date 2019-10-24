 ```
function fn() {
    console.log(new.target === fn)
}
fn()//false
new fn()//true

/* 函数是如何被调用的，在其中加入console.trace()方法就可以了   报错 */
function add(a, b) {
    console.trace();
    return a + b;
}
function one(a, b) { return add(a, b); }
function two(a, b) { return add(a, b); }
function three(a, b) { return add(a, b); }
three(1, 10)
```