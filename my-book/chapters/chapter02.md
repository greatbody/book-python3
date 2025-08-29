# 第2章：数据类型和变量

## 2.1 变量和赋值

### 2.1.1 变量的概念

在 Python 中，变量是用来存储数据的容器。变量名是指向数据的引用，而不是数据本身。

**变量的特点：**
- 变量名是数据的引用
- 变量可以随时改变指向的数据
- Python 是动态类型语言，变量类型可以改变

```python
# 变量赋值示例
name = "Alice"
age = 25
height = 1.68

print(name)    # 输出: Alice
print(age)     # 输出: 25
print(height)  # 输出: 1.68
```

### 2.1.2 赋值操作

**基本赋值：**
```python
# 基本赋值
x = 10
message = "Hello, Python!"

# 同时赋值给多个变量
a = b = c = 0

# 链式赋值
x = y = z = 100
```

**多重赋值：**
```python
# 多重赋值
name, age, city = "Bob", 30, "Beijing"
print(name)  # 输出: Bob
print(age)   # 输出: 30
print(city)  # 输出: Beijing

# 交换变量值
a = 1
b = 2
a, b = b, a
print(a, b)  # 输出: 2 1
```

### 2.1.3 变量的内存管理

Python 使用自动内存管理，通过引用计数和垃圾回收机制：

```python
# 引用计数示例
x = [1, 2, 3]  # 创建列表对象，引用计数为1
y = x          # 引用计数增加到2
z = x          # 引用计数增加到3

# 当变量不再使用时，引用计数减少
del y          # 引用计数减少到2
del z          # 引用计数减少到1
del x          # 引用计数减少到0，对象被垃圾回收
```

**内存管理要点：**
- Python 自动管理内存，无需手动分配和释放
- 使用 `id()` 函数查看对象的内存地址
- `is` 运算符比较对象的身份（内存地址）

```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a

print(a is b)  # False，不同对象
print(a is c)  # True，同一个对象
print(id(a), id(c))  # 相同的内存地址
```

### 2.1.4 多重赋值

多重赋值是 Python 的强大特性，可以同时给多个变量赋值：

```python
# 解包赋值
coordinates = (10, 20, 30)
x, y, z = coordinates
print(x, y, z)  # 输出: 10 20 30

# 忽略不需要的值
data = [1, 2, 3, 4, 5]
first, *middle, last = data
print(first)   # 1
print(middle)  # [2, 3, 4]
print(last)    # 5

# 扩展解包
*a, b, c = [1, 2, 3, 4]
print(a)  # [1, 2]
print(b)  # 3
print(c)  # 4
```

## 2.2 基本数据类型

Python 有四种基本数据类型：整数、浮点数、字符串和布尔值。

### 2.2.1 整数（int）

#### 2.2.1.1 整数的表示范围

Python 3 中的整数没有大小限制，只受限于计算机内存：

```python
# 大整数
very_large_number = 1234567890123456789012345678901234567890
print(very_large_number)  # 正常显示

# 整数运算
a = 10
b = 3
print(a + b)  # 13
print(a - b)  # 7
print(a * b)  # 30
print(a // b) # 3 (整除)
print(a % b)  # 1 (取余)
print(a ** b) # 1000 (幂运算)
```

#### 2.2.1.2 不同进制表示

```python
# 十进制（默认）
decimal = 42

# 二进制（以 0b 或 0B 开头）
binary = 0b101010
print(binary)  # 42

# 八进制（以 0o 或 0O 开头）
octal = 0o52
print(octal)   # 42

# 十六进制（以 0x 或 0X 开头）
hexadecimal = 0x2A
print(hexadecimal)  # 42

# 进制转换
print(bin(42))   # '0b101010'
print(oct(42))   # '0o52'
print(hex(42))   # '0x2a'
```

#### 2.2.1.3 大整数处理

Python 自动处理大整数，无需特殊处理：

```python
# 大整数计算
factorial_100 = 1
for i in range(1, 101):
    factorial_100 *= i

print(f"100! 有 {len(str(factorial_100))} 位数字")
# 输出: 100! 有 158 位数字
```

### 2.2.2 浮点数（float）

#### 2.2.2.1 浮点数的精度问题

浮点数使用 IEEE 754 标准，可能存在精度误差：

```python
# 精度问题示例
print(0.1 + 0.2)  # 0.30000000000000004
print(0.1 + 0.2 == 0.3)  # False

# 解决精度问题
from decimal import Decimal
a = Decimal('0.1')
b = Decimal('0.2')
print(a + b)  # 0.3
print(a + b == Decimal('0.3'))  # True
```

#### 2.2.2.2 decimal 模块

对于需要精确计算的场景，使用 decimal 模块：

```python
from decimal import Decimal, getcontext

# 设置精度
getcontext().prec = 28

# 精确计算
price = Decimal('19.99')
quantity = Decimal('3')
total = price * quantity
print(total)  # 59.97

# 金融计算
interest_rate = Decimal('0.05')
principal = Decimal('1000.00')
interest = principal * interest_rate
print(interest)  # 50.00
```

#### 2.2.2.3 科学计数法

```python
# 科学计数法
avogadro = 6.022e23    # 6.022 × 10^23
planck = 6.626e-34     # 6.626 × 10^-34

print(avogadro)  # 6.022e+23
print(planck)    # 6.626e-34

# 格式化输出
print(f"Avogadro constant: {avogadro:.2e}")
print(f"Planck constant: {planck:.2e}")
```

### 2.2.3 字符串（str）

#### 2.2.3.1 字符串的创建

```python
# 单引号字符串
single = 'Hello'

# 双引号字符串
double = "World"

# 三引号字符串（多行）
multiline = """This is a
multiline string"""

# 原始字符串（不转义）
raw_string = r"C:\Users\Documents\file.txt"
print(raw_string)  # C:\Users\Documents\file.txt
```

#### 2.2.3.2 字符串操作

```python
text = "Hello, Python!"

# 字符串长度
print(len(text))  # 14

# 字符串拼接
greeting = "Hello" + " " + "World"
print(greeting)  # Hello World

# 字符串重复
stars = "*" * 10
print(stars)  # **********

# 字符串索引
print(text[0])   # H
print(text[-1])  # !

# 字符串切片
print(text[0:5])   # Hello
print(text[7:])    # Python!
print(text[::-1])  # !nohtyP ,olleH (反转)
```

#### 2.2.3.3 字符串格式化

**传统格式化：**
```python
name = "Alice"
age = 25

# % 格式化
print("My name is %s and I am %d years old" % (name, age))

# str.format()
print("My name is {} and I am {} years old".format(name, age))
print("My name is {0} and I am {1} years old".format(name, age))
print("My name is {name} and I am {age} years old".format(name=name, age=age))
```

#### 2.2.3.4 f-string

f-string 是 Python 3.6+ 的新特性，最推荐的字符串格式化方式：

```python
# 基本 f-string
name = "Bob"
age = 30
print(f"My name is {name} and I am {age} years old")

# 表达式计算
a, b = 10, 20
print(f"Sum: {a + b}, Product: {a * b}")

# 格式化数字
pi = 3.14159265359
print(f"Pi to 2 decimal places: {pi:.2f}")
print(f"Pi in scientific notation: {pi:.2e}")

# 对齐和填充
names = ["Alice", "Bob", "Charlie"]
for name in names:
    print(f"{name:>10}")  # 右对齐
    print(f"{name:<10}")  # 左对齐
    print(f"{name:^10}")  # 居中对齐
```

### 2.2.4 布尔值（bool）

#### 2.2.4.1 真值和假值

```python
# 布尔值
is_active = True
is_deleted = False

print(is_active)   # True
print(is_deleted)  # False

# 其他类型的真值
print(bool(0))     # False
print(bool(1))     # True
print(bool(""))    # False
print(bool("hi"))  # True
print(bool([]))    # False
print(bool([1]))   # True
```

#### 2.2.4.2 布尔运算

```python
# 逻辑运算符
a = True
b = False

print(a and b)  # False (与)
print(a or b)   # True  (或)
print(not a)    # False (非)

# 比较运算符
x = 5
y = 10

print(x == y)   # False (等于)
print(x != y)   # True  (不等于)
print(x < y)    # True  (小于)
print(x > y)    # False (大于)
print(x <= y)   # True  (小于等于)
print(x >= y)   # False (大于等于)

# 链式比较
age = 25
print(18 <= age <= 65)  # True
```

## 2.3 类型转换

### 2.3.1 隐式类型转换

Python 在某些情况下会自动进行类型转换：

```python
# 整数和浮点数的混合运算
result = 5 + 3.2  # 整数 5 自动转换为浮点数
print(result)      # 8.2
print(type(result))  # <class 'float'>
```

### 2.3.2 显式类型转换

**基本类型转换：**
```python
# 转换为整数
print(int(3.7))     # 3 (截断)
print(int("42"))    # 42
print(int("101", 2)) # 5 (二进制转换)

# 转换为浮点数
print(float(5))      # 5.0
print(float("3.14")) # 3.14

# 转换为字符串
print(str(42))       # '42'
print(str(3.14))     # '3.14'
print(str(True))     # 'True'

# 转换为布尔值
print(bool(0))       # False
print(bool(1))       # True
print(bool(""))      # False
print(bool("hi"))    # True
```

**高级转换：**
```python
# 列表、元组、集合转换
numbers = [1, 2, 3, 2]
print(tuple(numbers))  # (1, 2, 3, 2)
print(set(numbers))    # {1, 2, 3} (去重)

# 字符串与列表转换
text = "hello"
char_list = list(text)
print(char_list)  # ['h', 'e', 'l', 'l', 'o']

words = "hello world python"
word_list = words.split()
print(word_list)  # ['hello', 'world', 'python']

# 连接列表为字符串
joined = " ".join(word_list)
print(joined)  # 'hello world python'
```

### 2.3.3 类型检查

```python
# type() 函数
print(type(42))      # <class 'int'>
print(type(3.14))    # <class 'float'>
print(type("hello")) # <class 'str'>
print(type(True))    # <class 'bool'>

# isinstance() 函数（推荐）
print(isinstance(42, int))      # True
print(isinstance(3.14, float))  # True
print(isinstance("hello", str)) # True

# 检查多种类型
print(isinstance(42, (int, float)))  # True
```

## 2.4 变量命名规范

### 2.4.1 Python 命名约定

**变量命名规则：**
- 只能包含字母、数字、下划线
- 不能以数字开头
- 区分大小写
- 不能使用 Python 关键字

```python
# 正确的命名
user_name = "Alice"
age = 25
is_active = True
total_price = 99.99

# 错误的命名（会报错）
# 2user = "Bob"      # 以数字开头
# user-name = "Bob"  # 包含连字符
# class = "Python"   # 使用关键字
```

**命名风格：**
```python
# 蛇形命名法（推荐）
user_name = "Alice"
total_count = 100
is_valid_input = True

# 驼峰命名法（类名使用）
class UserAccount:
    pass

# 常量命名
MAX_CONNECTIONS = 100
DEFAULT_TIMEOUT = 30
```

### 2.4.2 关键字和保留字

Python 有 35 个关键字，不能用作变量名：

```python
import keyword
print(keyword.kwlist)

# 输出:
# ['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await',
#  'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except',
#  'exec', 'finally', 'for', 'from', 'global', 'if', 'import', 'in',
#  'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return',
#  'try', 'while', 'with', 'yield']
```

### 2.4.3 好的命名习惯

**有意义的命名：**
```python
# 不好的命名
a = 25
b = "Alice"
c = True

# 好的命名
age = 25
user_name = "Alice"
is_logged_in = True
```

**命名长度适中：**
```python
# 太短
n = "John"
a = 30

# 太长
user_account_registration_confirmation_email_address = "user@example.com"

# 适中
user_email = "user@example.com"
account_age = 30
```

**使用英文命名：**
```python
# 避免中文变量名（虽然 Python 支持）
用户名 = "Alice"  # 不推荐
user_name = "Alice"  # 推荐
```

## 总结

本章介绍了 Python 的数据类型和变量：
- 变量的概念、赋值和内存管理
- 四种基本数据类型：整数、浮点数、字符串、布尔值
- 类型转换：隐式和显式转换
- 变量命名规范和最佳实践

通过本章的学习，你应该能够：
1. 理解变量的工作原理和内存管理
2. 熟练使用各种数据类型及其操作
3. 进行类型转换和类型检查
4. 遵循 Python 的命名规范

下一章将介绍运算符和表达式，这是进行数据操作的基础。
