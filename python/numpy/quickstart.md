
## fancy indexing and index  tricks

numpy 比普通的python序列提供了更多的索引方法，除了可以用整数或切片来索引，数组还可以通过整数数组，或者bool数组来索引。

### 整数索引数组

```py
>>> a = np.arange(12)**2
>>> a
array([  0,   1,   4,   9,  16,  25,  36,  49,  64,  81, 100, 121])
>>> i = np.array([1,1,3,8,5]) #下标数组
>>> a[i]
array([ 1,  1,  9, 64, 25])
>>> j = np.array([ [3,4], [9, 7]])
>>> j
array([[3, 4],
       [9, 7]])
>>> a[j] #j中的每一项都是下标，a[j]取出对应下标的值，构成和j一样形状的数组。
array([[ 9, 16],
       [81, 49]])

```

如果数组a是多维数组，一个单独的索引数组，索引的是a的第一维度。

如下： palette是一个二维数组，image是一个二维数组，image的每一项都索引palette的第一维度（一维数组），索引之后，image构成了一个三维数组。

```py
>>> palette = np.array( [ [0,0,0],
...                       [255,0,0],
...                       [0,255,0],
...                       [0,0,255],
...                       [255,255,255] ] )
>>> image = np.array( [ [0, 1, 2, 0], 
...                     [0, 3, 4, 0] ])
>>> palette[image]
array([[[  0,   0,   0],
        [255,   0,   0],
        [  0, 255,   0],
        [  0,   0,   0]],

       [[  0,   0,   0],
        [  0,   0, 255],
        [255, 255, 255],
        [  0,   0,   0]]])
```


我们可以给出多维索引数组，这个索引数组的每一维度必须是一样的形状。
```py
>>> a = np.arange(12).reshape(3,4)
>>> a
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
>>> i = np.array( [ [0,1],                        # indices for the first dim of a
...                 [1,2] ] )
>>> j = np.array( [ [2,1],                        # indices for the second dim
...                 [3,3] ] )
>>>
>>> a[i,j]                                     # i and j must have equal shape
array([[ 2,  5],
       [ 7, 11]])
>>>
>>> a[i,2]
array([[ 2,  6],
       [ 6, 10]])
>>>
>>> a[:,j]                                     # i.e., a[ : , j]
array([[[ 2,  1],
        [ 3,  3]],
       [[ 6,  5],
        [ 7,  7]],
       [[10,  9],
        [11, 11]]])
```

a[i,2]  相当于 
``` py
i = np.array[ [0,1], [1,2] ]
j = np.array[ [2,2], [2,2] ]
a [i, j]
``` 

a[ :, j ] 相当于
```py
i = [0:2] = [0, 1, 2] = [  [ [0,0], [0,0] ],
                           [ [1,1], [1,1] ],
                           [ [2,2], [2,2] ]  ]  
j =  np.array [ [2,2], [2,2] ]
a[i,j] = [ a[0,j], a[1,j], a[2,j] ]
```


自然地，我们可以将 i 和 j 放入一个序列中（比如list），之后通过这个list来进行索引。

```py
>>> l = [i,j]
>>> a[l]                                       # equivalent to a[i,j]
array([[ 2,  5],
       [ 7, 11]])
```

但是，我们不可以将 i 和 j 放入 array 中，因为这个数组会被解释为索引数组a的第一维度。

```py
>>> s = np.array( [i,j] )
>>> a[s]                                       # not what we want
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
IndexError: index (3) out of range (0<=index<=2) in dimension 0
>>>
>>> a[tuple(s)]                                # same as a[i,j]
array([[ 2,  5],
       [ 7, 11]])
```

Another common use of indexing with arrays is the search of the maximum value of time-dependent series:

```py
>>> time = np.linspace(20, 145, 5)                 # time scale
>>> data = np.sin(np.arange(20)).reshape(5,4)      # 4 time-dependent series
>>> time
array([  20.  ,   51.25,   82.5 ,  113.75,  145.  ])
>>> data
array([[ 0.        ,  0.84147098,  0.90929743,  0.14112001],
       [-0.7568025 , -0.95892427, -0.2794155 ,  0.6569866 ],
       [ 0.98935825,  0.41211849, -0.54402111, -0.99999021],
       [-0.53657292,  0.42016704,  0.99060736,  0.65028784],
       [-0.28790332, -0.96139749, -0.75098725,  0.14987721]])
>>>
>>> ind = data.argmax(axis=0)                  # index of the maxima for each series
>>> ind
array([2, 0, 3, 1])
>>>
>>> time_max = time[ind]                       # times corresponding to the maxima
>>>
>>> data_max = data[ind, range(data.shape[1])] # => data[ind[0],0], data[ind[1],1]...
>>>
>>> time_max
array([  82.5 ,   20.  ,  113.75,   51.25])
>>> data_max
array([ 0.98935825,  0.84147098,  0.99060736,  0.6569866 ])
>>>
>>> np.all(data_max == data.max(axis=0))
True
```

其中
- axis=0 取每一列的最大值
- axis=1 取每一行的最大值
```py
>>> data.shape
(5, 4)
>>> data.shape
(5, 4)
>>> data.shape[0]
5
>>> data.shape[1]
4
```



可以通过索引数组给对应的一系列元素赋值
```py
>>> a = np.arange(5)
>>> a
array([0, 1, 2, 3, 4])
>>> a[ [1,3,4] ] = 0
>>> a
array([0, 0, 2, 0, 0])
```

如果索引数组中包含重复的索引，赋值会多次执行,后面的赋值会替代前面的赋值
```py
>>> a = np.arange(5)
>>> a[[0,0,2]]=[1,2,3]
>>> a
array([2, 1, 3, 3, 4])
```
当使用`+=`运算符的时候，赋值不会多次执行
```py
>>> a = np.arange(5)
>>> a[[0,0,2]]+=1
>>> a
array([1, 1, 3, 3, 4])
```
> Even though 0 occurs twice in the list of indices, the 0th element is only incremented once. This is because Python requires “a+=1” to be equivalent to “a = a + 1”.


## bool 索引数组

当我们通过整数数组来索引的时候，我们通过给出索引来选择元素。但是使用bool来索引的时候，我们明确的说明那些元素是我们需要的，那些是我们不需要的。

人们可以想到的最自然的bool索引方法是使用与原始数组具有相同形状的布尔数组。


```py
>>> a = np.arange(12).reshape(3,4)
>>> b = a > 4
>>> b                                          # b is a boolean with a's shape
array([[False, False, False, False],
       [False,  True,  True,  True],
       [ True,  True,  True,  True]])
>>> a[b]                                       # 1d array with the selected elements
array([ 5,  6,  7,  8,  9, 10, 11])
```

布尔索引在赋值的时候非常有用

```py
>>> a[b] = 0                                   # All elements of 'a' higher than 4 become 0
>>> a
array([[0, 1, 2, 3],
       [4, 0, 0, 0],
       [0, 0, 0, 0]])
```

布尔索引可以像整数索引那样使用

```py
>>> a = np.arange(12).reshape(3,4)
>>> b1 = np.array([False,True,True])             # first dim selection
>>> b2 = np.array([True,False,True,False])       # second dim selection
>>>
>>> a[b1,:]                                   # selecting rows
array([[ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
>>>
>>> a[b1]                                     # same thing
array([[ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
>>>
>>> a[:,b2]                                   # selecting columns
array([[ 0,  2],
       [ 4,  6],
       [ 8, 10]])
>>>
>>> a[b1,b2]                                  # a weird thing to do
array([ 4, 10])
```