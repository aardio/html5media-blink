# HTML5Media for web.blink

[HTML5Media](https://github.com/etianen/html5media) web.blink( Miniblink 内核 ) 扩展库专用版。

相关文章：[《调用各种浏览器控件播放视频》](https://mp.weixin.qq.com/s/p3_Fz25MlHKgdURrHk9FlA)

示例：
```javascript
import web.blink.form;
import web.npPlugin.flash;
import wsock.tcp.asynHttpServer;
import win.ui;
/*DSG{{*/
var winform = win.form(text="web.blink 支持 HTML5 视频（基于 Flash）";right=1008;bottom=616)
/*}}*/

var httpServer = wsock.tcp.asynHttpServer(); 
httpServer.run( {
	["/index.html"] = /**
<html>
<head>
    <style type="text/css">
        html,body { height: 100%; width: 100%; margin: 0;overflow: hidden; }
    </style>
    <script src="https://cdn.jsdelivr.net/gh/aardio/html5media-blink@main/html5media.min.js"></script>
</head>
<body>
    <video src="https://media.html5media.info/video.mp4" width="100%" height="100%" style="height: 100%; width: 100%"
        controls preload></video>
</body>
</html>
**/;
}); 

//通过HTTP服务器 Flash 才能访问网络，否则会有警告对话框。
//aardio 仅用数句代码就可以启动一个嵌入式HTTP服务器，不会增加软件体积。
var mb = web.blink.form( winform );
mb.go( httpServer.getUrl("index.html") );

winform.show(); 
win.loopMessage();
```

如果使用 web.kit 扩展库，可以直接支持原版 HTML5Media，有 2 点要特别注意：  
1、web.blink 要加载我修改后的 html5media-blink, web.kit 加载原版 html5media 就可以了。  
2、web.blink 里 video 标签一定要在属性里指定大小，`width="100%" height="100%"` 然后在 style 属性里重新指定一次大小，也就是在video 节点添加属性 `style="height: 100%; width: 100%"`, 而且这个 style 一定要写到节点里，不然放大缩小视频的大小不会改变，web.kit 不需要添加这些。  

```javascript
import web.kit.form;
import web.npPlugin.flash;
import wsock.tcp.asynHttpServer;
import win.ui;
/*DSG{{*/
var winform = win.form(text="web.kit 支持 HTML5 视频（基于 Flash）";right=1008;bottom=616)
/*}}*/

var httpServer = wsock.tcp.asynHttpServer(); 
httpServer.run( {
	["/index.html"] = /**
<html>
<head>
    <style type="text/css">
        html,body { height: 100%; width: 100%; margin: 0;overflow: hidden; }
    </style>
    <script src="https://cdn.jsdelivr.net/npm/html5media@1.2.1/dist/api/1.2.1/html5media.js"></script>
</head>
<body>
    <video src="https://media.html5media.info/video.mp4" width="100%" height="100%"
        controls preload></video>
</body>
</html>
**/;
}); 

//通过HTTP服务器 Flash 才能访问网络，否则会有警告对话框。
//aardio 仅用数句代码就可以启动一个嵌入式HTTP服务器，不会增加软件体积。
var mb = web.kit.form( winform );
mb.go( httpServer.getUrl("index.html") );

winform.show(); 
win.loopMessage();
```

再赠送一个用自带 Flash 插件播放 Flash 动画的例子：

```javascript
import win.ui;
/*DSG{{*/
var winform = win.form(text="Flash 动画";right=1008;bottom=616)
/*}}*/

import web.npPlugin.flash;
import web.kit.form;
var wke = web.kit.form( winform  );

wke.html = /**
<embed quality=high bgcolor=#FFFFFF width="550"
    height="400" id="flash" type="application/x-shockwave-flash">
</embed>
<script>
loadSwf = function(url){
	document.getElementById("flash").LoadMovie(0,url)
}
</script>
**/

wke.wait();
wke.script.loadSwf("https://update.aardio.com/v10.files/demo/transparent.swf")

winform.show() 
win.loopMessage();
```


