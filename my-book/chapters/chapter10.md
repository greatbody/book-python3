# 第10章：函数基础

## 10.1 函数的定义和调用

函数是组织代码的基本单元，它将一段可重用的代码封装起来，可以通过函数名来调用。

### 10.1.1 函数的定义

```python
# 函数定义的基本语法
def function_name(parameters):
    """
    函数的文档字符串（可选）
    """
    # 函数体
    statements
    return value  # 可选的返回值

# 示例：简单的问候函数
def greet():
    """打印问候信息"""
    print("Hello, World!")

# 调用函数
greet()  # 输出: Hello, World!

# 示例：带参数的函数
def greet_person(name):
    """向指定人问候"""
    print(f"Hello, {name}!")

greet_person("Alice")  # 输出: Hello, Alice!
greet_person("Bob")    # 输出: Hello, Bob!
```

### 10.1.2 函数的调用

```python
# 定义一个计算圆面积的函数
def calculate_circle_area(radius):
    """计算圆的面积"""
    pi = 3.14159
    area = pi * radius ** 2
    return area

# 调用函数并使用返回值
radius = 5
area = calculate_circle_area(radius)
print(f"半径为 {radius} 的圆面积是: {area}")

# 函数可以作为表达式的一部分
total_area = calculate_circle_area(3) + calculate_circle_area(4)
print(f"两个圆的总面积: {total_area}")

# 函数调用可以嵌套
def square(x):
    return x ** 2

def sum_of_squares(a, b):
    return square(a) + square(b)

result = sum_of_squares(3, 4)
print(f"3² + 4² = {result}")  # 输出: 3² + 4² = 25
```

### 10.1.3 函数的返回值

```python
# 有返回值的函数
def add_numbers(a, b):
    """返回两个数的和"""
    return a + b

result = add_numbers(5, 3)
print(result)  # 8

# 多个返回值（实际上返回元组）
def get_user_info():
    name = "Alice"
    age = 25
    city = "Beijing"
    return name, age, city

user_name, user_age, user_city = get_user_info()
print(f"用户: {user_name}, 年龄: {user_age}, 城市: {user_city}")

# 没有返回值的函数（实际上返回 None）
def print_message(message):
    print(message)
    # 没有 return 语句，默认返回 None

result = print_message("Hello")
print(result)  # None

# 提前返回
def check_number(num):
    if num > 0:
        return "正数"
    elif num < 0:
        return "负数"
    else:
        return "零"

print(check_number(5))   # 正数
print(check_number(-3))  # 负数
print(check_number(0))   # 零
```

### 10.1.4 函数的文档字符串

```python
def calculate_bmi(weight_kg, height_m):
    """
    计算身体质量指数 (BMI)

    参数:
    weight_kg (float): 体重（公斤）
    height_m (float): 身高（米）

    返回:
    float: BMI 值

    示例:
    >>> calculate_bmi(70, 1.75)
    22.857142857142858
    """
    if height_m <= 0:
        raise ValueError("身高必须大于0")
    return weight_kg / (height_m ** 2)

# 查看函数的文档
print(calculate_bmi.__doc__)

# 使用 help() 函数查看帮助
help(calculate_bmi)

# 调用函数
bmi = calculate_bmi(70, 1.75)
print(f"BMI: {bmi:.2f}")
```

## 10.2 参数和返回值

### 10.2.1 位置参数

```python
# 位置参数函数
def power(base, exponent):
    """计算 base 的 exponent 次方"""
    return base ** exponent

# 调用时参数按位置对应
print(power(2, 3))    # 8 (2^3)
print(power(3, 2))    # 9 (3^2)

# 参数顺序很重要
print(power(2, 3))    # 8
print(power(3, 2))    # 9 (不同结果)
```

### 10.2.2 关键字参数

```python
def create_person(name, age, city="Beijing"):
    """创建人员信息"""
    return {
        "name": name,
        "age": age,
        "city": city
    }

# 使用关键字参数调用
person1 = create_person(name="Alice", age=25, city="Shanghai")
person2 = create_person(age=30, name="Bob")  # 关键字参数顺序可以改变

print(person1)
print(person2)

# 混合使用位置参数和关键字参数
# 位置参数必须在关键字参数之前
person3 = create_person("Charlie", age=35, city="Guangzhou")
print(person3)
```

### 10.2.3 默认参数

```python
# 带默认参数的函数
def greet(name, greeting="Hello", punctuation="!"):
    """问候函数"""
    return f"{greeting}, {name}{punctuation}"

# 使用默认参数
print(greet("Alice"))                    # Hello, Alice!
print(greet("Bob", "Hi"))               # Hi, Bob!
print(greet("Charlie", "Good morning"))  # Good morning, Charlie!
print(greet("David", punctuation="?"))   # Hello, David?

# 默认参数的值在函数定义时确定
def append_to_list(item, target_list=[]):
    target_list.append(item)
    return target_list

# 问题：默认参数是可变对象
list1 = append_to_list(1)
list2 = append_to_list(2)
print(list1)  # [1, 2] (不是期望的 [1])
print(list2)  # [1, 2]

# 正确的做法：使用 None 作为默认值
def append_to_list_fixed(item, target_list=None):
    if target_list is None:
        target_list = []
    target_list.append(item)
    return target_list

list3 = append_to_list_fixed(1)
list4 = append_to_list_fixed(2)
print(list3)  # [1]
print(list4)  # [2]
```

### 10.2.4 返回多个值

```python
# 返回多个值的函数
def analyze_numbers(numbers):
    """分析数字列表"""
    if not numbers:
        return 0, 0, 0, 0

    total = sum(numbers)
    count = len(numbers)
    average = total / count
    maximum = max(numbers)
    minimum = min(numbers)

    return count, average, maximum, minimum

# 调用函数
numbers = [10, 20, 30, 40, 50]
count, avg, max_val, min_val = analyze_numbers(numbers)

print(f"数量: {count}")
print(f"平均值: {avg}")
print(f"最大值: {max_val}")
print(f"最小值: {min_val}")

# 也可以只接收部分返回值
_, average, *_ = analyze_numbers(numbers)
print(f"平均值: {average}")

# 返回字典
def get_user_stats(user_id):
    # 模拟数据库查询
    return {
        "login_count": 15,
        "last_login": "2023-12-01",
        "account_status": "active"
    }

stats = get_user_stats(123)
print(stats["login_count"])  # 15
```

## 10.3 作用域和命名空间

### 10.3.1 局部作用域和全局作用域

```python
# 全局变量
global_variable = "I am global"

def demonstrate_scope():
    # 局部变量
    local_variable = "I am local"
    print(local_variable)   # 可以访问局部变量
    print(global_variable)  # 可以访问全局变量

    # 修改局部变量
    local_variable = "Modified local"
    print(local_variable)

demonstrate_scope()
# print(local_variable)  # NameError: 局部变量在函数外不可访问

print(global_variable)  # 全局变量仍然存在
```

### 10.3.2 global 关键字

```python
counter = 0

def increment_counter():
    global counter  # 声明使用全局变量
    counter += 1
    print(f"计数器: {counter}")

increment_counter()  # 计数器: 1
increment_counter()  # 计数器: 2
print(counter)       # 2

# 不使用 global 的错误示例
def try_modify_global():
    counter = 100  # 这创建了新的局部变量
    print(f"局部计数器: {counter}")

try_modify_global()
print(counter)  # 仍然是 2，全局变量未被修改
```

### 10.3.3 nonlocal 关键字

```python
def outer_function():
    outer_var = "outer"

    def inner_function():
        nonlocal outer_var  # 引用外层函数的变量
        outer_var = "modified by inner"
        print(f"inner: {outer_var}")

    inner_function()
    print(f"outer: {outer_var}")

outer_function()

# 多层嵌套
def multi_level():
    level1 = "level 1"

    def level2():
        level2_var = "level 2"

        def level3():
            nonlocal level1, level2_var
            level1 = "modified by level3"
            level2_var = "also modified by level3"

        level3()
        print(f"level2_var: {level2_var}")

    level2()
    print(f"level1: {level1}")

multi_level()
```

### 10.3.4 LEGB 规则

Python 的变量查找遵循 LEGB 规则：
- Local（局部）：函数内部定义的变量
- Enclosing（嵌套）：外层函数的局部变量
- Global（全局）：模块级别的变量
- Built-in（内置）：Python 内置的变量

```python
# 内置作用域
print(len("hello"))  # len 是内置函数

# 全局作用域
global_var = "global"

def demonstrate_legb():
    # 局部作用域
    local_var = "local"

    def inner():
        # 嵌套作用域可以访问外层作用域
        print(local_var)   # 访问 enclosing 作用域
        print(global_var)  # 访问 global 作用域
        print(len)         # 访问 built-in 作用域

    inner()

demonstrate_legb()

# 变量遮蔽
x = "global x"

def shadow_example():
    x = "local x"  # 遮蔽了全局变量 x
    print(x)

shadow_example()
print(x)  # 全局 x 仍然存在
```

## 10.4 文档字符串

### 10.4.1 文档字符串的格式

```python
def calculate_tax(income, tax_rate=0.2, deductions=0):
    """
    计算个人所得税

    根据收入、税率和扣除项计算应纳税额。

    参数:
    income (float): 年收入
    tax_rate (float): 税率，默认 0.2 (20%)
    deductions (float): 扣除项，默认 0

    返回:
    float: 应纳税额

    引发:
    ValueError: 当收入为负数时

    示例:
    >>> calculate_tax(50000, 0.25, 5000)
    11250.0
    """
    if income < 0:
        raise ValueError("收入不能为负数")

    taxable_income = income - deductions
    if taxable_income <= 0:
        return 0

    return taxable_income * tax_rate

# 查看文档
print(calculate_tax.__doc__)

# 使用 doctest 验证示例
import doctest
doctest.testmod()
```

### 10.4.2 help() 函数

```python
def fibonacci(n):
    """
    生成斐波那契数列的前 n 项

    斐波那契数列：1, 1, 2, 3, 5, 8, 13, ...

    参数:
    n (int): 生成的项数，必须大于 0

    返回:
    list: 包含斐波那契数列的列表

    示例:
    >>> fibonacci(5)
    [1, 1, 2, 3, 5]
    """
    if n <= 0:
        return []

    if n == 1:
        return [1]

    sequence = [1, 1]
    for i in range(2, n):
        sequence.append(sequence[i-1] + sequence[i-2])

    return sequence

# 使用 help 查看函数帮助
help(fibonacci)

# 调用函数
print(fibonacci(8))  # [1, 1, 2, 3, 5, 8, 13, 21]
```

### 10.4.3 文档规范

```python
def process_data(data, operation="sum", threshold=None):
    """
    处理数值数据

    对输入的数据进行指定的操作，并可选择应用阈值过滤。

    Args:
        data (list): 数值列表
        operation (str): 操作类型，可选 'sum', 'average', 'max', 'min'
        threshold (float, optional): 过滤阈值，只处理大于此值的数

    Returns:
        float: 处理结果

    Raises:
        ValueError: 当数据为空或操作无效时
        TypeError: 当数据包含非数值类型时

    Note:
        该函数假设输入数据为数值类型。

    Examples:
        >>> process_data([1, 2, 3, 4, 5])
        15
        >>> process_data([1, 2, 3, 4, 5], 'average')
        3.0
        >>> process_data([1, 2, 3, 4, 5], threshold=3)
        12
    """
    if not data:
        raise ValueError("数据不能为空")

    # 应用阈值过滤
    if threshold is not None:
        data = [x for x in data if x > threshold]

    if operation == "sum":
        return sum(data)
    elif operation == "average":
        return sum(data) / len(data)
    elif operation == "max":
        return max(data)
    elif operation == "min":
        return min(data)
    else:
        raise ValueError(f"不支持的操作: {operation}")

# 使用函数
data = [10, 20, 30, 40, 50]
print(process_data(data))                    # 150
print(process_data(data, "average"))         # 30.0
print(process_data(data, threshold=25))      # 120 (30+40+50)
```

## 总结

本章介绍了 Python 函数的基础知识：
- 函数的定义和调用语法
- 参数传递（位置参数、关键字参数、默认参数）
- 返回值处理（单个值、多个值）
- 作用域和命名空间（LEGB 规则）
- 文档字符串的编写和使用

通过本章的学习，你应该能够：
1. 定义和调用函数
2. 使用各种参数传递方式
3. 理解作用域规则
4. 编写良好的文档字符串
5. 处理函数的返回值

下一章将介绍函数进阶内容，包括可变参数、lambda 表达式等。
