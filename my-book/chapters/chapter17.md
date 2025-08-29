# 第17章：异常处理

## 17.1 异常的基本概念

异常是程序运行时发生的错误，Python 使用异常来处理这些错误情况。

### 17.1.1 什么是异常

```python
# 异常示例：除零错误
def divide(a, b):
    return a / b

# 这会引发异常
try:
    result = divide(10, 0)
except ZeroDivisionError as e:
    print(f"错误: {e}")

# 异常的类型
print("内置异常类型:")
print(f"ZeroDivisionError: {ZeroDivisionError}")
print(f"ValueError: {ValueError}")
print(f"TypeError: {TypeError}")
print(f"FileNotFoundError: {FileNotFoundError}")
print(f"KeyError: {KeyError}")
print(f"IndexError: {IndexError}")

# 异常层次结构
print("\n异常层次结构:")
print("BaseException")
print("├── Exception")
print("│   ├── ArithmeticError")
print("│   │   ├── ZeroDivisionError")
print("│   │   └── OverflowError")
print("│   ├── LookupError")
print("│   │   ├── IndexError")
print("│   │   └── KeyError")
print("│   ├── ValueError")
print("│   ├── TypeError")
print("│   └── FileNotFoundError")
print("├── KeyboardInterrupt")
print("└── SystemExit")
```

### 17.1.2 异常 vs 错误

```python
# 语法错误（SyntaxError）- 编译时错误
# def function(  # SyntaxError: invalid syntax

# 运行时错误 - 异常
def safe_divide(a, b):
    """安全的除法运算"""
    try:
        return a / b
    except ZeroDivisionError:
        print("除数不能为零！")
        return None

# 使用示例
print(safe_divide(10, 2))   # 5.0
print(safe_divide(10, 0))   # 除数不能为零！ None

# 异常可以被处理，程序可以继续运行
# 错误通常会导致程序终止
```

### 17.1.3 异常的传播

```python
def function1():
    """函数1"""
    return 1 / 0  # ZeroDivisionError

def function2():
    """函数2"""
    return function1() * 2

def function3():
    """函数3"""
    return function2() + 1

# 异常会沿着调用栈向上传播
try:
    result = function3()
except ZeroDivisionError as e:
    print(f"捕获到异常: {e}")
    print("异常类型:", type(e).__name__)
    import traceback
    print("调用栈:")
    traceback.print_exc()

# 输出：
# 捕获到异常: division by zero
# 异常类型: ZeroDivisionError
# 调用栈:
# Traceback (most recent call last):
#   File "example.py", line XX, in <module>
#     result = function3()
#   File "example.py", line XX, in function3
#     return function2() + 1
#   File "example.py", line XX, in function2
#     return function1() * 2
#   File "example.py", line XX, in function1
#     return 1 / 0
# ZeroDivisionError: division by zero
```

## 17.2 异常的类型

### 17.2.1 内置异常类

```python
# 常见的内置异常

# 算术错误
try:
    result = 1 / 0
except ZeroDivisionError:
    print("ZeroDivisionError: 除零错误")

try:
    result = 10 ** 1000  # 非常大的数
except OverflowError:
    print("OverflowError: 数值溢出")

# 类型错误
try:
    result = "hello" + 123
except TypeError:
    print("TypeError: 类型不匹配")

# 值错误
try:
    result = int("not_a_number")
except ValueError:
    print("ValueError: 无效的值")

# 索引错误
try:
    my_list = [1, 2, 3]
    result = my_list[10]
except IndexError:
    print("IndexError: 索引超出范围")

# 键错误
try:
    my_dict = {"a": 1, "b": 2}
    result = my_dict["c"]
except KeyError:
    print("KeyError: 键不存在")

# 文件错误
try:
    with open("nonexistent_file.txt", "r") as f:
        content = f.read()
except FileNotFoundError:
    print("FileNotFoundError: 文件不存在")

# 属性错误
class MyClass:
    pass

try:
    obj = MyClass()
    result = obj.nonexistent_attr
except AttributeError:
    print("AttributeError: 属性不存在")
```

### 17.2.2 自定义异常类

```python
# 自定义异常类
class CustomError(Exception):
    """自定义异常基类"""

    def __init__(self, message="发生自定义错误"):
        self.message = message
        super().__init__(self.message)

class ValidationError(CustomError):
    """验证错误"""

    def __init__(self, field, value, expected_type):
        self.field = field
        self.value = value
        self.expected_type = expected_type
        message = f"字段 '{field}' 的值 {value} 不是期望的类型 {expected_type}"
        super().__init__(message)

class DatabaseError(CustomError):
    """数据库错误"""

    def __init__(self, operation, table, error_details):
        self.operation = operation
        self.table = table
        self.error_details = error_details
        message = f"数据库操作 '{operation}' 在表 '{table}' 失败: {error_details}"
        super().__init__(message)

class NetworkError(CustomError):
    """网络错误"""

    def __init__(self, url, status_code, response=None):
        self.url = url
        self.status_code = status_code
        self.response = response
        message = f"网络请求失败: {url} (状态码: {status_code})"
        super().__init__(message)

# 使用自定义异常
def validate_user_data(user_data):
    """验证用户数据"""
    if not isinstance(user_data.get("name"), str):
        raise ValidationError("name", user_data.get("name"), "str")

    if not isinstance(user_data.get("age"), int):
        raise ValidationError("age", user_data.get("age"), "int")

    if user_data.get("age", 0) < 0:
        raise ValidationError("age", user_data.get("age"), "正整数")

def save_to_database(data):
    """模拟数据库保存"""
    # 模拟数据库连接失败
    raise DatabaseError("INSERT", "users", "连接超时")

def fetch_webpage(url):
    """模拟网页获取"""
    # 模拟网络错误
    raise NetworkError(url, 404, "页面不存在")

# 使用示例
try:
    # 验证数据
    user_data = {"name": "Alice", "age": "25"}  # age 应该是 int
    validate_user_data(user_data)

    # 保存到数据库
    save_to_database(user_data)

except ValidationError as e:
    print(f"验证错误: {e}")
    print(f"字段: {e.field}, 值: {e.value}, 期望类型: {e.expected_type}")

except DatabaseError as e:
    print(f"数据库错误: {e}")
    print(f"操作: {e.operation}, 表: {e.table}")

except NetworkError as e:
    print(f"网络错误: {e}")
    print(f"URL: {e.url}, 状态码: {e.status_code}")
```

## 17.3 try-except 语句

### 17.3.1 基本语法

```python
# try-except 基本语法
try:
    # 可能引发异常的代码
    risky_operation()
except ExceptionType:
    # 处理特定异常的代码
    handle_exception()
except AnotherExceptionType as e:
    # 处理另一种异常，并获取异常对象
    handle_another_exception(e)
except:
    # 处理所有其他异常（不推荐）
    handle_generic_exception()
else:
    # 如果没有异常执行的代码
    no_exception_occurred()
finally:
    # 无论是否发生异常都会执行的代码
    cleanup_code()
```

### 17.3.2 捕获特定异常

```python
def safe_operations():
    """安全的操作示例"""

    # 示例1: 捕获单个异常
    try:
        number = int(input("请输入一个数字: "))
        result = 100 / number
        print(f"结果: {result}")
    except ValueError:
        print("错误: 请输入有效的数字")
    except ZeroDivisionError:
        print("错误: 不能除以零")

    # 示例2: 捕获多个异常
    try:
        my_list = [1, 2, 3]
        index = int(input("请输入索引: "))
        print(f"元素: {my_list[index]}")
    except (ValueError, IndexError) as e:
        print(f"错误: {e}")

    # 示例3: 捕获所有异常（不推荐用于生产代码）
    try:
        # 一些复杂的操作
        import json
        data = json.loads("invalid json")
    except Exception as e:
        print(f"发生异常: {type(e).__name__}: {e}")

    # 示例4: 重新引发异常
    try:
        file = open("important_file.txt", "r")
        content = file.read()
    except FileNotFoundError:
        print("文件不存在，创建新文件")
        file = open("important_file.txt", "w")
        file.write("默认内容")
    except Exception as e:
        print(f"其他错误: {e}")
        raise  # 重新引发异常
    finally:
        if 'file' in locals():
            file.close()
```

### 17.3.3 异常对象的属性

```python
# 异常对象的属性和方法
try:
    # 引发一个异常
    raise ValueError("这是一个测试错误")
except ValueError as e:
    print(f"异常类型: {type(e)}")
    print(f"异常类名: {type(e).__name__}")
    print(f"异常消息: {e}")
    print(f"异常参数: {e.args}")

    # 检查异常类型
    print(f"是 ValueError: {isinstance(e, ValueError)}")
    print(f"是 Exception: {isinstance(e, Exception)}")
    print(f"是 BaseException: {isinstance(e, BaseException)}")

# 自定义异常的属性
class DetailedError(Exception):
    """详细错误信息"""

    def __init__(self, message, error_code, details=None):
        super().__init__(message)
        self.error_code = error_code
        self.details = details or {}

try:
    raise DetailedError("操作失败", 1001, {"user": "alice", "action": "login"})
except DetailedError as e:
    print(f"错误消息: {e}")
    print(f"错误代码: {e.error_code}")
    print(f"详细信息: {e.details}")
```

## 17.4 finally 子句

### 17.4.1 finally 的作用

```python
# finally 子句总是会执行
def process_file(filename):
    """处理文件"""

    file = None
    try:
        file = open(filename, "r")
        content = file.read()
        print(f"文件内容: {content}")

        # 模拟处理
        if len(content) == 0:
            raise ValueError("文件为空")

        # 正常处理
        processed_content = content.upper()
        return processed_content

    except FileNotFoundError:
        print(f"文件 {filename} 不存在")
        return None

    except ValueError as e:
        print(f"处理错误: {e}")
        return None

    except Exception as e:
        print(f"未知错误: {e}")
        return None

    finally:
        # 无论发生什么，都会执行清理代码
        if file:
            file.close()
            print(f"文件 {filename} 已关闭")

# 测试 finally
print("测试1: 正常情况")
result1 = process_file("existing_file.txt")

print("\n测试2: 文件不存在")
result2 = process_file("nonexistent_file.txt")

print("\n测试3: 处理错误")
# 创建一个空文件进行测试
with open("empty_file.txt", "w") as f:
    pass
result3 = process_file("empty_file.txt")
```

### 17.4.2 finally vs return

```python
def test_finally_return():
    """测试 finally 和 return 的交互"""

    try:
        print("try 块")
        return "从 try 返回"
    except:
        print("except 块")
        return "从 except 返回"
    finally:
        print("finally 块总是执行")
        # finally 中的 return 会覆盖之前的 return
        # return "从 finally 返回"

# 测试
result = test_finally_return()
print(f"函数返回: {result}")

def test_finally_exception():
    """测试 finally 和异常的交互"""

    try:
        print("try 块")
        raise ValueError("测试异常")
    except ValueError:
        print("except 块")
        raise  # 重新引发异常
    finally:
        print("finally 块总是执行，即使有异常")

# 测试
try:
    test_finally_exception()
except ValueError:
    print("在调用处捕获异常")
```

## 17.5 else 子句

### 17.5.1 else 的使用场景

```python
# else 子句在没有异常时执行
def safe_divide(a, b):
    """安全的除法"""

    try:
        result = a / b
    except ZeroDivisionError:
        print("错误: 除数不能为零")
        return None
    except TypeError:
        print("错误: 参数类型不正确")
        return None
    else:
        # 只有在没有异常时才执行
        print(f"计算成功: {a} / {b} = {result}")
        return result
    finally:
        # 总是执行
        print("清理工作完成")

# 测试
print("测试1: 正常情况")
safe_divide(10, 2)

print("\n测试2: 除零错误")
safe_divide(10, 0)

print("\n测试3: 类型错误")
safe_divide("10", 2)

# else 子句的另一个用途：只在没有异常时执行的代码
def read_and_process_file(filename):
    """读取并处理文件"""

    try:
        with open(filename, "r") as file:
            content = file.read()
    except FileNotFoundError:
        print(f"文件 {filename} 不存在")
        return None
    else:
        # 文件成功读取后处理
        lines = content.split('\n')
        word_count = sum(len(line.split()) for line in lines)
        print(f"文件 {filename}: {len(lines)} 行, {word_count} 个单词")
        return content
    finally:
        print("文件操作完成")

# 测试
read_and_process_file("existing_file.txt")
read_and_process_file("nonexistent_file.txt")
```

### 17.5.2 else vs 正常代码

```python
# else 子句 vs 将代码放在 try 块外

# 方式1: 使用 else
def method1():
    try:
        risky_operation()
    except SomeException:
        handle_exception()
    else:
        # 只有在没有异常时执行
        success_code()

# 方式2: 将成功代码放在外面
def method2():
    try:
        risky_operation()
        success_code()  # 这也会在异常时执行！
    except SomeException:
        handle_exception()

# 正确的比较
def process_data_v1(data):
    """版本1: 使用 else"""
    try:
        parsed = json.loads(data)
    except json.JSONDecodeError:
        print("JSON 解析失败")
        return None
    else:
        # 只有解析成功才处理
        return process_parsed_data(parsed)

def process_data_v2(data):
    """版本2: 错误的方式"""
    try:
        parsed = json.loads(data)
        return process_parsed_data(parsed)  # 如果解析失败，这行不会执行，但逻辑不清晰
    except json.JSONDecodeError:
        print("JSON 解析失败")
        return None

# else 让意图更清晰：这部分代码只有在 try 成功时才执行
```

## 17.6 自定义异常

### 17.6.1 创建自定义异常类

```python
# 自定义异常的最佳实践

class ApplicationError(Exception):
    """应用程序错误基类"""

    def __init__(self, message, error_code=None, details=None):
        super().__init__(message)
        self.error_code = error_code
        self.details = details or {}
        self.timestamp = datetime.now()

    def to_dict(self):
        """转换为字典格式"""
        return {
            "error_type": self.__class__.__name__,
            "message": str(self),
            "error_code": self.error_code,
            "details": self.details,
            "timestamp": self.timestamp.isoformat()
        }

class ValidationError(ApplicationError):
    """验证错误"""

    def __init__(self, field, value, constraint, details=None):
        message = f"字段 '{field}' 的值 '{value}' 违反约束 '{constraint}'"
        super().__init__(message, error_code="VALIDATION_ERROR", details=details)
        self.field = field
        self.value = value
        self.constraint = constraint

class BusinessLogicError(ApplicationError):
    """业务逻辑错误"""

    def __init__(self, operation, reason, details=None):
        message = f"业务操作 '{operation}' 失败: {reason}"
        super().__init__(message, error_code="BUSINESS_ERROR", details=details)
        self.operation = operation
        self.reason = reason

class SystemError(ApplicationError):
    """系统错误"""

    def __init__(self, component, operation, system_message, details=None):
        message = f"系统组件 '{component}' 执行 '{operation}' 时出错: {system_message}"
        super().__init__(message, error_code="SYSTEM_ERROR", details=details)
        self.component = component
        self.operation = operation
        self.system_message = system_message

# 使用示例
def validate_user(user_data):
    """验证用户数据"""

    if not user_data.get("name"):
        raise ValidationError("name", user_data.get("name"), "required")

    if len(user_data.get("name", "")) < 2:
        raise ValidationError("name", user_data.get("name"), "min_length_2")

    age = user_data.get("age")
    if age is not None and (age < 0 or age > 150):
        raise ValidationError("age", age, "range_0_150")

def process_business_logic(user_data):
    """处理业务逻辑"""

    if user_data.get("status") == "banned":
        raise BusinessLogicError("user_registration", "用户已被禁止注册")

    if user_data.get("credits", 0) < 0:
        raise BusinessLogicError("credit_check", "用户积分不足")

def system_operation():
    """系统操作"""

    try:
        # 模拟系统调用
        import os
        result = os.system("nonexistent_command")
        if result != 0:
            raise SystemError("os", "system_call", "命令执行失败")
    except OSError as e:
        raise SystemError("os", "system_call", str(e))

# 统一错误处理
def handle_errors(func):
    """错误处理装饰器"""

    def wrapper(*args, **kwargs):
        try:
            return func(*args, **kwargs)
        except ValidationError as e:
            print(f"验证错误 [{e.error_code}]: {e}")
            print(f"字段: {e.field}, 值: {e.value}, 约束: {e.constraint}")
        except BusinessLogicError as e:
            print(f"业务错误 [{e.error_code}]: {e}")
            print(f"操作: {e.operation}, 原因: {e.reason}")
        except SystemError as e:
            print(f"系统错误 [{e.error_code}]: {e}")
            print(f"组件: {e.component}, 操作: {e.operation}")
        except Exception as e:
            print(f"未知错误: {type(e).__name__}: {e}")
        finally:
            print("错误处理完成")

    return wrapper

@handle_errors
def register_user(user_data):
    """注册用户"""

    validate_user(user_data)
    process_business_logic(user_data)
    system_operation()

    return "用户注册成功"

# 测试
user_data = {"name": "", "age": -5, "status": "banned"}
register_user(user_data)
```

### 17.6.2 异常层次结构设计

```python
# 设计良好的异常层次结构

class MyAppError(Exception):
    """应用程序错误基类"""
    pass

class DataError(MyAppError):
    """数据相关错误"""
    pass

class ValidationError(DataError):
    """验证错误"""
    pass

class ParsingError(DataError):
    """解析错误"""
    pass

class NetworkError(MyAppError):
    """网络相关错误"""
    pass

class ConnectionError(NetworkError):
    """连接错误"""
    pass

class TimeoutError(NetworkError):
    """超时错误"""
    pass

class APIError(NetworkError):
    """API 错误"""

    def __init__(self, message, status_code, response=None):
        super().__init__(message)
        self.status_code = status_code
        self.response = response

# 使用层次结构
def handle_request(url):
    """处理网络请求"""

    try:
        # 模拟不同的网络错误
        if "timeout" in url:
            raise TimeoutError("请求超时")
        elif "connection" in url:
            raise ConnectionError("连接失败")
        elif "api" in url:
            raise APIError("API 调用失败", 500)
        else:
            return "请求成功"
    except ConnectionError:
        return "请检查网络连接"
    except TimeoutError:
        return "请求超时，请重试"
    except APIError as e:
        return f"API 错误 (状态码: {e.status_code})"
    except NetworkError:
        return "网络错误"
    except MyAppError:
        return "应用程序错误"

# 测试
print(handle_request("http://example.com"))      # 请求成功
print(handle_request("http://timeout.com"))      # 请求超时，请重试
print(handle_request("http://connection.com"))   # 请检查网络连接
print(handle_request("http://api.com"))          # API 错误 (状态码: 500)
```

## 17.7 异常链和回溯

### 17.7.1 异常链

```python
# 异常链：一个异常引发另一个异常

def process_data(data):
    """处理数据"""

    try:
        # 尝试解析 JSON
        import json
        parsed = json.loads(data)
        return parsed
    except json.JSONDecodeError as e:
        # 原始异常被链式保存
        raise ValueError("数据格式无效") from e

def save_to_database(data):
    """保存到数据库"""

    try:
        processed = process_data(data)
        # 模拟数据库保存
        if "error" in processed:
            raise RuntimeError("数据库保存失败")
        return "保存成功"
    except Exception as e:
        # 重新引发，保持异常链
        raise RuntimeError("数据保存过程失败") from e

# 测试异常链
try:
    result = save_to_database('{"name": "Alice", "error": true}')
except Exception as e:
    print(f"最终异常: {e}")
    print(f"异常链: {e.__cause__}")
    if e.__cause__:
        print(f"原因: {e.__cause__}")

# 使用 raise ... from None 打破异常链
def sanitize_error():
    """清理错误信息"""

    try:
        risky_operation()
    except Exception as e:
        # 不暴露内部实现细节
        raise ValueError("操作失败，请联系管理员") from None

try:
    sanitize_error()
except ValueError as e:
    print(f"用户看到的错误: {e}")
    print(f"异常链: {e.__cause__}")  # None
```

### 17.7.2 回溯信息

```python
import traceback
import sys

def function1():
    function2()

def function2():
    function3()

def function3():
    raise ValueError("这是一个测试错误")

# 获取回溯信息
try:
    function1()
except Exception as e:
    print("异常信息:")
    print(f"类型: {type(e).__name__}")
    print(f"消息: {e}")

    print("\n完整回溯:")
    traceback.print_exc()

    print("\n回溯为字符串:")
    tb_str = traceback.format_exc()
    print(tb_str)

    print("\n回溯对象:")
    tb = sys.exc_info()[2]
    for frame_info in traceback.extract_tb(tb):
        print(f"文件: {frame_info.filename}")
        print(f"行号: {frame_info.lineno}")
        print(f"函数: {frame_info.name}")
        print(f"代码: {frame_info.line}")
        print("---")

# 自定义回溯格式
def custom_traceback_handler(exc_type, exc_value, exc_traceback):
    """自定义异常处理器"""

    print("=== 自定义错误报告 ===")
    print(f"异常类型: {exc_type.__name__}")
    print(f"异常消息: {exc_value}")

    print("\n调用栈:")
    for frame_info in traceback.extract_tb(exc_traceback):
        print(f"  {frame_info.filename}:{frame_info.lineno} 在 {frame_info.name}()")
        if frame_info.line:
            print(f"    {frame_info.line}")

    print("=" * 30)

# 设置自定义异常处理器
sys.excepthook = custom_traceback_handler

# 测试
raise RuntimeError("测试自定义处理器")
```

## 17.8 异常处理的最佳实践

### 17.8.1 何时使用异常

```python
# 好的异常使用场景

# 1. 错误情况
def divide(a, b):
    """正确的异常使用"""
    if b == 0:
        raise ValueError("除数不能为零")
    return a / b

# 2. 资源管理
class DatabaseConnection:
    def __enter__(self):
        print("连接数据库")
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("断开数据库连接")

    def query(self, sql):
        if "error" in sql:
            raise RuntimeError("查询失败")
        return "查询结果"

# 3. 验证和约束
def create_user(name, email):
    if not name:
        raise ValueError("用户名不能为空")
    if "@" not in email:
        raise ValueError("邮箱格式无效")

    return {"name": name, "email": email}

# 不好的异常使用场景

# 1. 控制流
# 不要这样做
def find_item(items, target):
    for item in items:
        if item == target:
            return item
    raise ValueError("Item not found")  # 不好的做法

# 应该这样做
def find_item_better(items, target):
    for item in items:
        if item == target:
            return item
    return None  # 返回 None 或使用其他方式

# 2. 忽略异常
# 不要这样做
try:
    risky_operation()
except:
    pass  # 忽略所有异常

# 应该这样做
try:
    risky_operation()
except SpecificException as e:
    logger.warning(f"操作失败: {e}")
    # 处理异常
```

### 17.8.2 异常处理原则

```python
# 1. 具体异常优于通用异常
# 推荐
try:
    file = open("data.txt", "r")
except FileNotFoundError:
    print("文件不存在")
except PermissionError:
    print("权限不足")

# 不推荐
try:
    file = open("data.txt", "r")
except Exception:
    print("发生错误")

# 2. 保持异常信息的完整性
def process_file(filename):
    try:
        with open(filename, "r") as f:
            return f.read()
    except FileNotFoundError as e:
        # 添加上下文信息，但保留原始异常
        raise FileNotFoundError(f"配置文件 '{filename}' 不存在") from e

# 3. 使用 finally 进行清理
def safe_file_operation(filename):
    file = None
    try:
        file = open(filename, "w")
        file.write("重要数据")
        return "写入成功"
    finally:
        if file:
            file.close()

# 4. 避免过度使用异常
class Result:
    """结果类，避免使用异常进行控制流"""

    def __init__(self, success, value=None, error=None):
        self.success = success
        self.value = value
        self.error = error

def divide_safe(a, b):
    """安全的除法，返回 Result 对象"""
    if b == 0:
        return Result(False, error="除数不能为零")
    return Result(True, value=a / b)

# 使用
result = divide_safe(10, 0)
if result.success:
    print(f"结果: {result.value}")
else:
    print(f"错误: {result.error}")
```

### 17.8.3 日志和监控

```python
import logging
import sys
from datetime import datetime

# 配置日志
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('app.log'),
        logging.StreamHandler(sys.stdout)
    ]
)

logger = logging.getLogger(__name__)

class ErrorReporter:
    """错误报告器"""

    @staticmethod
    def report_error(error, context=None):
        """报告错误"""
        error_info = {
            "timestamp": datetime.now().isoformat(),
            "error_type": type(error).__name__,
            "error_message": str(error),
            "context": context or {},
            "traceback": traceback.format_exc()
        }

        # 记录到日志
        logger.error(f"错误发生: {error_info['error_type']}: {error_info['error_message']}")

        # 可以发送到监控系统
        # send_to_monitoring(error_info)

        return error_info

def robust_function():
    """健壮的函数"""

    try:
        # 模拟可能失败的操作
        result = risky_operation()
        logger.info("操作成功完成")
        return result

    except ValueError as e:
        error_info = ErrorReporter.report_error(e, {"operation": "value_validation"})
        return None

    except ConnectionError as e:
        error_info = ErrorReporter.report_error(e, {"operation": "network_request"})
        # 可以重试
        return retry_operation()

    except Exception as e:
        error_info = ErrorReporter.report_error(e, {"operation": "unknown"})
        # 对于未知错误，重新引发
        raise

def risky_operation():
    """模拟有风险的操作"""
    import random
    if random.choice([True, False]):
        raise ValueError("随机错误")
    return "操作成功"

def retry_operation():
    """重试操作"""
    logger.info("重试操作")
    return "重试成功"

# 测试
try:
    result = robust_function()
    print(f"最终结果: {result}")
except Exception as e:
    print(f"未处理的异常: {e}")
```

## 总结

本章介绍了异常处理的核心概念：
- 理解异常的基本概念和类型
- 掌握 try-except-else-finally 的使用
- 学习自定义异常类的创建
- 了解异常链和回溯信息
- 掌握异常处理的最佳实践

通过本章的学习，你应该能够：
1. 正确处理各种异常情况
2. 创建有意义的自定义异常
3. 使用适当的异常处理结构
4. 编写健壮的错误处理代码
5. 理解异常处理的设计原则

下一章将介绍文件操作的基础知识。
