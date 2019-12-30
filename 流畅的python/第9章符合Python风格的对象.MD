# 第 9 章　符合Python风格的对象 

## 9.1	对象表示形式 

- 获取对象的字符串表示形式 
  - repr()：以便于开发者理解的方式返回对象的字符串表示形式。
  - str()：以便于用户理解的方式返回对象的字符串表示形式 。print优先寻找
  
  - 对应特殊方法\_\_repr\_\_()和\_\_str\_\_()
  
- 对象的其他表示形式

  - bytes() 函数调用它获取对象的字节序列表示形式。 对应特殊方法\_\_bytes\_\_()
  - \_\_format\_\_ 方法会被内置的 format() 函数和 str.format() 方法调用，使用特殊的格式代码显示对象的字符串表示形式。 



## 9.2　再谈向量类 

- 示例 9-2 vector2d_v0.py：目前定义的都是特殊方法 

  ```python
  from array import array
  import math
  class Vector2d:
  	typecode = 'd' # ❶ typecode 是类属性，在 Vector2d 实例和字节序列之间转换时使用。
  	def __init__(self, x, y):
  		self.x = float(x) # ❷ 在 __init__ 方法中把 x 和 y 转换成浮点数，尽早捕获错误，以防调用 Vector2d 函数时传入不当参数。
  		self.y = float(y)
          
  	def __iter__(self):
  		return (i for i in (self.x, self.y)) # ❸ 定义 __iter__ 方法，把 Vector2d 实例变成可迭代的对象，这样才能拆包（例如， x, y = my_vector）。
      
  	def __repr__(self):
  		class_name = type(self).__name__
  		return '{}({!r}, {!r})'.format(class_name, *self) # ❹ __repr__ 方法使用 {!r} 获取各个分量的表示形式，然后插值，构成一个字符串；因为 Vector2d 实例是可迭代的对象，所以 *self 会把 x 和 y 分量提供给 format 函数。
      
  	def __str__(self):
  		return str(tuple(self)) # ❺ 从可迭代的 Vector2d 实例中可以轻松地得到一个元组，显示为一个有序对。
      
  	def __bytes__(self):
  		return (bytes([ord(self.typecode)]) + # ❻ 为了生成字节序列，我们把 typecode 转换成字节序列，然后……
  				bytes(array(self.typecode, self))) # ❼ ……迭代 Vector2d 实例，得到一个数组，再把数组转换成字节序列。
      
  	def __eq__(self, other):
  		return tuple(self) == tuple(other) # ❽ 为了快速比较所有分量，在操作数中构建元组。
      
  	def __abs__(self):
  		return math.hypot(self.x, self.y) # ❾ 模是 x 和 y 分量构成的直角三角形的斜边长。
      
  	def __bool__(self):
  		return bool(abs(self)) # ❿ __bool__ 方法使用 abs(self) 计算模，然后把结果转换成布尔值，因此， 0.0 是 False，非零值是 True。
  ```

  

## 9.3　备选构造方法（没理解什么意思） 

- 字节序列转换成 Vector2d 实例。 

  ```python
  @classmethod # ❶ 类方法使用 classmethod 装饰器修饰。
  def frombytes(cls, octets): # ❷ 不用传入 self 参数；相反，要通过 cls 传入类本身。
  	typecode = chr(octets[0]) # ❸ 从第一个字节中读取 typecode。
  	memv = memoryview(octets[1:]).cast(typecode) # ❹ 使用传入的 octets 字节序列创建一个 memoryview，然后使用typecode 转换。
  	return cls(*memv) # ❺ 拆包转换后的 memoryview，得到构造方法所需的一对参数。
  
  ```

  



























# 参考

[Python中\_\_repr\_\_和\_\_str\_\_区别](https://www.cnblogs.com/fengff/p/8650114.html)