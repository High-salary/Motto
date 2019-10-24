```
p {
  width: 200px;
  height: 200px;
}
```

#### 饼图 
```
p {
  /* background: conic-gradient(red, yellow, blue, red); */
  background: conic-gradient(red 0 30%, yellow 30% 60%, blue 60% 99%);
}
```
#### 居中 
```
p.parent {
  display: flex;
}
p.child {
  margin: auto;
}
```
#### 外边框   ---矩形 
```
p {
  outline: 5px dashed darkblue;
  outline-offset: -20px;
}
```
#### 背景虚化 
```
p {
  filter: blur(1px);
}
```
####     禁用标识 
```
p:disabled {
  cursor: not-allowed;
}
```
####     清除浮动对文档流的影响
```
p {
  display: flow-root;
}
```
#### 图片是否变形充填 
```
img {
  object-fit: contain;
  object-fit: cover;
  object-fit: fill;
}
```
#### 多列等高布局
```
p.parent {
  display: table;
}
p.children {
  display: table-cell;
}
```
