# 第11章：函数进阶

## 11.1 默认参数

默认参数允许函数在调用时省略某些参数，使用预设的默认值。

### 11.1.1 基本用法

```python
def greet(name, greeting="Hello", punctuation="!"):
    """带默认参数的问候函数"""
    return f"{greeting}, {name}{punctuation}"

# 使用所有默认参数
print(greet("Alice"))  # Hello, Alice!

# 覆盖部分默认参数
print(greet("Bob", "Hi"))  # Hi, Bob!

# 覆盖所有默认参数
print(greet("Charlie", "Good morning", "?"))  # Good morning, Charlie?

# 使用关键字参数
print(greet("David", punctuation="."))  # Hello, David.
```

### 11.1.2 默认参数的注意事项

```python
# 默认参数在函数定义时求值一次
def append_to_list(item, target_list=[]):
    """错误的默认参数使用"""
    target_list.append(item)
    return target_list

# 问题演示
list1 = append_to_list(1)
list2 = append_to_list(2)
print(list1)  # [1, 2] (不是期望的 [1])
print(list2)  # [1, 2] (共享同一个列表对象)

# 正确的做法
def append_to_list_fixed(item, target_list=None):
    """正确的默认参数使用"""
    if target_list is None:
        target_list = []
    target_list.append(item)
    return target_list

list3 = append_to_list_fixed(1)
list4 = append_to_list_fixed(2)
print(list3)  # [1]
print(list4)  # [2]
```

### 11.1.3 默认参数的应用场景

```python
# 文件处理函数
def read_file(filename, mode="r", encoding="utf-8"):
    """读取文件内容"""
    with open(filename, mode, encoding=encoding) as file:
        return file.read()

# 网络请求函数
def make_request(url, method="GET", timeout=30, headers=None):
    """模拟网络请求"""
    if headers is None:
        headers = {}

    print(f"请求: {method} {url}")
    print(f"超时: {timeout}秒")
    print(f"请求头: {headers}")
    return "响应内容"

# 调用示例
make_request("https://api.example.com/data")
make_request("https://api.example.com/data", method="POST", timeout=60)

# 数据库查询函数
def query_database(query, limit=100, offset=0, sort_by="id"):
    """数据库查询"""
    print(f"查询: {query}")
    print(f"限制: {limit} 条")
    print(f"偏移: {offset}")
    print(f"排序: {sort_by}")
    return f"查询结果 (前{limit}条)"

query_database("SELECT * FROM users")
query_database("SELECT * FROM users", limit=50, sort_by="name")
```

## 11.2 可变参数（*args 和 **kwargs）

可变参数允许函数接受不定数量的参数。

### 11.2.1 *args：可变位置参数

```python
def sum_all(*args):
    """计算所有参数的和"""
    print(f"参数类型: {type(args)}")  # <class 'tuple'>
    print(f"参数值: {args}")
    return sum(args)

print(sum_all(1, 2, 3))      # 6
print(sum_all(1, 2, 3, 4, 5)) # 15
print(sum_all())             # 0

# 结合普通参数使用
def multiply_and_sum(multiplier, *numbers):
    """先乘以倍数，然后求和"""
    multiplied = [num * multiplier for num in numbers]
    return sum(multiplied)

print(multiply_and_sum(2, 1, 2, 3))  # 2*1 + 2*2 + 2*3 = 12
```

### 11.2.2 **kwargs：可变关键字参数

```python
def print_info(**kwargs):
    """打印所有关键字参数"""
    print(f"参数类型: {type(kwargs)}")  # <class 'dict'>
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=25, city="Beijing")

# 结合普通参数使用
def create_person(name, **additional_info):
    """创建人员信息"""
    person = {"name": name}
    person.update(additional_info)
    return person

person = create_person("Bob", age=30, job="Engineer", city="Shanghai")
print(person)
```

### 11.2.3 参数顺序和组合使用

```python
def complex_function(a, b=10, *args, **kwargs):
    """
    演示各种参数类型的组合
    参数顺序：普通参数 -> 默认参数 -> *args -> **kwargs
    """
    print(f"a: {a}")
    print(f"b: {b}")
    print(f"args: {args}")
    print(f"kwargs: {kwargs}")

    result = a + b
    result += sum(args)
    for value in kwargs.values():
        if isinstance(value, (int, float)):
            result += value

    return result

# 调用示例
print(complex_function(1))                          # a=1, b=10, args=(), kwargs={}
print(complex_function(1, 20))                      # a=1, b=20, args=(), kwargs={}
print(complex_function(1, 20, 100, 200))            # a=1, b=20, args=(100, 200), kwargs={}
print(complex_function(1, 20, 100, 200, x=5, y=10)) # a=1, b=20, args=(100, 200), kwargs={'x': 5, 'y': 10}
```

### 11.2.4 参数解包

```python
# 使用 * 解包序列作为位置参数
def add_three_numbers(a, b, c):
    return a + b + c

numbers = [1, 2, 3]
result = add_three_numbers(*numbers)  # 等价于 add_three_numbers(1, 2, 3)
print(result)  # 6

# 使用 ** 解包字典作为关键字参数
def create_profile(name, age, city):
    return f"{name} ({age}岁) 住在 {city}"

person_info = {"name": "Alice", "age": 25, "city": "Beijing"}
profile = create_profile(**person_info)
print(profile)  # Alice (25岁) 住在 Beijing

# 混合使用
args = (10, 20)
kwargs = {"c": 30, "d": 40}

def test_function(a, b, c=0, d=0):
    return a + b + c + d

result = test_function(*args, **kwargs)
print(result)  # 10 + 20 + 30 + 40 = 100
```

## 11.3 lambda 表达式

lambda 表达式是创建匿名函数的简洁方式。

### 11.3.1 基本语法

```python
# lambda 基本语法
# lambda 参数列表: 表达式

# 简单函数
square = lambda x: x ** 2
print(square(5))  # 25

# 多个参数
add = lambda x, y: x + y
print(add(3, 4))  # 7

# 默认参数
power = lambda x, n=2: x ** n
print(power(3))    # 9
print(power(3, 3)) # 27
```

### 11.3.2 lambda 表达式的应用

```python
# 在 sorted() 中使用
students = [
    {"name": "Alice", "score": 85},
    {"name": "Bob", "score": 92},
    {"name": "Charlie", "score": 78}
]

# 按分数排序
sorted_by_score = sorted(students, key=lambda student: student["score"])
print(sorted_by_score)

# 在 filter() 中使用
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
even_numbers = list(filter(lambda x: x % 2 == 0, numbers))
print(even_numbers)  # [2, 4, 6, 8, 10]

# 在 map() 中使用
squared = list(map(lambda x: x ** 2, numbers))
print(squared)  # [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

# 条件表达式
check_age = lambda age: "成年" if age >= 18 else "未成年"
print(check_age(20))  # 成年
print(check_age(15))  # 未成年
```

### 11.3.3 lambda vs 普通函数

```python
# 普通函数
def get_full_name(first_name, last_name):
    return f"{first_name} {last_name}"

# lambda 版本
get_full_name_lambda = lambda first_name, last_name: f"{first_name} {last_name}"

print(get_full_name("John", "Doe"))
print(get_full_name_lambda("John", "Doe"))

# lambda 的限制
# 不能包含多条语句
# lambda x: print(x); return x  # 错误

# 不能有默认参数值（除非所有参数都有默认值）
# lambda x, y=10: x + y  # 错误

# 正确的写法
add_with_default = lambda x, y=10: x + y
print(add_with_default(5))    # 15
print(add_with_default(5, 3)) # 8
```

### 11.3.4 实际应用场景

```python
# 数据处理
data = [
    {"name": "Alice", "age": 25, "score": 85},
    {"name": "Bob", "age": 30, "score": 92},
    {"name": "Charlie", "age": 22, "score": 78}
]

# 复杂排序：先按分数降序，再按年龄升序
sorted_data = sorted(data, key=lambda x: (-x["score"], x["age"]))
print("排序结果:")
for person in sorted_data:
    print(f"{person['name']}: {person['score']}分, {person['age']}岁")

# 数据转换
products = [
    {"name": "苹果", "price": 5.0, "quantity": 10},
    {"name": "香蕉", "price": 3.0, "quantity": 20},
    {"name": "橙子", "price": 4.0, "quantity": 15}
]

# 计算总价值
total_values = list(map(lambda p: p["price"] * p["quantity"], products))
print(f"总价值: {total_values}")

# 筛选高价值产品
valuable_products = list(filter(lambda p: p["price"] * p["quantity"] > 50, products))
print("高价值产品:")
for product in valuable_products:
    print(f"{product['name']}: {product['price'] * product['quantity']}")

# 创建格式化函数
formatters = {
    "currency": lambda x: f"¥{x:.2f}",
    "percentage": lambda x: f"{x:.1%}",
    "integer": lambda x: f"{int(x)}"
}

print(formatters["currency"](123.456))   # ¥123.46
print(formatters["percentage"](0.123))   # 12.3%
print(formatters["integer"](45.67))      # 45
```

## 11.4 函数作为参数

函数可以作为参数传递给其他函数，实现高阶函数。

### 11.4.1 函数作为参数的基本用法

```python
def apply_operation(x, y, operation):
    """应用操作到两个数"""
    return operation(x, y)

def add(x, y):
    return x + y

def multiply(x, y):
    return x * y

result1 = apply_operation(5, 3, add)
result2 = apply_operation(5, 3, multiply)

print(f"5 + 3 = {result1}")
print(f"5 * 3 = {result2}")

# 使用 lambda
result3 = apply_operation(5, 3, lambda x, y: x ** y)
print(f"5 ^ 3 = {result3}")
```

### 11.4.2 内置高阶函数

```python
# map() 函数
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))
print(squared)  # [1, 4, 9, 16, 25]

# filter() 函数
even_numbers = list(filter(lambda x: x % 2 == 0, numbers))
print(even_numbers)  # [2, 4]

# sorted() 函数
words = ["apple", "Banana", "cherry", "Date"]
sorted_words = sorted(words, key=lambda x: x.lower())
print(sorted_words)  # ['apple', 'Banana', 'cherry', 'Date']

# reduce() 函数（需要导入）
from functools import reduce
product = reduce(lambda x, y: x * y, numbers)
print(product)  # 120 (1*2*3*4*5)
```

### 11.4.3 自定义高阶函数

```python
def apply_to_list(data, func):
    """对列表中的每个元素应用函数"""
    return [func(item) for item in data]

def apply_condition(data, condition):
    """筛选满足条件的元素"""
    return [item for item in data if condition(item)]

# 使用示例
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# 应用函数
squared = apply_to_list(numbers, lambda x: x ** 2)
print(f"平方: {squared}")

# 应用条件
even_numbers = apply_condition(numbers, lambda x: x % 2 == 0)
print(f"偶数: {even_numbers}")

# 组合使用
large_even_squares = apply_condition(
    apply_to_list(numbers, lambda x: x ** 2),
    lambda x: x > 10
)
print(f"大于10的平方数: {large_even_squares}")
```

### 11.4.4 函数工厂

```python
def create_multiplier(factor):
    """创建乘法函数"""
    return lambda x: x * factor

double = create_multiplier(2)
triple = create_multiplier(3)
quadruple = create_multiplier(4)

print(double(5))    # 10
print(triple(5))    # 15
print(quadruple(5)) # 20

# 更复杂的函数工厂
def create_power_function(exponent):
    """创建幂函数"""
    return lambda base: base ** exponent

square = create_power_function(2)
cube = create_power_function(3)

print(square(4))  # 16
print(cube(4))    # 64

# 配置化函数
def create_formatter(prefix="", suffix="", multiplier=1):
    """创建格式化函数"""
    return lambda value: f"{prefix}{value * multiplier}{suffix}"

currency_formatter = create_formatter("¥", ".00", 1)
percentage_formatter = create_formatter("", "%", 100)

print(currency_formatter(123.456))   # ¥123.456.00
print(percentage_formatter(0.123))   # 12.3%
```

## 11.5 递归函数

递归函数是调用自身的函数。

### 11.5.1 基本递归

```python
def factorial(n):
    """计算阶乘"""
    if n == 0 or n == 1:
        return 1
    else:
        return n * factorial(n - 1)

print(factorial(5))  # 120 (5! = 5×4×3×2×1)

def fibonacci(n):
    """计算斐波那契数列的第n项"""
    if n <= 1:
        return n
    else:
        return fibonacci(n - 1) + fibonacci(n - 2)

print(fibonacci(6))  # 8 (斐波那契数列: 0, 1, 1, 2, 3, 5, 8)
```

### 11.5.2 递归 vs 迭代

```python
# 递归版本
def sum_recursive(n):
    if n == 0:
        return 0
    return n + sum_recursive(n - 1)

# 迭代版本
def sum_iterative(n):
    total = 0
    for i in range(1, n + 1):
        total += i
    return total

print(sum_recursive(5))  # 15
print(sum_iterative(5))  # 15

# 递归处理列表
def sum_list_recursive(data):
    if not data:
        return 0
    return data[0] + sum_list_recursive(data[1:])

# 迭代处理列表
def sum_list_iterative(data):
    total = 0
    for item in data:
        total += item
    return total

numbers = [1, 2, 3, 4, 5]
print(sum_list_recursive(numbers))  # 15
print(sum_list_iterative(numbers))  # 15
```

### 11.5.3 尾递归优化

```python
# 非尾递归
def factorial_non_tail(n):
    if n == 0 or n == 1:
        return 1
    return n * factorial_non_tail(n - 1)

# 尾递归版本
def factorial_tail(n, accumulator=1):
    if n == 0 or n == 1:
        return accumulator
    return factorial_tail(n - 1, n * accumulator)

print(factorial_tail(5))  # 120

# 尾递归求和
def sum_tail(n, accumulator=0):
    if n == 0:
        return accumulator
    return sum_tail(n - 1, accumulator + n)

print(sum_tail(5))  # 15
```

### 11.5.4 递归的实际应用

```python
# 目录遍历
import os

def list_files_recursive(directory, level=0):
    """递归列出目录中的所有文件"""
    indent = "  " * level

    try:
        for item in os.listdir(directory):
            item_path = os.path.join(directory, item)
            if os.path.isdir(item_path):
                print(f"{indent}[DIR] {item}")
                list_files_recursive(item_path, level + 1)
            else:
                print(f"{indent}[FILE] {item}")
    except PermissionError:
        print(f"{indent}[ACCESS DENIED] {directory}")

# list_files_recursive(".")

# 二叉树遍历（模拟）
def TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def inorder_traversal(node):
    """中序遍历二叉树"""
    if node is None:
        return []

    result = []
    result.extend(inorder_traversal(node.left))
    result.append(node.value)
    result.extend(inorder_traversal(node.right))

    return result

# 汉诺塔问题
def hanoi(n, source, target, auxiliary):
    """解决汉诺塔问题"""
    if n == 1:
        print(f"移动盘子 1 从 {source} 到 {target}")
        return

    hanoi(n - 1, source, auxiliary, target)
    print(f"移动盘子 {n} 从 {source} 到 {target}")
    hanoi(n - 1, auxiliary, target, source)

print("3层汉诺塔的解法:")
hanoi(3, "A", "C", "B")
```

## 总结

本章介绍了函数的进阶特性：
- 默认参数的使用和注意事项
- 可变参数（*args 和 **kwargs）的应用
- lambda 表达式的创建和使用
- 函数作为参数的高阶函数
- 递归函数的实现和应用

通过本章的学习，你应该能够：
1. 使用各种参数传递方式
2. 创建和使用 lambda 表达式
3. 编写高阶函数
4. 实现递归算法
5. 选择合适的函数定义方式

下一章将介绍模块和包，学习如何组织和重用代码。
