# ES6语法
```
	- 变量
		var: 可以重复声明,没有块级作用域, 不能限制
		let(声明变量) const(声明常量): 禁止重复声明, 控制修改, 支持块级作用域
	- 作用域
		传统 函数级
		ES6 块级 {} if(){} for(){}都是
	- 解构赋值
		数组和对象的解构赋值
			“模式匹配”，只要等号两边的模式相同
			默认值:
				数组成员严格等于undefined，默认值才会生效
				默认值是一个表达式，那么这个表达式是惰性求值
				默认值可以引用解构赋值的其他变量，但该变量必须已经声明
		字符串的解构赋:
			字符和length属性的解构赋值
		数值和布尔值的解构赋值:
			解构赋值时，如果等号右边是数值和布尔值，则会先转为对象
		可以使用圆括号的情况只有一种：赋值语句的非模式部分，可以使用圆括号
```