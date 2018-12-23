# 下划线
	
- `__xxx__` 也就是双下划线开头,双下划线结尾的,是特殊变量,特殊变量可以直接访问,不是pvivate变量,所以,不能使用`__name__`, `__score__`这样的变量名
- `_xxx` 以一个下划线开头的,比如`_name`,这样的变量外部是可以访问的,但是按照约定俗称的约定,当你看到这样的变量时,意思就是,"虽然我可以被访问,但是,请把我视为私有变量,不要随便访问"
	
# 判断类型
## 使用`type()`
基本类型都可以用type()判断:

```py
>>> type(123)
<class 'int'>
>>> type('str')
<class 'str'>
>>> type(None)
<type(None) 'NoneType'>
```
如果一个变量指向函数或者类，也可以用type()判断：

```py
>>> type(abs)
<class 'builtin_function_or_method'>
>>> type(a)
<class '__main__.Animal'>
```
但是type()函数返回的是什么类型呢？它返回对应的Class类型。如果我们要在if语句中判断，就需要比较两个变量的type类型是否相同：

```py
>>> type(123)==type(456)
True
>>> type(123)==int
True
>>> type('abc')==type('123')
True
>>> type('abc')==str
True
>>> type('abc')==type(123)
False
```

判断基本数据类型可以直接写int，str等，但如果要判断一个对象是否是函数怎么办？可以使用types模块中定义的常量：

```py
>>> import types
>>> def fn():
...     pass
...
>>> type(fn)==types.FunctionType
True
>>> type(abs)==types.BuiltinFunctionType
True
>>> type(lambda x: x)==types.LambdaType
True
>>> type((x for x in range(10)))==types.GeneratorType
True
```


## 使用`isinstance ()`

- isinstance()判断的是一个对象是否是该类型本身，或者位于该类型的父继承链上。
- 能用type()判断的基本类型也可以用isinstance()判断：

	```py
	>>> isinstance('a', str)
	True
	>>> isinstance(123, int)
	True
	>>> isinstance(b'a', bytes)
	True
	```
- 可以判断一个变量是否是某些类型中的一种，比如下面的代码就可以判断是否是list或者tuple：

	```py
	>>> isinstance([1, 2, 3], (list, tuple))
	True
	>>> isinstance((1, 2, 3), (list, tuple))
	True
	```

## 使用 `dir()`

如果要获得一个对象的所有属性和方法，可以使用dir()函数，它返回一个包含字符串的list，比如，获得一个str对象的所有属性和方法：

```py
>>> dir('ABC')
['__add__', '__class__',..., '__subclasshook__', 'capitalize', 'casefold',..., 'zfill']
```

类似`__xxx__`的属性和方法在Python中都是有特殊用途的，比如`__len__`方法返回长度。在Python中，如果你调用`len()`函数试图获取一个对象的长度，实际上，在`len()`函数内部，它自动去调用该对象的`__len__()`方法，所以，下面的代码是等价的：

```py
>>> len('ABC')
3
>>> 'ABC'.__len__()
3
```
我们自己写的类，如果也想用len(myObj)的话，就自己写一个__len__()方法：

```py
>>> class MyDog(object):
...     def __len__(self):
...         return 100
...
>>> dog = MyDog()
>>> len(dog)
100
```
剩下的都是普通属性或方法，比如lower()返回小写的字符串：

```py
>>> 'ABC'.lower()
'abc'
```

仅仅把属性和方法列出来是不够的，配合`getattr()`、`setattr()`以及`hasattr()`，我们可以直接操作一个对象的状态：

```py
>>> class MyObject(object):
...     def __init__(self):
...         self.x = 9
...     def power(self):
...         return self.x * self.x
...
>>> obj = MyObject()
```
紧接着，可以测试该对象的属性：

```py
>>> hasattr(obj, 'x') # 有属性'x'吗？
True
>>> obj.x
9
>>> hasattr(obj, 'y') # 有属性'y'吗？
False
>>> setattr(obj, 'y', 19) # 设置一个属性'y'
>>> hasattr(obj, 'y') # 有属性'y'吗？
True
>>> getattr(obj, 'y') # 获取属性'y'
19
>>> obj.y # 获取属性'y'
19
```

如果试图获取不存在的属性，会抛出AttributeError的错误：

```py
>>> getattr(obj, 'z') # 获取属性'z'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'MyObject' object has no attribute 'z'
```

可以传入一个default参数，如果属性不存在，就返回默认值：

```py
>>> getattr(obj, 'z', 404) # 获取属性'z'，如果不存在，返回默认值404
404
```
也可以获得对象的方法：

```py
>>> hasattr(obj, 'power') # 有属性'power'吗？
True
>>> getattr(obj, 'power') # 获取属性'power'
<bound method MyObject.power of <__main__.MyObject object at 0x10077a6a0>>
>>> fn = getattr(obj, 'power') # 获取属性'power'并赋值到变量fn
>>> fn # fn指向obj.power
<bound method MyObject.power of <__main__.MyObject object at 0x10077a6a0>>
>>> fn() # 调用fn()与调用obj.power()是一样的
81
```

