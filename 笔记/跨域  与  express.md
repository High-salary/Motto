## 跨域

```
const formateOpt = params => {
	let temp = '';
	for (let key in params) {
		temp += `${key}=${params[key]}&`;
	}
	return temp;
};
const defaultConfig = {
	callbackName: 'jsonpCallback'
};
const jsonp = (url, params, options = defaultConfig) => {
	// 返回一个可以链式调用的promise对象
	return new Promise((resolve, reject) => {
		const hasParams = url.indexOf('?') > -1;
		//将前端传递querystring查询字符串的参数，拼接到地址栏
		url += hasParams
			? formateOpt(params, options.callbackName)
			: '?' + formateOpt(params, options.callbackName);
		url += `callback=${options.callbackName}`;
		//动态创建script标记
		const script = document.createElement('script');
		//设置接口的请求地址
		script.setAttribute('src', url);
		//设置请求jsonp接口的回调函数
		window[options.callbackName] = result => {
			//请求jsonp接口成功后，删除该函数 - 不污染window
			delete window[options.callbackName];
			//从页面中删除请求接口动态创建的script标记
			document.body.removeChild(script);
			//判断接口的数据返回
			if (result) {
				resolve(result);
			} else {
				reject('服务器没有返回信息');
			}
		};
		//动态创建script标记，错误的监听
		script.addEventListener('error', () => {
			delete window['jsonpCallback'];
			document.body.removeChild(script);
			reject('服务器加载失败！');
		});
		document.body.append(script);
	});
};
function postDataFormat(obj) {
	if (typeof obj != 'object') {
		alert('输入的参数必须是对象');
		return;
	}
	// 支持有FormData的浏览器（Firefox 4+ , Safari 5+, Chrome和Android 3+版的Webkit）
	if (typeof FormData == 'function') {
		var data = new FormData();
		for (var attr in obj) {
			data.append(attr, obj[attr]);
		}
		return data;
	} else {
		// 不支持FormData的浏览器的处理
		var arr = new Array();
		var i = 0;
		for (var attr in obj) {
			arr[i] =
				encodeURIComponent(attr) + '=' + encodeURIComponent(obj[attr]);
			i++;
		}
		return arr.join('&');
	}
}
const request = (url, params = null, type = 'GET') => {
	return new Promise((resolve, reject) => {
		const xhr = new XMLHttpRequest();
		xhr.open(type, url);
		if (type === 'GET' && params) {
			// xhr.setRequestHeader('Content-type', 'application/json');
			const hasParams = url.indexOf('?') > -1;
			//将前端传递querystring查询字符串的参数，拼接到地址栏
			url += hasParams ? formateOpt(params) : '?' + formateOpt(params);
			xhr.send(null);
		} else if (type === 'POST') {
			xhr.setRequestHeader(
				'Content-type',
				'application/x-www-form-urlencoded'
			);
			xhr.send(postDataFormat(params));
		}

		xhr.onreadystatechange = () => {
			if (xhr.readyState !== 4) return;
			if (xhr.status === 200) {
				resolve(JSON.parse(xhr.responseText));
			} else {
				reject(new Error('request failed!'));
			}
		};
	});
};

```



```
var createError = require('http-errors');
var express = require('express');
var path = require('path');
var cookieParser = require('cookie-parser');
var logger = require('morgan');
var bodyParser = require('body-parser');

var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');
var url = require('url');
var querystring = require('querystring');
var app = express();

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'jade');

app.use(logger('dev'));
app.use(express.json());
// app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', indexRouter);
app.use('/users', usersRouter);

app.post('/postApi', bodyParser.urlencoded({ extended: false }), (req, res) => {
	res.writeHead(200, {
		'Access-Control-Allow-Origin': '*'
	});
	res.end(
		JSON.stringify({
			code: 1,
			type: 'post'
		})
	);
});
app.get('/api', (req, res) => {
	const oUrl = url.parse(req.url);
	const { callback } = querystring.parse(oUrl.query);
	const formatJsonp = (callbackName, params) => {
		return callbackName + '(' + JSON.stringify(params) + ')';
	};

	//判断是不是jsonp接口
	if (callback) {
		//设置响应状态码、响应类型
		res.writeHead(200, {
			'Content-Type': 'text/javascript'
		});
		const result = { code: 1, type: 'jsonp' };
		const data = formatJsonp(callback, result);
		// res.end(callback + '(' + JSON.stringify({ code: 1 }) + ')');
		res.end(data);
	} else {
		res.writeHead(200, {
			'Access-Control-Allow-Origin': '*'
		});
		const json = {
			code: 1,
			type: 'json'
		};
		res.end(JSON.stringify(json));
	}
});
// catch 404 and forward to error handler
app.use(function (req, res, next) {
	next(createError(404));
});

// error handler
app.use(function (err, req, res, next) {
	// set locals, only providing error in development
	res.locals.message = err.message;
	res.locals.error = req.app.get('env') === 'development' ? err : {};

	// render the error page
	res.status(err.status || 500);
	res.render('error');
});

module.exports = app;

```