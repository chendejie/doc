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
	- string.format()方法和%号的用法类似,用冒号替换 如:%03.2f->{:03.2f}
		1).位置 '{1} {0}'.format('a','b')
		2).位置 '{0[1]} {0[0]}'.format(('a','b'))
		3).具名参数 '{name} {age}'.format(name='',age=0)
		4).属性方式 
			负数:'{0.real} {0.imag}'.format(3-5j)
			对象: '{a.attr1} {a.attr2}'.format(a=obj)
		5).取代 %s的str() %r的repr(), '{!r} {!s}'.format('a','b')
		6).对齐 <左 >右 ^居中 =数字补0  '{:>10}'.format('a') 对齐符号前可有填充字符,没有为空格
		7).浮点数 
			{:+f}对正负数都会显示数字符号,f默认6为小数,,如:'{:+f}; {:+f}'.format(3.14, -3.14)
			{: f}空格对整数起作用,显示整数前面有空格
			{:-f}对整数没效果
		8).取代%x %o %b,若要显示前缀0x,0o,0b需要在符号前添加#号
			无前缀: "int: {0:d};  hex: {0:x};  oct: {0:o};  bin: {0:b}".format(42)#'int: 42;  hex: 2a;  oct: 52;  bin: 101010'
			有前缀: "int: {0:d};  hex: {0:#x};  oct: {0:#o};  bin: {0:#b}".format(42)#'int: 42;  hex: 0x2a;  oct: 0o52;  bin: 0b101010'
		9).数字的千分位号
			逗号(v3.1新增): 
				'{:,}'.format(123456789.123456)#'123,456,789.123456'
				'{:10,}'.format(9.123456)#'  9.123456'
				'{:010,}'.format(9.1234)#'0,009.1234'
			下划线(v3.6新增):同上
		10).百分数和小数位数
			'{:08.2%}'.format(111/13)#'0853.85%',其中8是位数不足补0,保留2位小数
			'{:+08.2f}'.format(111/13)#'+0008.54'
		11).datetime的格式化显示
			'{:%Y-%m-%d %H:%M:%S}'.format(datetime.datetime(2010, 7, 4, 12, 15, 58))
	- 字符串方法
		1).通用的序列操作
			包含: x in s | x not in s 
			合并: s + t | s * n or n * s 
			索引: s[i] | s[i:j] | s[i:j:k] 
			计算: len(s) | min(s) | max(s) 
			查找统计: s.index(x[, i[, j]]) | s.count(x)
		2).首字母大写--str.capitalize()
		3).转小写
			str.casefold()#对其他语言有效,比如德语'ß'=>'ss'
			str.lower()#只对ASCII有效,其他语言无效 'ß'不变
		4).字符串居中对齐--str.center(width[, fillchar])#默认的填充的ASCII空格
		5).计算字符串中子字符串出现的次数不能重叠计算--str.count(sub[, start[, end]])
		6).编码字符字符串为字节对象--str.encode(encoding="utf-8", errors="strict")#errors=strict会抛出UnicodeError,errors=ignore会忽略,errors=replace会用?替换
		7).结尾判断--str.endswith(suffix[, start[, end]])#多个判断可以使用元组
		8).tab替换为空格--str.expandtabs(tabsize=8)
		9).查找字符串
			str.find(sub[, start[, end]])#找不到返回-1, 如果只需判断是否包含可以用in
			str.index(sub[, start[, end]])#和find的区别是index找不到抛出ValueError,而find返回-1
		10).格式化字符串
			str.format(*args, **kwargs)
			str.format_map(mapping):(v3.2)#和str.format(**mapping)类似,区别是直接使用mapping不拷贝到字典
		11).判断字符
			str.isalnum()#数字字母组成
			str.isalpha()#字母
			str.isascii():(v3.7)#从U+0000-U+007F
			str.isdecimal()#小数,unicode数字,全角双字节数字为true,罗马以及汉字数字,小数位false,byte数字报错
			str.isdigit()#unicode数字,byte数字,全角数字为true, 汉字罗马小数为false
			str.isnumeric()#unicode数字,全角数字,罗马数字汉字数字为true,小数false,byte数字报错
		12).
		13)
			
```
	