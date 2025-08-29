# 第3章：运算符和表达式

## 3.1 算术运算符

算术运算符用于执行数学运算，是编程中最基础的运算符。

### 3.1.1 基本算术运算

```python
# 加法
result = 10 + 5
print(result)  # 15

# 减法
result = 10 - 5
print(result)  # 5

# 乘法
result = 10 * 5
print(result)  # 50

# 除法（浮点除法）
result = 10 / 3
print(result)  # 3.3333333333333335

# 整除（地板除）
result = 10 // 3
print(result)  # 3

# 取余
result = 10 % 3
print(result)  # 1

# 幂运算
result = 2 ** 3
print(result)  # 8

# 平方根
import math
result = math.sqrt(16)
print(result)  # 4.0
```

### 3.1.2 算术运算的特性

**整数除法的变化（Python 3 vs Python 2）：**
```python
# Python 3 中的除法
print(10 / 3)   # 3.3333333333333335 (浮点数)
print(10 // 3)  # 3 (整数)

# Python 2 中的除法（仅供参考）
# print(10 / 3)   # 3 (整数)
# print(10 // 3)  # 3 (整数)
```

**负数运算的注意事项：**
```python
print(-10 // 3)   # -4 (向负无穷取整)
print(-10 % 3)    #  2 (余数总是正数)

print(10 // -3)   # -4
print(10 % -3)    # -2 (余数符号与除数相同)
```

### 3.1.3 复合算术运算

```python
# 计算圆的面积
radius = 5
area = 3.14159 * radius ** 2
print(f"圆的面积: {area}")

# 计算复利
principal = 1000
rate = 0.05
years = 10
amount = principal * (1 + rate) ** years
print(f"本金: {amount:.2f}")

# 温度转换
celsius = 25
fahrenheit = celsius * 9/5 + 32
print(f"{celsius}°C = {fahrenheit}°F")
```

## 3.2 比较运算符

比较运算符用于比较两个值，返回布尔值（True 或 False）。

### 3.2.1 基本比较运算

```python
a = 10
b = 20

print(a == b)   # False (等于)
print(a != b)   # True  (不等于)
print(a < b)    # True  (小于)
print(a > b)    # False (大于)
print(a <= b)   # True  (小于等于)
print(a >= b)   # False (大于等于)
```

### 3.2.2 字符串比较

```python
# 字符串按字典序比较
print("apple" < "banana")    # True ('a' < 'b')
print("Apple" < "apple")     # True (大写字母ASCII值小)
print("apple" == "Apple")    # False (大小写敏感)

# 字符串长度比较
print(len("hello") < len("world"))  # True
```

### 3.2.3 浮点数比较

```python
# 浮点数比较的精度问题
a = 0.1 + 0.2
b = 0.3
print(a == b)  # False (精度误差)

# 解决方法：使用 math.isclose()
import math
print(math.isclose(a, b))  # True

# 或者设置容差
tolerance = 1e-9
print(abs(a - b) < tolerance)  # True
```

### 3.2.4 链式比较

```python
# 链式比较
age = 25
print(18 <= age <= 65)  # True

# 等价于
print(18 <= age and age <= 65)  # True

# 多个比较
x = 5
print(1 < x < 10 > 3)  # True
```

## 3.3 逻辑运算符

逻辑运算符用于组合布尔值或条件表达式。

### 3.3.1 基本逻辑运算

```python
a = True
b = False

print(a and b)  # False (与运算)
print(a or b)   # True  (或运算)
print(not a)    # False (非运算)
```

### 3.3.2 短路求值

Python 的逻辑运算符使用短路求值，提高效率：

```python
# and 运算的短路
def check_first():
    print("检查第一个条件")
    return False

def check_second():
    print("检查第二个条件")
    return True

# 短路求值：第一个条件为False，后面的不执行
result = check_first() and check_second()
print(result)  # False

# or 运算的短路
result = check_first() or check_second()
print(result)  # True
```

### 3.3.3 逻辑运算的返回值

```python
# and 运算返回最后一个为真的值，或第一个为假的值
print(1 and 2)      # 2
print(1 and 0)      # 0
print(0 and 2)      # 0

# or 运算返回第一个为真的值，或最后一个为假的值
print(1 or 2)       # 1
print(0 or 2)       # 2
print(0 or False)   # False
```

### 3.3.4 复杂条件表达式

```python
# 检查用户权限
user_role = "admin"
is_logged_in = True
has_permission = False

# 复杂的权限检查
can_access = (user_role == "admin" and is_logged_in) or \
             (user_role == "user" and is_logged_in and has_permission)

print(can_access)  # True

# 检查输入有效性
def is_valid_input(value):
    return value is not None and \
           isinstance(value, (int, float)) and \
           value > 0

print(is_valid_input(5))     # True
print(is_valid_input(-1))    # False
print(is_valid_input("5"))   # False
```

## 3.4 赋值运算符

赋值运算符用于给变量赋值，可以与算术运算符结合使用。

### 3.4.1 基本赋值

```python
# 基本赋值
x = 10
name = "Python"

# 多重赋值
a = b = c = 0

# 解包赋值
x, y = 1, 2
```

### 3.4.2 复合赋值运算符

```python
x = 10

# 加法赋值
x += 5   # x = x + 5
print(x) # 15

# 减法赋值
x -= 3   # x = x - 3
print(x) # 12

# 乘法赋值
x *= 2   # x = x * 2
print(x) # 24

# 除法赋值
x /= 4   # x = x / 4
print(x) # 6.0

# 整除赋值
x //= 2  # x = x // 2
print(x) # 3.0

# 取余赋值
x %= 2   # x = x % 2
print(x) # 1.0

# 幂赋值
x **= 3  # x = x ** 3
print(x) # 1.0
```

### 3.4.3 赋值表达式的返回值

```python
# 赋值表达式有返回值
x = 5
y = (x = 10)  # 错误！赋值不是表达式

# 使用海象运算符（Python 3.8+）
if (n := len("hello")) > 3:
    print(f"字符串长度为: {n}")

# 在循环中使用
numbers = [1, 2, 3, 4, 5]
filtered = []
while (num := numbers.pop()) > 2:
    filtered.append(num)

print(filtered)  # [5, 4, 3]
```

## 3.5 运算符优先级

运算符优先级决定了表达式中运算的执行顺序。

### 3.5.1 运算符优先级表

从高到低：

1. **括号**: `()`
2. **成员访问**: `x.attribute`
3. **索引/切片**: `x[key]`
4. **函数调用**: `func()`
5. **正负号**: `+x`, `-x`
6. **幂运算**: `**`
7. **乘除**: `*`, `/`, `//`, `%`
8. **加减**: `+`, `-`
9. **比较**: `==`, `!=`, `<`, `>`, `<=`, `>=`
10. **逻辑非**: `not`
11. **逻辑与**: `and`
12. **逻辑或**: `or`
13. **赋值**: `=`, `+=`, `-=`, etc.

### 3.5.2 优先级示例

```python
# 算术运算优先级
result = 2 + 3 * 4
print(result)  # 14 (先乘后加)

# 使用括号改变优先级
result = (2 + 3) * 4
print(result)  # 20

# 比较和逻辑运算
x = 5
result = x > 3 and x < 10
print(result)  # True

# 复杂的表达式
result = 2 ** 3 * 4 + 5
print(result)  # 37 (2^3 = 8, 8*4 = 32, 32+5 = 37)

# 使用括号提高可读性
result = (2 ** 3) * 4 + 5
print(result)  # 37
```

### 3.5.3 避免优先级陷阱

```python
# 常见的优先级错误
x = 5
y = 10

# 错误：期望 (x > 3 and x < 10) or y == 5
result = x > 3 and x < 10 or y == 5
print(result)  # True (and 优先级高于 or)

# 正确写法
result = (x > 3 and x < 10) or y == 5
print(result)  # True

# 另一个例子
# 错误：期望 not (x == 5 or y == 10)
result = not x == 5 or y == 10
print(result)  # True (== 优先级高于 not)

# 正确写法
result = not (x == 5 or y == 10)
print(result)  # False
```

### 3.5.4 最佳实践

**使用括号提高可读性：**
```python
# 不推荐
if a and b or c and d:
    pass

# 推荐
if (a and b) or (c and d):
    pass

# 复杂表达式
# 不推荐
result = a + b * c - d / e

# 推荐
result = a + (b * c) - (d / e)
```

**避免过度复杂的表达式：**
```python
# 不推荐：一行代码做太多事
result = (x if x > 0 else -x) ** 2 + math.sqrt(abs(y)) if y != 0 else 0

# 推荐：拆分成多行
if y != 0:
    abs_x = abs(x)
    sqrt_y = math.sqrt(abs(y))
    result = abs_x ** 2 + sqrt_y
else:
    result = 0
```

## 总结

本章介绍了 Python 的运算符和表达式：
- 算术运算符：加减乘除、整除、取余、幂运算
- 比较运算符：等于、不等于、大小比较
- 逻辑运算符：and、or、not，以及短路求值
- 赋值运算符：基本赋值和复合赋值
- 运算符优先级：执行顺序和常见陷阱

通过本章的学习，你应该能够：
1. 熟练使用各种算术运算
2. 编写复杂的条件表达式
3. 理解运算符的优先级规则
4. 避免常见的优先级错误

下一章将介绍条件语句，这是控制程序流程的基础。
