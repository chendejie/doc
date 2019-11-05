# python doc
## 编码
`# -*- coding: utf-8 -*-`
- 考虑兼容Emacs编辑器,使用上面写法, 不考虑兼容,使用下面写法
`# coding: utf-8`
- 其它自定义格式（不推荐），只要满足以下正则表达式即可
`# ^[ \t\f]*#.*?coding[=:]\s*([-\w.]+)`
## 类型
### 数字
```

	+ - * / %
	/: 返回float类型
	//:真除,返回最大的整数结果int
	除了int,float,还有decimal.Decimal,fractions.Fraction,complex
```
### 字符串
```
	- 单行字符串: 
		单引号: '字符串'
		双引号: "字符串"
	- 多行字符串:
		'''可以是多行字符串'''
		"""可以是多行字符串"""
	- 字符串中都可以有转移字符
	- 原生字符串: 如果想字符串中\x不被转移,可以在字符串引号前添加r或R如: r'c:\some\name'
	- bytes类型字符串: b'abc' 或 B'abc' --普通字符串编码之后为bytes类型字符串
	
	- 字符串重复: *
	- 字符串合并: +
	- 两个具体字符串相邻会自动合并
	- 字符串可以被下标索引: 
		正数下标:0到len(string)-1
		负数下标:-len(string)到-1
		支持切片: string[a:b]表示截取下标a开始到下标b结束(包含a不包含b)
		切片支持超过字符串长度比如: string[a:len(string)+n],则是到结尾
	- 字符串是不可变类型,不能通过下标更改原字符串:比如string[0] = 'j'

	- str(object='')和str(object=b'', encoding='utf-8', errors='strict')
		1).encoding和errors都没有,返回object.__str__(), 如果没__str__则返回repr(object)
		2).encoding或errors有,则object需要是类bytes对象(如bytes,bytesarray)
		3).str(bytes,encoding,errors)和bytes.decode(encoding,errors)一样
		4).str(类bytes)值友好显示,调用__str__方法

	- 格式化字符串: f'abc{value}',更多使用%号或string.format()
	- string.format()方法
		1).位置 '{1} {0}'.format('a','b')
		2).位置 '{0[1]} {0[0]}'.format(('a','b'))
		3).具名参数 '{name} {age}'.format(name='',age=0)

```
