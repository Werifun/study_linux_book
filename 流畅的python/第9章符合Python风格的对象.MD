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

  

## 9.4 classmethod与staticmethod  

- classmethod 改变了调用方法的方式，因此类方法的第一个参数是类本身，而不是实例。  
- staticmethod 装饰器也会改变方法的调用方式，但是第一个参数不是特殊的值。  



## 9.5　格式化显示(没细看)

- format

- 中间有一堆没看

  ```python
  #eg9-5
  	# 在Vector2d类中定义
  	def __format__(self, fmt_spec=''):
  		components = (format(c, fmt_spec) for c in self) # ➊ 使用内置的 format 函数把 fmt_spec 应用到向量的各个分量上，构建一个可迭代的格式化字符串。
  		return '({}, {})'.format(*components) # ➋ 把格式化字符串代入公式 '(x, y)' 中。
  ```

  - 代码中format(c, fmt_spec)究竟是什么样的呀

  

- ```python
  # eg9-6
  # 在Vector2d类中定义
  	def angle(self):  # 计算角度
  		return math.atan2(self.y, self.x)
      
      def __format__(self, fmt_spec=''):
  		if fmt_spec.endswith('p'): # ❶ 如果格式代码以 'p' 结尾，使用极坐标。
  			fmt_spec = fmt_spec[:-1] # ❷ 从 fmt_spec 中删除 'p' 后缀。
  			coords = (abs(self), self.angle()) #❸ 构建一个元组，表示极坐标： (magnitude, angle)。
  			outer_fmt = '<{}, {}>' # ❹ 把外层格式设为一对尖括号。
  		else:
  			coords = self # ❺ 如果不以 'p' 结尾，使用 self 的 x 和 y 分量构建直角坐标。
  			outer_fmt = '({}, {})' # ❻ 把外层格式设为一对圆括号。
  		components = (format(c, fmt_spec) for c in coords) # ❼ 使用各个分量生成可迭代的对象，构成格式化字符串。
  		return outer_fmt.format(*components) # ❽ 把格式化字符串代入外层格式。
  ```

  - 本质还是使用 format函数，然后

  

  ## 9.6　可散列的Vector2d

- 目前 Vector2d 实例是不可散列的，因此不能放入集合（set）中  。

- 需要定义\_\_hash\_\_方法。

  ```python
  # eg 9-7 9-8
  # vector2d_v3.py
  class Vector2d:
  	typecode = 'd'
  	def __init__(self, x, y):
  		self.__x = float(x) # ❶ 使用两个前导下划线（尾部没有下划线，或者有一个下划线），把属性标记为私有的。
  		self.__y = float(y)
  	@property # ❷ @property 装饰器把读值方法标记为特性。后面遇到了好好学习
  	def x(self): # ❸ 读值方法与公开属性同名，都是 x。
  		return self.__x # ❹ 直接返回 self.__x。
  	@property # ❺ 以同样的方式处理 y 特性。
  	def y(self):
  		return self.__y
  	def __iter__(self):
  		return (i for i in (self.x, self.y)) # ❻ 需要读取 x 和 y 分量的方法可以保持不变，通过 self.x 和self.y 读取公开特性，而不必读取私有属性，因此上述代码清单省略了这个类的其他代码。
  	
  	def __hash__(self):
  		return hash(self.x) ^ hash(self.y)
  ```

  

  

## 9.7 Python的私有属性和“受保护的”属性

- （两个前导下划线，尾部没有或最多有一个下划线）命名实例属性  Python 会把属性名存入实例的\_\_dict\_\_ 属性中，而且会在前面加上一个下划线和类名。因此，对Dog 类来说， \_\_mood 会变成 \_Dog\_\_mood  。

- 一个下划线前缀编写“受保护”的属性（如 self._x）  



## 9.8　使用 __slots__ 类属性节省空间（没有细看）

- 通过 \_\_slots\_\_类属性，能节省大量内存，方法是让解释器在元组中存储实例属性，而不用字典。  





## 9.9　覆盖类属性

- Python 有个很独特的特性：类属性可用于为实例属性提供默认值。   
- 如果为不存在的实例属性赋值，会新建实例属性  。
- 如果为实例属性赋值，**但是同名类属性不受影响**。  





# 参考

[Python中\_\_repr\_\_和\_\_str\_\_区别](https://www.cnblogs.com/fengff/p/8650114.html)

[`string` — Common string  operations](https://docs.python.org/2/library/string.html#module-string)

