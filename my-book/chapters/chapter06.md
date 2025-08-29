# 第6章：列表（List）

## 6.1 列表的创建和访问

列表是 Python 中最常用的数据结构之一，用于存储有序的可变元素集合。

### 6.1.1 列表的创建

```python
# 空列表
empty_list = []
empty_list2 = list()

# 有元素的列表
numbers = [1, 2, 3, 4, 5]
fruits = ["apple", "banana", "orange"]
mixed = [1, "hello", 3.14, True]

# 使用 list() 函数转换
string_to_list = list("hello")  # ['h', 'e', 'l', 'l', 'o']
tuple_to_list = list((1, 2, 3))  # [1, 2, 3]
range_to_list = list(range(5))   # [0, 1, 2, 3, 4]

# 重复元素
zeros = [0] * 5  # [0, 0, 0, 0, 0]
stars = ["*"] * 3  # ['*', '*', '*']
```

### 6.1.2 列表的访问

```python
fruits = ["apple", "banana", "orange", "grape", "kiwi"]

# 索引访问（从0开始）
print(fruits[0])   # apple
print(fruits[2])   # orange
print(fruits[-1])  # kiwi (最后一个元素)
print(fruits[-2])  # grape (倒数第二个元素)

# 切片访问
print(fruits[1:4])   # ['banana', 'orange', 'grape']
print(fruits[:3])    # ['apple', 'banana', 'orange'] (前3个)
print(fruits[2:])    # ['orange', 'grape', 'kiwi'] (从索引2到末尾)
print(fruits[::2])   # ['apple', 'orange', 'kiwi'] (步长为2)
print(fruits[::-1])  # ['kiwi', 'grape', 'orange', 'banana', 'apple'] (反转)

# 检查元素是否存在
print("apple" in fruits)     # True
print("pineapple" in fruits) # False
print("apple" not in fruits) # False
```

### 6.1.3 列表的长度和遍历

```python
numbers = [10, 20, 30, 40, 50]

# 获取长度
print(len(numbers))  # 5

# 遍历元素
for num in numbers:
    print(num)

# 遍历索引和元素
for index, num in enumerate(numbers):
    print(f"索引 {index}: {num}")

# 遍历时获取索引（传统方式）
for i in range(len(numbers)):
    print(f"索引 {i}: {numbers[i]}")

# 反向遍历
for num in reversed(numbers):
    print(num)
```

## 6.2 列表操作

### 6.2.1 添加元素

```python
fruits = ["apple", "banana"]

# append() - 在末尾添加单个元素
fruits.append("orange")
print(fruits)  # ['apple', 'banana', 'orange']

# insert() - 在指定位置插入元素
fruits.insert(1, "grape")
print(fruits)  # ['apple', 'grape', 'banana', 'orange']

# extend() - 扩展列表（添加多个元素）
fruits.extend(["kiwi", "melon"])
print(fruits)  # ['apple', 'grape', 'banana', 'orange', 'kiwi', 'melon']

# 使用 + 运算符连接列表
more_fruits = fruits + ["pear", "plum"]
print(more_fruits)

# 使用 *= 运算符重复列表
repeated = ["A"] * 3
print(repeated)  # ['A', 'A', 'A']
```

### 6.2.2 删除元素

```python
numbers = [1, 2, 3, 2, 4, 2, 5]

# remove() - 删除第一个匹配的元素
numbers.remove(2)
print(numbers)  # [1, 3, 2, 4, 2, 5]

# pop() - 删除并返回指定位置的元素（默认最后一个）
last_item = numbers.pop()
print(last_item)  # 5
print(numbers)    # [1, 3, 2, 4, 2]

# 删除指定位置的元素
second_item = numbers.pop(1)
print(second_item)  # 3
print(numbers)      # [1, 2, 4, 2]

# del 语句 - 删除指定位置或切片的元素
del numbers[0]      # 删除第一个元素
print(numbers)      # [2, 4, 2]

del numbers[1:3]    # 删除索引1到2的元素
print(numbers)      # [2]

# clear() - 清空列表
numbers.clear()
print(numbers)      # []
```

### 6.2.3 修改元素

```python
fruits = ["apple", "banana", "orange"]

# 直接修改指定位置的元素
fruits[1] = "grape"
print(fruits)  # ['apple', 'grape', 'orange']

# 修改切片
fruits[0:2] = ["kiwi", "melon"]
print(fruits)  # ['kiwi', 'melon', 'orange']

# 使用切片替换多个元素
numbers = [1, 2, 3, 4, 5]
numbers[1:4] = [20, 30]
print(numbers)  # [1, 20, 30, 5]

# 替换为不同数量的元素
numbers[1:3] = [100, 200, 300, 400]
print(numbers)  # [1, 100, 200, 300, 400, 5]
```

### 6.2.4 查找和计数

```python
numbers = [1, 2, 3, 2, 4, 2, 5]

# index() - 查找元素第一次出现的索引
print(numbers.index(2))  # 1

# 指定查找范围
print(numbers.index(2, 2))  # 从索引2开始查找，找到索引3

# count() - 统计元素出现次数
print(numbers.count(2))  # 3
print(numbers.count(6))  # 0

# 检查元素是否存在
if 3 in numbers:
    print("3 在列表中")

# 查找最大值和最小值
scores = [85, 92, 78, 95, 88]
print(max(scores))  # 95
print(min(scores))  # 78

# 查找最大值/最小值的索引
print(scores.index(max(scores)))  # 3
print(scores.index(min(scores)))  # 2
```

## 6.3 列表方法

### 6.3.1 排序方法

```python
numbers = [3, 1, 4, 1, 5, 9, 2, 6]

# sort() - 原地排序（修改原列表）
numbers.sort()
print(numbers)  # [1, 1, 2, 3, 4, 5, 6, 9]

# 降序排序
numbers.sort(reverse=True)
print(numbers)  # [9, 6, 5, 4, 3, 2, 1, 1]

# sorted() - 返回新列表，不修改原列表
original = [3, 1, 4, 1, 5]
sorted_list = sorted(original)
print(original)     # [3, 1, 4, 1, 5] (原列表不变)
print(sorted_list)  # [1, 1, 3, 4, 5]

# 按绝对值排序
numbers = [-3, -1, 4, -2, 5]
numbers.sort(key=abs)
print(numbers)  # [-1, -2, -3, 4, 5]

# 按字符串长度排序
words = ["apple", "a", "banana", "cat"]
words.sort(key=len)
print(words)  # ['a', 'cat', 'apple', 'banana']
```

### 6.3.2 反转方法

```python
fruits = ["apple", "banana", "orange"]

# reverse() - 原地反转
fruits.reverse()
print(fruits)  # ['orange', 'banana', 'apple']

# reversed() - 返回反转的迭代器
original = ["a", "b", "c", "d"]
reversed_list = list(reversed(original))
print(original)      # ['a', 'b', 'c', 'd'] (原列表不变)
print(reversed_list) # ['d', 'c', 'b', 'a']

# 使用切片反转
reversed_slice = original[::-1]
print(reversed_slice)  # ['d', 'c', 'b', 'a']
```

### 6.3.3 复制方法

```python
original = [1, 2, 3, 4, 5]

# copy() - 浅拷贝
copied = original.copy()
print(copied)  # [1, 2, 3, 4, 5]

# list() - 创建新列表
new_list = list(original)
print(new_list)  # [1, 2, 3, 4, 5]

# 切片复制
sliced_copy = original[:]
print(sliced_copy)  # [1, 2, 3, 4, 5]

# 浅拷贝 vs 深拷贝
nested = [[1, 2], [3, 4]]
shallow = nested.copy()

shallow[0][0] = 999
print(nested)  # [[999, 2], [3, 4]] (原列表也被修改)

# 深拷贝
import copy
deep = copy.deepcopy(nested)
deep[0][0] = 888
print(nested)  # [[999, 2], [3, 4]] (原列表不变)
print(deep)    # [[888, 2], [3, 4]]
```

## 6.4 列表推导式

列表推导式是一种简洁的创建列表的方法。

### 6.4.1 基本语法

```python
# 传统方式
squares = []
for x in range(1, 6):
    squares.append(x ** 2)
print(squares)  # [1, 4, 9, 16, 25]

# 列表推导式
squares = [x ** 2 for x in range(1, 6)]
print(squares)  # [1, 4, 9, 16, 25]

# 语法结构
# [expression for item in iterable]
```

### 6.4.2 带条件的列表推导式

```python
# 带条件的列表推导式
# [expression for item in iterable if condition]

# 筛选偶数
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
even_numbers = [x for x in numbers if x % 2 == 0]
print(even_numbers)  # [2, 4, 6, 8, 10]

# 筛选字符串
words = ["apple", "banana", "cat", "dog", "elephant"]
long_words = [word for word in words if len(word) > 3]
print(long_words)  # ['apple', 'banana', 'elephant']

# 多个条件
numbers = range(1, 21)
special_numbers = [x for x in numbers if x % 2 == 0 and x % 3 == 0]
print(special_numbers)  # [6, 12, 18]
```

### 6.4.3 嵌套列表推导式

```python
# 嵌套循环的传统方式
matrix = []
for i in range(3):
    row = []
    for j in range(3):
        row.append(i * 3 + j)
    matrix.append(row)
print(matrix)  # [[0, 1, 2], [3, 4, 5], [6, 7, 8]]

# 嵌套列表推导式
matrix = [[i * 3 + j for j in range(3)] for i in range(3)]
print(matrix)  # [[0, 1, 2], [3, 4, 5], [6, 7, 8]]

# 展平嵌套列表
nested = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flattened = [x for row in nested for x in row]
print(flattened)  # [1, 2, 3, 4, 5, 6, 7, 8, 9]

# 带条件的嵌套推导式
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
even_from_matrix = [x for row in matrix for x in row if x % 2 == 0]
print(even_from_matrix)  # [2, 4, 6, 8]
```

### 6.4.4 实际应用

```python
# 数据转换
temperatures_c = [0, 10, 20, 30, 40]
temperatures_f = [(c * 9/5) + 32 for c in temperatures_c]
print(temperatures_f)  # [32.0, 50.0, 68.0, 86.0, 104.0]

# 字符串处理
text = "Hello World Python"
words = text.split()
word_lengths = [len(word) for word in words]
print(word_lengths)  # [5, 5, 6]

# 文件处理
with open("data.txt", "r") as file:
    lines = [line.strip() for line in file if line.strip()]
    print(lines)

# 创建字典
keys = ["name", "age", "city"]
values = ["Alice", 25, "Beijing"]
person = {keys[i]: values[i] for i in range(len(keys))}
print(person)  # {'name': 'Alice', 'age': 25, 'city': 'Beijing'}

# 集合推导式
numbers = [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]
unique_squares = {x**2 for x in numbers}
print(unique_squares)  # {1, 4, 9, 16}
```

## 6.5 多维列表

### 6.5.1 二维列表

```python
# 创建二维列表
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

# 访问元素
print(matrix[0][0])  # 1 (第一行第一列)
print(matrix[1][2])  # 6 (第二行第三列)
print(matrix[2][1])  # 8 (第三行第二列)

# 遍历二维列表
for row in matrix:
    for element in row:
        print(element, end=" ")
    print()  # 换行

# 使用索引遍历
for i in range(len(matrix)):
    for j in range(len(matrix[i])):
        print(f"matrix[{i}][{j}] = {matrix[i][j]}")
```

### 6.5.2 不规则二维列表

```python
# 不规则的二维列表
irregular = [
    [1, 2],
    [3, 4, 5, 6],
    [7, 8, 9]
]

# 安全访问
for i, row in enumerate(irregular):
    for j, element in enumerate(row):
        print(f"[{i}][{j}]: {element}")

# 获取每行的长度
row_lengths = [len(row) for row in irregular]
print(row_lengths)  # [2, 4, 3]
```

### 6.5.3 三维列表

```python
# 创建三维列表（立方体）
cube = [
    [[1, 2], [3, 4]],
    [[5, 6], [7, 8]]
]

# 访问三维列表
print(cube[0][0][0])  # 1
print(cube[1][1][1])  # 8

# 遍历三维列表
for i, layer in enumerate(cube):
    print(f"第 {i+1} 层:")
    for j, row in enumerate(layer):
        for k, element in enumerate(row):
            print(f"  [{i}][{j}][{k}]: {element}")
```

### 6.5.4 动态创建多维列表

```python
# 创建 n x m 的矩阵
def create_matrix(rows, cols, default_value=0):
    return [[default_value for _ in range(cols)] for _ in range(rows)]

matrix = create_matrix(3, 4, 0)
print(matrix)  # [[0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]]

# 创建单位矩阵
def identity_matrix(n):
    return [[1 if i == j else 0 for j in range(n)] for i in range(n)]

identity = identity_matrix(3)
for row in identity:
    print(row)

# 创建杨辉三角
def pascal_triangle(n):
    triangle = [[1]]
    for i in range(1, n):
        row = [1]
        for j in range(1, i):
            row.append(triangle[i-1][j-1] + triangle[i-1][j])
        row.append(1)
        triangle.append(row)
    return triangle

p_triangle = pascal_triangle(5)
for row in p_triangle:
    print(row)
```

## 总结

本章介绍了 Python 的列表数据结构：
- 列表的创建、访问和基本操作
- 列表的添加、删除、修改方法
- 常用的列表方法（排序、反转、复制）
- 列表推导式的强大功能
- 多维列表的使用

通过本章的学习，你应该能够：
1. 熟练创建和操作列表
2. 使用各种列表方法处理数据
3. 编写简洁的列表推导式
4. 处理多维数据结构
5. 理解列表的引用特性

下一章将介绍元组，这是另一种重要的序列类型。
