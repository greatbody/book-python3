# 第4章：条件语句

## 4.1 if 语句

if 语句是编程中最基本的控制结构，用于根据条件执行不同的代码块。

### 4.1.1 基本语法

```python
# 基本 if 语句
age = 18

if age >= 18:
    print("您已成年")
    print("可以观看成人内容")

print("程序继续执行")  # 这行代码总会执行
```

**语法结构：**
```python
if 条件表达式:
    # 条件为真时执行的代码块
    语句1
    语句2
    ...
```

### 4.1.2 条件表达式

条件表达式可以是任何返回布尔值的表达式：

```python
# 比较表达式
temperature = 25
if temperature > 30:
    print("天气炎热")

# 布尔变量
is_raining = True
if is_raining:
    print("记得带伞")

# 函数返回值
def is_even(number):
    return number % 2 == 0

if is_even(4):
    print("4 是偶数")

# 字符串非空检查
name = "Alice"
if name:
    print(f"欢迎, {name}")

# 列表非空检查
numbers = [1, 2, 3]
if numbers:
    print("列表不为空")
```

### 4.1.3 代码块和缩进

Python 使用缩进来定义代码块，这是强制性的语法要求：

```python
# 正确的缩进
if True:
    print("这是 if 代码块")
    print("这些语句都属于 if")
    if False:
        print("嵌套的 if")

print("这行不在 if 代码块内")

# 错误的缩进（会导致 IndentationError）
# if True:
# print("错误的缩进")
```

**缩进规则：**
- 使用 4 个空格（推荐）或 1 个 Tab
- 同一代码块的缩进必须一致
- 空行不影响缩进，但不能用空行分隔代码块

### 4.1.4 常见的条件模式

```python
# 检查变量是否存在
user = None
if user is not None:
    print(f"用户: {user}")

# 检查值在范围内
score = 85
if 60 <= score <= 100:
    print("及格")

# 检查字符串包含子串
message = "Hello, World!"
if "World" in message:
    print("找到了 World")

# 检查文件是否存在
import os
if os.path.exists("example.txt"):
    print("文件存在")

# 检查列表包含元素
fruits = ["apple", "banana", "orange"]
if "apple" in fruits:
    print("有苹果")
```

## 4.2 if-else 语句

if-else 语句提供两种执行路径：条件为真时执行一种，条件为假时执行另一种。

### 4.2.1 二分支选择

```python
# 基本 if-else
age = 16

if age >= 18:
    print("您已成年，可以开车")
else:
    print("您未成年，不能开车")

# 另一个例子
is_weekend = True

if is_weekend:
    print("今天是周末，放松一下")
else:
    print("今天是工作日，要上班")
```

**语法结构：**
```python
if 条件表达式:
    # 条件为真时执行
    语句1
    语句2
else:
    # 条件为假时执行
    语句3
    语句4
```

### 4.2.2 条件表达式的应用

```python
# 简单的值选择
x = 10
result = "正数" if x > 0 else "非正数"
print(result)  # 正数

# 函数返回值
def get_grade(score):
    if score >= 90:
        return "A"
    elif score >= 80:
        return "B"
    elif score >= 70:
        return "C"
    elif score >= 60:
        return "D"
    else:
        return "F"

print(get_grade(85))  # B

# 文件处理
import os

filename = "data.txt"
if os.path.exists(filename):
    with open(filename, 'r') as file:
        content = file.read()
        print("文件内容:", content)
else:
    print("文件不存在")
    with open(filename, 'w') as file:
        file.write("新文件内容")
```

## 4.3 if-elif-else 语句

if-elif-else 语句用于处理多个条件分支。

### 4.3.1 多分支选择

```python
# 成绩等级判断
score = 85

if score >= 90:
    grade = "A"
    print("优秀")
elif score >= 80:
    grade = "B"
    print("良好")
elif score >= 70:
    grade = "C"
    print("中等")
elif score >= 60:
    grade = "D"
    print("及格")
else:
    grade = "F"
    print("不及格")

print(f"最终等级: {grade}")
```

**语法结构：**
```python
if 条件1:
    # 条件1为真时执行
    语句块1
elif 条件2:
    # 条件2为真时执行
    语句块2
elif 条件3:
    # 条件3为真时执行
    语句块3
else:
    # 所有条件都为假时执行
    语句块4
```

### 4.3.2 elif 的使用

```python
# 用户权限判断
user_role = "moderator"
user_level = 5

if user_role == "admin":
    permissions = ["read", "write", "delete", "manage_users"]
elif user_role == "moderator":
    if user_level >= 3:
        permissions = ["read", "write", "delete"]
    else:
        permissions = ["read", "write"]
elif user_role == "user":
    permissions = ["read"]
else:
    permissions = []

print(f"用户权限: {permissions}")

# 时间段判断
hour = 14

if hour < 6:
    period = "凌晨"
elif hour < 12:
    period = "上午"
elif hour < 18:
    period = "下午"
else:
    period = "晚上"

print(f"现在是{period}")
```

### 4.3.3 条件顺序的重要性

```python
# 错误的顺序（范围大的条件在前）
age = 25

if age >= 18:
    category = "成人"
elif age >= 13:
    category = "青少年"
elif age >= 6:
    category = "儿童"
else:
    category = "婴儿"

print(category)  # 成人（正确，因为 25 >= 18）

# 正确的顺序（范围小的条件在前）
if age >= 65:
    category = "老年人"
elif age >= 18:
    category = "成人"
elif age >= 13:
    category = "青少年"
elif age >= 6:
    category = "儿童"
else:
    category = "婴儿"

print(category)  # 成人
```

## 4.4 嵌套条件语句

在条件语句中可以嵌套其他条件语句，用于处理复杂的逻辑。

### 4.4.1 嵌套的结构

```python
# 用户登录验证
username = "admin"
password = "123456"
is_active = True

if username:
    if password:
        if is_active:
            print("登录成功")
            # 执行登录后的操作
        else:
            print("账户已被禁用")
    else:
        print("密码不能为空")
else:
    print("用户名不能为空")
```

### 4.4.2 可读性考虑

嵌套过深会降低代码可读性，应该尽量避免：

```python
# 不推荐：嵌套过深
def check_access(user, resource):
    if user:
        if user.is_authenticated:
            if user.has_permission(resource):
                if resource.is_available:
                    if not resource.is_locked:
                        return True
    return False

# 推荐：尽早返回或使用逻辑运算符
def check_access_improved(user, resource):
    if not user:
        return False
    if not user.is_authenticated:
        return False
    if not user.has_permission(resource):
        return False
    if not resource.is_available:
        return False
    if resource.is_locked:
        return False
    return True

# 或者使用逻辑运算符
def check_access_logical(user, resource):
    return (user and
            user.is_authenticated and
            user.has_permission(resource) and
            resource.is_available and
            not resource.is_locked)
```

### 4.4.3 逻辑运算符的替代

```python
# 使用逻辑运算符简化嵌套
age = 25
has_license = True

# 嵌套方式
if age >= 18:
    if has_license:
        print("可以开车")
    else:
        print("需要驾照")
else:
    print("年龄不够")

# 逻辑运算符方式
if age >= 18 and has_license:
    print("可以开车")
elif age >= 18 and not has_license:
    print("需要驾照")
else:
    print("年龄不够")

# 更简洁的方式
if age < 18:
    print("年龄不够")
elif not has_license:
    print("需要驾照")
else:
    print("可以开车")
```

## 4.5 条件表达式

条件表达式（三元运算符）提供了一种简洁的条件赋值方式。

### 4.5.1 三元运算符

```python
# 基本语法
result = 值1 if 条件 else 值2

# 示例
age = 20
status = "成年" if age >= 18 else "未成年"
print(status)  # 成年

# 数值比较
a = 10
b = 20
max_value = a if a > b else b
print(max_value)  # 20

# 函数调用
def get_greeting(hour):
    return "早上好" if hour < 12 else "下午好"

print(get_greeting(9))   # 早上好
print(get_greeting(15))  # 下午好
```

### 4.5.2 条件表达式的嵌套

```python
# 嵌套条件表达式
score = 85

# 不推荐：嵌套过深
grade = "A" if score >= 90 else ("B" if score >= 80 else ("C" if score >= 70 else "D"))
print(grade)  # B

# 推荐：使用函数或 if-elif
def get_grade(score):
    if score >= 90:
        return "A"
    elif score >= 80:
        return "B"
    elif score >= 70:
        return "C"
    else:
        return "D"

print(get_grade(85))  # B

# 或者使用字典映射
def get_grade_dict(score):
    grade_map = {
        90: "A", 80: "B", 70: "C", 60: "D"
    }
    for threshold in sorted(grade_map.keys(), reverse=True):
        if score >= threshold:
            return grade_map[threshold]
    return "F"

print(get_grade_dict(85))  # B
```

### 4.5.3 实际应用场景

```python
# 列表推导式中的条件表达式
numbers = [1, 2, 3, 4, 5, 6]
result = ["偶数" if x % 2 == 0 else "奇数" for x in numbers]
print(result)  # ['奇数', '偶数', '奇数', '偶数', '奇数', '偶数']

# 默认值设置
user_name = input("请输入用户名: ") or "匿名用户"
print(f"欢迎, {user_name}")

# 安全访问嵌套属性
class Person:
    def __init__(self, name, address=None):
        self.name = name
        self.address = address

person = Person("Alice")

# 传统方式
if person.address and person.address.city:
    city = person.address.city
else:
    city = "未知"

# 使用条件表达式
city = person.address.city if person.address else "未知"
print(city)
```

## 总结

本章介绍了 Python 的条件语句：
- if 语句：基本的条件执行
- if-else 语句：二分支选择
- if-elif-else 语句：多分支选择
- 嵌套条件语句：复杂逻辑处理
- 条件表达式：简洁的条件赋值

通过本章的学习，你应该能够：
1. 编写基本的条件判断语句
2. 处理多分支的条件逻辑
3. 避免过度嵌套，提高代码可读性
4. 使用条件表达式简化代码
5. 理解条件执行的控制流程

下一章将介绍循环语句，这是处理重复操作的重要工具。
