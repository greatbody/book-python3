# 第15章：继承和多态

## 15.1 多态的基本概念

多态是面向对象编程的三大特性之一，指同一接口可以有多种不同的实现方式。

### 15.1.1 多态的定义

```python
# 多态：同一方法在不同对象上有不同的行为

class Animal:
    """动物基类"""

    def __init__(self, name):
        self.name = name

    def speak(self):
        """发出声音（基类方法）"""
        return "动物发出声音"

class Dog(Animal):
    """狗类"""

    def speak(self):
        return "汪汪!"

class Cat(Animal):
    """猫类"""

    def speak(self):
        return "喵喵!"

class Bird(Animal):
    """鸟类"""

    def speak(self):
        return "叽叽!"

# 多态函数
def animal_sounds(animals):
    """让所有动物发出声音"""
    for animal in animals:
        print(f"{animal.name}: {animal.speak()}")

# 使用示例
animals = [
    Dog("旺财"),
    Cat("小白"),
    Bird("小黄")
]

animal_sounds(animals)
# 输出：
# 旺财: 汪汪!
# 小白: 喵喵!
# 小黄: 叽叽!
```

### 15.1.2 多态的好处

```python
# 多态的优势：代码复用和扩展性

class Shape:
    """形状基类"""

    def area(self):
        """计算面积"""
        raise NotImplementedError

    def perimeter(self):
        """计算周长"""
        raise NotImplementedError

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

    def perimeter(self):
        return 2 * (self.width + self.height)

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        import math
        return math.pi * self.radius ** 2

    def perimeter(self):
        import math
        return 2 * math.pi * self.radius

class Triangle(Shape):
    def __init__(self, a, b, c):
        self.a = a
        self.b = b
        self.c = c

    def area(self):
        # 海伦公式
        s = (self.a + self.b + self.c) / 2
        return (s * (s - self.a) * (s - self.b) * (s - self.c)) ** 0.5

    def perimeter(self):
        return self.a + self.b + self.c

# 多态函数
def print_shape_info(shapes):
    """打印形状信息"""
    for shape in shapes:
        print(f"{type(shape).__name__}:")
        print(f"  面积: {shape.area():.2f}")
        print(f"  周长: {shape.perimeter():.2f}")
        print()

# 使用示例
shapes = [
    Rectangle(4, 5),
    Circle(3),
    Triangle(3, 4, 5)
]

print_shape_info(shapes)
```

### 15.1.3 运行时多态 vs 编译时多态

```python
# Python 是动态语言，主要支持运行时多态
# 运行时多态：方法调用在运行时确定

class Calculator:
    """计算器类"""

    def add(self, a, b):
        return a + b

    def add(self, a, b, c=0):  # 这不是重载，而是覆盖
        return a + b + c

# Python 不支持传统意义上的方法重载
calc = Calculator()
print(calc.add(1, 2))      # 3
print(calc.add(1, 2, 3))   # 6

# 使用 *args 模拟重载
class AdvancedCalculator:
    """高级计算器"""

    def add(self, *args):
        """支持多个参数的加法"""
        return sum(args)

    def multiply(self, *args):
        """支持多个参数的乘法"""
        result = 1
        for arg in args:
            result *= arg
        return result

adv_calc = AdvancedCalculator()
print(adv_calc.add(1, 2))           # 3
print(adv_calc.add(1, 2, 3, 4))     # 10
print(adv_calc.multiply(2, 3, 4))   # 24
```

## 15.2 方法重写和重载

### 15.2.1 方法重写的深入理解

```python
class Vehicle:
    """交通工具基类"""

    def __init__(self, brand, model):
        self.brand = brand
        self.model = model

    def start(self):
        return f"{self.brand} {self.model} 启动"

    def stop(self):
        return f"{self.brand} {self.model} 停止"

    def move(self):
        return f"{self.brand} {self.model} 移动"

class Car(Vehicle):
    """汽车类"""

    def __init__(self, brand, model, fuel_type):
        super().__init__(brand, model)
        self.fuel_type = fuel_type

    def start(self):
        # 扩展父类方法
        parent_result = super().start()
        return f"{parent_result}，使用 {self.fuel_type} 燃料"

    def move(self):
        return f"{self.brand} {self.model} 在路上行驶"

class Boat(Vehicle):
    """船类"""

    def __init__(self, brand, model, max_speed):
        super().__init__(brand, model)
        self.max_speed = max_speed

    def start(self):
        parent_result = super().start()
        return f"{parent_result}，准备起航"

    def move(self):
        return f"{self.brand} {self.model} 在水中航行，最高时速 {self.max_speed} 海里"

class Plane(Vehicle):
    """飞机类"""

    def __init__(self, brand, model, altitude):
        super().__init__(brand, model)
        self.altitude = altitude

    def start(self):
        parent_result = super().start()
        return f"{parent_result}，滑行到跑道"

    def move(self):
        return f"{self.brand} {self.model} 在 {self.altitude} 高度飞行"

# 多态函数
def start_all_vehicles(vehicles):
    """启动所有交通工具"""
    for vehicle in vehicles:
        print(vehicle.start())

def move_all_vehicles(vehicles):
    """移动所有交通工具"""
    for vehicle in vehicles:
        print(vehicle.move())

# 使用示例
vehicles = [
    Car("Toyota", "Camry", "汽油"),
    Boat("Yamaha", "Speedboat", 50),
    Plane("Boeing", "747", "35000英尺")
]

print("启动所有交通工具:")
start_all_vehicles(vehicles)

print("\n移动所有交通工具:")
move_all_vehicles(vehicles)
```

### 15.2.2 使用 super() 的最佳实践

```python
class A:
    def __init__(self, a):
        self.a = a
        print(f"A.__init__: a = {a}")

class B(A):
    def __init__(self, a, b):
        super().__init__(a)
        self.b = b
        print(f"B.__init__: b = {b}")

class C(A):
    def __init__(self, a, c):
        super().__init__(a)
        self.c = c
        print(f"C.__init__: c = {c}")

class D(B, C):
    def __init__(self, a, b, c, d):
        super().__init__(a, b)  # 调用 B.__init__
        # 注意：这里不会自动调用 C.__init__
        # 需要手动处理
        C.__init__(self, a, c)
        self.d = d
        print(f"D.__init__: d = {d}")

# 查看 MRO
print("D 的 MRO:", D.__mro__)

# 创建实例
d = D(1, 2, 3, 4)
print(f"最终结果: a={d.a}, b={d.b}, c={d.c}, d={d.d}")
```

### 15.2.3 方法重写的注意事项

```python
class Parent:
    def method(self, x, y=10):
        return x + y

class Child(Parent):
    def method(self, x, y=20, z=30):
        # 重写时参数签名可以不同
        return x + y + z

# 使用示例
parent = Parent()
child = Child()

print(parent.method(5))      # 15 (x=5, y=10)
print(child.method(5))       # 55 (x=5, y=20, z=30)

# 但是通过父类引用调用时会有问题
def call_method(obj):
    return obj.method(5)

print(call_method(parent))   # 15
print(call_method(child))    # 55

# 更好的做法：保持参数签名兼容
class BetterChild(Parent):
    def method(self, x, y=10):
        # 保持与父类相同的参数签名
        result = super().method(x, y)
        return result * 2

better_child = BetterChild()
print(better_child.method(5))     # 30 ( (5+10) * 2 )
print(call_method(better_child))  # 30
```

## 15.3 鸭子类型

鸭子类型是 Python 的动态类型特性：如果一个对象走起来像鸭子，叫起来像鸭子，那么它就是鸭子。

### 15.3.1 鸭子类型的概念

```python
# 鸭子类型：不关心对象的类型，只关心对象的行为

class Duck:
    """鸭子类"""

    def quack(self):
        return "嘎嘎!"

    def fly(self):
        return "鸭子飞走了"

class Person:
    """人会模仿鸭子"""

    def quack(self):
        return "人模仿嘎嘎!"

    def fly(self):
        return "人跳着飞走了"

class Car:
    """车不会叫也不会飞"""

    def drive(self):
        return "车开走了"

def make_it_quack(obj):
    """让对象发出叫声"""
    if hasattr(obj, 'quack'):
        print(obj.quack())
    else:
        print("这个对象不会叫")

def make_it_fly(obj):
    """让对象飞起来"""
    if hasattr(obj, 'fly'):
        print(obj.fly())
    else:
        print("这个对象不会飞")

# 使用示例
objects = [Duck(), Person(), Car()]

for obj in objects:
    print(f"{type(obj).__name__}:")
    make_it_quack(obj)
    make_it_fly(obj)
    print()
```

### 15.3.2 鸭子类型的优势

```python
# 鸭子类型让代码更灵活

def process_data(data):
    """处理数据（支持任何可迭代对象）"""
    result = []
    for item in data:  # 只要是可迭代的就可以
        if hasattr(item, 'upper'):  # 只要有 upper 方法就可以
            result.append(item.upper())
        else:
            result.append(str(item))
    return result

# 测试不同的数据类型
print(process_data(["hello", "world"]))      # 列表
print(process_data(("hello", "world")))      # 元组
print(process_data({"hello", "world"}))      # 集合
print(process_data("hello world"))           # 字符串

# 文件对象也是可迭代的
with open(__file__, 'r') as f:
    lines = process_data(f)
    print(f"文件有 {len(lines)} 行")

# 自定义可迭代对象
class MyIterable:
    def __iter__(self):
        return iter(["custom", "iterable"])

print(process_data(MyIterable()))
```

### 15.3.3 协议和接口

```python
# Python 使用协议（Protocol）而不是显式接口

from typing import Protocol

class Speakable(Protocol):
    """可说话的协议"""

    def speak(self) -> str:
        """说话方法"""
        ...

class Flyable(Protocol):
    """可飞行的协议"""

    def fly(self) -> str:
        """飞行方法"""
        ...

class SuperHero:
    """超级英雄"""

    def speak(self):
        return "我是超级英雄！"

    def fly(self):
        return "超级英雄飞走了！"

class Robot:
    """机器人"""

    def speak(self):
        return "机器人报告"

    def walk(self):
        return "机器人行走"

def make_speakable_speak(obj: Speakable):
    """让可说话的对象说话"""
    return obj.speak()

def make_flyable_fly(obj: Flyable):
    """让可飞行的对象飞行"""
    return obj.fly()

# 使用示例
hero = SuperHero()
robot = Robot()

print(make_speakable_speak(hero))   # 超级英雄说话
print(make_speakable_speak(robot))  # 机器人说话

print(make_flyable_fly(hero))      # 超级英雄飞行
# make_flyable_fly(robot)  # 会报错：robot 没有 fly 方法
```

## 15.4 协议和结构化类型

### 15.4.1 使用 Protocol 定义接口

```python
from typing import Protocol, runtime_checkable

@runtime_checkable
class Drawable(Protocol):
    """可绘制协议"""

    def draw(self, canvas) -> None:
        """在画布上绘制"""
        ...

@runtime_checkable
class Measurable(Protocol):
    """可测量协议"""

    @property
    def area(self) -> float:
        """面积"""
        ...

    @property
    def perimeter(self) -> float:
        """周长"""
        ...

class Circle:
    """圆形"""

    def __init__(self, radius):
        self.radius = radius

    def draw(self, canvas):
        print(f"在 {canvas} 上绘制圆形，半径 {self.radius}")

    @property
    def area(self):
        import math
        return math.pi * self.radius ** 2

    @property
    def perimeter(self):
        import math
        return 2 * math.pi * self.radius

class Rectangle:
    """矩形"""

    def __init__(self, width, height):
        self.width = width
        self.height = height

    def draw(self, canvas):
        print(f"在 {canvas} 上绘制矩形，尺寸 {self.width}x{self.height}")

    @property
    def area(self):
        return self.width * self.height

    @property
    def perimeter(self):
        return 2 * (self.width + self.height)

# 使用协议
def draw_shapes(shapes: list[Drawable], canvas: str):
    """绘制形状列表"""
    for shape in shapes:
        if isinstance(shape, Drawable):
            shape.draw(canvas)

def calculate_total_area(shapes: list[Measurable]) -> float:
    """计算总面积"""
    total = 0
    for shape in shapes:
        if isinstance(shape, Measurable):
            total += shape.area
    return total

# 使用示例
shapes = [Circle(5), Rectangle(4, 6)]

print("绘制形状:")
draw_shapes(shapes, "屏幕")

print(f"\n总面积: {calculate_total_area(shapes):.2f}")

# 检查协议实现
print(f"\nCircle 实现了 Drawable: {isinstance(Circle(1), Drawable)}")
print(f"Rectangle 实现了 Measurable: {isinstance(Rectangle(1, 1), Measurable)}")
```

### 15.4.2 结构化类型 vs 名义类型

```python
# Python 使用结构化类型（鸭子类型）
# 而不是名义类型（显式继承）

class Logger:
    """日志器"""

    def log(self, message: str) -> None:
        print(f"日志: {message}")

class FileLogger:
    """文件日志器（没有继承 Logger）"""

    def __init__(self, filename):
        self.filename = filename

    def log(self, message: str) -> None:
        with open(self.filename, 'a') as f:
            f.write(f"{message}\n")
        print(f"文件日志: {message}")

class DatabaseLogger:
    """数据库日志器（没有继承 Logger）"""

    def log(self, message: str) -> None:
        # 模拟数据库操作
        print(f"数据库日志: {message}")

def process_logs(logger, messages):
    """处理日志（接受任何有 log 方法的对象）"""
    for message in messages:
        logger.log(message)

# 使用示例
loggers = [
    Logger(),
    FileLogger("app.log"),
    DatabaseLogger()
]

messages = ["用户登录", "数据更新", "系统错误"]

for logger in loggers:
    print(f"\n使用 {type(logger).__name__}:")
    process_logs(logger, messages)
```

## 15.5 抽象类和接口

### 15.5.1 抽象基类的应用

```python
from abc import ABC, abstractmethod
from typing import List

class DataProcessor(ABC):
    """数据处理器抽象基类"""

    def __init__(self, data_source):
        self.data_source = data_source

    @abstractmethod
    def load_data(self) -> List[dict]:
        """加载数据"""
        pass

    @abstractmethod
    def process_data(self, data: List[dict]) -> List[dict]:
        """处理数据"""
        pass

    @abstractmethod
    def save_data(self, data: List[dict]) -> None:
        """保存数据"""
        pass

    def execute(self) -> None:
        """执行完整流程（模板方法）"""
        data = self.load_data()
        processed_data = self.process_data(data)
        self.save_data(processed_data)
        print("数据处理完成")

class CSVProcessor(DataProcessor):
    """CSV 文件处理器"""

    def load_data(self):
        # 模拟 CSV 读取
        return [
            {"name": "Alice", "age": 25},
            {"name": "Bob", "age": 30}
        ]

    def process_data(self, data):
        # 添加处理标记
        for item in data:
            item["processed"] = True
        return data

    def save_data(self, data):
        print(f"保存到 CSV 文件: {self.data_source}")
        for item in data:
            print(f"  {item}")

class JSONProcessor(DataProcessor):
    """JSON 文件处理器"""

    def load_data(self):
        # 模拟 JSON 读取
        return [
            {"product": "Apple", "price": 1.5},
            {"product": "Banana", "price": 0.8}
        ]

    def process_data(self, data):
        # 计算折扣价
        for item in data:
            item["discount_price"] = item["price"] * 0.9
        return data

    def save_data(self, data):
        print(f"保存到 JSON 文件: {self.data_source}")
        for item in data:
            print(f"  {item}")

# 使用示例
processors = [
    CSVProcessor("data.csv"),
    JSONProcessor("data.json")
]

for processor in processors:
    print(f"\n执行 {type(processor).__name__}:")
    processor.execute()
```

### 15.5.2 接口的模拟

```python
# Python 没有显式的 interface 关键字
# 但可以使用抽象基类或 Protocol 来实现接口

from abc import ABC, abstractmethod
from typing import Protocol

# 方法1: 使用抽象基类
class PaymentInterface(ABC):
    """支付接口"""

    @abstractmethod
    def pay(self, amount: float) -> bool:
        """支付方法"""
        pass

    @abstractmethod
    def refund(self, amount: float) -> bool:
        """退款方法"""
        pass

# 方法2: 使用 Protocol
class PaymentProtocol(Protocol):
    """支付协议"""

    def pay(self, amount: float) -> bool:
        """支付方法"""
        ...

    def refund(self, amount: float) -> bool:
        """退款方法"""
        ...

# 实现类
class CreditCardPayment:
    """信用卡支付"""

    def pay(self, amount):
        print(f"使用信用卡支付 {amount} 元")
        return True

    def refund(self, amount):
        print(f"信用卡退款 {amount} 元")
        return True

class PayPalPayment:
    """PayPal 支付"""

    def pay(self, amount):
        print(f"使用 PayPal 支付 {amount} 元")
        return True

    def refund(self, amount):
        print(f"PayPal 退款 {amount} 元")
        return True

# 使用接口
def process_payment(payment_method, amount):
    """处理支付"""
    if payment_method.pay(amount):
        print("支付成功")
        # 模拟退款
        payment_method.refund(amount * 0.1)
    else:
        print("支付失败")

# 使用示例
payments = [
    CreditCardPayment(),
    PayPalPayment()
]

for payment in payments:
    print(f"\n使用 {type(payment).__name__}:")
    process_payment(payment, 100.0)
```

## 15.6 组合 vs 继承

### 15.6.1 组合的基本概念

```python
# 组合：一个类包含其他类的实例

class Engine:
    """引擎"""

    def __init__(self, fuel_type):
        self.fuel_type = fuel_type

    def start(self):
        return f"{self.fuel_type} 引擎启动"

    def stop(self):
        return f"{self.fuel_type} 引擎停止"

class Wheels:
    """车轮"""

    def __init__(self, count):
        self.count = count

    def rotate(self):
        return f"{self.count} 个车轮转动"

class Car:
    """汽车（使用组合）"""

    def __init__(self, brand, model, fuel_type, wheel_count):
        self.brand = brand
        self.model = model
        self.engine = Engine(fuel_type)
        self.wheels = Wheels(wheel_count)

    def start(self):
        return f"{self.brand} {self.model}: {self.engine.start()}"

    def drive(self):
        return f"{self.brand} {self.model}: {self.wheels.rotate()}"

    def stop(self):
        return f"{self.brand} {self.model}: {self.engine.stop()}"

# 使用示例
car = Car("Toyota", "Camry", "汽油", 4)
print(car.start())
print(car.drive())
print(car.stop())
```

### 15.6.2 继承 vs 组合

```python
# 继承示例（可能不太合适）
class Vehicle:
    def move(self):
        return "车辆移动"

class Car(Vehicle):
    def move(self):
        return "汽车在路上行驶"

class Boat(Vehicle):
    def move(self):
        return "船在水中航行"

# 组合示例（更灵活）
class MovementBehavior:
    """移动行为"""

    def move(self):
        raise NotImplementedError

class RoadMovement(MovementBehavior):
    def move(self):
        return "在路上行驶"

class WaterMovement(MovementBehavior):
    def move(self):
        return "在水中航行"

class AirMovement(MovementBehavior):
    def move(self):
        return "在空中飞行"

class Vehicle:
    """交通工具"""

    def __init__(self, name, movement_behavior):
        self.name = name
        self.movement = movement_behavior

    def move(self):
        return f"{self.name}: {self.movement.move()}"

# 使用示例
car = Vehicle("汽车", RoadMovement())
boat = Vehicle("船", WaterMovement())
plane = Vehicle("飞机", AirMovement())

vehicles = [car, boat, plane]
for vehicle in vehicles:
    print(vehicle.move())

# 组合的优势：运行时改变行为
car.movement = AirMovement()  # 汽车突然会飞了！
print(car.move())
```

### 15.6.3 组合的最佳实践

```python
# 组合设计模式：委托

class Printer:
    """打印机"""

    def print_document(self, document):
        print(f"打印文档: {document}")

class Scanner:
    """扫描仪"""

    def scan_document(self, document):
        return f"扫描文档: {document}"

class Fax:
    """传真机"""

    def fax_document(self, document, number):
        print(f"传真文档到 {number}: {document}")

class AllInOnePrinter:
    """多功能一体机（使用组合）"""

    def __init__(self):
        self.printer = Printer()
        self.scanner = Scanner()
        self.fax = Fax()

    def print_document(self, document):
        self.printer.print_document(document)

    def scan_document(self, document):
        return self.scanner.scan_document(document)

    def fax_document(self, document, number):
        self.fax.fax_document(document, number)

    def copy_document(self, document):
        """复印功能（组合已有功能）"""
        scanned = self.scan_document(document)
        self.print_document(scanned)
        return "复印完成"

# 使用示例
printer = AllInOnePrinter()

printer.print_document("报告.pdf")
print(printer.scan_document("合同.pdf"))
printer.fax_document("发票.pdf", "123-456-7890")
print(printer.copy_document("手册.pdf"))
```

## 15.7 设计模式简介

### 15.7.1 工厂模式

```python
# 工厂模式：创建对象的接口

from abc import ABC, abstractmethod

class Animal(ABC):
    """动物抽象类"""

    @abstractmethod
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        return "汪汪!"

class Cat(Animal):
    def speak(self):
        return "喵喵!"

class AnimalFactory:
    """动物工厂"""

    @staticmethod
    def create_animal(animal_type, name):
        """创建动物"""
        if animal_type == "dog":
            return Dog()
        elif animal_type == "cat":
            return Cat()
        else:
            raise ValueError(f"未知动物类型: {animal_type}")

# 使用示例
dog = AnimalFactory.create_animal("dog", "旺财")
cat = AnimalFactory.create_animal("cat", "小白")

print(dog.speak())  # 汪汪!
print(cat.speak())  # 喵喵!
```

### 15.7.2 策略模式

```python
# 策略模式：定义一系列算法，将每个算法封装起来

from abc import ABC, abstractmethod

class SortingStrategy(ABC):
    """排序策略抽象类"""

    @abstractmethod
    def sort(self, data):
        pass

class BubbleSort(SortingStrategy):
    """冒泡排序"""

    def sort(self, data):
        arr = data.copy()
        n = len(arr)
        for i in range(n):
            for j in range(0, n - i - 1):
                if arr[j] > arr[j + 1]:
                    arr[j], arr[j + 1] = arr[j + 1], arr[j]
        return arr

class QuickSort(SortingStrategy):
    """快速排序"""

    def sort(self, data):
        arr = data.copy()
        if len(arr) <= 1:
            return arr

        pivot = arr[len(arr) // 2]
        left = [x for x in arr if x < pivot]
        middle = [x for x in arr if x == pivot]
        right = [x for x in arr if x > pivot]

        return self.sort(left) + middle + self.sort(right)

class Sorter:
    """排序器"""

    def __init__(self, strategy):
        self.strategy = strategy

    def sort(self, data):
        return self.strategy.sort(data)

# 使用示例
data = [64, 34, 25, 12, 22, 11, 90]

bubble_sorter = Sorter(BubbleSort())
quick_sorter = Sorter(QuickSort())

print("原始数据:", data)
print("冒泡排序:", bubble_sorter.sort(data))
print("快速排序:", quick_sorter.sort(data))
```

### 15.7.3 观察者模式

```python
# 观察者模式：定义对象间的一对多依赖

class Subject:
    """主题（被观察者）"""

    def __init__(self):
        self._observers = []
        self._state = None

    def attach(self, observer):
        """添加观察者"""
        self._observers.append(observer)

    def detach(self, observer):
        """移除观察者"""
        self._observers.remove(observer)

    def notify(self):
        """通知所有观察者"""
        for observer in self._observers:
            observer.update(self)

    def set_state(self, state):
        """设置状态"""
        self._state = state
        self.notify()

    def get_state(self):
        return self._state

class Observer(ABC):
    """观察者抽象类"""

    @abstractmethod
    def update(self, subject):
        pass

class ConcreteObserver(Observer):
    """具体观察者"""

    def __init__(self, name):
        self.name = name

    def update(self, subject):
        print(f"{self.name} 收到通知: 状态变为 {subject.get_state()}")

# 使用示例
subject = Subject()

observer1 = ConcreteObserver("观察者1")
observer2 = ConcreteObserver("观察者2")

subject.attach(observer1)
subject.attach(observer2)

subject.set_state("状态A")
subject.set_state("状态B")

subject.detach(observer1)
subject.set_state("状态C")
```

## 总结

本章介绍了继承和多态的核心概念：
- 理解多态的基本概念和优势
- 掌握方法重写和 super() 的使用
- 学习鸭子类型和协议
- 了解抽象类和接口的设计
- 掌握组合 vs 继承的选择
- 学习常见设计模式

通过本章的学习，你应该能够：
1. 设计合理的继承层次结构
2. 使用多态编写灵活的代码
3. 理解鸭子类型的优势
4. 正确使用抽象基类
5. 选择合适的组合或继承
6. 应用常见的设计模式

下一章将介绍特殊方法和魔法方法。
