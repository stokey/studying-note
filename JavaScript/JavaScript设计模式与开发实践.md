### JavaScript设计模式与开发实践
+ this
	+ 作为对象的方法调用：指向该对象
	+ 作为普通函数调用：指向全局对象[`当函数不作为对象的属性被调用时`]
	+ 构造器调用：构造器里的this就指向返回的这个对象
+ Function.prototype.call/Function.prototype.apply[`动态改变函数的作用域—this`] 
	+ apply接受两个参数，第一个参数指定了函数体内this对象的指向，第二个参数为一个带下标的集合[`数组或类数组集合`]
	+ call传入的参数数量不固定，第一个参数代表函数体内的this指向，从第二个参数开始往后，每个参数依次传入函数[`call是包装在apply上面的一颗语法糖`] 
+ 变量作用域
	+ 声明变量时如果该变量前面没有带上关键字`var`,则这个变量则为全局变量。
	+ 使用`var`关键字在函数中声明变量，此时的变量就是局部变量
+ 闭包的作用
	+ 封装变量：把某些变量封装成`私有变量`
	+ 延长局部变量的寿命
+ 高阶函数
	+ 函数可以作为参数被传递
	+ 函数可以被作为返回值输出 
+ 函数柯里化：柯里化又称为部分求值，一个柯里化的函数首先会接受一些参数，接受这些参数之后，该函数并不会立即求值。而是继续返回另一个函数，刚才传入的参数在函数形成的闭包中被保存起来，待到函数呗真正需要求值的时候，之前传入的所有参数都会被一次性用于求值

```
var currying = function(fn) {
	var args = [];
	return function() {
		if ( arguments.length === 0 ) {
			return fn.apply(this,args);
		} else {
			[].push.apply(this,arguments);
			return arguments.callee;
		}
	}
}
var cost = (function() {
	var money = 0;
	return function() {
		for (var i = 0; l = arguments.length; i < l; i++) {
			money += arguments[i];
		}
		return money;
	}
})();
var cost = currying(cost); // 转化成currying函数
cost(100); // 未真正求值
cost(200); // 未真正求值
console.log(cost()); // 求值并输出：300
```
+ 非柯里化：

```
Function.prototype.uncurrying = function() {
	var self = this;
	return function() {
		var obj = Array.prototype.shift.call(arguments);
		return self.apply(obj,arguments);
	}
}
// 另一种实现
Function.prototype.uncurrying = function() {
	var self = this;
	return function() {
		return Function.prototype.call.apply(self,arguments);
	}
}
```

+ 函数节流：函数被触发的频率太高，降低触发频率的手段

```
var throttle = function (fn, interval) {
	var _self = fn, // 保存需要被延迟的执行的函数引用
	timer, // 定时器
	firstTime = true; // 是否是第一次调用
	return function() {
		var args = arguments, _me = this;
		if (firstTime) {
			_self.apply(_me, args); // 如果是第一次调用，不需要延迟执行
			return firstTime = false;
		}
		if (timer) {
			// 如果定时器还在，说明前一次延迟执行还没有完成
			return false;
		}
		// 延迟一段时间执行
		timer = setTimeout(function () {
			clearTimeout(timer);
			timer = null;
			_self.apply(_me, args);
		}, interval || 500);
	};
};
```

+ 分时函数：短时间内需要做大量操作，按时间段分批处理

```
var timeChunk = function (ary, fn, count) {
	var obj, t;
	var len = arg.length;
	var start = function() {
		for (var i = 0; i < Math.min(count || 1 , ary.length);  i++) {
			var obj = ary.shift();
			fn(obj);
		}
	};
	return function () {
		t = setInterval(function () {
			if (ary.length === 0) {
				return clearInterval(t);
			}
			start();
		}, 200);
	}
}
```

+ 惰性加载函数：避免重复的函数调用[`第一次进入条件分支之后，在函数内部会重写这个函数`]
