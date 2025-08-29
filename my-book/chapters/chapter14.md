# 第14章：类和对象

## 14.1 继承的基本概念

继承是面向对象编程的核心特性之一，允许子类继承父类的属性和方法。

### 14.1.1 继承的基本语法

```python
# 父类（基类）
class Animal:
    """动物类"""

    def __init__(self, name, species):
        self.name = name
        self.species = species

    def eat(self):
        return f"{self.name} 正在吃东西"

    def sleep(self):
        return f"{self.name} 正在睡觉"

    def make_sound(self):
        return "动物发出声音"

# 子类（派生类）
class Dog(Animal):
    """狗类 - 继承自动物类"""

    def __init__(self, name, breed):
        # 调用父类构造函数
        super().__init__(name, "狗")
        # 子类特有的属性
        self.breed = breed

    def make_sound(self):
        # 重写父类方法
        return "汪汪!"

    def fetch(self):
        # 子类特有的方法
        return f"{self.name} 正在捡球"

# 子类（派生类）
class Cat(Animal):
    """猫类 - 继承自动物类"""

    def __init__(self, name, color):
        super().__init__(name, "猫")
        self.color = color

    def make_sound(self):
        return "喵喵!"

    def purr(self):
        return f"{self.name} 正在 purr"

# 使用示例
dog = Dog("旺财", "金毛")
cat = Cat("小白", "白色")

print(dog.eat())        # 旺财 正在吃东西 (继承的方法)
print(dog.make_sound()) # 汪汪! (重写的方法)
print(dog.fetch())      # 旺财 正在捡球 (子类特有的方法)

print(cat.sleep())      # 小白 正在睡觉 (继承的方法)
print(cat.make_sound()) # 喵喵! (重写的方法)
print(cat.purr())       # 小白 正在 purr (子类特有的方法)
```

### 14.1.2 继承的层次结构

```python
# 多层继承
class Vehicle:
    """交通工具基类"""

    def __init__(self, brand, model):
        self.brand = brand
        self.model = model

    def start(self):
        return f"{self.brand} {self.model} 启动"

    def stop(self):
        return f"{self.brand} {self.model} 停止"

class Car(Vehicle):
    """汽车类"""

    def __init__(self, brand, model, seats):
        super().__init__(brand, model)
        self.seats = seats
        self.fuel_level = 100

    def drive(self):
        return f"{self.brand} {self.model} 正在驾驶"

    def refuel(self):
        self.fuel_level = 100
        return "加油完成"

class ElectricCar(Car):
    """电动汽车类"""

    def __init__(self, brand, model, seats, battery_capacity):
        super().__init__(brand, model, seats)
        self.battery_capacity = battery_capacity
        self.charge_level = 100

    def drive(self):
        return f"{self.brand} {self.model} 正在电动驾驶"

    def charge(self):
        self.charge_level = 100
        return "充电完成"

# 使用示例
tesla = ElectricCar("Tesla", "Model 3", 5, 75)

print(tesla.start())    # Tesla Model 3 启动 (继承自 Vehicle)
print(tesla.drive())    # Tesla Model 3 正在电动驾驶 (重写 Car 的方法)
print(tesla.charge())   # 充电完成 (ElectricCar 特有的方法)

# 检查继承关系
print(isinstance(tesla, ElectricCar))  # True
print(isinstance(tesla, Car))          # True
print(isinstance(tesla, Vehicle))      # True

print(issubclass(ElectricCar, Car))    # True
print(issubclass(ElectricCar, Vehicle)) # True
print(issubclass(Car, ElectricCar))    # False
```

### 14.1.3 继承的类型

```python
# 1. 单继承
class A:
    def method_a(self):
        return "方法 A"

class B(A):
    def method_b(self):
        return "方法 B"

# 2. 多重继承
class C:
    def method_c(self):
        return "方法 C"

class D(A, C):
    def method_d(self):
        return "方法 D"

# 3. 抽象继承（概念）
class AbstractBase:
    def abstract_method(self):
        raise NotImplementedError("子类必须实现此方法")

class ConcreteClass(AbstractBase):
    def abstract_method(self):
        return "具体实现"

# 使用示例
b = B()
print(b.method_a())  # 方法 A (继承自 A)
print(b.method_b())  # 方法 B (自身方法)

d = D()
print(d.method_a())  # 方法 A (继承自 A)
print(d.method_c())  # 方法 C (继承自 C)
print(d.method_d())  # 方法 D (自身方法)

concrete = ConcreteClass()
print(concrete.abstract_method())  # 具体实现
```

## 14.2 方法重写

方法重写允许子类重新定义父类的方法以提供特定的实现。

### 14.2.1 基本方法重写

```python
class Shape:
    """形状基类"""

    def __init__(self, name):
        self.name = name

    def area(self):
        """计算面积"""
        raise NotImplementedError("子类必须实现 area 方法")

    def perimeter(self):
        """计算周长"""
        raise NotImplementedError("子类必须实现 perimeter 方法")

    def describe(self):
        """描述形状"""
        return f"这是一个 {self.name}"

class Rectangle(Shape):
    """矩形类"""

    def __init__(self, width, height):
        super().__init__("矩形")
        self.width = width
        self.height = height

    def area(self):
        """重写面积计算"""
        return self.width * self.height

    def perimeter(self):
        """重写周长计算"""
        return 2 * (self.width + self.height)

class Circle(Shape):
    """圆形类"""

    def __init__(self, radius):
        super().__init__("圆形")
        self.radius = radius

    def area(self):
        """重写面积计算"""
        import math
        return math.pi * self.radius ** 2

    def perimeter(self):
        """重写周长计算（圆的周长叫 circumference）"""
        import math
        return 2 * math.pi * self.radius

# 使用示例
shapes = [
    Rectangle(4, 5),
    Circle(3)
]

for shape in shapes:
    print(shape.describe())
    print(f"面积: {shape.area():.2f}")
    print(f"周长: {shape.perimeter():.2f}")
    print()
```

### 14.2.2 调用父类方法

```python
class Person:
    """人类"""

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def introduce(self):
        return f"我是 {self.name}，{self.age} 岁"

    def work(self):
        return f"{self.name} 正在工作"

class Student(Person):
    """学生类"""

    def __init__(self, name, age, student_id, major):
        super().__init__(name, age)
        self.student_id = student_id
        self.major = major

    def introduce(self):
        # 调用父类方法并扩展
        parent_intro = super().introduce()
        return f"{parent_intro}，学号 {self.student_id}，专业 {self.major}"

    def work(self):
        # 完全重写
        return f"{self.name} 正在学习"

    def study(self):
        # 调用父类方法的不同方式
        return f"{super().work()} 和学习"

class Employee(Person):
    """员工类"""

    def __init__(self, name, age, employee_id, department):
        super().__init__(name, age)
        self.employee_id = employee_id
        self.department = department

    def introduce(self):
        parent_intro = super().introduce()
        return f"{parent_intro}，工号 {self.employee_id}，部门 {self.department}"

    def work(self):
        parent_work = super().work()
        return f"{parent_work} 在 {self.department} 部门"

# 使用示例
student = Student("Alice", 20, "2023001", "计算机科学")
employee = Employee("Bob", 30, "E001", "技术部")

print(student.introduce())
print(student.work())
print(student.study())
print()

print(employee.introduce())
print(employee.work())
```

### 14.2.3 重写特殊方法

```python
class Vector:
    """向量类"""

    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        """字符串表示"""
        return f"Vector({self.x}, {self.y})"

    def __repr__(self):
        """正式字符串表示"""
        return f"Vector({self.x}, {self.y})"

    def __add__(self, other):
        """向量加法"""
        if isinstance(other, Vector):
            return Vector(self.x + other.x, self.y + other.y)
        return NotImplemented

    def __mul__(self, scalar):
        """向量乘法（标量）"""
        if isinstance(scalar, (int, float)):
            return Vector(self.x * scalar, self.y * scalar)
        return NotImplemented

    def __eq__(self, other):
        """相等比较"""
        if isinstance(other, Vector):
            return self.x == other.x and self.y == other.y
        return False

    def __len__(self):
        """向量的"长度"（维度）"""
        return 2

    def __getitem__(self, index):
        """索引访问"""
        if index == 0:
            return self.x
        elif index == 1:
            return self.y
        else:
            raise IndexError("向量只有两个分量")

# 使用示例
v1 = Vector(1, 2)
v2 = Vector(3, 4)

print(v1)           # Vector(1, 2)
print(repr(v2))     # Vector(3, 4)

v3 = v1 + v2        # Vector(4, 6)
print(v3)

v4 = v1 * 3         # Vector(3, 6)
print(v4)

print(v1 == Vector(1, 2))  # True
print(len(v1))      # 2

print(v1[0], v1[1]) # 1 2
```

## 14.3 super() 函数

super() 函数用于调用父类的方法，是实现继承和方法重写的重要工具。

### 14.3.1 super() 的基本用法

```python
class Parent:
    def __init__(self, name):
        self.name = name
        print(f"Parent.__init__ called with {name}")

    def method(self):
        print("Parent.method called")

class Child(Parent):
    def __init__(self, name, age):
        # 调用父类构造函数
        super().__init__(name)
        self.age = age
        print(f"Child.__init__ called with {age}")

    def method(self):
        # 调用父类方法
        super().method()
        print("Child.method called")

# 使用示例
child = Child("Alice", 25)
child.method()
```

### 14.3.2 super() 在多重继承中的使用

```python
class A:
    def __init__(self):
        print("A.__init__")
        self.a = "A"

    def method(self):
        print("A.method")

class B:
    def __init__(self):
        print("B.__init__")
        self.b = "B"

    def method(self):
        print("B.method")

class C(A, B):
    def __init__(self):
        print("C.__init__")
        # 调用所有父类的 __init__
        super().__init__()
        self.c = "C"

    def method(self):
        print("C.method")
        # 调用下一个父类的方法
        super().method()

# 使用示例
c = C()
print(f"c.a = {c.a}, c.b = {c.b}, c.c = {c.c}")
c.method()
```

### 14.3.3 super() 的工作原理

```python
# MRO (Method Resolution Order) 示例
class A:
    def method(self):
        print("A.method")

class B(A):
    def method(self):
        print("B.method")
        super().method()

class C(A):
    def method(self):
        print("C.method")
        super().method()

class D(B, C):
    def method(self):
        print("D.method")
        super().method()

# 查看 MRO
print(D.__mro__)
# (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)

d = D()
d.method()
# 输出：
# D.method
# B.method
# C.method
# A.method
```

## 14.4 多重继承

多重继承允许一个类继承多个父类的特性。

### 14.4.1 多重继承的基本语法

```python
class Flyable:
    """可飞行的"""

    def fly(self):
        return "正在飞行"

    def altitude(self):
        return "1000米高度"

class Swimmable:
    """可游泳的"""

    def swim(self):
        return "正在游泳"

    def depth(self):
        return "10米深度"

class Walkable:
    """可行走的"""

    def walk(self):
        return "正在行走"

    def speed(self):
        return "5 km/h"

class Duck(Flyable, Swimmable, Walkable):
    """鸭子类 - 多重继承"""

    def __init__(self, name):
        self.name = name

    def quack(self):
        return "嘎嘎!"

# 使用示例
duck = Duck("唐老鸭")

print(duck.fly())      # 正在飞行
print(duck.swim())     # 正在游泳
print(duck.walk())     # 正在行走
print(duck.quack())    # 嘎嘎!

print(duck.altitude()) # 1000米高度
print(duck.depth())    # 10米深度
print(duck.speed())    # 5 km/h
```

### 14.4.2 菱形继承问题

```python
# 菱形继承问题
class A:
    def method(self):
        print("A.method")

class B(A):
    def method(self):
        print("B.method")
        super().method()

class C(A):
    def method(self):
        print("C.method")
        super().method()

class D(B, C):
    def method(self):
        print("D.method")
        super().method()

# 查看 MRO
print("D 的 MRO:", D.__mro__)

d = D()
d.method()
# 输出：
# D.method
# B.method
# C.method
# A.method

# A 只被调用一次，避免了菱形继承问题
```

### 14.4.3 多重继承的最佳实践

```python
# 使用组合而不是多重继承
class Engine:
    """引擎"""

    def start(self):
        return "引擎启动"

    def stop(self):
        return "引擎停止"

class Wheels:
    """车轮"""

    def rotate(self):
        return "车轮转动"

class Car:
    """汽车 - 使用组合"""

    def __init__(self):
        self.engine = Engine()
        self.wheels = Wheels()

    def start(self):
        return self.engine.start()

    def drive(self):
        return f"{self.start()}，{self.wheels.rotate()}"

# 使用 Mixin 类
class LoggerMixin:
    """日志 Mixin"""

    def log(self, message):
        print(f"[{self.__class__.__name__}] {message}")

class SerializerMixin:
    """序列化 Mixin"""

    def to_dict(self):
        return self.__dict__

    def from_dict(self, data):
        self.__dict__.update(data)

class User(LoggerMixin, SerializerMixin):
    """用户类 - 使用 Mixin"""

    def __init__(self, name, email):
        self.name = name
        self.email = email

# 使用示例
user = User("Alice", "alice@example.com")

user.log("用户创建成功")  # [User] 用户创建成功

user_data = user.to_dict()
print(user_data)  # {'name': 'Alice', 'email': 'alice@example.com'}

new_user = User("", "")
new_user.from_dict(user_data)
print(new_user.name)  # Alice
```

## 14.5 方法解析顺序（MRO）

MRO 决定了在多重继承中方法调用的顺序。

### 14.5.1 MRO 的概念

```python
class A:
    def method(self):
        return "A"

class B(A):
    def method(self):
        return "B"

class C(A):
    def method(self):
        return "C"

class D(B, C):
    pass

# 查看 MRO
print("D 的 MRO:", D.__mro__)
# D 的 MRO: (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)

d = D()
print(d.method())  # B (遵循 MRO)

# 手动遍历 MRO
for cls in D.__mro__:
    print(f"{cls.__name__}: {hasattr(cls, 'method')}")
```

### 14.5.2 C3 线性化算法

```python
# C3 算法示例
class O: pass
class A(O): pass
class B(O): pass
class C(O): pass
class D(A, B): pass
class E(B, C): pass
class F(D, E): pass

print("F 的 MRO:", F.__mro__)
# F 的 MRO: (<class '__main__.F'>, <class '__main__.D'>, <class '__main__.A'>,
#           <class '__main__.E'>, <class '__main__.B'>, <class '__main__.C'>,
#           <class '__main__.O'>, <class 'object'>)

# C3 算法确保：
# 1. 子类在父类之前
# 2. 保持继承列表的顺序
# 3. 每个类只出现一次
```

### 14.5.3 MRO 的实际应用

```python
class DatabaseConnection:
    """数据库连接"""

    def connect(self):
        return "连接到数据库"

class LoggingMixin:
    """日志 Mixin"""

    def connect(self):
        result = super().connect()
        print(f"日志: {result}")
        return result

class CachingMixin:
    """缓存 Mixin"""

    def connect(self):
        result = super().connect()
        print(f"缓存: {result}")
        return result

class AdvancedDatabase(DatabaseConnection, LoggingMixin, CachingMixin):
    """高级数据库类"""

    def connect(self):
        print("开始连接...")
        result = super().connect()
        print("连接完成")
        return result

# 查看 MRO
print("AdvancedDatabase 的 MRO:", AdvancedDatabase.__mro__)

# 使用示例
db = AdvancedDatabase()
db.connect()
# 输出：
# 开始连接...
# 缓存: 连接到数据库
# 日志: 连接到数据库
# 连接完成
```

## 14.6 isinstance() 和 issubclass()

这两个函数用于检查对象的类型和类的继承关系。

### 14.6.1 isinstance() 函数

```python
class Animal:
    pass

class Dog(Animal):
    pass

class Cat(Animal):
    pass

# 创建实例
dog = Dog()
cat = Cat()
animal = Animal()

# isinstance 检查
print(isinstance(dog, Dog))      # True
print(isinstance(dog, Animal))   # True (子类实例是父类类型)
print(isinstance(cat, Animal))   # True
print(isinstance(animal, Dog))   # False

# 检查基本类型
print(isinstance(42, int))       # True
print(isinstance("hello", str))  # True
print(isinstance([1, 2, 3], list)) # True

# 检查多个类型
print(isinstance(42, (int, float)))  # True
print(isinstance(3.14, (int, float))) # True
print(isinstance("hello", (int, float))) # False
```

### 14.6.2 issubclass() 函数

```python
class Animal:
    pass

class Mammal(Animal):
    pass

class Dog(Mammal):
    pass

class Cat(Mammal):
    pass

# issubclass 检查
print(issubclass(Dog, Animal))   # True
print(issubclass(Dog, Mammal))   # True
print(issubclass(Cat, Animal))   # True
print(issubclass(Animal, Dog))   # False

# 检查自身
print(issubclass(Dog, Dog))      # True

# 检查多个父类
print(issubclass(Dog, (Animal, Mammal)))  # True
print(issubclass(Dog, (str, list)))       # False
```

### 14.6.3 实际应用

```python
def process_animal(animal):
    """处理动物对象"""

    if not isinstance(animal, Animal):
        raise TypeError("参数必须是 Animal 类型")

    if isinstance(animal, Dog):
        return f"狗狗 {animal.name} 汪汪叫"
    elif isinstance(animal, Cat):
        return f"猫咪 {animal.name} 喵喵叫"
    else:
        return f"动物 {animal.name} 发出声音"

def create_animal(animal_type, name):
    """工厂函数创建动物"""

    if not issubclass(animal_type, Animal):
        raise TypeError("animal_type 必须是 Animal 的子类")

    return animal_type(name)

# 使用示例
class Animal:
    def __init__(self, name):
        self.name = name

class Dog(Animal):
    pass

class Cat(Animal):
    pass

dog = Dog("旺财")
cat = Cat("小白")

print(process_animal(dog))   # 狗狗 旺财 汪汪叫
print(process_animal(cat))   # 猫咪 小白 喵喵叫

# 使用工厂函数
new_dog = create_animal(Dog, "大黄")
print(f"创建了新狗: {new_dog.name}")
```

## 14.7 抽象基类

抽象基类定义了子类必须实现的方法接口。

### 14.7.1 使用 abc 模块

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    """形状抽象基类"""

    def __init__(self, name):
        self.name = name

    @abstractmethod
    def area(self):
        """计算面积（必须由子类实现）"""
        pass

    @abstractmethod
    def perimeter(self):
        """计算周长（必须由子类实现）"""
        pass

    def describe(self):
        """描述形状（有默认实现）"""
        return f"这是一个 {self.name}"

class Rectangle(Shape):
    """矩形类"""

    def __init__(self, width, height):
        super().__init__("矩形")
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

    def perimeter(self):
        return 2 * (self.width + self.height)

class Circle(Shape):
    """圆形类"""

    def __init__(self, radius):
        super().__init__("圆形")
        self.radius = radius

    def area(self):
        import math
        return math.pi * self.radius ** 2

    def perimeter(self):
        import math
        return 2 * math.pi * self.radius

# 使用示例
shapes = [Rectangle(4, 5), Circle(3)]

for shape in shapes:
    print(shape.describe())
    print(f"面积: {shape.area():.2f}")
    print(f"周长: {shape.perimeter():.2f}")
    print()

# 尝试创建抽象类实例会报错
try:
    shape = Shape("测试")
except TypeError as e:
    print(f"错误: {e}")
```

### 14.7.2 注册抽象基类

```python
from abc import ABC, abstractmethod

class Drawable(ABC):
    """可绘制对象抽象基类"""

    @abstractmethod
    def draw(self):
        """绘制方法"""
        pass

class Circle:
    """圆形类（不继承抽象基类）"""

    def __init__(self, radius):
        self.radius = radius

    def draw(self):
        return f"绘制圆形，半径为 {self.radius}"

# 注册 Circle 为 Drawable 的子类
Drawable.register(Circle)

# 现在 Circle 的实例被认为是 Drawable 类型
circle = Circle(5)
print(isinstance(circle, Drawable))  # True
print(issubclass(Circle, Drawable))  # True

# 使用抽象基类的好处
def render_objects(objects):
    """渲染对象列表"""
    for obj in objects:
        if isinstance(obj, Drawable):
            print(obj.draw())
        else:
            print(f"对象 {type(obj).__name__} 不可绘制")

# 使用示例
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def draw(self):
        return f"绘制矩形，尺寸 {self.width}x{self.height}"

# 注册多个类
Drawable.register(Rectangle)

objects = [Circle(3), Rectangle(4, 5), "string"]
render_objects(objects)
```

### 14.7.3 抽象属性

```python
from abc import ABC, abstractmethod

class Employee(ABC):
    """员工抽象基类"""

    def __init__(self, name):
        self.name = name

    @property
    @abstractmethod
    def salary(self):
        """薪资属性（必须由子类实现）"""
        pass

    @abstractmethod
    def work(self):
        """工作方法（必须由子类实现）"""
        pass

    def introduce(self):
        """介绍自己"""
        return f"我是 {self.name}，薪资 {self.salary}"

class Manager(Employee):
    """经理类"""

    def __init__(self, name, base_salary, bonus):
        super().__init__(name)
        self.base_salary = base_salary
        self.bonus = bonus

    @property
    def salary(self):
        return self.base_salary + self.bonus

    def work(self):
        return "管理团队和项目"

class Developer(Employee):
    """开发者类"""

    def __init__(self, name, hourly_rate, hours_worked):
        super().__init__(name)
        self.hourly_rate = hourly_rate
        self.hours_worked = hours_worked

    @property
    def salary(self):
        return self.hourly_rate * self.hours_worked

    def work(self):
        return "编写代码和解决问题"

# 使用示例
employees = [
    Manager("Alice", 80000, 20000),
    Developer("Bob", 50, 160)
]

for employee in employees:
    print(employee.introduce())
    print(f"工作内容: {employee.work()}")
    print()
```

## 总结

本章深入介绍了类和对象的继承特性：
- 掌握继承的基本概念和语法
- 学习方法重写和 super() 函数的使用
- 理解多重继承和方法解析顺序
- 掌握 isinstance() 和 issubclass() 的应用
- 学习抽象基类的定义和使用

通过本章的学习，你应该能够：
1. 设计合理的继承层次结构
2. 正确使用方法重写和 super()
3. 处理多重继承的复杂情况
4. 使用抽象基类定义接口
5. 编写健壮的面向对象代码

下一章将介绍特殊方法和魔法方法。
