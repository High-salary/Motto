服务端返回的字符串分为普通的字符串和JSON 字符串
分别对应res.text()
 res.json()
 的解析方式

 ```
 fetch('http://localhost:3000')
    .then(res => res.text())
    .then(res => {
        console.log(res);
    })
fetch('http://localhost:3000')
    .then(res => res.json())
    .then(res => {
        console.log(res);
    })
```