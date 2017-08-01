# 前提

在学js之前，就总听说js闭包，对于使用java语言作Android开发的我当时不知道什么是闭包。最近没事开始学js，在学到闭包时瞬间就蒙了，虽然简单的知道闭包就是```在一个函数内部再去定义函数，然后内部函数去引用外部函数的局部变量，这样就可以在外部函数执行完之后仍然可以去访问它的局部变量```。但是仍然不知道怎么去使用，就开始从网上找这方面的博客，博客没少看，大部分都是越看越迷糊。所以决定自己花一天时间慢慢研究研究并记录方便以后查看。如果你也因为闭包的问题在网上查阅资料，碰巧看到了这篇文章，请静下心慢慢看，按顺序往下一个例子一个例子的跟着走一遍，不要跳读，你一定可以深刻理解闭包的概念。  
>声明：该文不是本人原创， [原文](https://stackoverflow.com/questions/111102/how-do-javascript-closures-work)在这里，我一个刚学js时间不到一个星期的小白不可能理解这么透彻，我只是翻译而已，想看原文的请自行跳转。

# 先从一个简单的例子开始

下面是一段js代码：

```

    function sayHello(name){
			var text="Hello "+name;
			var say=function(){
				console.log(text);
			}
			say();
		}

		sayHello("Jerry");  // Hello jerry

```

> 注：我刚开始用Sublime3写代码，不知道格式化的快捷键，所以代码中缺少空格，请勿喷。知道劳烦留言告知，谢谢。还有无法输入中文，(*^__^*) 嘻嘻……

如果上面这段代码看不懂请补充js函数相关知识。

闭包就是一块栈结构，它在一个函数开始执行时分配空间，但是不会在这个函数执行完毕后被释放，就好像它存在于堆中而不是栈中(其实确实是栈，下面会看出)。

# 第二个例子

下面这段代码中返回一个函数的引用：

```         
    function sayHello(name){
			var text="Hello "+name;
			var say=function(){
				console.log(text);
			}
			return say;
		}

		var say2=sayHello("Jerry");
		say2();		// logs "Hello Jerry"

```

大家都能理解一个函数的引用是怎么被返回并赋值给一个变量(say2)的.  
上面的代码中有一个闭包，就是在外部函数sayHello()中声明的匿名函数```function(){ console.log(text); }```.简单来说：在js中，只要你在一个函数内部使用了```function```关键字，那么你就创建了一个闭包。  
在C、Java等大多数语言中，一旦一个函数执行完毕，那么它里面的所有局部变量都不可再被访问，因为它们的栈结构会被销毁。  
js中，如果你在一个函数内部声明了一个内部函数，那么外部函数的局部变量在该函数执行完之后依然可以被访问。上面的代码已经表明了这一点：我们在sayHello()函数执行完毕之后再去调用say2()，say2()函数的内容其实是  
```
function() { console.log(text); }

```
可以看出say2()引用了sayHello()函数的局部变量```text```.之所以可以引用它是因为，sayHello()的这个局部变量被保存在了闭包里。  
在js中，一个函数引用还会对包含它的闭包有一个```secret reference```。

这里如果有点蒙的话

以后有时间完成这篇关于闭包的博客。  https://stackoverflow.com/questions/111102/how-do-javascript-closures-work#
