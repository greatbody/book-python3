# 第7章：元组（Tuple）

## 7.1 元组的创建和特性

元组是 Python 中的不可变序列类型，与列表类似，但是一旦创建就不能修改。

### 7.1.1 元组的创建

```python
# 空元组
empty_tuple = ()
empty_tuple2 = tuple()

# 有元素的元组
numbers = (1, 2, 3, 4, 5)
fruits = ("apple", "banana", "orange")
mixed = (1, "hello", 3.14, True)

# 单个元素的元组（注意逗号）
single_element = (42,)  # 正确
not_a_tuple = (42)      # 这是一个整数

print(type(single_element))  # <class 'tuple'>
print(type(not_a_tuple))     # <class 'int'>

# 不使用括号（元组打包）
coordinates = 10, 20, 30
print(coordinates)  # (10, 20, 30)
print(type(coordinates))  # <class 'tuple'>

# 使用 tuple() 函数转换
list_to_tuple = tuple([1, 2, 3, 4])
string_to_tuple = tuple("hello")
range_to_tuple = tuple(range(5))

print(list_to_tuple)   # (1, 2, 3, 4)
print(string_to_tuple) # ('h', 'e', 'l', 'l', 'o')
print(range_to_tuple)  # (0, 1, 2, 3, 4)
```

### 7.1.2 元组的不可变性

```python
numbers = (1, 2, 3, 4, 5)

# 尝试修改元素（会报错）
# numbers[0] = 10  # TypeError: 'tuple' object does not support item assignment

# 尝试添加元素（会报错）
# numbers.append(6)  # AttributeError: 'tuple' object has no attribute 'append'

# 但是可以包含可变对象
mutable_tuple = ([1, 2], [3, 4])
mutable_tuple[0].append(99)  # 可以修改列表内容
print(mutable_tuple)  # ([1, 2, 99], [3, 4])

# 元组本身仍然不可变
# mutable_tuple[0] = [5, 6]  # TypeError: 'tuple' object does not support item assignment
```

### 7.1.3 元组的基本操作

```python
fruits = ("apple", "banana", "orange", "grape")

# 访问元素（与列表相同）
print(fruits[0])    # apple
print(fruits[-1])   # grape
print(fruits[1:3])  # ('banana', 'orange')

# 长度
print(len(fruits))  # 4

# 成员测试
print("apple" in fruits)      # True
print("pineapple" in fruits)  # False

# 计数和索引
numbers = (1, 2, 3, 2, 4, 2)
print(numbers.count(2))  # 3
print(numbers.index(3))  # 2

# 连接
tuple1 = (1, 2, 3)
tuple2 = (4, 5, 6)
combined = tuple1 + tuple2
print(combined)  # (1, 2, 3, 4, 5, 6)

# 重复
repeated = tuple1 * 3
print(repeated)  # (1, 2, 3, 1, 2, 3, 1, 2, 3)

# 比较
print((1, 2, 3) < (1, 2, 4))  # True (按字典序比较)
print((1, 2) == (1, 2))       # True
```

### 7.1.4 元组的遍历

```python
coordinates = (10, 20, 30)

# for 循环遍历
for coord in coordinates:
    print(coord)

# 带索引遍历
for index, coord in enumerate(coordinates):
    print(f"坐标 {index}: {coord}")

# 同时遍历多个元组
points = [(1, 2), (3, 4), (5, 6)]
for x, y in points:
    print(f"点: ({x}, {y})")
```

## 7.2 元组解包

元组解包是将元组的元素分配给多个变量的过程。

### 7.2.1 基本解包

```python
# 基本元组解包
point = (10, 20)
x, y = point
print(x)  # 10
print(y)  # 20

# 解包不同类型的元素
person = ("Alice", 25, "Engineer")
name, age, job = person
print(f"{name} is {age} years old and works as {job}")

# 交换变量（利用解包）
a = 1
b = 2
a, b = b, a
print(a, b)  # 2 1
```

### 7.2.2 扩展解包

```python
# 使用 * 收集剩余元素
numbers = (1, 2, 3, 4, 5)
first, *middle, last = numbers
print(first)   # 1
print(middle)  # [2, 3, 4]
print(last)    # 5

# 只收集开头或结尾
*a, b, c = numbers
print(a)  # [1, 2, 3]
print(b)  # 4
print(c)  # 5

# 忽略不需要的值
name, _, _, job = ("Alice", 25, "Female", "Engineer")
print(name)  # Alice
print(job)   # Engineer
```

### 7.2.3 嵌套元组解包

```python
# 解包嵌套元组
nested = ((1, 2), (3, 4))
(a, b), (c, d) = nested
print(a, b, c, d)  # 1 2 3 4

# 解包坐标列表
points = [(1, 2), (3, 4), (5, 6)]
for x, y in points:
    print(f"x={x}, y={y}")

# 解包字典项
person = {"name": "Bob", "age": 30}
for key, value in person.items():
    print(f"{key}: {value}")
```

### 7.2.4 函数返回值解包

```python
# 函数返回元组
def get_user_info():
    return "Alice", 25, "Engineer"

name, age, job = get_user_info()
print(f"Name: {name}, Age: {age}, Job: {job}")

# 内置函数返回值的解包
text = "hello world"
first_word, second_word = text.split()[:2]
print(first_word, second_word)  # hello world

# divmod 函数
quotient, remainder = divmod(17, 5)
print(f"17 ÷ 5 = {quotient} ... {remainder}")  # 17 ÷ 5 = 3 ... 2
```

## 7.3 命名元组

命名元组是 collections.namedtuple 的扩展，它允许通过名称访问元组元素。

### 7.3.1 创建命名元组

```python
from collections import namedtuple

# 定义命名元组类型
Point = namedtuple('Point', ['x', 'y'])
Person = namedtuple('Person', 'name age job')

# 创建实例
p1 = Point(10, 20)
p2 = Point(x=30, y=40)

person1 = Person('Alice', 25, 'Engineer')
person2 = Person(name='Bob', age=30, job='Designer')

print(p1)       # Point(x=10, y=20)
print(person1)  # Person(name='Alice', age=25, job='Engineer')
```

### 7.3.2 访问命名元组元素

```python
Point = namedtuple('Point', ['x', 'y', 'z'])

point = Point(1, 2, 3)

# 通过名称访问
print(point.x)  # 1
print(point.y)  # 2
print(point.z)  # 3

# 通过索引访问（仍然支持）
print(point[0])  # 1
print(point[-1]) # 3

# 解包
x, y, z = point
print(x, y, z)  # 1 2 3
```

### 7.3.3 命名元组的方法

```python
Point = namedtuple('Point', ['x', 'y'])

p1 = Point(1, 2)
p2 = Point(3, 4)

# 转换为字典
print(p1._asdict())  # {'x': 1, 'y': 2}

# 替换字段值（返回新实例）
p3 = p1._replace(x=10)
print(p1)  # Point(x=1, y=2)
print(p3)  # Point(x=10, y=2)

# 获取字段名
print(p1._fields)  # ('x', 'y')

# 创建新实例
values = (5, 6)
p4 = Point._make(values)
print(p4)  # Point(x=5, y=6)
```

### 7.3.4 实际应用场景

```python
# 表示坐标点
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])

def distance(p1, p2):
    return ((p1.x - p2.x) ** 2 + (p1.y - p2.y) ** 2) ** 0.5

p1 = Point(0, 0)
p2 = Point(3, 4)
print(distance(p1, p2))  # 5.0

# 表示学生信息
Student = namedtuple('Student', 'name age grade major')

students = [
    Student('Alice', 20, 'A', 'Computer Science'),
    Student('Bob', 21, 'B', 'Mathematics'),
    Student('Charlie', 19, 'A', 'Physics')
]

# 按年龄排序
sorted_students = sorted(students, key=lambda s: s.age)
for student in sorted_students:
    print(f"{student.name}: {student.age} years old")

# 表示 RGB 颜色
Color = namedtuple('Color', ['red', 'green', 'blue'])

def mix_colors(c1, c2):
    return Color(
        (c1.red + c2.red) // 2,
        (c1.green + c2.green) // 2,
        (c1.blue + c2.blue) // 2
    )

red = Color(255, 0, 0)
blue = Color(0, 0, 255)
purple = mix_colors(red, blue)
print(purple)  # Color(red=127, green=0, blue=127)
```

### 7.3.5 命名元组 vs 字典 vs 类

```python
from collections import namedtuple

# 使用命名元组
Person = namedtuple('Person', 'name age job')
person_nt = Person('Alice', 25, 'Engineer')

# 使用字典
person_dict = {'name': 'Alice', 'age': 25, 'job': 'Engineer'}

# 使用类
class PersonClass:
    def __init__(self, name, age, job):
        self.name = name
        self.age = age
        self.job = job

person_class = PersonClass('Alice', 25, 'Engineer')

# 比较访问方式
print(person_nt.name)      # Alice (清晰的属性访问)
print(person_dict['name']) # Alice (需要键访问)
print(person_class.name)   # Alice (属性访问)

# 比较内存使用
import sys
print(sys.getsizeof(person_nt))    # 较小
print(sys.getsizeof(person_dict))  # 较大
print(sys.getsizeof(person_class)) # 中等

# 不可变性
# person_nt.age = 26  # AttributeError (不可变)
person_dict['age'] = 26    # 可以修改
person_class.age = 26      # 可以修改
```

## 总结

本章介绍了 Python 的元组数据结构：
- 元组的创建和不可变特性
- 元组的基本操作和方法
- 强大的元组解包功能
- 命名元组的高级用法

通过本章的学习，你应该能够：
1. 理解元组与列表的区别和适用场景
2. 熟练使用元组解包简化代码
3. 创建和使用命名元组
4. 选择合适的不可变数据结构

下一章将介绍字典，这是另一种重要的数据结构。
