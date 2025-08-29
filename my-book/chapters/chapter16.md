# 第16章：特殊方法

## 16.1 特殊方法的概述

特殊方法（也叫魔法方法）是以双下划线开头和结尾的方法，它们允许自定义类的行为。

### 16.1.1 特殊方法的命名约定

```python
# 特殊方法的基本命名规则：
# __method_name__

class Example:
    """特殊方法示例"""

    def __init__(self, value):
        """构造函数"""
        self.value = value

    def __str__(self):
        """字符串表示（用户友好）"""
        return f"Example(value={self.value})"

    def __repr__(self):
        """字符串表示（开发者友好）"""
        return f"Example({self.value!r})"

    def __len__(self):
        """长度"""
        return 1

    def __call__(self):
        """使对象可调用"""
        return f"调用 Example，值为 {self.value}"

# 使用示例
obj = Example(42)

print(str(obj))    # Example(value=42)
print(repr(obj))   # Example(42)
print(len(obj))    # 1
print(obj())       # 调用 Example，值为 42
```

### 16.1.2 特殊方法的分类

```python
# 特殊方法可以分为几类：

# 1. 创建和销毁
# __new__, __init__, __del__

# 2. 字符串表示
# __str__, __repr__, __format__

# 3. 算术运算
# __add__, __sub__, __mul__, __truediv__, etc.

# 4. 比较运算
# __eq__, __lt__, __le__, __gt__, __ge__, __ne__

# 5. 容器操作
# __len__, __getitem__, __setitem__, __delitem__, __iter__, __contains__

# 6. 属性访问
# __getattr__, __setattr__, __delattr__, __getattribute__

# 7. 上下文管理
# __enter__, __exit__

# 8. 可调用对象
# __call__

# 9. 其他
# __hash__, __bool__, __getstate__, __setstate__, etc.
```

### 16.1.3 特殊方法的调用方式

```python
class Demo:
    """演示特殊方法调用"""

    def __init__(self, x):
        self.x = x

    def __add__(self, other):
        return Demo(self.x + other.x)

    def __str__(self):
        return f"Demo({self.x})"

# 特殊方法通常由 Python 解释器自动调用
a = Demo(5)
b = Demo(3)

# __add__ 被 + 运算符调用
c = a + b  # 等价于 a.__add__(b)
print(c)   # Demo(8)

# __str__ 被 str() 函数和 print() 调用
print(a)   # 等价于 str(a)，调用 a.__str__()

# 直接调用特殊方法
print(a.__str__())   # 直接调用
print(str(a))        # 通过内置函数调用
```

## 16.2 构造和析构方法

### 16.2.1 __new__ 方法

```python
class Singleton:
    """单例模式示例"""

    _instance = None

    def __new__(cls, *args, **kwargs):
        """控制实例创建"""
        if cls._instance is None:
            print("创建新实例")
            cls._instance = super().__new__(cls)
        else:
            print("返回现有实例")
        return cls._instance

    def __init__(self, value):
        self.value = value

# 测试单例
s1 = Singleton(1)
s2 = Singleton(2)

print(s1 is s2)      # True
print(s1.value)      # 1 (第一次创建时的值)
print(s2.value)      # 1 (不是 2，因为是同一个实例)

class ImmutablePoint:
    """不可变点类"""

    def __new__(cls, x, y):
        # 验证参数
        if not isinstance(x, (int, float)) or not isinstance(y, (int, float)):
            raise TypeError("坐标必须是数字")

        # 创建实例
        instance = super().__new__(cls)
        instance.x = x
        instance.y = y
        return instance

    def __str__(self):
        return f"Point({self.x}, {self.y})"

point = ImmutablePoint(1, 2)
print(point)  # Point(1, 2)
```

### 16.2.2 __init__ 方法

```python
class Person:
    """人"""

    def __init__(self, name, age=0):
        """初始化方法"""
        print(f"初始化 Person: {name}")
        self.name = name
        self.age = age

    def __str__(self):
        return f"{self.name} ({self.age}岁)"

class Student(Person):
    """学生"""

    def __init__(self, name, age, student_id):
        # 调用父类 __init__
        super().__init__(name, age)
        print(f"初始化 Student: {student_id}")
        self.student_id = student_id

    def __str__(self):
        return f"{super().__str__()} - 学号: {self.student_id}"

# 创建实例
person = Person("Alice", 25)
student = Student("Bob", 20, "2023001")

print(person)   # Alice (25岁)
print(student)  # Bob (20岁) - 学号: 2023001
```

### 16.2.3 __del__ 方法

```python
class Resource:
    """资源管理"""

    def __init__(self, name):
        self.name = name
        print(f"资源 {name} 已创建")

    def __del__(self):
        """析构方法"""
        print(f"资源 {self.name} 已被清理")

class FileHandler:
    """文件处理器"""

    def __init__(self, filename):
        self.filename = filename
        self.file = None

    def open_file(self):
        """打开文件"""
        self.file = open(self.filename, 'w')
        print(f"文件 {self.filename} 已打开")

    def __del__(self):
        """确保文件被关闭"""
        if self.file:
            self.file.close()
            print(f"文件 {self.filename} 已关闭")

# 使用示例
def test_resource():
    resource = Resource("测试资源")
    # 函数结束时，resource 会被自动清理

test_resource()
print("函数执行完毕")

# 文件处理示例
handler = FileHandler("test.txt")
handler.open_file()
# 程序结束时，文件会被自动关闭
```

## 16.3 字符串表示方法

### 16.3.1 __str__ vs __repr__

```python
class Book:
    """图书类"""

    def __init__(self, title, author, year):
        self.title = title
        self.author = author
        self.year = year

    def __str__(self):
        """用户友好的字符串表示"""
        return f"《{self.title}》 - {self.author} ({self.year})"

    def __repr__(self):
        """开发者友好的字符串表示"""
        return f"Book(title='{self.title}', author='{self.author}', year={self.year})"

book = Book("Python编程", "张三", 2023)

print(str(book))   # 《Python编程》 - 张三 (2023)
print(repr(book))  # Book(title='Python编程', author='张三', year=2023)

# 在容器中
books = [book]
print(books)  # [Book(title='Python编程', author='张三', year=2023)]

# 交互式环境
# >>> book
# Book(title='Python编程', author='张三', year=2023)
```

### 16.3.2 __format__ 方法

```python
class Vector:
    """向量类"""

    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return f"Vector({self.x}, {self.y})"

    def __format__(self, format_spec):
        """自定义格式化"""
        if format_spec == 'polar':
            # 极坐标格式
            import math
            magnitude = math.sqrt(self.x**2 + self.y**2)
            angle = math.degrees(math.atan2(self.y, self.x))
            return f"Vector(magnitude={magnitude:.2f}, angle={angle:.1f}°)"
        elif format_spec == 'complex':
            # 复数格式
            return f"{self.x} + {self.y}j"
        else:
            # 默认格式
            return f"Vector({self.x}, {self.y})"

v = Vector(3, 4)

print(f"{v}")           # Vector(3, 4)
print(f"{v:polar}")     # Vector(magnitude=5.00, angle=53.1°)
print(f"{v:complex}")   # 3 + 4j

class Temperature:
    """温度类"""

    def __init__(self, celsius):
        self.celsius = celsius

    def __format__(self, format_spec):
        if format_spec.endswith('F'):
            # 华氏度
            fahrenheit = self.celsius * 9/5 + 32
            return f"{fahrenheit:.1f}°F"
        elif format_spec.endswith('K'):
            # 开尔文
            kelvin = self.celsius + 273.15
            return f"{kelvin:.1f}K"
        else:
            # 摄氏度
            return f"{self.celsius:.1f}°C"

temp = Temperature(20)
print(f"摄氏度: {temp}")      # 20.0°C
print(f"华氏度: {temp:F}")    # 68.0°F
print(f"开尔文: {temp:K}")    # 293.1K
```

## 16.4 算术运算符重载

### 16.4.1 基本算术运算

```python
class ComplexNumber:
    """复数类"""

    def __init__(self, real, imag=0):
        self.real = real
        self.imag = imag

    def __str__(self):
        if self.imag >= 0:
            return f"{self.real} + {self.imag}i"
        else:
            return f"{self.real} - {abs(self.imag)}i"

    def __add__(self, other):
        """加法"""
        if isinstance(other, ComplexNumber):
            return ComplexNumber(self.real + other.real, self.imag + other.imag)
        elif isinstance(other, (int, float)):
            return ComplexNumber(self.real + other, self.imag)
        else:
            return NotImplemented

    def __sub__(self, other):
        """减法"""
        if isinstance(other, ComplexNumber):
            return ComplexNumber(self.real - other.real, self.imag - other.imag)
        elif isinstance(other, (int, float)):
            return ComplexNumber(self.real - other, self.imag)
        else:
            return NotImplemented

    def __mul__(self, other):
        """乘法"""
        if isinstance(other, ComplexNumber):
            real = self.real * other.real - self.imag * other.imag
            imag = self.real * other.imag + self.imag * other.real
            return ComplexNumber(real, imag)
        elif isinstance(other, (int, float)):
            return ComplexNumber(self.real * other, self.imag * other)
        else:
            return NotImplemented

    def __truediv__(self, other):
        """除法"""
        if isinstance(other, ComplexNumber):
            denominator = other.real**2 + other.imag**2
            real = (self.real * other.real + self.imag * other.imag) / denominator
            imag = (self.imag * other.real - self.real * other.imag) / denominator
            return ComplexNumber(real, imag)
        elif isinstance(other, (int, float)):
            return ComplexNumber(self.real / other, self.imag / other)
        else:
            return NotImplemented

# 使用示例
a = ComplexNumber(3, 4)    # 3 + 4i
b = ComplexNumber(1, 2)    # 1 + 2i

print(f"a = {a}")
print(f"b = {b}")
print(f"a + b = {a + b}")
print(f"a - b = {a - b}")
print(f"a * b = {a * b}")
print(f"a / b = {a / b}")
print(f"a + 5 = {a + 5}")
```

### 16.4.2 反向运算和原地运算

```python
class Vector:
    """向量类"""

    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return f"Vector({self.x}, {self.y})"

    def __add__(self, other):
        """加法"""
        if isinstance(other, Vector):
            return Vector(self.x + other.x, self.y + other.y)
        return NotImplemented

    def __radd__(self, other):
        """反向加法（当 Vector 在右边时）"""
        return self.__add__(other)

    def __iadd__(self, other):
        """原地加法 (+=)"""
        if isinstance(other, Vector):
            self.x += other.x
            self.y += other.y
            return self
        return NotImplemented

    def __mul__(self, scalar):
        """乘法（标量）"""
        if isinstance(scalar, (int, float)):
            return Vector(self.x * scalar, self.y * scalar)
        return NotImplemented

    def __rmul__(self, scalar):
        """反向乘法"""
        return self.__mul__(scalar)

    def __imul__(self, scalar):
        """原地乘法 (*=)"""
        if isinstance(scalar, (int, float)):
            self.x *= scalar
            self.y *= scalar
            return self
        return NotImplemented

# 使用示例
v1 = Vector(1, 2)
v2 = Vector(3, 4)

# 正常运算
print(f"v1 + v2 = {v1 + v2}")

# 反向运算
print(f"5 + v1 = {5 + v1}")  # 调用 v1.__radd__(5)

# 原地运算
v3 = Vector(1, 2)
print(f"v3 = {v3}")
v3 += v2  # 调用 v3.__iadd__(v2)
print(f"v3 += v2 -> {v3}")

v4 = Vector(2, 3)
print(f"v4 = {v4}")
v4 *= 3   # 调用 v4.__imul__(3)
print(f"v4 *= 3 -> {v4}")
```

### 16.4.3 一元运算符

```python
class Number:
    """数字类"""

    def __init__(self, value):
        self.value = value

    def __str__(self):
        return str(self.value)

    def __neg__(self):
        """负号 (-)"""
        return Number(-self.value)

    def __pos__(self):
        """正号 (+)"""
        return Number(+self.value)

    def __abs__(self):
        """绝对值 (abs())"""
        return Number(abs(self.value))

    def __invert__(self):
        """按位取反 (~)"""
        return Number(~int(self.value))

    def __round__(self, ndigits=None):
        """四舍五入 (round())"""
        return Number(round(self.value, ndigits))

# 使用示例
num = Number(5.7)

print(f"num = {num}")
print(f"-num = {-num}")
print(f"+num = {+num}")
print(f"abs(num) = {abs(num)}")
print(f"~num = {~num}")
print(f"round(num) = {round(num)}")
print(f"round(num, 1) = {round(num, 1)}")

class BooleanLike:
    """布尔类"""

    def __init__(self, value):
        self.value = bool(value)

    def __str__(self):
        return str(self.value)

    def __bool__(self):
        """布尔转换"""
        return self.value

    def __int__(self):
        """整数转换"""
        return int(self.value)

    def __float__(self):
        """浮点数转换"""
        return float(self.value)

# 使用示例
bool_obj = BooleanLike(1)
print(f"bool_obj = {bool_obj}")
print(f"bool(bool_obj) = {bool(bool_obj)}")
print(f"int(bool_obj) = {int(bool_obj)}")
print(f"float(bool_obj) = {float(bool_obj)}")

# 在条件中使用
if bool_obj:
    print("对象为真")
else:
    print("对象为假")
```

## 16.5 比较运算符重载

### 16.5.1 比较运算符

```python
class Person:
    """人"""

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        return f"{self.name} ({self.age}岁)"

    def __eq__(self, other):
        """等于 (==)"""
        if isinstance(other, Person):
            return self.name == other.name and self.age == other.age
        return False

    def __ne__(self, other):
        """不等于 (!=)"""
        return not self.__eq__(other)

    def __lt__(self, other):
        """小于 (<)"""
        if isinstance(other, Person):
            return self.age < other.age
        return NotImplemented

    def __le__(self, other):
        """小于等于 (<=)"""
        if isinstance(other, Person):
            return self.age <= other.age
        return NotImplemented

    def __gt__(self, other):
        """大于 (>)"""
        if isinstance(other, Person):
            return self.age > other.age
        return NotImplemented

    def __ge__(self, other):
        """大于等于 (>=)"""
        if isinstance(other, Person):
            return self.age >= other.age
        return NotImplemented

# 使用示例
p1 = Person("Alice", 25)
p2 = Person("Bob", 30)
p3 = Person("Alice", 25)

print(f"p1 == p2: {p1 == p2}")  # False
print(f"p1 == p3: {p1 == p3}")  # True
print(f"p1 != p2: {p1 != p2}")  # True

print(f"p1 < p2: {p1 < p2}")    # True (按年龄比较)
print(f"p1 > p2: {p1 > p2}")    # False

# 排序
people = [p2, p1, p3]
people.sort()  # 使用 < 运算符排序
for person in people:
    print(person)
```

### 16.5.2 哈希和相等性

```python
class Point:
    """点类"""

    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return f"Point({self.x}, {self.y})"

    def __eq__(self, other):
        """相等性比较"""
        if isinstance(other, Point):
            return self.x == other.x and self.y == other.y
        return False

    def __hash__(self):
        """哈希值"""
        return hash((self.x, self.y))

# 使用示例
p1 = Point(1, 2)
p2 = Point(1, 2)
p3 = Point(2, 3)

print(f"p1 == p2: {p1 == p2}")  # True
print(f"p1 == p3: {p1 == p3}")  # False

print(f"hash(p1): {hash(p1)}")
print(f"hash(p2): {hash(p2)}")
print(f"hash(p3): {hash(p3)}")

# 相等的对象应该有相同的哈希值
print(f"hash(p1) == hash(p2): {hash(p1) == hash(p2)}")

# 可以在集合和字典中使用
points = {p1, p2, p3}  # p1 和 p2 被认为是相同的
print(f"集合大小: {len(points)}")  # 2

point_dict = {p1: "第一个点", p3: "第二个点"}
print(f"字典: {point_dict}")
```

## 16.6 容器方法

### 16.6.1 序列协议

```python
class MyList:
    """自定义列表"""

    def __init__(self, items=None):
        self.items = list(items) if items else []

    def __str__(self):
        return str(self.items)

    def __len__(self):
        """长度"""
        return len(self.items)

    def __getitem__(self, index):
        """获取元素"""
        return self.items[index]

    def __setitem__(self, index, value):
        """设置元素"""
        self.items[index] = value

    def __delitem__(self, index):
        """删除元素"""
        del self.items[index]

    def __iter__(self):
        """迭代器"""
        return iter(self.items)

    def __reversed__(self):
        """反向迭代器"""
        return reversed(self.items)

    def __contains__(self, item):
        """包含检查"""
        return item in self.items

# 使用示例
my_list = MyList([1, 2, 3, 4, 5])

print(f"列表: {my_list}")
print(f"长度: {len(my_list)}")
print(f"第一个元素: {my_list[0]}")
print(f"最后一个元素: {my_list[-1]}")

# 修改元素
my_list[2] = 99
print(f"修改后: {my_list}")

# 切片
print(f"切片 [1:4]: {my_list[1:4]}")

# 迭代
print("正向迭代:")
for item in my_list:
    print(item, end=" ")
print()

print("反向迭代:")
for item in reversed(my_list):
    print(item, end=" ")
print()

# 包含检查
print(f"包含 99: {99 in my_list}")
print(f"包含 10: {10 in my_list}")
```

### 16.6.2 映射协议

```python
class MyDict:
    """自定义字典"""

    def __init__(self, data=None):
        self.data = dict(data) if data else {}

    def __str__(self):
        return str(self.data)

    def __len__(self):
        """长度"""
        return len(self.data)

    def __getitem__(self, key):
        """获取值"""
        return self.data[key]

    def __setitem__(self, key, value):
        """设置值"""
        self.data[key] = value

    def __delitem__(self, key):
        """删除键值对"""
        del self.data[key]

    def __iter__(self):
        """键的迭代器"""
        return iter(self.data)

    def __contains__(self, key):
        """包含检查"""
        return key in self.data

    def keys(self):
        """获取所有键"""
        return self.data.keys()

    def values(self):
        """获取所有值"""
        return self.data.values()

    def items(self):
        """获取所有键值对"""
        return self.data.items()

# 使用示例
my_dict = MyDict({'a': 1, 'b': 2, 'c': 3})

print(f"字典: {my_dict}")
print(f"长度: {len(my_dict)}")
print(f"获取 'a': {my_dict['a']}")

# 设置值
my_dict['d'] = 4
print(f"添加后: {my_dict}")

# 删除
del my_dict['b']
print(f"删除后: {my_dict}")

# 迭代
print("键:")
for key in my_dict:
    print(key, end=" ")
print()

print("值:")
for value in my_dict.values():
    print(value, end=" ")
print()

print("键值对:")
for key, value in my_dict.items():
    print(f"{key}: {value}", end=" ")
print()

# 包含检查
print(f"包含 'a': {'a' in my_dict}")
print(f"包含 'z': {'z' in my_dict}")
```

### 16.6.3 自定义集合

```python
class MySet:
    """自定义集合"""

    def __init__(self, items=None):
        self.items = set(items) if items else set()

    def __str__(self):
        return str(self.items)

    def __len__(self):
        """长度"""
        return len(self.items)

    def __contains__(self, item):
        """包含检查"""
        return item in self.items

    def __iter__(self):
        """迭代器"""
        return iter(self.items)

    def __and__(self, other):
        """交集 (&)"""
        if isinstance(other, MySet):
            return MySet(self.items & other.items)
        return NotImplemented

    def __or__(self, other):
        """并集 (|)"""
        if isinstance(other, MySet):
            return MySet(self.items | other.items)
        return NotImplemented

    def __xor__(self, other):
        """对称差集 (^)"""
        if isinstance(other, MySet):
            return MySet(self.items ^ other.items)
        return NotImplemented

    def __sub__(self, other):
        """差集 (-)"""
        if isinstance(other, MySet):
            return MySet(self.items - other.items)
        return NotImplemented

    def add(self, item):
        """添加元素"""
        self.items.add(item)

    def remove(self, item):
        """移除元素"""
        self.items.remove(item)

# 使用示例
set1 = MySet([1, 2, 3, 4])
set2 = MySet([3, 4, 5, 6])

print(f"集合1: {set1}")
print(f"集合2: {set2}")

print(f"交集: {set1 & set2}")
print(f"并集: {set1 | set2}")
print(f"差集: {set1 - set2}")
print(f"对称差集: {set1 ^ set2}")

print(f"长度: {len(set1)}")
print(f"包含 3: {3 in set1}")
print(f"包含 7: {7 in set1}")

# 迭代
print("集合1的元素:")
for item in set1:
    print(item, end=" ")
print()
```

## 16.7 上下文管理器

### 16.7.1 __enter__ 和 __exit__

```python
class FileManager:
    """文件管理器"""

    def __init__(self, filename, mode='r'):
        self.filename = filename
        self.mode = mode
        self.file = None

    def __enter__(self):
        """进入上下文"""
        self.file = open(self.filename, self.mode)
        print(f"打开文件: {self.filename}")
        return self.file

    def __exit__(self, exc_type, exc_val, exc_tb):
        """退出上下文"""
        if self.file:
            self.file.close()
            print(f"关闭文件: {self.filename}")

        # 返回 False 表示不处理异常
        # 返回 True 表示异常已被处理
        return False

# 使用示例
with FileManager('example.txt', 'w') as f:
    f.write("Hello, World!")
    f.write("\nThis is a test file.")

print("文件操作完成")

# 异常处理
class SafeFileManager:
    """安全的文件管理器"""

    def __init__(self, filename, mode='r'):
        self.filename = filename
        self.mode = mode
        self.file = None

    def __enter__(self):
        self.file = open(self.filename, self.mode)
        return self.file

    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.file:
            self.file.close()

        if exc_type is not None:
            print(f"发生异常: {exc_type.__name__}: {exc_val}")
            return True  # 处理异常

        return False

# 使用示例
try:
    with SafeFileManager('example.txt', 'r') as f:
        content = f.read()
        print(f"文件内容: {repr(content)}")
        # 故意引发异常
        raise ValueError("测试异常")
except ValueError:
    print("异常被处理了")
```

### 16.7.2 contextlib 模块

```python
from contextlib import contextmanager

@contextmanager
def database_connection(connection_string):
    """数据库连接上下文管理器"""
    connection = None
    try:
        # 模拟连接数据库
        connection = f"Connected to {connection_string}"
        print(f"连接数据库: {connection_string}")
        yield connection
    except Exception as e:
        print(f"数据库操作出错: {e}")
        raise
    finally:
        if connection:
            print(f"断开数据库连接: {connection_string}")

# 使用示例
with database_connection("postgresql://localhost/mydb") as conn:
    print(f"执行查询: {conn}")
    # 执行数据库操作
    print("查询完成")

@contextmanager
def timer(description):
    """计时器上下文管理器"""
    import time
    start = time.time()
    print(f"开始: {description}")
    try:
        yield
    finally:
        end = time.time()
        print(f"结束: {description}，耗时: {end - start:.3f}秒")

# 使用示例
with timer("文件处理"):
    import time
    time.sleep(1)  # 模拟耗时操作
    print("处理文件...")

@contextmanager
def suppress_exceptions(*exception_types):
    """抑制指定异常"""
    try:
        yield
    except exception_types as e:
        print(f"抑制异常: {type(e).__name__}: {e}")

# 使用示例
with suppress_exceptions(ValueError, TypeError):
    int("not a number")  # ValueError
    "string" + 123       # TypeError

print("继续执行...")
```

### 16.7.3 嵌套上下文管理器

```python
class Indenter:
    """缩进管理器"""

    def __init__(self, indent_size=2):
        self.indent_size = indent_size
        self.level = 0

    def __enter__(self):
        self.level += 1
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        self.level -= 1
        return False

    def print(self, text):
        indent = " " * (self.level * self.indent_size)
        print(f"{indent}{text}")

# 使用示例
with Indenter() as indenter:
    indenter.print("第一级")
    with indenter:
        indenter.print("第二级")
        with indenter:
            indenter.print("第三级")
        indenter.print("回到第二级")
    indenter.print("回到第一级")

# 多个上下文管理器
class Database:
    def __enter__(self):
        print("连接数据库")
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("断开数据库连接")

class Cache:
    def __enter__(self):
        print("连接缓存")
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("断开缓存连接")

# 嵌套使用
with Database() as db, Cache() as cache:
    print("使用数据库和缓存")
    print("执行业务逻辑...")

print("所有连接已关闭")
```

## 总结

本章介绍了 Python 的特殊方法（魔法方法）：
- 理解特殊方法的基本概念和命名约定
- 掌握构造和析构方法的使用
- 学习字符串表示方法的实现
- 掌握算术运算符和比较运算符的重载
- 了解容器协议的实现
- 学习上下文管理器的创建和使用

通过本章的学习，你应该能够：
1. 自定义类的基本行为
2. 实现运算符重载
3. 创建容器类
4. 使用上下文管理器管理资源
5. 编写更 Pythonic 的代码

下一章将介绍异常处理的基础知识。
