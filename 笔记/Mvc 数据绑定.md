### 知识点

DOM 
```
document.createDocumentFragment()
node.textContent
node.value
el.firstChild
node.nodeType

```

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=F, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <input type="text" cc-simle="name" cs-dd=asdf>
        <input type="text" cc-simle="name">
        {{name}}
        {{name}}
        <div>{{name}}</div>
    </div>
</body>

<script>
    class Module {
        constructor(el, data) {
            this.data = data
            this.nodes = []
            this.reg = /\{\{(.+?)\}\}/g
            this.el = document.querySelector(el);
            this.compiler()
        }

        bindNode(node) {
            this.nodes.push(node)
        }
        setVal(str) {//改变数据
            if (str !== this.data) {
                this.data = str
                this.update()
            }
        }
        update(node) {//更新Dom
            this.nodes.forEach(node => {
                if (node.nodeType === 1) {
                    node.value = this.data
                } else {
                    node.textContent = node.value.replace(this.reg, () => {
                        return this.data
                    })
                }
            })
        }

        compiler() {
            const { setVal, bindNode } = this
            // 文档碎片// 内存
            let fragment = document.createDocumentFragment()
            let firstNode;
            while (firstNode = this.el.firstChild) {
                fragment.appendChild(firstNode)
            }

            [...fragment.childNodes].forEach(node => {

                if (node.nodeType == 1) {//input 框
                    [...node.attributes].forEach(attr => {
                        console.log(attr)
                        let { name, value } = attr
                        if (name.startsWith('cc-')) {
                            // console.log((node))
                            this.bindNode(node)
                            node.addEventListener('input', (e) => {
                                this.setVal(e.target.value)
                            })
                        }
                    })
                }
                let content = node.textContent
                if (node.nodeType === 3 && this.reg.test(content)) {
                    console.log(node)
                    node.value = content;
                    content.replace(this.reg, () => {
                        this.bindNode(node)
                    })
                }
            })
            this.el.appendChild(fragment)
        }
    }
    const obj = new Module('#app', '白小哥')
</script>
</html>
```