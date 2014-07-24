JS基础
=========

## 保留字(reversed words) 

> http://www.ecma-international.org/ecma-262/5.1/#sec-7.6.1.2

## 数字

* 不区分精度 , 1 === 1.0 
* isNaN, 判断是否为一个非数字
* 常用的`Math`方法 http://www.w3school.com.cn/jsref/jsref_obj_math.asp

    ```javascript
        Math.floor(3.5) -->  3
        Math.round(3.5) -->  4
        Math.random()   -->  0.5169974472373724
        ( Math.floor( Math.random() * 10 ) + 1 ) --> 5
    ```

* parseInt( str , radix )， 需要写进制

    ```javascript
	    parseInt("08") //在IE下，会返回0
    ```

## 字符串

* 保持源代码为utf-8(无bom)编码，关于[BOM(Byte Order Mark)](http://atedev.wordpress.com/2007/09/19/bom-bom-bom/)
* 使用 \u 指定字符编码(16进制) , 如 \u0061
* 使用 \ 连接换行
* charAt , charCodeAt


## 对象(Object)

* 可以当成 hashtable 使用
* 使用 for in 循环，但无序

## 数组

* 可以当作栈来使用, 入栈push，出栈pop
* 删除下标 splice(start,count) 
* 截取下标 slice(start,end) 
* 连接数组 concat 
* 变为字符串 join 
* 反转 reverse 
* 排序 sort

## 正则表达式

* 替换, 简单的模板引擎

    ```javascript
    var obj = {
        name : "john" , 
        num : 999
    }
        
	"hello, #{name}! The number is #{num}. ".replace(/\#{(.*?)}/g , function($0, $1){
		return obj[$1];
	})
    ```
    
* 使用工具网站 http://regexpal.com


## 日期类型

* 如何计算时间

    ```javascript
	var d = new Date()  //获取当前时间
	d.getTime() 	    //返回距1970年1月1日之间的毫秒数
	d.getMonth() + 1    //+1后才是真正月份
	new Date(1406013266879)
	new Date("1970/01/01")
    ```


## 原型链

* 理解 

    ```javascript
    // 类: Array
    // 实例: 
    var a = new Array("a","b","c"); 
    var a = ["a","b","c"];
             
    a.push(5);
	// a ---> Array.prototype ---> Object.prototype ---> null
        	 
	a.push = function(){
	    alert('wakaka');
	}
    ```

* hasOwnProperty , 判断是否是非原型链上的属性

    ```javascript
    a.hasOwnProperty('push')  // -->  true
    ```


## 函数

* 常见的几种声明&调用方式

    ```javascript
    function A(){}; A();    		//FD(declarations), 先声明后使用
        
    var A = function(){}; a();      //FE(expressions)
        
    (function(){})(); 			    //FE(self invoke)
    ```

* 关于 this 

 `this`关键字指向上下文对象(context object)，但在不同的执行环境会发生变化

    ```javascript
    function foo() {
      alert(this);
    }
                
    foo(); // global object
    foo.prototype.constructor(); // foo.prototype
                
    var bar = {
      baz: foo
    };
                
    bar.baz(); // bar
                
    (bar.baz)(); // also bar
    (bar.baz = bar.baz)(); // but here is global object
    (bar.baz, bar.baz)(); // also global object
    (false || bar.baz)(); // also global object
                
    var otherFoo = bar.baz;
    otherFoo(); // again global object
    ```

* apply 与 call , 调用函数并可以修改上下文对象(context object)

    ```javascript
    foo.apply( bar , [ 1 , 2 , 3 ] )  // bar
    foo.call( bar , 1 , 2 , 3 )  // bar 
        
    bar.baz( 1 , 2 , 3 )
    ```

* arguments 

 得到 `function` 的所有参数
 
    ```javascript
    var add = function( a1, a2 , a3 , a4 , ..... ) 
        
    var add = function(){
        var i = 0 , sum = 0;
        while(typeof arguments[i] != 'undefined') { 
            sum += arguments[i]; i++; 
        } 
        return sum;
    }
    ```

* 闭包，内部函数可以访问外部函数的参数与变量

	_如何从外部读取局部变量？_
	
	那就是在`函数1`的内部，再定义一个`函数2`，再把`函数2`交给大家使用
	
    ```javascript
    function f1(){
        var n = 999;
        return function f2(){
            alert(n); // 999
        }
    }
    　　
    var f = f1();
    f(); <--- alert 999
	```
	
	

* 如何设计一个`jquery core`


    ```javascript
   	(function() {

       	var jQuery = function( selector, context ) {
       		return new jQuery.fn.init( selector, context );
   	    };

       	jQuery.fn = jQuery.prototype = {
           	init: function( selector, context ) {
                return this;
   	        }
        }

        jQuery.fn.init.prototype = jQuery.fn;

   		return (window.jQuery = window.$ = jQuery);

	})();

	```


* 减少全局变量污染

    ```javascript
	(function( window, undefined ) {
		// balabala....
	})(window); 
	```


* 回调

	> _回调函数以参数形式扔给执行函数，并不会马上被执行。它会在包含它的函数内的某个特定时间点被“回调”_

    ```javascript
	var done = function(){
		alert('注册完毕')
	}
	
	register( username , password , done ); // 3秒后注册完毕
	```
	
	关于异步回调：在设计上，只要是回调均认为是异步的。 
	
	> setTimeout , ajax 的应用均为异步
	
	[理解并使用回调](http://javascriptissexy.com/understand-javascript-callback-functions-and-use-them/)


## 异步编程

* 理解

 同步是这样
    	
    ```javascript
    f1();
    f2();
    ```
    	
 异步是这样
    	
    ```javascript
    function f1(callback){
        setTimeout(function () {
            // f1's code
            callback();
        }, 1000);
    }
    
    f1(f2)
    ```
        
* 回调地狱 **callback hell**

    ```javascript
    ajax(a, function() {
      ajax(b(a.somedata), function() {
        ajax(c(b.somedata), function() {
          c.finish()
        }
      }) 
    })
    ```

    改进版本
    
    ```javascript
    var step_3 = function() {
        c.finish();
    };
    
    var step_2 = function(c, b) {
        ajax(c(b.somedata), step_3);
    };
    
    var step_1 = function(b, a) {
      ajax(b(a.somedata), step_2);
    };
    
    ajax(a, step_1);
    ```
    

* 异步编程解决方案

 ##### observer pattern

    ```javascript
    var app = $({});

    app.bind('a_done',function(data){
        ajax(b(data),done('b'));
    });
    
    app.bind('b_done',function(data){
        ajax(c(data),done('c'));
    });
    
    app.bind('c_done',function(data){
        finish();
    });

    var done = function(type){
        return function(){
            var args = Array.prototype.slice.apply(arguments);
            app.trigger( type + '_done' , args )
        }
    }    
    
    ajax(a,done('a'))

    ``` 


 #####  jquery deferred 
 
    * 基于 [CommonJS](http://wiki.commonjs.org/wiki/Promises/A)
    * 理解

    ```javascript
    function foo() {
        var deferred = $.Deferred();
        setTimeout(function() {
            deferred.resolve("foo");
        }, 1000);
        return deferred.promise();
    }
    
    foo().then(function(value) {
        console.log('成功',value);
    });
    ```
    
    * 改进版本

    ```javascript
    function a(){
        return $.ajax(a);
    }    
    
    function b(data){
        return $.ajax(b(data));
    }
    
    function c(data){
        return $.ajax(c(data));
    }

    a().then(b).then(c).then(function(){
        finish();
    });
    ```


 #####  async 

    * https://github.com/caolan/async
    
    ```javascript
    async.series([
        function(callback){
            // do some stuff ...
            callback(null, 'one');
        },
        function(callback){
            // do some more stuff ...
            callback(null, 'two');
        }
    ],
    // optional callback
    function(err, results){
        // results is now equal to ['one', 'two']
    });
    ```
    
    ```javascript
    async.parallel([
        function(callback){
            setTimeout(function(){
                callback(null, 'one');
            }, 200);
        },
        function(callback){
            setTimeout(function(){
                callback(null, 'two');
            }, 100);
        }
    ],
    // optional callback
    function(err, results){
        // the results array will equal ['one','two'] even though
        // the second function had a shorter timeout.
    });
    ``` 

    ```javascript
    async.waterfall([
        function(callback){
            callback(null, 'one', 'two');
        },
        function(arg1, arg2, callback){
          // arg1 now equals 'one' and arg2 now equals 'two'
            callback(null, 'three');
        },
        function(arg1, callback){
            // arg1 now equals 'three'
            callback(null, 'done');
        }
    ], function (err, result) {
       // result now equals 'done'    
    });
    ```


 #####  其他
     * wind.js (看看即可,仅供学习)
     * Q
    

## OO编程


#### QClass完整源码

```javascript
(function (global) {

    var objectEach = function (obj, fn , scope  ) {
        for (var x in obj)
            fn.call( scope, x, obj[x] );
    };

    var arrayEach = Array.prototype.forEach ? function (array, fn ,scope) {
        return array.forEach( fn ,scope )
    } : function (obj, fn , scope) {
        for (var i = 0 , len = obj && obj.length || 0; i < len; i++)
            fn.call( scope , obj[i], i);
    };

    var extend = function (params, notOverridden) {
        var me = this;
        objectEach(params, function (name, value) {
            var prev = me[ name ];
            if (prev && notOverridden === true)
                return;
            me[ name ] = value;
            if (typeof value === 'function') {
                value.$name = name;
                value.$owner = this;
                if (prev)
                    value.$prev = prev;
            }
        });
    };

    var ns = function ( name , root ) {
        var part = root || global,
            parts = name && name.split('.') || [];

        arrayEach(parts, function (partName) {
            if (partName) {
                part = part[ partName ] || ( part[ partName ] = {});
            }
        });

        return part;
    };
 
    var XNative = function (params) {

    };
 
    XNative.mixin = function ( mixinClass , name  ) {
        this.mixins.push( { name : name , mixin : mixinClass } );
        return this;
    };
 
    XNative.implement = function ( params , safe ) {
        extend.call(this.prototype, params , safe );
        return this;
    };
 

    XNative.prototype.implement = function ( params , safe ) {
        extend.call(this, params , safe );
        return this;
    };
 
    XNative.prototype.parent = function () {
        var caller = this.parent.caller ,
            func = caller.$prev;
        if (!func)
            throw new Error('can not call parent');
        else {
            return func.apply(this, arguments);
        }
    };

    var PROCESSOR = {
        'statics':function (newClass, methods) {
            objectEach(methods, function (k, v) {
                newClass[ k ] = v;
            });
        },
        'extend':function (newClass, superClass) {
            var superClass = superClass || XNative , prototype = newClass.prototype , superPrototype = superClass.prototype;


            //process statics
            objectEach(superClass, function (k, v) {
                if( k !== 'prototype' )
                    newClass[ k ] = v;
            })

            //process prototype
            objectEach(superPrototype, function (k, v) {
                prototype[ k ] = v;
            });

            //process mixins
            var mixins = newClass.mixins = [];

            if (superClass.mixins)
                mixins.push.apply( mixins , superClass.mixins );


            newClass.superclass = prototype.superclass = superClass;
        },
        'mixins':function (newClass, value) {
            if( value )
                arrayEach(value, function (v) {
                    if( typeof v === 'function')
                        newClass.mixin( v );
                    else if( v.mixin )
                        newClass.mixin( v.mixin , v.name )
                });
        }
    };

    var PROCESSOR_KEYS = ['statics', 'extend', 'mixins'];
 
    function XClass( params ){

        var params = params || {};

        var XNative = function(){
            return _.call( this , XNative , params , arguments );
        };

        var methods = {};

        arrayEach(PROCESSOR_KEYS, function (key) {
            PROCESSOR[ key ](XNative, params[ key ], key);
        });

        objectEach(params, function (k, v) {
            if (!PROCESSOR[ k ])
                methods[ k ] = v;
        });

        extend.call(XNative.prototype, methods);

        return XNative;
    }

    function _( XNative , params , args ){

        var me = this;

        if ( params.singleton )
            if (XNative.singleton)
                return XNative.singleton;
            else
                XNative.singleton = me;

        var mixins = XNative.mixins;

        arrayEach( mixins , function (mixin) {

            var name = mixin.name ,
                mixinClass = mixin.mixin ,
                fn = function(){ return mixinClass.apply( this , args );};

            fn.prototype = mixinClass.prototype;

            if( name )
                ns( 'mixins' , me )[ name ] = new fn();
            else
                me.implement( new fn() , true );

        });

        return me.initialize && me.initialize.apply(me, args ) || me;
    }
 
    XClass.utils = { 
        objectEach:objectEach, 
        arrayEach:arrayEach, 
        ns:ns
    };
 
    XClass.define = function (className, params) {
        if (className) {
            var lastIndex = className.lastIndexOf('.') , newClass;
            return ns(lastIndex === -1 ? null : className.substr(0, lastIndex))[ className.substr(lastIndex + 1) ] = new XClass(params);
        } else
            throw new Error('empty class name!');
    };

    global.QClass = XClass;


})(window);
```

* 使用 QClass 

    ```javascript
    QClass.define("Hotel.base",{
        initialize : function(){}
        statics : {
          static_method : function(){}
        },
        method1 : function(){},
        method2 : function(){},
        extend : ExtendedClass,
        mixins : [ MixedClass1 , MixedClass2 ],
        singleton : false
    })
    
    QClass.define("Hotel.app",{ 
        method1 : function(){},
        method2 : function(){},
        extend : Hotel.base
        singleton : false
    })
    
    ```
    
* 命名空间
* 继承
* 构造器
* 静态方法
* 实例方法
* 单例模式
* mixin


## 作业

完成随意排序的动画 http://www.cs.usfca.edu/~galles/visualization/ComparisonSort.html
* 不要使用现成的库

## 扩展内容

* node 
* coffeescript 


## 进阶阅读

http://dmitrysoshnikov.com/ecmascript/javascript-the-core/


