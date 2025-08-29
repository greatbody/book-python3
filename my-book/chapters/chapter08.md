# 第8章：字典（Dictionary）

## 8.1 字典的创建和访问

字典是 Python 中最强大的数据结构之一，用于存储键值对映射关系。

### 8.1.1 字典的创建

```python
# 空字典
empty_dict = {}
empty_dict2 = dict()

# 有元素的字典
person = {
    "name": "Alice",
    "age": 25,
    "city": "Beijing"
}

# 使用不同类型的键
mixed_dict = {
    "string_key": "value",
    42: "number_key",
    (1, 2): "tuple_key",
    # [1, 2]: "list_key"  # 错误：列表不能作为键
}

# 使用 dict() 函数创建
dict_from_pairs = dict([("name", "Bob"), ("age", 30)])
dict_from_kwargs = dict(name="Charlie", age=35, city="Shanghai")

print(dict_from_pairs)  # {'name': 'Bob', 'age': 30}
print(dict_from_kwargs) # {'name': 'Charlie', 'age': 35, 'city': 'Shanghai'}

# 从其他映射创建
original = {"a": 1, "b": 2}
copied = dict(original)
print(copied)  # {'a': 1, 'b': 2}
```

### 8.1.2 字典的访问

```python
person = {
    "name": "Alice",
    "age": 25,
    "city": "Beijing",
    "job": "Engineer"
}

# 通过键访问值
print(person["name"])   # Alice
print(person["age"])    # 25

# 使用 get() 方法（更安全）
print(person.get("name"))     # Alice
print(person.get("salary"))   # None (键不存在时返回 None)
print(person.get("salary", 0)) # 0 (指定默认值)

# 检查键是否存在
if "name" in person:
    print("姓名:", person["name"])

if "salary" not in person:
    print("薪资信息不存在")

# 获取所有键、值和键值对
print(person.keys())    # dict_keys(['name', 'age', 'city', 'job'])
print(person.values())  # dict_values(['Alice', 25, 'Beijing', 'Engineer'])
print(person.items())   # dict_items([('name', 'Alice'), ('age', 25), ('city', 'Beijing'), ('job', 'Engineer')])
```

### 8.1.3 字典的遍历

```python
student = {
    "name": "Bob",
    "age": 20,
    "grades": {"math": 85, "english": 92, "physics": 78}
}

# 遍历键
for key in student:
    print(f"键: {key}")

# 遍历值
for value in student.values():
    print(f"值: {value}")

# 遍历键值对
for key, value in student.items():
    print(f"{key}: {value}")

# 同时遍历键和值（传统方式）
for key in student:
    print(f"{key}: {student[key]}")

# 枚举索引和键值对
for index, (key, value) in enumerate(student.items()):
    print(f"{index}: {key} = {value}")
```

## 8.2 字典操作

### 8.2.1 添加和修改元素

```python
person = {"name": "Alice", "age": 25}

# 添加新键值对
person["city"] = "Beijing"
person["job"] = "Engineer"
print(person)  # {'name': 'Alice', 'age': 25, 'city': 'Beijing', 'job': 'Engineer'}

# 修改现有值
person["age"] = 26
print(person["age"])  # 26

# 使用 update() 方法批量更新
person.update({"age": 27, "salary": 50000, "department": "IT"})
print(person)

# 使用 setdefault() 设置默认值
person.setdefault("hobbies", [])
person["hobbies"].append("reading")
print(person["hobbies"])  # ['reading']

# 如果键已存在，setdefault 不改变值
person.setdefault("name", "Default Name")
print(person["name"])  # Alice (保持原值)
```

### 8.2.2 删除元素

```python
data = {
    "name": "Alice",
    "age": 25,
    "city": "Beijing",
    "job": "Engineer",
    "salary": 50000
}

# 使用 del 关键字删除
del data["salary"]
print(data)

# 使用 pop() 删除并返回值
removed_value = data.pop("job")
print(f"删除的值: {removed_value}")
print(data)

# pop() 指定默认值（键不存在时）
default_value = data.pop("department", "Not Found")
print(default_value)  # Not Found

# 使用 popitem() 删除最后一个键值对
last_item = data.popitem()
print(f"删除的最后一项: {last_item}")
print(data)

# 清空字典
data.clear()
print(data)  # {}
```

### 8.2.3 字典的合并

```python
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}
dict3 = {"a": 10, "e": 5}

# 使用 update() 合并（修改原字典）
dict1_copy = dict1.copy()
dict1_copy.update(dict2)
print(dict1_copy)  # {'a': 1, 'b': 2, 'c': 3, 'd': 4}

# 合并时处理重复键（后面的覆盖前面的）
combined = dict1.copy()
combined.update(dict3)
print(combined)  # {'a': 10, 'b': 2, 'e': 5}

# Python 3.9+ 的合并运算符
merged = dict1 | dict2 | dict3
print(merged)  # {'a': 10, 'b': 2, 'c': 3, 'd': 4, 'e': 5}

# 使用 ** 解包合并
merged2 = {**dict1, **dict2, **dict3}
print(merged2)  # {'a': 10, 'b': 2, 'c': 3, 'd': 4, 'e': 5}
```

## 8.3 字典方法

### 8.3.1 查找和统计

```python
inventory = {
    "apples": 10,
    "bananas": 5,
    "oranges": 8,
    "grapes": 3
}

# 检查键是否存在
print("apples" in inventory)      # True
print("pineapples" in inventory)  # False

# 获取值（带默认值）
print(inventory.get("apples", 0))   # 10
print(inventory.get("pineapples", 0)) # 0

# 统计元素数量
print(len(inventory))  # 4

# 检查是否为空
print(bool(inventory))  # True
print(bool({}))         # False
```

### 8.3.2 视图对象

```python
person = {
    "name": "Alice",
    "age": 25,
    "city": "Beijing"
}

# 获取视图对象
keys_view = person.keys()
values_view = person.values()
items_view = person.items()

print(keys_view)   # dict_keys(['name', 'age', 'city'])
print(values_view) # dict_values(['Alice', 25, 'Beijing'])
print(items_view)  # dict_items([('name', 'Alice'), ('age', 25), ('city', 'Beijing')])

# 视图对象是动态的
person["job"] = "Engineer"
print(keys_view)   # dict_keys(['name', 'age', 'city', 'job'])

# 转换为其他类型
keys_list = list(keys_view)
values_list = list(values_view)
items_list = list(items_view)

print(keys_list)   # ['name', 'age', 'city', 'job']
print(values_list) # ['Alice', 25, 'Beijing', 'Engineer']
```

### 8.3.3 复制和比较

```python
original = {"a": 1, "b": [1, 2, 3]}

# 浅拷贝
shallow_copy = original.copy()
shallow_copy2 = dict(original)

# 深拷贝
import copy
deep_copy = copy.deepcopy(original)

# 修改原字典
original["b"].append(4)

print(original)     # {'a': 1, 'b': [1, 2, 3, 4]}
print(shallow_copy) # {'a': 1, 'b': [1, 2, 3, 4]} (受影响)
print(deep_copy)    # {'a': 1, 'b': [1, 2, 3]} (不受影响)

# 字典比较
dict1 = {"a": 1, "b": 2}
dict2 = {"a": 1, "b": 2}
dict3 = {"b": 2, "a": 1}

print(dict1 == dict2)  # True (值相等)
print(dict1 == dict3)  # True (键值对相同，顺序无关)
print(dict1 is dict2)  # False (不同对象)
```

## 8.4 字典推导式

字典推导式是一种创建字典的简洁方式。

### 8.4.1 基本语法

```python
# 基本字典推导式
# {key_expression: value_expression for item in iterable}

# 创建平方映射
numbers = [1, 2, 3, 4, 5]
squares = {x: x**2 for x in numbers}
print(squares)  # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# 从列表创建字典
fruits = ["apple", "banana", "orange"]
fruit_lengths = {fruit: len(fruit) for fruit in fruits}
print(fruit_lengths)  # {'apple': 5, 'banana': 6, 'orange': 6}

# 从字符串创建字符计数
text = "hello world"
char_count = {char: text.count(char) for char in set(text)}
print(char_count)  # {'h': 1, 'e': 1, 'l': 3, 'o': 2, ' ': 1, 'w': 1, 'r': 1, 'd': 1}
```

### 8.4.2 带条件的字典推导式

```python
# 带条件的字典推导式
# {key: value for item in iterable if condition}

# 筛选偶数平方
numbers = range(1, 11)
even_squares = {x: x**2 for x in numbers if x % 2 == 0}
print(even_squares)  # {2: 4, 4: 16, 6: 36, 8: 64, 10: 100}

# 筛选长单词
words = ["apple", "a", "banana", "cat", "elephant"]
long_words = {word: len(word) for word in words if len(word) > 3}
print(long_words)  # {'apple': 5, 'banana': 6, 'elephant': 8}

# 多重条件
scores = {"Alice": 85, "Bob": 92, "Charlie": 78, "David": 95}
passed_students = {name: score for name, score in scores.items() if score >= 80}
print(passed_students)  # {'Alice': 85, 'Bob': 92, 'David': 95}
```

### 8.4.3 复杂的字典推导式

```python
# 从两个列表创建字典
keys = ["name", "age", "city"]
values = ["Alice", 25, "Beijing"]
person = {keys[i]: values[i] for i in range(len(keys))}
print(person)  # {'name': 'Alice', 'age': 25, 'city': 'Beijing'}

# 使用 zip() 函数
person2 = {key: value for key, value in zip(keys, values)}
print(person2)  # {'name': 'Alice', 'age': 25, 'city': 'Beijing'}

# 反转字典
original = {"a": 1, "b": 2, "c": 3}
reversed_dict = {value: key for key, value in original.items()}
print(reversed_dict)  # {1: 'a', 2: 'b', 3: 'c'}

# 合并多个字典
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}
merged = {**dict1, **dict2}
print(merged)  # {'a': 1, 'b': 2, 'c': 3, 'd': 4}
```

### 8.4.4 实际应用

```python
# 创建学生成绩统计
students = [
    {"name": "Alice", "math": 85, "english": 92},
    {"name": "Bob", "math": 78, "english": 88},
    {"name": "Charlie", "math": 95, "english": 85}
]

# 计算每个学生的平均分
averages = {
    student["name"]: (student["math"] + student["english"]) / 2
    for student in students
}
print(averages)  # {'Alice': 88.5, 'Bob': 83.0, 'Charlie': 90.0}

# 创建成绩等级映射
def get_grade(score):
    if score >= 90:
        return "A"
    elif score >= 80:
        return "B"
    elif score >= 70:
        return "C"
    else:
        return "D"

math_grades = {
    student["name"]: get_grade(student["math"])
    for student in students
}
print(math_grades)  # {'Alice': 'B', 'Bob': 'C', 'Charlie': 'A'}

# 文件扩展名统计
import os
files = ["document.txt", "image.jpg", "script.py", "data.csv", "readme.md"]

extension_count = {}
for file in files:
    ext = os.path.splitext(file)[1]
    extension_count[ext] = extension_count.get(ext, 0) + 1

print(extension_count)  # {'.txt': 1, '.jpg': 1, '.py': 1, '.csv': 1, '.md': 1}

# 使用字典推导式
extension_count2 = {
    ext: sum(1 for file in files if os.path.splitext(file)[1] == ext)
    for ext in set(os.path.splitext(file)[1] for file in files)
}
print(extension_count2)
```

## 总结

本章介绍了 Python 的字典数据结构：
- 字典的创建、访问和基本操作
- 字典的添加、修改、删除方法
- 常用的字典方法和视图对象
- 字典推导式的强大功能

通过本章的学习，你应该能够：
1. 熟练创建和操作字典
2. 使用各种字典方法处理数据
3. 编写简洁的字典推导式
4. 理解字典的键值映射特性
5. 选择字典处理映射关系的场景

下一章将介绍集合，这是另一种重要的数据结构。
