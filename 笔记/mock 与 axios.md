```
Mock.mock('/getList',[1,2,3,4,5,6])
axios.get('/getList').then(({data})=>console.log(data))
```