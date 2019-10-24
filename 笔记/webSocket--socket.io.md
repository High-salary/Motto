#### public/index.html
```
<!doctype html>
<html>

<head>
    <title>Socket.IO chat</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font: 13px Helvetica, Arial;
        }
        form {
            background: #000;
            padding: 3px;
            position: fixed;
            bottom: 0;
            width: 100%;
        }
        form input {
            border: 0;
            padding: 10px;
            width: 90%;
            margin-right: .5%;
        }
        form button {
            width: 9%;
            background: rgb(130, 224, 255);
            border: none;
            padding: 10px;
        }
        #messages {
            list-style-type: none;
            margin: 0;
            padding: 0;
        }
        #messages li {
            padding: 5px 10px;
        }
        #messages li:nth-child(odd) {
            background: #eee;
        }
    </style>
</head>

<body>
    <ul id="messages"></ul>
    <button id="btn1">加入群聊1</button>
    <form>
        <input id="m" autocomplete="off" />
        <button>Send</button>
    </form>

</body>
<script src="https://code.jquery.com/jquery-1.11.1.js"></script>
<script src="/socket.io/socket.io.js"></script>

<script>
    var socket = io();

    btn1.onclick = () => {
        socket.emit('group1')
    }

    $('form').submit(function (e) {
        e.preventDefault(); // prevents page reloading
        // socket.emit('chat message', $('#m').val());
        // $('#m').val('');
        // return false;

        socket.emit('group1-talk', $('#m').val())

    });
    socket.on('group1-talk1', data => {
        console.log(data)
    })
    socket.on('all', el => {
        console.log(el, 'emit')
        $('#messages').append(`<li>${el}</li>`)
    })
    socket.on('message', (el) => {//与send方法相对应
        console.log(el, 'message')
    })

</script>

</html>


```

#### server.js ||  app.js

```

var app = require('express')();
var http = require('http').createServer(app);
var io = require('socket.io')(http);

app.get('/', function (req, res) {
    res.sendFile(__dirname + '/public/index.html');
});
const count = 0
io.on('connection', function (socket) {
    // console.log(socketid)
    // ++count
    // console.log(count)
    console.log('a user connected');
    socket.on('reconnect', function () {
        console.log("重新连接到服务器");
    });
    socket.on('group1', function (data) {
        socket.join('group1');
        console.log('join to group1')
        // console.log(io.sockets.clients('group1'))
        // console.log(io.sockets.manager.group1)
    });
    socket.on('group1-talk', data => {
        socket.broadcast.to('group1')
            .emit('group1-talk', data);
        //不包括自己
        // socket.broadcast.to('group1').emit('group1-talk1', data);
        //包括自己
        io.sockets.in('group1').emit('group1-talk1', data);
    })
    socket.on('disconnect', function () {
        console.log('user disconnected');
    });
    socket.on('chat message', function (msg) {
        console.log('message: ' + msg);
        // socket.send(msg)
        socket.broadcast.emit('all', msg)//发给所有人除了自己
        io.sockets.emit('all', msg)//发给所有

    });
});
// 模拟QQ聊天的模式

// 固定的昵称
// 指定对象聊天
// 一对一

// 群组聊天
// 一对多

// 判断同一用户上线与离线  用户的唯一性
// 群聊之中显示在线与否?

// 增加离线缓存         上线推送
// 分为单独用户的的聊天
// 和群聊的处理

// 消息特别多的时候   默认接收100条？  
// 其他的靠上拉加载？   上线之后拉取？？
http.listen(3000, function () {
    console.log('listening on *:3000');
});

```