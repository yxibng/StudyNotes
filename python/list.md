1. Lists
	
	```python
	>>> squares = [1, 4, 9, 16, 25]
	>>> squares
	[1, 4, 9, 16, 25]
	```
	
2. 数组下标,下标可以为负
	
	```python
	>>> squares[0]  # indexing returns the item
	1
	>>> squares[-1]
	25
	```

3. 数组是可变的
	
	```python
	>>> cubes = [1, 8, 27, 65, 125]  # something's wrong here
	>>> 4 ** 3  # the cube of 4 is 64, not 65!
	64
	>>> cubes[3] = 64  # replace the wrong value
	>>> cubes
	[1, 8, 27, 64, 125]
	>>> cubes.append(216)  # add the cube of 6
	>>> cubes.append(7 ** 3)  # and the cube of 7
	>>> cubes
	[1, 8, 27, 64, 125, 216, 343]
	```

4. 数组切片
	
	```python
	>>> squares[-3:]  # slicing returns a new list
	[9, 16, 25]
	>>> squares + [36, 49, 64, 81, 100]
	[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
	```

	给切片赋值是允许的,这个可以改变原始数组,甚至清空数组
	
	```python
	>>> letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g']
	>>> letters
	['a', 'b', 'c', 'd', 'e', 'f', 'g']
	>>> # replace some values
	>>> letters[2:5] = ['C', 'D', 'E']
	>>> letters
	['a', 'b', 'C', 'D', 'E', 'f', 'g']
	>>> # now remove them
	>>> letters[2:5] = []
	>>> letters
	['a', 'b', 'f', 'g']
	>>> # clear the list by replacing all the elements with an empty list
	>>> letters[:] = []
	>>> letters
	[]
	```
5. `len()`求数组的长度

	```python
	>>> letters = ['a', 'b', 'c', 'd']
	>>> len(letters)
	4
	```
	
6. 数组可以嵌套数组

	```python
	>>> a = ['a', 'b', 'c']
	>>> n = [1, 2, 3]
	>>> x = [a, n]
	>>> x
	[['a', 'b', 'c'], [1, 2, 3]]
	>>> x[0]
	['a', 'b', 'c']
	>>> x[0][1]
	'b'
	```
