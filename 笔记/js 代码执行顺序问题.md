```

const error = new Error()
console.log(error);//执行步骤   1

const waitPromise = new Promise((resolve) => {
    console.log('promise-------------------------------------------start')
    //  执行步骤   2
    let promise = new Promise((res, rej) => {
        throw Error
    })
    promise.then(res => console.log(res))
        .catch(err => {
            console.log(err, 'err-----------------------------for catch')
            // 执行步骤   5  //ƒ Error() { [native code] } "err-----------------------------for catch"
        })
})

console.log('asdfda--------')//执行步骤  3

throw Error////执行步骤   4  报错 Uncaught f Error(){[native code]}
// 阻断之后的所有代码

console.log('asdfda+++++++++++++')//不执行  同步代码的报错会阻断之后的代码（同步和异步的）的编译和执行
// 但是不能阻断已经执行的异步代码的执行

setTimeout(() => console.log(111111111), 1000)//不执行

```