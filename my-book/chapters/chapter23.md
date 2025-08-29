# 第23章：装饰器

## 23.1 函数装饰器

### 23.1.1 装饰器基础

```python
# 装饰器基础
print("装饰器基础:")

# 1. 简单装饰器
def simple_decorator(func):
    """最简单的装饰器"""
    def wrapper():
        print("装饰器执行前")
        func()
        print("装饰器执行后")
    return wrapper

@simple_decorator
def greet():
    print("Hello, World!")

print("使用装饰器:")
greet()

# 2. 手动装饰
def say_goodbye():
    print("Goodbye!")

print("\n手动装饰:")
decorated_goodbye = simple_decorator(say_goodbye)
decorated_goodbye()

# 3. 装饰器的执行时机
print("\n装饰器的执行时机:")

def execution_time_decorator(func):
    print(f"函数 {func.__name__} 被装饰了")
    return func

@execution_time_decorator
def target_function():
    print("目标函数执行")

print("导入时装饰器就执行了")
target_function()

# 4. 保留元数据
print("\n保留元数据:")

def decorator_without_wraps(func):
    def wrapper():
        """包装函数"""
        return func()
    return wrapper

def decorator_with_wraps(func):
    from functools import wraps
    @wraps(func)
    def wrapper():
        """包装函数"""
        return func()
    return wrapper

@decorator_without_wraps
def function_a():
    """原始函数A"""
    pass

@decorator_with_wraps
def function_b():
    """原始函数B"""
    pass

print(f"不使用 wraps: {function_a.__name__}, {function_a.__doc__}")
print(f"使用 wraps: {function_b.__name__}, {function_b.__doc__}")

# 5. 多个装饰器
print("\n多个装饰器:")

def decorator1(func):
    def wrapper():
        print("装饰器1 开始")
        result = func()
        print("装饰器1 结束")
        return result
    return wrapper

def decorator2(func):
    def wrapper():
        print("装饰器2 开始")
        result = func()
        print("装饰器2 结束")
        return result
    return wrapper

@decorator1
@decorator2
def multi_decorated():
    print("原始函数")

print("多个装饰器的执行顺序:")
multi_decorated()
```

### 23.1.2 带参数的装饰器

```python
# 带参数的装饰器
print("带参数的装饰器:")

# 1. 装饰器工厂
def repeat(times):
    """重复执行装饰器工厂"""
    def decorator(func):
        def wrapper(*args, **kwargs):
            for i in range(times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def say_hello(name):
    print(f"Hello, {name}!")

print("重复执行3次:")
say_hello("Alice")

# 2. 通用参数装饰器
def log_with_level(level):
    """带日志级别的装饰器"""
    def decorator(func):
        def wrapper(*args, **kwargs):
            print(f"[{level}] 调用函数: {func.__name__}")
            result = func(*args, **kwargs)
            print(f"[{level}] 函数 {func.__name__} 执行完成")
            return result
        return wrapper
    return decorator

@log_with_level("INFO")
def add_numbers(a, b):
    return a + b

@log_with_level("DEBUG")
def multiply_numbers(a, b):
    return a * b

print("\n带参数的装饰器:")
result1 = add_numbers(5, 3)
result2 = multiply_numbers(4, 7)
print(f"结果: {result1}, {result2}")

# 3. 可选参数装饰器
def flexible_decorator(prefix="INFO", suffix="DONE"):
    """灵活参数装饰器"""
    def decorator(func):
        def wrapper(*args, **kwargs):
            print(f"[{prefix}] 开始执行 {func.__name__}")
            result = func(*args, **kwargs)
            print(f"[{suffix}] {func.__name__} 执行完成")
            return result
        return wrapper
    return decorator

@flexible_decorator()
def func1():
    print("函数1")

@flexible_decorator("DEBUG", "FINISHED")
def func2():
    print("函数2")

@flexible_decorator(prefix="WARNING")
def func3():
    print("函数3")

print("\n灵活参数装饰器:")
func1()
func2()
func3()

# 4. 类作为装饰器工厂
class ConfigurableDecorator:
    """可配置的装饰器类"""

    def __init__(self, config):
        self.config = config

    def __call__(self, func):
        def wrapper(*args, **kwargs):
            print(f"配置: {self.config}")
            print(f"执行函数: {func.__name__}")
            result = func(*args, **kwargs)
            print(f"函数 {func.__name__} 执行完毕")
            return result
        return wrapper

config_decorator = ConfigurableDecorator({"debug": True, "timeout": 30})

@config_decorator
def configured_function():
    print("配置化函数")

print("\n类作为装饰器工厂:")
configured_function()
```

### 23.1.3 装饰器的实际应用

```python
# 装饰器的实际应用
print("装饰器的实际应用:")

# 1. 计时装饰器
import time
from functools import wraps

def timing_decorator(func):
    """计时装饰器"""
    @wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        execution_time = end_time - start_time
        print(f"函数 {func.__name__} 执行时间: {execution_time:.6f} 秒")
        return result
    return wrapper

@timing_decorator
def slow_function(n):
    """模拟慢函数"""
    time.sleep(n * 0.1)
    return f"处理了 {n} 项数据"

@timing_decorator
def fibonacci(n):
    """计算斐波那契数"""
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

print("计时装饰器:")
result1 = slow_function(3)
result2 = fibonacci(10)
print(f"结果: {result1}, {result2}")

# 2. 缓存装饰器
def simple_cache(func):
    """简单缓存装饰器"""
    cache = {}
    @wraps(func)
    def wrapper(*args, **kwargs):
        # 创建缓存键
        key = str(args) + str(sorted(kwargs.items()))
        if key not in cache:
            print(f"计算 {func.__name__}{args}")
            cache[key] = func(*args, **kwargs)
        else:
            print(f"从缓存获取 {func.__name__}{args}")
        return cache[key]
    return wrapper

@simple_cache
def expensive_computation(x, y):
    """模拟耗时计算"""
    time.sleep(0.5)
    return x ** y

print("\n缓存装饰器:")
result1 = expensive_computation(2, 10)
result2 = expensive_computation(2, 10)  # 从缓存获取
result3 = expensive_computation(3, 5)   # 重新计算

# 3. 重试装饰器
def retry_decorator(max_attempts=3, delay=1):
    """重试装饰器"""
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(max_attempts):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    if attempt == max_attempts - 1:
                        raise e
                    print(f"尝试 {attempt + 1} 失败: {e}")
                    time.sleep(delay)
        return wrapper
    return decorator

@retry_decorator(max_attempts=3, delay=0.5)
def unreliable_function():
    """不可靠的函数"""
    import random
    if random.random() < 0.7:  # 70% 失败率
        raise ValueError("随机错误")
    return "成功执行"

print("\n重试装饰器:")
try:
    result = unreliable_function()
    print(f"最终结果: {result}")
except ValueError as e:
    print(f"所有重试都失败了: {e}")

# 4. 权限检查装饰器
def require_permission(permission):
    """权限检查装饰器"""
    def decorator(func):
        @wraps(func)
        def wrapper(user, *args, **kwargs):
            if not hasattr(user, 'permissions'):
                raise ValueError("用户没有权限信息")

            if permission not in user.permissions:
                raise PermissionError(f"用户没有 {permission} 权限")

            return func(user, *args, **kwargs)
        return wrapper
    return decorator

class User:
    def __init__(self, name, permissions):
        self.name = name
        self.permissions = permissions

@require_permission("admin")
def delete_user(admin_user, target_user):
    """删除用户（需要管理员权限）"""
    print(f"管理员 {admin_user.name} 删除了用户 {target_user.name}")

@require_permission("read")
def view_profile(user, profile_user):
    """查看用户资料（需要读取权限）"""
    print(f"用户 {user.name} 查看了 {profile_user.name} 的资料")

print("\n权限检查装饰器:")
admin = User("Alice", ["admin", "read", "write"])
normal_user = User("Bob", ["read"])

try:
    delete_user(admin, normal_user)
    view_profile(normal_user, admin)
    # 这会失败
    delete_user(normal_user, admin)
except PermissionError as e:
    print(f"权限错误: {e}")

# 5. 输入验证装饰器
def validate_input(**validators):
    """输入验证装饰器"""
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            # 获取函数参数
            import inspect
            sig = inspect.signature(func)
            bound_args = sig.bind(*args, **kwargs)
            bound_args.apply_defaults()

            # 验证参数
            for param_name, validator in validators.items():
                if param_name in bound_args.arguments:
                    value = bound_args.arguments[param_name]
                    if not validator(value):
                        raise ValueError(f"参数 {param_name} 验证失败: {value}")

            return func(*args, **kwargs)
        return wrapper
    return decorator

def is_positive(x):
    """检查是否为正数"""
    return isinstance(x, (int, float)) and x > 0

def is_non_empty_string(s):
    """检查是否为非空字符串"""
    return isinstance(s, str) and len(s.strip()) > 0

@validate_input(a=is_positive, b=is_positive, name=is_non_empty_string)
def create_item(a, b, name):
    """创建项目"""
    return f"项目 {name}: {a} x {b} = {a * b}"

print("\n输入验证装饰器:")
try:
    item1 = create_item(5, 3, "测试项目")
    print(f"创建成功: {item1}")

    # 这会失败
    item2 = create_item(-1, 3, "无效项目")
except ValueError as e:
    print(f"验证错误: {e}")

# 6. 数据库事务装饰器
def transactional(func):
    """数据库事务装饰器"""
    @wraps(func)
    def wrapper(*args, **kwargs):
        print("开始数据库事务")
        try:
            result = func(*args, **kwargs)
            print("提交事务")
            return result
        except Exception as e:
            print(f"回滚事务: {e}")
            raise
    return wrapper

@transactional
def transfer_money(from_account, to_account, amount):
    """转账函数"""
    if amount <= 0:
        raise ValueError("转账金额必须大于0")

    print(f"从账户 {from_account} 转出 {amount}")
    print(f"向账户 {to_account} 转入 {amount}")
    return "转账成功"

print("\n数据库事务装饰器:")
try:
    result = transfer_money("A001", "B002", 100)
    print(f"结果: {result}")

    # 这会触发回滚
    transfer_money("A001", "B002", -50)
except ValueError as e:
    print(f"事务回滚: {e}")
```

## 23.2 类装饰器

### 23.2.1 类装饰器基础

```python
# 类装饰器基础
print("类装饰器基础:")

# 1. 简单类装饰器
class SimpleClassDecorator:
    """简单的类装饰器"""

    def __init__(self, cls):
        self.cls = cls
        print(f"类 {cls.__name__} 被装饰了")

    def __call__(self, *args, **kwargs):
        print(f"创建 {self.cls.__name__} 实例")
        instance = self.cls(*args, **kwargs)
        print(f"{self.cls.__name__} 实例创建完成")
        return instance

@SimpleClassDecorator
class MyClass:
    def __init__(self, value):
        self.value = value

    def __str__(self):
        return f"MyClass(value={self.value})"

print("类装饰器示例:")
obj = MyClass(42)
print(f"对象: {obj}")

# 2. 修改类行为的装饰器
class AddMethodDecorator:
    """添加方法的类装饰器"""

    def __init__(self, cls):
        self.cls = cls

    def __call__(self, *args, **kwargs):
        instance = self.cls(*args, **kwargs)
        # 动态添加方法
        instance.added_method = lambda: f"这是动态添加的方法，值为 {instance.value}"
        return instance

@AddMethodDecorator
class SimpleClass:
    def __init__(self, value):
        self.value = value

print("\n添加方法装饰器:")
obj = SimpleClass(100)
print(f"调用动态方法: {obj.added_method()}")

# 3. 属性验证装饰器
class PropertyValidator:
    """属性验证装饰器"""

    def __init__(self, cls):
        self.cls = cls

    def __call__(self, *args, **kwargs):
        instance = self.cls(*args, **kwargs)

        # 为所有属性添加验证
        for attr_name in dir(instance):
            if not attr_name.startswith('_') and not callable(getattr(instance, attr_name)):
                original_value = getattr(instance, attr_name)
                setattr(instance, f"_{attr_name}", original_value)

                # 创建属性描述符
                def getter(self, name=attr_name):
                    return getattr(self, f"_{name}")

                def setter(self, value, name=attr_name):
                    if name == 'age' and (not isinstance(value, int) or value < 0):
                        raise ValueError("年龄必须是正整数")
                    elif name == 'name' and (not isinstance(value, str) or not value.strip()):
                        raise ValueError("姓名不能为空")
                    setattr(self, f"_{name}", value)

                # 替换为属性
                setattr(instance.__class__, attr_name, property(getter, setter))

        return instance

@PropertyValidator
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

print("\n属性验证装饰器:")
person = Person("Alice", 30)
print(f"初始: 姓名={person.name}, 年龄={person.age}")

try:
    person.age = -5  # 这会失败
except ValueError as e:
    print(f"验证错误: {e}")

try:
    person.name = ""  # 这会失败
except ValueError as e:
    print(f"验证错误: {e}")

# 4. 单例模式装饰器
class Singleton:
    """单例模式装饰器"""

    def __init__(self, cls):
        self.cls = cls
        self.instance = None

    def __call__(self, *args, **kwargs):
        if self.instance is None:
            self.instance = self.cls(*args, **kwargs)
        return self.instance

@Singleton
class DatabaseConnection:
    def __init__(self, host, port):
        self.host = host
        self.port = port
        print(f"创建数据库连接: {host}:{port}")

    def query(self, sql):
        return f"执行查询: {sql}"

print("\n单例模式装饰器:")
db1 = DatabaseConnection("localhost", 5432)
db2 = DatabaseConnection("remotehost", 5432)  # 这不会创建新实例

print(f"db1 is db2: {db1 is db2}")
print(f"db1 连接: {db1.host}:{db1.port}")
print(f"查询结果: {db1.query('SELECT * FROM users')}")
```

### 23.2.2 高级类装饰器

```python
# 高级类装饰器
print("高级类装饰器:")

# 1. 带参数的类装饰器
class ConfigurableClassDecorator:
    """可配置的类装饰器"""

    def __init__(self, **config):
        self.config = config

    def __call__(self, cls):
        # 修改类
        original_init = cls.__init__

        def new_init(self, *args, **kwargs):
            print(f"使用配置: {self.config}")
            original_init(self, *args, **kwargs)
            # 添加配置信息
            self._config = self.config

        cls.__init__ = new_init
        return cls

@ConfigurationClassDecorator(debug=True, timeout=30)
class Service:
    def __init__(self, name):
        self.name = name

print("可配置类装饰器:")
service = Service("WebService")
print(f"服务配置: {service._config}")

# 2. 注册装饰器
class Registry:
    """类注册器"""

    def __init__(self):
        self.classes = {}

    def register(self, name=None):
        def decorator(cls):
            class_name = name or cls.__name__
            self.classes[class_name] = cls
            print(f"注册类: {class_name}")
            return cls
        return decorator

    def get_class(self, name):
        return self.classes.get(name)

# 创建全局注册器
registry = Registry()

@registry.register()
class Dog:
    def speak(self):
        return "汪汪"

@registry.register("Cat")
class Feline:
    def speak(self):
        return "喵喵"

print("\n类注册器:")
dog_class = registry.get_class("Dog")
cat_class = registry.get_class("Cat")

dog = dog_class()
cat = cat_class()

print(f"狗叫: {dog.speak()}")
print(f"猫叫: {cat.speak()}")

# 3. 序列化装饰器
class Serializable:
    """序列化装饰器"""

    def __init__(self, cls):
        self.cls = cls

    def __call__(self, *args, **kwargs):
        instance = self.cls(*args, **kwargs)
        # 添加序列化方法
        instance.to_dict = self._to_dict
        instance.from_dict = classmethod(self._from_dict)
        return instance

    def _to_dict(self):
        """转换为字典"""
        result = {}
        for attr in dir(self):
            if not attr.startswith('_') and not callable(getattr(self, attr)):
                result[attr] = getattr(self, attr)
        return result

    @classmethod
    def _from_dict(cls, data):
        """从字典创建实例"""
        instance = cls.__new__(cls)
        for key, value in data.items():
            setattr(instance, key, value)
        return instance

@Serializable
class Product:
    def __init__(self, name, price, category):
        self.name = name
        self.price = price
        self.category = category

print("\n序列化装饰器:")
product = Product("iPhone", 999, "Electronics")

# 序列化
product_dict = product.to_dict()
print(f"序列化结果: {product_dict}")

# 反序列化
restored_product = Product.from_dict(product_dict)
print(f"反序列化结果: {restored_product.name}, {restored_product.price}")

# 4. 缓存类装饰器
class CachedClass:
    """缓存类装饰器"""

    def __init__(self, cls):
        self.cls = cls
        self.cache = {}

    def __call__(self, *args, **kwargs):
        # 创建缓存键
        key = str(args) + str(sorted(kwargs.items()))

        if key not in self.cache:
            print(f"创建新实例: {self.cls.__name__}")
            self.cache[key] = self.cls(*args, **kwargs)
        else:
            print(f"从缓存返回实例: {self.cls.__name__}")

        return self.cache[key]

@CachedClass
class ExpensiveObject:
    def __init__(self, value):
        self.value = value
        print(f"耗时初始化: {value}")

print("\n缓存类装饰器:")
obj1 = ExpensiveObject(42)
obj2 = ExpensiveObject(42)  # 从缓存返回
obj3 = ExpensiveObject(100)  # 创建新实例

print(f"obj1 is obj2: {obj1 is obj2}")
print(f"obj1.value: {obj1.value}, obj2.value: {obj2.value}")

# 5. 事件系统装饰器
class EventEmitter:
    """事件发射器装饰器"""

    def __init__(self, cls):
        self.cls = cls

    def __call__(self, *args, **kwargs):
        instance = self.cls(*args, **kwargs)

        # 添加事件系统
        instance._events = {}
        instance.on = self._on
        instance.emit = self._emit
        instance.remove_listener = self._remove_listener

        return instance

    def _on(self, event, callback):
        """注册事件监听器"""
        if event not in self._events:
            self._events[event] = []
        self._events[event].append(callback)

    def _emit(self, event, *args, **kwargs):
        """触发事件"""
        if event in self._events:
            for callback in self._events[event]:
                callback(*args, **kwargs)

    def _remove_listener(self, event, callback):
        """移除事件监听器"""
        if event in self._events:
            self._events[event].remove(callback)

@EventEmitter
class Button:
    def __init__(self, label):
        self.label = label

    def click(self):
        print(f"按钮 '{self.label}' 被点击")
        self.emit('click', self.label)

print("\n事件系统装饰器:")
button = Button("提交")

# 注册事件监听器
button.on('click', lambda label: print(f"事件处理: {label} 按钮被点击"))
button.on('click', lambda label: print(f"日志: 用户点击了 {label}"))

# 触发事件
button.click()

# 6. 数据类装饰器
def dataclass(cls):
    """简单的数据类装饰器"""
    # 获取 __init__ 参数
    import inspect
    init_signature = inspect.signature(cls.__init__)
    params = list(init_signature.parameters.keys())[1:]  # 跳过 self

    # 创建 __repr__ 方法
    def __repr__(self):
        values = [f"{param}={getattr(self, param)}" for param in params]
        return f"{cls.__name__}({', '.join(values)})"

    # 创建 __eq__ 方法
    def __eq__(self, other):
        if not isinstance(other, cls):
            return False
        return all(getattr(self, param) == getattr(other, param) for param in params)

    # 添加方法
    cls.__repr__ = __repr__
    cls.__eq__ = __eq__

    return cls

@dataclass
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

@dataclass
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

print("\n数据类装饰器:")
p1 = Point(1, 2)
p2 = Point(1, 2)
p3 = Point(3, 4)

person = Person("Alice", 30)

print(f"点: {p1}")
print(f"相等: {p1 == p2}")
print(f"不相等: {p1 == p3}")
print(f"人: {person}")
```

## 23.3 内置装饰器

### 23.3.1 @staticmethod 和 @classmethod

```python
# 内置装饰器
print("内置装饰器:")

# 1. @staticmethod 和 @classmethod
class MathUtils:
    """数学工具类"""

    @staticmethod
    def add(a, b):
        """静态方法：不需要实例"""
        return a + b

    @staticmethod
    def multiply(a, b):
        """静态方法：不需要实例"""
        return a * b

    @classmethod
    def create_from_string(cls, expression):
        """类方法：可以访问类本身"""
        # 解析表达式如 "2+3"
        if '+' in expression:
            a, b = expression.split('+')
            return cls.add(int(a), int(b))
        elif '*' in expression:
            a, b = expression.split('*')
            return cls.multiply(int(a), int(b))
        else:
            raise ValueError("不支持的表达式")

    def instance_method(self):
        """实例方法：需要实例"""
        return "这是一个实例方法"

print("@staticmethod 和 @classmethod:")

# 调用静态方法
result1 = MathUtils.add(5, 3)
result2 = MathUtils.multiply(4, 7)
print(f"静态方法: 5+3={result1}, 4*7={result2}")

# 调用类方法
result3 = MathUtils.create_from_string("10+5")
result4 = MathUtils.create_from_string("6*8")
print(f"类方法: 10+5={result3}, 6*8={result4}")

# 实例方法需要实例
utils = MathUtils()
result5 = utils.instance_method()
print(f"实例方法: {result5}")

# 2. 工厂方法模式
class Shape:
    """形状基类"""

    def __init__(self, name):
        self.name = name

    def area(self):
        raise NotImplementedError

    @classmethod
    def create_circle(cls, radius):
        """创建圆形"""
        return Circle(radius)

    @classmethod
    def create_rectangle(cls, width, height):
        """创建矩形"""
        return Rectangle(width, height)

class Circle(Shape):
    def __init__(self, radius):
        super().__init__("Circle")
        self.radius = radius

    def area(self):
        import math
        return math.pi * self.radius ** 2

class Rectangle(Shape):
    def __init__(self, width, height):
        super().__init__("Rectangle")
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

print("\n工厂方法模式:")
circle = Shape.create_circle(5)
rectangle = Shape.create_rectangle(4, 6)

print(f"圆形面积: {circle.area():.2f}")
print(f"矩形面积: {rectangle.area()}")

# 3. 单例模式的类方法实现
class SingletonClass:
    """单例类"""

    _instance = None

    def __init__(self, value):
        self.value = value

    @classmethod
    def get_instance(cls, value=None):
        """获取单例实例"""
        if cls._instance is None:
            if value is None:
                raise ValueError("首次创建需要提供值")
            cls._instance = cls(value)
        return cls._instance

print("\n单例模式的类方法实现:")
singleton1 = SingletonClass.get_instance(42)
singleton2 = SingletonClass.get_instance()  # 返回同一个实例

print(f"单例1: {singleton1.value}")
print(f"单例2: {singleton2.value}")
print(f"是否同一个实例: {singleton1 is singleton2}")
```

### 23.3.2 @property 装饰器

```python
# @property 装饰器
print("@property 装饰器:")

# 1. 基本属性
class Person:
    """人"""

    def __init__(self, first_name, last_name):
        self.first_name = first_name
        self.last_name = last_name

    @property
    def full_name(self):
        """完整姓名"""
        return f"{self.first_name} {self.last_name}"

    @property
    def initials(self):
        """姓名首字母"""
        return f"{self.first_name[0]}{self.last_name[0]}"

print("基本属性:")
person = Person("John", "Doe")
print(f"姓名: {person.full_name}")
print(f"首字母: {person.initials}")

# 2. 可设置的属性
class Temperature:
    """温度类"""

    def __init__(self, celsius=0):
        self._celsius = celsius

    @property
    def celsius(self):
        """摄氏温度"""
        return self._celsius

    @celsius.setter
    def celsius(self, value):
        """设置摄氏温度"""
        if value < -273.15:
            raise ValueError("温度不能低于绝对零度")
        self._celsius = value

    @property
    def fahrenheit(self):
        """华氏温度"""
        return self._celsius * 9/5 + 32

    @fahrenheit.setter
    def fahrenheit(self, value):
        """设置华氏温度"""
        self._celsius = (value - 32) * 5/9

print("\n可设置的属性:")
temp = Temperature(20)
print(f"摄氏度: {temp.celsius}°C")
print(f"华氏度: {temp.fahrenheit}°F")

# 修改温度
temp.celsius = 30
print(f"修改后摄氏度: {temp.celsius}°C")
print(f"对应的华氏度: {temp.fahrenheit}°F")

temp.fahrenheit = 68
print(f"修改后华氏度: {temp.fahrenheit}°F")
print(f"对应的摄氏度: {temp.celsius}°C")

# 3. 只读属性
class Circle:
    """圆形"""

    def __init__(self, radius):
        self._radius = radius

    @property
    def radius(self):
        """半径"""
        return self._radius

    @property
    def diameter(self):
        """直径"""
        return self._radius * 2

    @property
    def area(self):
        """面积"""
        import math
        return math.pi * self._radius ** 2

    @property
    def circumference(self):
        """周长"""
        import math
        return 2 * math.pi * self._radius

print("\n只读属性:")
circle = Circle(5)
print(f"半径: {circle.radius}")
print(f"直径: {circle.diameter}")
print(f"面积: {circle.area:.2f}")
print(f"周长: {circle.circumference:.2f}")

# 4. 删除器
class BankAccount:
    """银行账户"""

    def __init__(self, account_number, balance=0):
        self.account_number = account_number
        self._balance = balance

    @property
    def balance(self):
        """账户余额"""
        return self._balance

    @balance.setter
    def balance(self, amount):
        """设置余额"""
        if amount < 0:
            raise ValueError("余额不能为负数")
        self._balance = amount

    @balance.deleter
    def balance(self):
        """删除余额属性"""
        print(f"删除账户 {self.account_number} 的余额")
        del self._balance

print("\n删除器:")
account = BankAccount("123456", 1000)
print(f"账户余额: {account.balance}")

account.balance = 1500
print(f"修改后余额: {account.balance}")

del account.balance  # 触发删除器
```

### 23.3.3 其他内置装饰器

```python
# 其他内置装饰器
print("其他内置装饰器:")

# 1. @functools.lru_cache
import functools

@functools.lru_cache(maxsize=128)
def fibonacci(n):
    """带缓存的斐波那契数"""
    print(f"计算 fibonacci({n})")
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

print("@functools.lru_cache:")
print(f"fibonacci(10) = {fibonacci(10)}")
print(f"再次调用 fibonacci(10) = {fibonacci(10)}")  # 从缓存获取

# 查看缓存信息
print(f"缓存信息: {fibonacci.cache_info()}")

# 2. @functools.singledispatch
@functools.singledispatch
def process_data(data):
    """处理不同类型的数据"""
    return f"未知类型: {type(data)}"

@process_data.register
def _(data: str):
    return f"字符串: {data.upper()}"

@process_data.register
def _(data: int):
    return f"整数: {data * 2}"

@process_data.register
def _(data: list):
    return f"列表长度: {len(data)}"

print("\n@functools.singledispatch:")
test_data = ["hello", 42, [1, 2, 3], 3.14]
for data in test_data:
    result = process_data(data)
    print(f"  {data} -> {result}")

# 3. @functools.total_ordering
@functools.total_ordering
class Version:
    """版本号"""

    def __init__(self, version):
        self.parts = [int(x) for x in version.split('.')]

    def __eq__(self, other):
        if not isinstance(other, Version):
            return NotImplemented
        return self.parts == other.parts

    def __lt__(self, other):
        if not isinstance(other, Version):
            return NotImplemented
        return self.parts < other.parts

    def __repr__(self):
        return '.'.join(str(p) for p in self.parts)

print("\n@functools.total_ordering:")
versions = [
    Version("1.0.0"),
    Version("1.10.0"),
    Version("1.2.0"),
    Version("2.0.0")
]

print("排序前:", versions)
versions.sort()
print("排序后:", versions)

# 测试所有比较操作
v1 = Version("1.2.3")
v2 = Version("1.2.4")

print(f"v1 == v2: {v1 == v2}")
print(f"v1 < v2: {v1 < v2}")
print(f"v1 <= v2: {v1 <= v2}")
print(f"v1 > v2: {v1 > v2}")
print(f"v1 >= v2: {v1 >= v2}")

# 4. @dataclasses.dataclass
from dataclasses import dataclass, field

@dataclass
class Employee:
    """员工"""
    name: str
    age: int
    salary: float
    skills: list = field(default_factory=list)

@dataclass
class Department:
    """部门"""
    name: str
    manager: Employee
    employees: list = field(default_factory=list)

print("\n@dataclasses.dataclass:")
emp1 = Employee("Alice", 30, 50000, ["Python", "Java"])
emp2 = Employee("Bob", 25, 45000)

dept = Department("Engineering", emp1, [emp1, emp2])

print(f"员工1: {emp1}")
print(f"员工2: {emp2}")
print(f"部门: {dept}")
print(f"相等: {emp1 == emp2}")

# 5. @typing.final (Python 3.8+)
try:
    from typing import final

    class BaseClass:
        @final
        def final_method(self):
            """这个方法不能被子类重写"""
            return "final method"

    class SubClass(BaseClass):
        # 这会触发类型检查器的警告
        # def final_method(self):
        #     return "overridden"

    print("\n@typing.final:")
    base = BaseClass()
    print(f"基类方法: {base.final_method()}")

except ImportError:
    print("\n@typing.final 需要 Python 3.8+")

# 6. @contextlib.contextmanager
from contextlib import contextmanager

@contextmanager
def database_connection(host, port):
    """数据库连接上下文管理器"""
    print(f"连接到数据库 {host}:{port}")
    connection = f"Connection to {host}:{port}"
    try:
        yield connection
    finally:
        print(f"断开数据库连接 {host}:{port}")

print("\n@contextlib.contextmanager:")
with database_connection("localhost", 5432) as conn:
    print(f"使用连接: {conn}")
    print("执行数据库操作...")

print("上下文管理器已退出")
```

## 总结

本章介绍了 Python 的装饰器：
- 掌握函数装饰器的基本概念、语法和执行时机
- 学会创建带参数的装饰器和装饰器工厂
- 理解类装饰器的实现方式和应用场景
- 熟练使用内置装饰器 @staticmethod、@classmethod 和 @property
- 掌握 functools 模块提供的装饰器工具

通过本章的学习，你应该能够：
1. 编写简单的函数装饰器来修改函数行为
2. 创建复杂的装饰器工厂来处理不同场景
3. 使用类装饰器来修改类的创建过程
4. 正确使用 @property 来创建受控属性
5. 利用内置装饰器来实现常见的设计模式
6. 编写健壮的装饰器代码，包括错误处理和元数据保留

下一章将介绍生成器和迭代器。
