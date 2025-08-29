# 第13章：面向对象编程基础

## 13.1 面向对象编程简介

面向对象编程（Object-Oriented Programming，OOP）是一种编程范式，它将数据和操作数据的函数组织成对象。

### 13.1.1 OOP 的核心概念

```python
# 面向对象编程的核心概念：
# 1. 类（Class）：对象的模板或蓝图
# 2. 对象（Object）：类的实例
# 3. 封装（Encapsulation）：隐藏内部实现细节
# 4. 继承（Inheritance）：子类继承父类的特性
# 5. 多态（Polymorphism）：同一接口，不同实现

# 现实世界的类比
class Car:
    """汽车类 - 现实世界对象的抽象"""

    def __init__(self, brand, model, year):
        self.brand = brand      # 属性：品牌
        self.model = model      # 属性：型号
        self.year = year        # 属性：年份
        self.mileage = 0        # 属性：里程

    def drive(self, distance):  # 方法：驾驶
        """驾驶汽车"""
        self.mileage += distance
        return f"驾驶 {self.brand} {self.model} 行驶了 {distance} 公里"

    def get_info(self):         # 方法：获取信息
        """获取汽车信息"""
        return f"{self.year} {self.brand} {self.model}, 里程: {self.mileage} 公里"

# 创建对象（实例化）
my_car = Car("Toyota", "Camry", 2020)
print(my_car.get_info())  # 2020 Toyota Camry, 里程: 0 公里

# 使用对象
print(my_car.drive(100))  # 驾驶 Toyota Camry 行驶了 100 公里
print(my_car.get_info())  # 2020 Toyota Camry, 里程: 100 公里
```

### 13.1.2 OOP 的优势

```python
# 对比面向过程和面向对象

# 面向过程的方式
def create_student(name, age, grade):
    return {"name": name, "age": age, "grade": grade}

def print_student(student):
    print(f"学生: {student['name']}, 年龄: {student['age']}, 年级: {student['grade']}")

def update_grade(student, new_grade):
    student['grade'] = new_grade

# 使用
student1 = create_student("Alice", 16, "高一")
print_student(student1)
update_grade(student1, "高二")
print_student(student1)

# 面向对象的方式
class Student:
    """学生类"""

    def __init__(self, name, age, grade):
        self.name = name
        self.age = age
        self.grade = grade

    def print_info(self):
        print(f"学生: {self.name}, 年龄: {self.age}, 年级: {self.grade}")

    def update_grade(self, new_grade):
        self.grade = new_grade

# 使用
student2 = Student("Bob", 17, "高二")
student2.print_info()
student2.update_grade("高三")
student2.print_info()

# OOP 的优势：
# 1. 更好的组织结构
# 2. 数据和行为封装在一起
# 3. 易于维护和扩展
# 4. 支持继承和多态
```

### 13.1.3 Python 中的一切都是对象

```python
# 在 Python 中，一切都是对象
print(type(42))        # <class 'int'>
print(type("hello"))   # <class 'str'>
print(type([1, 2, 3])) # <class 'list'>
print(type(print))     # <class 'builtin_function_or_method'>

# 甚至函数也是对象
def greet():
    return "Hello!"

print(type(greet))     # <class 'function'>

# 函数可以赋值给变量
say_hello = greet
print(say_hello())     # Hello!

# 函数可以作为参数传递
def call_function(func):
    return func()

result = call_function(greet)
print(result)          # Hello!

# 函数可以作为返回值
def get_greeter():
    return greet

greeter_func = get_greeter()
print(greeter_func())  # Hello!
```

## 13.2 类和对象的概念

### 13.2.1 类的定义

```python
# 类定义的基本语法
class ClassName:
    """类的文档字符串"""
    # 类属性
    class_attribute = "类属性"

    # 构造函数
    def __init__(self, parameter1, parameter2):
        # 实例属性
        self.parameter1 = parameter1
        self.parameter2 = parameter2

    # 实例方法
    def instance_method(self):
        return f"实例方法: {self.parameter1}"

    # 类方法
    @classmethod
    def class_method(cls):
        return f"类方法: {cls.class_attribute}"

    # 静态方法
    @staticmethod
    def static_method():
        return "静态方法"

# 类的命名约定：使用 PascalCase（首字母大写）
class BankAccount:
    """银行账户类"""

class DataProcessor:
    """数据处理器类"""

class HttpClient:
    """HTTP 客户端类"""
```

### 13.2.2 对象（实例）

```python
class Dog:
    """狗类"""

    # 类属性
    species = "Canis familiaris"

    def __init__(self, name, age, breed):
        # 实例属性
        self.name = name
        self.age = age
        self.breed = breed

    def bark(self):
        return f"{self.name} 汪汪叫!"

    def get_info(self):
        return f"{self.name} 是一只 {self.age} 岁的 {self.breed}"

# 创建对象（实例化）
dog1 = Dog("旺财", 3, "金毛")
dog2 = Dog("小黑", 2, "拉布拉多")

print(dog1.name)       # 旺财
print(dog2.name)       # 小黑

print(dog1.species)    # Canis familiaris (类属性)
print(dog2.species)    # Canis familiaris (类属性)

print(dog1.bark())     # 旺财 汪汪叫!
print(dog2.bark())     # 小黑 汪汪叫!

print(dog1.get_info()) # 旺财 是一只 3 岁的 金毛
print(dog2.get_info()) # 小黑 是一只 2 岁的 拉布拉多

# 检查对象类型
print(type(dog1))      # <class '__main__.Dog'>
print(isinstance(dog1, Dog))  # True
```

### 13.2.3 类和对象的区别

```python
class Circle:
    """圆类"""

    # 类属性：所有实例共享
    pi = 3.14159

    def __init__(self, radius):
        # 实例属性：每个实例独有
        self.radius = radius

    def area(self):
        """计算面积"""
        return self.pi * self.radius ** 2

    def circumference(self):
        """计算周长"""
        return 2 * self.pi * self.radius

# 类属性可以通过类名访问
print(Circle.pi)       # 3.14159

# 创建实例
circle1 = Circle(5)
circle2 = Circle(10)

print(circle1.radius)  # 5 (实例属性)
print(circle2.radius)  # 10 (实例属性)

print(circle1.area())  # 78.53975
print(circle2.area())  # 314.159

# 修改类属性
Circle.pi = 3.14
print(circle1.area())  # 78.5 (使用新的 pi 值)
print(circle2.area())  # 314.0 (使用新的 pi 值)

# 修改实例属性
circle1.radius = 7
print(circle1.area())  # 153.86
print(circle2.area())  # 314.0 (不受影响)
```

## 13.3 类的定义和实例化

### 13.3.1 完整的类定义

```python
class Person:
    """
    人类 - 完整的类定义示例

    这个类演示了类的各个组成部分：
    - 类属性
    - 实例属性
    - 各种方法类型
    - 特殊方法
    """

    # 类属性
    species = "Homo sapiens"
    count = 0  # 实例计数器

    def __init__(self, name, age, gender="未知"):
        """
        构造函数

        Args:
            name (str): 姓名
            age (int): 年龄
            gender (str): 性别
        """
        # 实例属性
        self.name = name
        self.age = age
        self.gender = gender

        # 增加实例计数
        Person.count += 1

    def __str__(self):
        """字符串表示"""
        return f"{self.name} ({self.age}岁, {self.gender})"

    def __repr__(self):
        """正式字符串表示"""
        return f"Person(name='{self.name}', age={self.age}, gender='{self.gender}')"

    def greet(self):
        """问候方法"""
        return f"你好，我是 {self.name}"

    def have_birthday(self):
        """过生日"""
        self.age += 1
        return f"{self.name} 现在 {self.age} 岁了！"

    @classmethod
    def get_species(cls):
        """类方法：获取物种"""
        return cls.species

    @classmethod
    def get_count(cls):
        """类方法：获取实例数量"""
        return cls.count

    @staticmethod
    def is_adult(age):
        """静态方法：检查是否成年"""
        return age >= 18

# 使用示例
person1 = Person("Alice", 25, "女")
person2 = Person("Bob", 17, "男")

print(person1)          # Alice (25岁, 女)
print(repr(person2))    # Person(name='Bob', age=17, gender='男')

print(person1.greet())  # 你好，我是 Alice
print(person2.have_birthday())  # Bob 现在 18 岁了！

print(Person.get_species())    # Homo sapiens
print(Person.get_count())      # 2

print(Person.is_adult(20))     # True
print(Person.is_adult(16))     # False
```

### 13.3.2 实例化过程

```python
# 实例化过程详解
class Example:
    def __init__(self, value):
        print(f"__init__ 被调用，value = {value}")
        self.value = value

    def __new__(cls, *args, **kwargs):
        print(f"__new__ 被调用，cls = {cls}")
        instance = super().__new__(cls)
        print(f"创建的实例: {instance}")
        return instance

print("开始创建实例...")
obj = Example(42)
print(f"实例创建完成: {obj}")
print(f"实例属性: {obj.value}")

# 实例化过程：
# 1. 调用 __new__ 创建实例
# 2. 调用 __init__ 初始化实例
# 3. 返回实例

# 自定义 __new__ 的应用场景
class Singleton:
    """单例模式"""
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance

singleton1 = Singleton()
singleton2 = Singleton()
print(singleton1 is singleton2)  # True (同一个实例)
```

## 13.4 属性和方法

### 13.4.1 实例属性和类属性

```python
class Employee:
    """员工类"""

    # 类属性
    company = "Tech Corp"
    employee_count = 0

    def __init__(self, name, salary):
        # 实例属性
        self.name = name
        self.salary = salary

        # 修改类属性
        Employee.employee_count += 1

    def get_info(self):
        return f"{self.name} 在 {self.company} 工作，薪资: {self.salary}"

# 创建实例
emp1 = Employee("Alice", 50000)
emp2 = Employee("Bob", 60000)

print(emp1.company)     # Tech Corp (类属性)
print(emp2.company)     # Tech Corp (类属性)

print(emp1.get_info())  # Alice 在 Tech Corp 工作，薪资: 50000
print(emp2.get_info())  # Bob 在 Tech Corp 工作，薪资: 60000

print(Employee.employee_count)  # 2

# 修改类属性
Employee.company = "New Tech Corp"
print(emp1.get_info())  # Alice 在 New Tech Corp 工作，薪资: 50000
print(emp2.get_info())  # Bob 在 New Tech Corp 工作，薪资: 60000

# 实例属性覆盖类属性
emp1.company = "Different Corp"
print(emp1.get_info())  # Alice 在 Different Corp 工作，薪资: 50000
print(emp2.get_info())  # Bob 在 New Tech Corp 工作，薪资: 60000
```

### 13.4.2 属性访问控制

```python
class BankAccount:
    """银行账户"""

    def __init__(self, account_number, balance):
        self.account_number = account_number
        self._balance = balance  # 受保护的属性（约定）
        self.__pin = "1234"     # 私有属性

    def get_balance(self):
        """获取余额"""
        return self._balance

    def set_balance(self, amount):
        """设置余额"""
        if amount >= 0:
            self._balance = amount
        else:
            raise ValueError("余额不能为负数")

    def _validate_pin(self, pin):
        """验证 PIN 码（受保护的方法）"""
        return self.__pin == pin

    def __encrypt_pin(self, pin):
        """加密 PIN 码（私有方法）"""
        return hash(pin)

    def change_pin(self, old_pin, new_pin):
        """修改 PIN 码"""
        if self._validate_pin(old_pin):
            self.__pin = new_pin
            print("PIN 码修改成功")
        else:
            print("PIN 码错误")

# 使用示例
account = BankAccount("123456789", 1000)

print(account.account_number)  # 123456789 (公开属性)
print(account.get_balance())   # 1000 (通过方法访问受保护属性)

# 访问受保护属性（不推荐，但可以访问）
print(account._balance)        # 1000

# 访问私有属性（会报错）
try:
    print(account.__pin)
except AttributeError as e:
    print(f"错误: {e}")

# 通过名称修饰访问私有属性（不推荐）
print(account._BankAccount__pin)  # 1234
```

### 13.4.3 动态属性

```python
class DynamicObject:
    """动态属性示例"""

    def __init__(self):
        self.base_attr = "基础属性"

# 创建实例
obj = DynamicObject()

# 动态添加属性
obj.dynamic_attr = "动态添加的属性"
obj.number_attr = 42

print(obj.base_attr)      # 基础属性
print(obj.dynamic_attr)   # 动态添加的属性
print(obj.number_attr)    # 42

# 动态添加方法
def dynamic_method(self):
    return "这是动态添加的方法"

obj.dynamic_method = dynamic_method.__get__(obj, DynamicObject)
print(obj.dynamic_method())  # 这是动态添加的方法

# 使用 setattr 和 getattr
setattr(obj, "setattr_attr", "通过 setattr 设置")
print(getattr(obj, "setattr_attr"))  # 通过 setattr 设置

# 检查属性是否存在
print(hasattr(obj, "base_attr"))      # True
print(hasattr(obj, "nonexistent"))    # False

# 删除属性
del obj.dynamic_attr
try:
    print(obj.dynamic_attr)
except AttributeError:
    print("属性已被删除")
```

## 13.5 构造函数和析构函数

### 13.5.1 __init__ 方法

```python
class Book:
    """图书类"""

    def __init__(self, title, author, isbn=None, year=None):
        """
        构造函数

        Args:
            title (str): 书名
            author (str): 作者
            isbn (str, optional): ISBN 号
            year (int, optional): 出版年份
        """
        self.title = title
        self.author = author
        self.isbn = isbn
        self.year = year
        self.is_available = True

        print(f"创建图书: {title}")

    def __str__(self):
        info = f"《{self.title}》 - {self.author}"
        if self.year:
            info += f" ({self.year})"
        return info

# 不同方式的实例化
book1 = Book("Python 编程", "张三")
book2 = Book("数据结构", "李四", "978-7-123-45678-9", 2020)

print(book1)  # 《Python 编程》 - 张三
print(book2)  # 《数据结构》 - 李四 (2020)
```

### 13.5.2 __del__ 方法

```python
class ResourceManager:
    """资源管理器"""

    def __init__(self, name):
        self.name = name
        print(f"资源 {name} 已创建")

    def __del__(self):
        """析构函数"""
        print(f"资源 {self.name} 已被清理")

    def use_resource(self):
        print(f"正在使用资源 {self.name}")

# 使用示例
def test_resource():
    resource = ResourceManager("数据库连接")
    resource.use_resource()
    # 函数结束时，resource 会被自动清理

test_resource()
print("函数执行完毕")

# 手动删除对象
resource2 = ResourceManager("文件句柄")
del resource2
print("手动删除了 resource2")

# 注意：不要依赖 __del__ 进行重要的清理工作
# 更好的做法是使用上下文管理器或显式的 close 方法
```

### 13.5.3 初始化模式的比较

```python
# 方式1: 传统的 __init__
class TraditionalInit:
    def __init__(self, x, y, z=None):
        self.x = x
        self.y = y
        self.z = z if z is not None else 0

# 方式2: 使用默认参数
class DefaultArgs:
    def __init__(self, x, y, z=0):
        self.x = x
        self.y = y
        self.z = z

# 方式3: 使用 *args 和 **kwargs
class FlexibleInit:
    def __init__(self, *args, **kwargs):
        if len(args) >= 2:
            self.x = args[0]
            self.y = args[1]
        else:
            self.x = kwargs.get('x', 0)
            self.y = kwargs.get('y', 0)

        self.z = kwargs.get('z', 0)

# 方式4: 类方法构造器
class ClassMethodConstructor:
    def __init__(self, x, y, z=0):
        self.x = x
        self.y = y
        self.z = z

    @classmethod
    def from_tuple(cls, data_tuple):
        """从元组创建实例"""
        return cls(*data_tuple)

    @classmethod
    def from_dict(cls, data_dict):
        """从字典创建实例"""
        return cls(**data_dict)

# 使用示例
obj1 = TraditionalInit(1, 2, 3)
obj2 = DefaultArgs(1, 2)
obj3 = FlexibleInit(1, 2, z=3)
obj4 = ClassMethodConstructor.from_tuple((1, 2, 3))
obj5 = ClassMethodConstructor.from_dict({'x': 1, 'y': 2, 'z': 3})
```

## 13.6 实例方法、类方法和静态方法

### 13.6.1 实例方法

```python
class Calculator:
    """计算器类"""

    def __init__(self):
        self.history = []  # 计算历史

    def add(self, a, b):
        """实例方法：加法"""
        result = a + b
        self.history.append(f"{a} + {b} = {result}")
        return result

    def subtract(self, a, b):
        """实例方法：减法"""
        result = a - b
        self.history.append(f"{a} - {b} = {result}")
        return result

    def get_history(self):
        """获取计算历史"""
        return self.history

# 使用实例方法
calc = Calculator()
print(calc.add(5, 3))       # 8
print(calc.subtract(10, 4)) # 6
print(calc.get_history())   # ['5 + 3 = 8', '10 - 4 = 6']
```

### 13.6.2 类方法

```python
class DateUtils:
    """日期工具类"""

    # 类属性
    DAYS_IN_MONTH = {
        1: 31, 2: 28, 3: 31, 4: 30, 5: 31, 6: 30,
        7: 31, 8: 31, 9: 30, 10: 31, 11: 30, 12: 31
    }

    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day

    @classmethod
    def from_string(cls, date_string):
        """从字符串创建日期对象"""
        year, month, day = map(int, date_string.split('-'))
        return cls(year, month, day)

    @classmethod
    def today(cls):
        """获取今天的日期"""
        from datetime import datetime
        now = datetime.now()
        return cls(now.year, now.month, now.day)

    @classmethod
    def is_leap_year(cls, year):
        """检查是否为闰年"""
        return year % 4 == 0 and (year % 100 != 0 or year % 400 == 0)

    @classmethod
    def days_in_month(cls, year, month):
        """获取某年某月的天数"""
        days = cls.DAYS_IN_MONTH[month]
        if month == 2 and cls.is_leap_year(year):
            days = 29
        return days

    def __str__(self):
        return f"{self.year}-{self.month:02d}-{self.day:02d}"

# 使用类方法
date1 = DateUtils.from_string("2023-12-25")
date2 = DateUtils.today()

print(date1)  # 2023-12-25
print(date2)  # 2023-XX-XX (当前日期)

print(DateUtils.is_leap_year(2024))     # True
print(DateUtils.days_in_month(2024, 2)) # 29 (闰年2月)
print(DateUtils.days_in_month(2023, 2)) # 28 (平年2月)
```

### 13.6.3 静态方法

```python
class MathUtils:
    """数学工具类"""

    @staticmethod
    def gcd(a, b):
        """计算最大公约数"""
        while b:
            a, b = b, a % b
        return a

    @staticmethod
    def lcm(a, b):
        """计算最小公倍数"""
        return abs(a * b) // MathUtils.gcd(a, b)

    @staticmethod
    def is_prime(n):
        """检查是否为质数"""
        if n < 2:
            return False
        for i in range(2, int(n ** 0.5) + 1):
            if n % i == 0:
                return False
        return True

    @staticmethod
    def factorial(n):
        """计算阶乘"""
        if n == 0 or n == 1:
            return 1
        return n * MathUtils.factorial(n - 1)

# 使用静态方法
print(MathUtils.gcd(12, 18))    # 6
print(MathUtils.lcm(12, 18))    # 36
print(MathUtils.is_prime(17))   # True
print(MathUtils.is_prime(15))   # False
print(MathUtils.factorial(5))   # 120

# 静态方法可以通过实例调用，但不推荐
utils = MathUtils()
print(utils.gcd(24, 36))        # 12 (可以调用，但不推荐)
```

### 13.6.4 方法类型比较

```python
class MethodComparison:
    """方法类型比较"""

    class_var = "类变量"

    def __init__(self, instance_var):
        self.instance_var = instance_var

    def instance_method(self):
        """实例方法：可以访问实例变量和类变量"""
        return f"实例变量: {self.instance_var}, 类变量: {self.class_var}"

    @classmethod
    def class_method(cls):
        """类方法：可以访问类变量，不能访问实例变量"""
        return f"类变量: {cls.class_var}"

    @staticmethod
    def static_method():
        """静态方法：不能访问实例变量和类变量"""
        return "静态方法"

# 使用示例
obj = MethodComparison("实例值")

print(obj.instance_method())     # 实例变量: 实例值, 类变量: 类变量
print(MethodComparison.class_method())  # 类变量: 类变量
print(MethodComparison.static_method()) # 静态方法

# 错误示例
try:
    print(MethodComparison.instance_method())  # 会报错
except TypeError as e:
    print(f"错误: {e}")

try:
    print(obj.class_method())  # 可以调用，但不推荐
except:
    print("类方法通过实例调用")
```

## 13.7 封装和私有属性

### 13.7.1 私有属性和方法

```python
class BankAccount:
    """银行账户 - 封装示例"""

    def __init__(self, account_number, initial_balance):
        self.account_number = account_number  # 公开属性
        self._balance = initial_balance       # 受保护属性
        self.__pin = "1234"                   # 私有属性
        self.__transaction_history = []       # 私有属性

    def get_balance(self):
        """获取余额（公开方法）"""
        return self._balance

    def deposit(self, amount):
        """存款"""
        if amount > 0:
            self._balance += amount
            self.__add_transaction(f"存款: +{amount}")
        else:
            raise ValueError("存款金额必须大于0")

    def withdraw(self, amount):
        """取款"""
        if 0 < amount <= self._balance:
            self._balance -= amount
            self.__add_transaction(f"取款: -{amount}")
        else:
            raise ValueError("取款金额无效")

    def _validate_amount(self, amount):
        """验证金额（受保护方法）"""
        return isinstance(amount, (int, float)) and amount > 0

    def __add_transaction(self, description):
        """添加交易记录（私有方法）"""
        from datetime import datetime
        timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        self.__transaction_history.append(f"{timestamp}: {description}")

    def get_transaction_history(self):
        """获取交易历史"""
        return self.__transaction_history.copy()

# 使用示例
account = BankAccount("123456789", 1000)

print(f"账户: {account.account_number}")
print(f"余额: {account.get_balance()}")

account.deposit(500)
account.withdraw(200)

print(f"最终余额: {account.get_balance()}")
print("交易历史:")
for transaction in account.get_transaction_history():
    print(f"  {transaction}")
```

### 13.7.2 属性装饰器

```python
class Temperature:
    """温度类 - 使用属性装饰器"""

    def __init__(self, celsius=0):
        self._celsius = celsius

    @property
    def celsius(self):
        """获取摄氏温度"""
        return self._celsius

    @celsius.setter
    def celsius(self, value):
        """设置摄氏温度"""
        if value < -273.15:
            raise ValueError("温度不能低于绝对零度")
        self._celsius = value

    @property
    def fahrenheit(self):
        """获取华氏温度"""
        return self._celsius * 9/5 + 32

    @fahrenheit.setter
    def fahrenheit(self, value):
        """设置华氏温度"""
        self._celsius = (value - 32) * 5/9

    @property
    def kelvin(self):
        """获取开尔文温度"""
        return self._celsius + 273.15

    @kelvin.setter
    def kelvin(self, value):
        """设置开尔文温度"""
        if value < 0:
            raise ValueError("开尔文温度不能为负数")
        self._celsius = value - 273.15

# 使用示例
temp = Temperature(20)

print(f"摄氏度: {temp.celsius}°C")
print(f"华氏度: {temp.fahrenheit}°F")
print(f"开尔文: {temp.kelvin}K")

# 修改温度
temp.celsius = 25
print(f"修改后摄氏度: {temp.celsius}°C")
print(f"对应的华氏度: {temp.fahrenheit}°F")

temp.fahrenheit = 68
print(f"设置华氏度68°F: {temp.celsius}°C")
```

### 13.7.3 封装的最佳实践

```python
class User:
    """用户类 - 封装最佳实践"""

    def __init__(self, username, email):
        self.__username = username
        self.__email = email
        self.__password_hash = None
        self.__is_active = True
        self.__login_attempts = 0

    @property
    def username(self):
        """获取用户名"""
        return self.__username

    @property
    def email(self):
        """获取邮箱"""
        return self.__email

    @email.setter
    def email(self, new_email):
        """设置邮箱（包含验证）"""
        if '@' not in new_email:
            raise ValueError("无效的邮箱地址")
        self.__email = new_email

    def set_password(self, password):
        """设置密码（加密存储）"""
        import hashlib
        self.__password_hash = hashlib.sha256(password.encode()).hexdigest()

    def check_password(self, password):
        """验证密码"""
        if self.__password_hash is None:
            return False
        import hashlib
        return self.__password_hash == hashlib.sha256(password.encode()).hexdigest()

    def login(self, password):
        """登录"""
        if self.check_password(password) and self.__is_active:
            self.__login_attempts = 0
            return True
        else:
            self.__login_attempts += 1
            if self.__login_attempts >= 3:
                self.__is_active = False
            return False

    def is_active(self):
        """检查用户是否激活"""
        return self.__is_active

# 使用示例
user = User("alice", "alice@example.com")
user.set_password("secret123")

print(f"用户名: {user.username}")
print(f"邮箱: {user.email}")

# 修改邮箱
user.email = "alice@newdomain.com"
print(f"新邮箱: {user.email}")

# 登录测试
print(f"登录成功: {user.login('secret123')}")
print(f"用户激活状态: {user.is_active()}")

# 多次失败登录
for i in range(3):
    print(f"错误登录 {i+1}: {user.login('wrong')}")

print(f"用户激活状态: {user.is_active()}")
```

## 总结

本章介绍了面向对象编程的基础概念：
- 理解类和对象的区别
- 掌握类的定义和实例化
- 学习属性和方法的各种类型
- 了解构造函数和析构函数
- 掌握封装和私有属性的使用

通过本章的学习，你应该能够：
1. 创建和使用类和对象
2. 定义实例方法、类方法和静态方法
3. 使用属性装饰器实现封装
4. 理解私有属性和方法的命名约定
5. 设计合理的类结构

下一章将介绍类和对象的深入概念，包括继承和多态。
