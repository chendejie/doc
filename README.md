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
### function
```
	def funcName(params:str,egg:str='eggs')->str:
		'''docstring'''
		return ''
	- 变量的作用域: 本地局部变量->闭包变量->全局变量->内置变量
	- 全局变量和闭包变量不能,不能在函数中直接更改来改变该变量的值, 除非用global定义全局变量,或nonlocal定义闭包变量
	- 按值传参和引用传参
	- 没有return 函数返回值为None
	- 默认参 def funcName(arg1,arg2=''):pass
	- 默认的在函数定时的时候决定的, 不是调用的时候决定的
	- 如果默认值是可变类型, 默认值将是一个引用传值
	- 传参
		先位置参数, 然后关键字参数
		任意参数只能就是不超过一个值
		**name为dict包含所有关键字参数 *name为元组包含所有位置参数 其中*name要在**name之前
		参数的定义: def(pos1,pos2,/,pos_or_kwd,*,kwd1,kwd2):pass
			/将用于逻辑上区分位置参数和其他, /之前的为仅能位置传参, /之后的可以为位置参数或者关键字参数
			*用于区分其他参数和关键字参数, *之后为仅关键字参数
			pos1,pos2为仅能位置传参, pos_or_kwd为位置和关键字都可以, kwd1,kwd2仅能关键字传参
			如果定义没有/和*,参数可以使用位置和关键字
		可变参数: def(file,*args):pass
			在可变参之前可以有0个或多个正常参数
			可变参数之后的参数必须是关键字参数
		*args: 在调用函数时, 对参数进行解包, 可有列表和元组解包
		**kwargs: 在函数调用是, 对关键字参数进行解包, 可由字典解包
	- 函数的注解放在函数的__annotations__属性中
```
### lambda表达式
```
	lambda a,b: a+b
	lambda 多用于作为函数调用的参数
```
### 文档字符串说明
```
	def my_function():
		'''简短说明目的用途
			空一行
			调用约定和效果
		 '''
		pass
	print(my_function.__doc__)
```
## 代码格式
```
	https://www.python.org/dev/peps/pep-0008/
```
## 数据结构
```
	- 列表
		方法
		1). 追加,扩展
			list.append(x): <=> a[len(a):] = [x]
			list.extend(iterable) <=> a[len(a):] = iterable
		2). 插入,移除
			list.insert(i,x): 在i位置插入x
			list.pop([i]): 移除i位置的值,若没有指定,移除最后一个
			list.remove(x): 移除第一次匹配的x, 若没有找到,抛出异常ValueError
		3). 清除,拷贝, 翻转
			list.clear(): <=> del a[:]
			list.copy(): <=> list[:]
			list.reverse(): 倒序原列表 和 list[::-1]效果一样
		4). 索引位置
			list.index(x[,start[,end]]): 查找x的索引位置, 找不到抛出ValueError
		5). 统计
			list.count(x): 统计x在列表中出现的次数
		6). 排序
			list.sort(key=None,reverse=False): 排序list,和sorted(list,key=None,reverse=False)效果一样, sorted返回新列表
		#其中在sort使用中, integer和string不能对比,None不能和其他类型对比, 复数也不能对比
		列表堆栈:后进先出
			append()和pop()
		列表:先进先出
			由于列表移除第一个元素,后面元素都要改变,不是O(1)操作
			collections.deque就是先进先出的高效实现
			collections.deque([]).append('').popleft()
		列表推导
			[x for x in x], [x for x in x if x]
	- 列表的del语句
		删除列表指定元素,通过下标del a[0]
		可以通过切片删除del a[2:4],及清空del a[:]
		del也可以删除整个元素
	- 元组
		元组是不可变类型
		初始化空元组 e = (), 如果元组只有一个值 e = (1,)
		元组和列表一样可以解包
	- 集合
		无序无重复
		支持 并|,交&,差-,补^
		初始化 set() 或 {}
		集合推导
	- 字典
		字典的key需要是不可变类型,且一个字典中唯一的,如果如果多个相同key后面会替代前面, 常用的有数字,字符串,如果元组作为键只能包含字符串,数字,元组; 如果元组直接或间接包含可变类型则不能作为键
		字典形式: {}或{键值对},通过键获取值,如果键不存在则报错,in判断判断键是否存在, 可以通过 del dic[key]删除键值对
		dict()可以将键值对列表转成字典, 也可以使用关键词参数 dict(a=1,b=2)
		字典推导
		d.items() 返回dict_items对象 用于循环遍历 返回key,value # 列表可以使用enumerate(list)来获得位置索引和值, 可以用zip函数打包对应列表
	- 比较
		in和not in判断值在不在序列中
		is和is not两个对象是不是一样
		比较的优先级比四则运算低
		比较支持链式操作: a<b==c 为=> a<b && b==c
		逻辑操作 and or not, 优先级比比较运算符低,优先级 not>and>or, and和or有短路现象
		:= 海象运算符 (v>3.8) 赋值右边给左边,并且表达式为该值,避免多次赋值
		相同类型序列比较,按照字典顺序,字符串的字典顺序是Unicode码
```
## 模块
## 模块相关
```
	模块可以被其他模块导入
	模块文件需要是.py扩展名
	格式: 
		import a
		from a import xx,yy
		from a import * # _打头的变量不会导入
		import a as am
		from a import xx as xxm
	每个对话中, 模块只被导入一次, 如果模块改动你需要再次导入, 使用importlib.reload(modulename)
	执行脚本: python filename.py <arguments>
		if __name__=='__main__':pass #直接运行该模块执行, 作为模块导入则不会执行
	当导入一个spam,先去找内置模块是否有, 没有再去sys.path寻找
		sys.path包含当前脚本路径, PYTHONPATH, 以及安装依赖默认路径
		可以添加需要的 如: sys.path.append('/some/path')
	模块编译缓存在 __pycache__/module.version.pyc
	通过dir(modulename)函数可以获得模块定义的成员方法,dir()不会列出内置built-in函数变量,可以用dir(builtins)
```
### 包相关
```
	包通过 点的模块命名 A.B来构建命名空间
	__init__.py文件使 路径中的文件作为包对待, __init__.py文件可以是一个空文件
	使用方式: 
		import a.b.c #中间必须是一个包
		from a.b import c
	没有找到抛出ImportError异常
	对于 from a.b import *
		如果一个包下面的__init__.py定义了一个列表__all__,则返回该列表的模块
		如果没有定义__all__, 确保a.b被导入, 并会运行__init__.py中初始化代码,以及一些在包中定义的变量, 之前导入的包也会导入
	包的相对路径导入
		from  . import echo
		from  .. import echo
		from  ..filters import echo
	包的__path__属性: 初始化为包好__init__.py的目录名列表
```
## 输入和输出
### 格式输出
```
	1. 使用字符串格式化方式,以f或F在字符串前面,可以替换字符串中的变量:如, year=2016 f'year={year}'
	2. 使用str.format()方式
	3. 手动设置
	4. 老版本的格式化 %
```
### 文件读写
```
	open(filename, mode)
	mode:
		r: 读
		w: 覆盖写
		a: 追加
		r+: 读写
		b: 二进制模式
	with open() as f:
		pass
	try-finally 捕获open
	f.read(size): 如果没有size或size为负数,则整个文件内容会读取返回, 否则返回size大小内容,如果达到文件结尾返回空字符串
	f.readline(): 读取一行, 如果到达文件结尾返回空字符串
	list(f): 读取所有
	f.readlines(): 读取所有
	for line in f: 循环读取
	f.write(str): 写入str返回写入字符数
	f.tell(): 返回当前指针的位置
	f.seek(offset,whence): 设置文件指针whence默认为0, 表示从文件开始, 1为当前位置开始, 2位文件末尾开始, offset为偏移量, 在python3中如果模式不是b,只允许从文件开头计算相对偏移位置, 或者偏移位置为0
	f.isatty(): 检测文件是否连接到一个终端设备
	f.truncate(): 清空文件
	json数据的文件序列化和反序列化
	json.dump(x,f)#将数据序列化到文件
```
##异常报错
###异常捕获
```
	格式:
		try:
			pass
		except:
			pass
		try:
			pass
		except Exception as e:
			pass
	捕获多个异常可以写一起,或分开多个except
		try:
			pass
		except(RuntimeError,TypeError,NameError) as e:
			pass
	else可选项:
			pass
		except:
			pass
		else:
			pass#在没有抛出异常的情况下执行
	使用sys.exc_info()获得异常信息
	获得报错信息:
		try:
			pass
		except Exception as e:
			print(e)#调用__str__
			print(e.args)#参数
	抛出异常
		raise ...#如 raise NameError('haha'),该参数将会给except捕获的异常e.args
	finally语句:
		无论是否有异常,都会执行finally
		如果异常没被捕获到, 则在执行完finally后抛出异常
		如果try中有break,continue,return语句,finally会在触发break,continue,return之前执行
		如果finally中有return会先与try中的return执行返回
```
## 类对象
### 命名空间与作用域
```
	局部作用域,闭包作用域,全局作用域,内置作用域
	如果局部变量有nonlocal修改,则会修改直接外层(非全局)的变量值,如果外层的局部变量是全局变量,则不能用nonlocal修饰
	global修饰的局部变量变成全局变量,修改全局变量的值而不会改变局部外层的局部变量的值
	函数的作用域有定义时决定的,而不是调用的时候
```
###类
```
	定义:
		class ClassName:
			pass
	类的定义也可以在if判断中或函数内
	类对象支持属性引用和实例化
		MyClass.attrName
		MyClass.funcName
		MyClass.__doc__
		x = MyClass()
	__init__方法:实例化之前会先调用该方法
	类变量被所用实例共享
	object.__class__存储类
	isinstance()#实例判断
	issubclass()#判断是否是子类
	多继承属性: 深度优先,从左往右
	使用super()调用父类,然后调用父类方法
	私有属性: 以_开头
	双下划线属性会被转成_classname__x
	对象的迭代: 定义 __iter__和__next__方法
```
##标准库简介
### os和shutil
```
	os模块提供操作系统相关的函数
	文件读写参考: open()函数,
	目录操作可以参考: os.path
	在命令行模式下读取所有文件的行,参考:fileinput
	临时文件操作,参考tempfile模块
	其他更高级的文件目录操作可以参考shutil模块
	os模块的所有函数抛出OSError当文件名路径无法访问,其他参数类型不对,或者操作系统不接受
	os.name: 操作系统依赖模块导入的名称,目前: posix, nt, java
	os.ctermid():返回终端名称,unix可用
	os.environ: 获得环境的对象映射,使用os.putenv()更改变量, os.unsetenv()删除变量
```


















	