前端小知识 
============


# 事件

## event bubbling

![事件模型](http://images.cnitblog.com/blog/477973/201302/18141423-8bd09a9c1e184df9a13b6e26b88348f3.jpg)

#### 监听事件 

* `W3C Model` addEventListener( type, listener, useCapture);
* `MS Model` attachEvent( type , listener )

#### 事件目标

* `W3C Model` event.target
* `MS Model` event.srcElement

#### 阻止事件

* `W3C Model` event.stopPropagation()
* `MS Model` event.cancelBubble = true

#### 阻止默认事件(browser action)

之所以需要它，是因为默认事件(browser action)是发生在事件触发(click action)之前

* `W3C Model` event.preventDefault()
* `MS Model` event.returnValue = false


## event loop

```javascript

	console.log(1)
	setTimeout(function(){
		console.log(2)
	},0);	
	
	function a(x) {
		console.log(3);
		b(x);
		console.log(4);
	}
	
	function b(y) {
		console.log(5);
	}
	
	console.log(6);
	a(42);
	console.log(7);
	
```

## bind , delegate , live , on 事件绑定的四种方法

![bind](http://images.51cto.com/files/uploadimg/20110317/1603571.png)


	```javascript
	// Bind
	$( "#members li a" ).on( "click", function( e ) {} ); 
	$( "#members li a" ).bind( "click", function( e ) {} ); 

	// Live
	$( document ).on( "click", "#members li a", function( e ) {} ); 
	$( "#members li a" ).live( "click", function( e ) {} );

	// Delegate
	$( "#members" ).on( "click", "li a", function( e ) {} ); 
	$( "#members" ).delegate( "li a", "click", function( e ) {} );
	```

# 浏览器是如何工作的?

## 结构

![browser](http://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/layers.png)

## URL是怎么被浏览器处理的

network timeline

![timeline](https://developer.chrome.com/devtools/docs/network-files/timeline-view-hover.png)

## 浏览器如何解析HTML

主过程

![main](http://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/flow.png)

* Rendering engine 解析 HTML 并生成 DOM树
* Rendering engine 解析外部 CSS 文件以及样式元素中的样式数据生成 Render树
* 将 Render树 的每个节点，为其在屏幕分配确切坐标，这个过程叫 Layout 或 Reflow
* 显示 Render对


Webkit是如何处理主流程的

![maindetail](http://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/webkitflow.png)

* 对于连接DOM节点和可视信息并创建Render Tree的过程，Webkit称之为 Attachment



# website

* http://www.quirksmode.org/compatibility.html 
* http://www.w3.org/TR/
* http://javascript.info/root


