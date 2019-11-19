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
		2).填充
			str.zfill(width)#按照指定宽度填充'0'字符
		3).大小写变化
			str.capitalize()#首字母大写
			str.casefold()#对其他语言有效,比如德语'ß'=>'ss'
			str.lower()#只对ASCII有效,其他语言无效 'ß'不变
			str.upper()#转大写
			str.swapcase()#大小写切换
			str.title()#转换成标题型字符串
		4).字符串对齐
			str.center(width[, fillchar])#默认的填充的ASCII空格
			str.ljust(width[,fillchar])#左对齐
			str.rjust(width[,fillchar])#右对齐
		5).计算字符串中子字符串出现的次数不能重叠计算--str.count(sub[, start[, end]])
		6).编码字符字符串为字节对象--str.encode(encoding="utf-8", errors="strict")#errors=strict会抛出UnicodeError,errors=ignore会忽略,errors=replace会用?替换
		7).开头结尾判断
			str.endswith(suffix[, start[, end]])#多个判断可以使用元组
			str.startswith(prefix[, start[, end]])#判断开头
		8).tab替换为空格--str.expandtabs(tabsize=8)
		9).查找字符串
			str.find(sub[, start[, end]])#找不到返回-1, 如果只需判断是否包含可以用in
			str.rfind(sub[,start[,end]])#返回最高的索引,如果找不到返回-1
			str.index(sub[, start[, end]])#和find的区别是index找不到抛出ValueError,而find返回-1
			str.rindex(sub[,start[,end]])#和rfind类型,找不到会抛出ValueError
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
			str.isidentifier()#判断是否是有效命名,a-zA-Z0-9_不以数字开始
			str.iskeyword()#是保留关键字
			str.isprintable()#是否是可打印字符,转移字符不是
			str.isspace()#是否是空格
			str.istitle()#判断是否是标题,大写字母只能跟在无大小写字符之后,小写只能跟在大小写之后
			str.islower()#判断小写,'_a'=>True,'_'=>False
			str.isupper()#判断大写,同上
		12).字符串串联
			str.join(iterable)#用str连接可迭代对象
		13).去除字符串
			str.lstrip([chars])#去除左端指定字符,没有指定或None则去除空格
			str.rstrip([chars])#去除右端指定字符
			str.strip([chars])#去除两端指定字符
		14).生成Unicode序列号的对应字典
			static str.maketrans(x[,y[,z]])#用于str.translate()
		15).分割字符串
			str.partition(sep)#按sep第一次出现的位置分割字符串成元组(前,sep,后),如果没有找到sep返回(str,'','')
			str.rpartition(sep)#按sep最后一次出现的位置分割字符串成元组(前,sep,后),如果没有找到sep返回(str,'','')
			str.rsplit(sep=None,maxsplit=-1)#从右向左依次按照sep分割成列表,如果sep为None,则按空格分割, 如果没有指定maxsplit为-1则为全部分割,和split()一样效果
			str.split(sep=None, maxsplit=-1)#分割字符串为列表
				#如果指定maxsplit且不为-1,将按sep切割个不多于maxsplit+1份,'1,2,3'.split(',',maxsplit=1)=>['1','2,3']
				#不指定sep或为None, 空字符串为空列表  ''.split()=>[]
				#不指定sep或为None,非空字符串为该字符串列表 'a'.split()=>['a']
				#不指定sep或为None,被分割的有空格,则分隔符为连续空格 ' a  b c'.split()=>['a','b','c']
				#空字符串+sep不为None ''.split('a')=>['']
				#字符串两边含有分割符 'aba'.split('a')=>['','b','']
			str.splitlines([keepends])
				#按照换行符分割字符串,首先将字符串+一个换行符 进行 分组,如果没有keepends=True,则将去除换行符,生成没有换行符的列表,有则保留换行符
				#'a\n\nb\r\nc'.splitlines()==>['a','','b','c']
				#'a\n\nb\r\nc'.splitlines(keepends=True)==>['a\n','\n','b\r\n','c']
				#空字符串 ''.splitlines()=>[]
		16).替换字符串
			str.replace(old,new[,count])#可以指定替换次数
			str.translate(table)#按照table映射替换字符串
				#table需要是实现__getitem__()
				#table需要 索引 为 Unicode序数
				#如果删除,则table中的索引对应的是None   如:table={97:None}
				#可以使用str.maketrans()创建table
		17).
```
### 列表
```
	- 列表可以包含一系列不同类型的数据,用方括号包裹,如: squares=[1,2,3]
	- 和字符串一样列表可以索引切片操作,如:squares[0],squares[-3:]
	- 可以使用squares[:]创建浅拷贝
	- 可以使用+号连接两个列表
	- 列表是可变类型 可以通过下标进行对值的修改,如: squares[0] = 100
	- 列表长度计算: len(squares)
	- 列表可以嵌套
```
## 控制语句
### if语句
```
	- python没有`switch case`
	- if
		if condition:
			pass
	- if-else
		if condition:
			pass
		else:
			pass
	-if-elif-else
		if condition:
			pass
		elif condition2:
			pass
		else:
			pass
```
### for语句
```
	遍历可迭代的
	for _ in f:
		pass
	遍历同时修改集合容易出问题,应该需要创建副本,如:
	for user,status in user.copy().items():
		if status=='inactive':
			del users[user]
```
### range
```
	生成数字序列: 
		range(num) => [0,num)#要前不要后
		range(num1,num2) => [num1,num2)
		range(num1,num2,step) =>[num1,num2)#步长为step
	range只要调用的时候才会分配空间,这和列表的区别
```
### 循环中的break,continue,else
```
	- break: 跳出最内层的循环
	- else: for循环的执行结束, while为Flase的时候执行 else语句, 但当有break跳出循环的else不会被执行
		循环else和try else类似
	- continue: 跳过当前循环,执行下一步
```
### pass用于 语法上需要,程序上没有操作的情况

























	