# 第5章：循环语句

## 5.1 while 循环

while 循环在条件为真时重复执行代码块，适用于不确定循环次数的情况。

### 5.1.1 基本语法

```python
# 基本 while 循环
count = 1

while count <= 5:
    print(f"这是第 {count} 次循环")
    count += 1

print("循环结束")
```

**语法结构：**
```python
while 条件表达式:
    # 循环体
    语句1
    语句2
    ...
```

### 5.1.2 计数器控制的循环

```python
# 使用计数器
i = 0
while i < 10:
    print(f"当前数字: {i}")
    i += 1

# 倒计时
countdown = 10
while countdown > 0:
    print(f"倒计时: {countdown}")
    countdown -= 1
print("发射！")
```

### 5.1.3 条件控制的循环

```python
# 用户输入验证
password = ""
while len(password) < 6:
    password = input("请输入至少6位密码: ")
    if len(password) < 6:
        print("密码太短，请重新输入")

print("密码设置成功")

# 游戏循环
import random

target = random.randint(1, 100)
guess = 0

while guess != target:
    guess = int(input("猜一个1-100的数字: "))
    if guess > target:
        print("太大了")
    elif guess < target:
        print("太小了")
    else:
        print("猜对了！")

print(f"目标数字是: {target}")
```

### 5.1.4 无限循环和 break

```python
# 无限循环
while True:
    user_input = input("输入 'quit' 退出: ")
    if user_input == 'quit':
        break  # 跳出循环
    print(f"你输入了: {user_input}")

print("程序结束")

# 菜单系统
while True:
    print("\n=== 菜单 ===")
    print("1. 查看余额")
    print("2. 存款")
    print("3. 取款")
    print("4. 退出")

    choice = input("请选择: ")

    if choice == '1':
        print("您的余额是: 1000元")
    elif choice == '2':
        amount = float(input("存款金额: "))
        print(f"存款 {amount} 元成功")
    elif choice == '3':
        amount = float(input("取款金额: "))
        print(f"取款 {amount} 元成功")
    elif choice == '4':
        print("感谢使用，再见！")
        break
    else:
        print("无效选择，请重新输入")
```

## 5.2 for 循环

for 循环用于遍历序列（如列表、字符串、元组）或其他可迭代对象。

### 5.2.1 基本语法

```python
# 遍历列表
fruits = ["apple", "banana", "orange"]

for fruit in fruits:
    print(f"我喜欢吃 {fruit}")

# 遍历字符串
message = "Hello"
for char in message:
    print(char)

# 遍历元组
coordinates = (10, 20, 30)
for coord in coordinates:
    print(f"坐标: {coord}")
```

**语法结构：**
```python
for 变量 in 可迭代对象:
    # 循环体
    语句1
    语句2
    ...
```

### 5.2.2 索引和值同时遍历

```python
# 使用 enumerate() 获取索引
fruits = ["apple", "banana", "orange"]

for index, fruit in enumerate(fruits):
    print(f"第 {index + 1} 个水果是 {fruit}")

# 从指定索引开始
for index, fruit in enumerate(fruits, start=1):
    print(f"第 {index} 个水果是 {fruit}")
```

### 5.2.3 字典遍历

```python
# 遍历字典的键
student = {"name": "Alice", "age": 20, "grade": "A"}

for key in student:
    print(f"{key}: {student[key]}")

# 遍历键值对
for key, value in student.items():
    print(f"{key}: {value}")

# 只遍历值
for value in student.values():
    print(value)

# 只遍历键
for key in student.keys():
    print(key)
```

### 5.2.4 文件遍历

```python
# 逐行读取文件
with open("example.txt", "r") as file:
    for line in file:
        print(line.strip())  # strip() 移除换行符

# 读取所有行到列表
with open("example.txt", "r") as file:
    lines = file.readlines()
    for i, line in enumerate(lines, 1):
        print(f"第 {i} 行: {line.strip()}")
```

## 5.3 range() 函数

range() 函数生成一个整数序列，常用于 for 循环。

### 5.3.1 基本用法

```python
# range(stop)
for i in range(5):
    print(i)  # 0, 1, 2, 3, 4

# range(start, stop)
for i in range(2, 8):
    print(i)  # 2, 3, 4, 5, 6, 7

# range(start, stop, step)
for i in range(1, 10, 2):
    print(i)  # 1, 3, 5, 7, 9

# 负步长
for i in range(10, 0, -1):
    print(i)  # 10, 9, 8, 7, 6, 5, 4, 3, 2, 1
```

### 5.3.2 range() 的特性

```python
# range() 返回 range 对象，不是列表
r = range(5)
print(type(r))  # <class 'range'>
print(list(r))  # [0, 1, 2, 3, 4]

# 内存效率
import sys
r = range(1000000)
print(sys.getsizeof(r))  # 占用很少内存

# 比较
r1 = range(5)
r2 = range(5)
print(r1 == r2)  # True
print(r1 is r2)  # False（不同对象）
```

### 5.3.3 实际应用

```python
# 生成数列
numbers = list(range(1, 11))  # [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# 重复执行
for _ in range(3):
    print("Hello, World!")

# 创建矩阵
rows = 3
cols = 4
matrix = [[i * cols + j for j in range(cols)] for i in range(rows)]
print(matrix)
# [[0, 1, 2, 3], [4, 5, 6, 7], [8, 9, 10, 11]]

# 分页处理
items = list(range(1, 101))  # 100个项目
page_size = 10

for page in range(0, len(items), page_size):
    current_page = items[page:page + page_size]
    print(f"第 {page // page_size + 1} 页: {current_page}")
```

## 5.4 break 和 continue 语句

break 和 continue 用于控制循环的执行流程。

### 5.4.1 break 语句

break 语句立即终止整个循环：

```python
# 查找元素
numbers = [1, 3, 5, 7, 9, 2, 4, 6, 8, 10]
target = 7

for num in numbers:
    if num == target:
        print(f"找到了 {target}")
        break
    print(f"检查 {num}")

print("搜索结束")

# 无限循环中的 break
while True:
    command = input("输入命令 (quit 退出): ")
    if command == 'quit':
        break
    print(f"执行命令: {command}")
```

### 5.4.2 continue 语句

continue 语句跳过当前迭代，继续下一次循环：

```python
# 跳过偶数
for i in range(1, 11):
    if i % 2 == 0:
        continue
    print(f"奇数: {i}")

# 处理异常数据
data = [1, 2, None, 4, None, 6, 7, None, 9, 10]

for item in data:
    if item is None:
        continue
    print(f"处理数据: {item}")

# 过滤和处理
students = [
    {"name": "Alice", "score": 85},
    {"name": "Bob", "score": 92},
    {"name": "Charlie", "score": 78},
    {"name": "David", "score": 95}
]

for student in students:
    if student["score"] < 80:
        continue
    print(f"优秀学生: {student['name']} ({student['score']})")
```

### 5.4.3 break 和 continue 的区别

```python
# break vs continue
print("使用 break:")
for i in range(1, 6):
    if i == 3:
        break
    print(i)
# 输出: 1, 2

print("\n使用 continue:")
for i in range(1, 6):
    if i == 3:
        continue
    print(i)
# 输出: 1, 2, 4, 5
```

## 5.5 循环中的 else 子句

Python 的循环可以有 else 子句，在循环正常结束（不是被 break 终止）时执行。

### 5.5.1 while 循环的 else

```python
# 密码尝试
password = "secret"
attempts = 3

while attempts > 0:
    user_input = input("请输入密码: ")
    if user_input == password:
        print("登录成功")
        break
    attempts -= 1
    print(f"密码错误，还剩 {attempts} 次机会")
else:
    print("密码尝试次数过多，账户锁定")

# 寻找质数
def is_prime(n):
    if n < 2:
        return False
    for i in range(2, int(n ** 0.5) + 1):
        if n % i == 0:
            return False
    return True

number = 17
for i in range(2, number):
    if number % i == 0:
        print(f"{number} 不是质数")
        break
else:
    print(f"{number} 是质数")
```

### 5.5.2 for 循环的 else

```python
# 搜索元素
numbers = [1, 3, 5, 7, 9]
target = 4

for num in numbers:
    if num == target:
        print(f"找到了 {target}")
        break
else:
    print(f"{target} 不在列表中")

# 检查所有元素是否满足条件
scores = [85, 92, 78, 95, 88]

for score in scores:
    if score < 60:
        print("有不及格的成绩")
        break
else:
    print("所有成绩都及格")

# 文件搜索
def find_in_file(filename, search_text):
    with open(filename, 'r') as file:
        for line_num, line in enumerate(file, 1):
            if search_text in line:
                print(f"在第 {line_num} 行找到: {line.strip()}")
                break
        else:
            print(f"未在文件中找到: {search_text}")
```

### 5.5.3 else 子句的实际应用

```python
# 验证用户权限
def check_permissions(user, required_permissions):
    for permission in required_permissions:
        if permission not in user.get('permissions', []):
            print(f"缺少权限: {permission}")
            return False
    else:
        return True

user = {'permissions': ['read', 'write']}
required = ['read', 'admin']

if check_permissions(user, required):
    print("权限验证通过")
else:
    print("权限验证失败")

# 网络连接重试
import time

def connect_with_retry(host, max_attempts=3):
    for attempt in range(max_attempts):
        try:
            print(f"尝试连接 {host} (第 {attempt + 1} 次)")
            # 模拟连接
            if attempt == 1:  # 第二次尝试成功
                print("连接成功")
                return True
            time.sleep(1)
        except Exception as e:
            print(f"连接失败: {e}")
    else:
        print("所有连接尝试都失败了")
        return False
```

## 5.6 嵌套循环

在循环内部可以嵌套其他循环，用于处理多维数据或复杂逻辑。

### 5.6.1 基本嵌套结构

```python
# 打印乘法表
for i in range(1, 10):
    for j in range(1, i + 1):
        print(f"{j}×{i}={i*j:2}", end=" ")
    print()  # 换行

# 矩阵遍历
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

for i in range(len(matrix)):
    for j in range(len(matrix[i])):
        print(f"matrix[{i}][{j}] = {matrix[i][j]}")

# 坐标系遍历
for x in range(3):
    for y in range(3):
        print(f"点 ({x}, {y})")
```

### 5.6.2 break 和 continue 在嵌套循环中

```python
# 在嵌套循环中使用 break
for i in range(3):
    for j in range(3):
        if i == 1 and j == 1:
            print(f"跳出内层循环 at ({i}, {j})")
            break
        print(f"处理 ({i}, {j})")
    print(f"内层循环结束 for i={i}")
    print()

# 使用标签跳出多层循环（Python 不支持标签，但可以用其他方式）
def find_element(matrix, target):
    for i, row in enumerate(matrix):
        for j, element in enumerate(row):
            if element == target:
                return i, j
    return None, None

matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

row, col = find_element(matrix, 5)
if row is not None:
    print(f"找到元素 5 在 ({row}, {col})")
```

### 5.6.3 列表推导式中的嵌套

```python
# 传统嵌套循环
result = []
for i in range(3):
    for j in range(3):
        result.append((i, j))

print(result)  # [(0, 0), (0, 1), (0, 2), (1, 0), (1, 1), (1, 2), (2, 0), (2, 1), (2, 2)]

# 列表推导式
result = [(i, j) for i in range(3) for j in range(3)]
print(result)  # 相同结果

# 带条件的嵌套推导式
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
even_numbers = [num for row in matrix for num in row if num % 2 == 0]
print(even_numbers)  # [2, 4, 6, 8]
```

### 5.6.4 实际应用场景

```python
# 冒泡排序
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
    return arr

numbers = [64, 34, 25, 12, 22, 11, 90]
print("排序前:", numbers)
print("排序后:", bubble_sort(numbers))

# 生成组合
colors = ['红', '绿', '蓝']
sizes = ['大', '中', '小']

combinations = []
for color in colors:
    for size in sizes:
        combinations.append(f"{color}{size}")

print("所有组合:", combinations)

# 学生成绩统计
students = ['Alice', 'Bob', 'Charlie']
subjects = ['数学', '英语', '物理']
scores = [
    [85, 92, 78],  # Alice
    [90, 88, 95],  # Bob
    [75, 85, 82]   # Charlie
]

for i, student in enumerate(students):
    print(f"\n{student} 的成绩:")
    total = 0
    for j, subject in enumerate(subjects):
        score = scores[i][j]
        print(f"  {subject}: {score}")
        total += score
    print(f"  总分: {total}, 平均分: {total / len(subjects):.1f}")
```

## 总结

本章介绍了 Python 的循环语句：
- while 循环：条件控制的循环
- for 循环：遍历序列的循环
- range() 函数：生成整数序列
- break 和 continue：循环控制语句
- else 子句：循环正常结束时的处理
- 嵌套循环：多层循环结构

通过本章的学习，你应该能够：
1. 使用 while 和 for 循环处理重复任务
2. 熟练使用 range() 函数生成序列
3. 使用 break 和 continue 控制循环流程
4. 理解和使用循环的 else 子句
5. 编写嵌套循环处理复杂数据结构

下一章将介绍数据结构，这是组织和存储数据的重要工具。
