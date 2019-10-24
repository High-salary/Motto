####  * generator  三种执行状态
suspended   暂停态      待执行状态

running     执行态

closed      结束态       执行完毕状态

##### 自动将一个generator函数执行完
类似于async await 的方式将一个函数执行完

```
while (!gen.next().done) {}
        
```

```
function* outer() {
    console.log('start')
    yield 'open'
    yield inner()
    yield 'closeeeeeee'
    console.log('end')
}

function* inner() {
    yield 'hello!'
}

var gen = outer()
// 当执行到outer 函数内部时变为running 态
console.log(gen, 'gen')     //outer {<suspended>}       待执行态
//  start

console.log(gen.next())     // -> {value: "open", done: false}
console.log(gen, 'gen')     //outer {<suspended>}       待执行态
console.log(gen.next())     // -> {value: inner, done: false}
console.log(gen, 'gen')     //outer {<suspended>}       待执行态
console.log(gen.next())     // -> {value: "closeeeeeee", done: false}
console.log(gen, 'gen')     //outer {<suspended>}       待执行态
// end

console.log(gen.next())     // -> {value: undefined, done: true}
console.log(gen, 'gen')     //outer {<closed>} "gen"    结束态
console.log(gen.next())     // -> {value: undefined, done: true}
console.log(gen, 'gen')     //outer {<closed>} "gen"    结束态

```

next 方法

一般情况下，next 方法不传入参数的时候，yield 表达式的返回值是 undefined 。当 next 传入参数的时候，该参数会作为上一步yield的返回值。


实现 Iterator

为不具备 Iterator 接口的对象提供遍历方法。

```
function* objectEntries(obj) {
    const propKeys = Reflect.ownKeys(obj);
    for (const propKey of propKeys) {
        yield [propKey, obj[propKey]];
    }
}
 
const jane = { first: 'Jane', last: 'Doe' };
for (const [key,value] of objectEntries(jane)) {
    console.log(`${key}: ${value}`);
}
// first: Jane
// last: Doe

```

Reflect.ownKeys() 返回对象所有的属性，不管属性是否可枚举，包括 Symbol。

jane 原生是不具备 Iterator 接口无法通过 for... of遍历。这边用了 Generator 函数加上了 Iterator 接口，所以就可以遍历 jane 对象了。



#### async await
```
