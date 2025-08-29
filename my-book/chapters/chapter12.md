# 第12章：模块和包

## 12.1 模块的基本概念

模块是 Python 中组织代码的基本单位，它可以将相关的代码组织在一起，便于管理和重用。

### 12.1.1 什么是模块

```python
# my_module.py - 一个简单的模块示例
"""
这是一个示例模块，演示模块的基本结构和功能。
"""

# 模块级变量
VERSION = "1.0.0"
AUTHOR = "Python教程"

# 模块级函数
def greet(name):
    """问候函数"""
    return f"Hello, {name}! Welcome to {__name__}"

def get_version():
    """获取版本信息"""
    return VERSION

# 模块级类
class Calculator:
    """简单的计算器类"""

    def add(self, a, b):
        return a + b

    def multiply(self, a, b):
        return a * b

# 模块初始化代码
print(f"模块 {__name__} 已加载")

# 只有在直接运行模块时才会执行的代码
if __name__ == "__main__":
    print("这是模块的测试代码")
    calc = Calculator()
    print(f"2 + 3 = {calc.add(2, 3)}")
    print(f"4 * 5 = {calc.multiply(4, 5)}")
```

### 12.1.2 模块的命名空间

```python
# 每个模块都有自己的命名空间
import math
import random

# 访问模块中的函数
print(math.pi)        # 3.141592653589793
print(math.sqrt(16))  # 4.0

print(random.random())  # 随机数
print(random.randint(1, 10))  # 1-10之间的随机整数

# 查看模块的命名空间
print(dir(math)[:5])  # ['__doc__', '__file__', '__name__', 'acos', 'acosh']
```

### 12.1.3 模块的搜索路径

```python
import sys

# 查看模块搜索路径
print("模块搜索路径:")
for i, path in enumerate(sys.path):
    print(f"{i}: {path}")

# 添加自定义路径
sys.path.append("/path/to/your/modules")

# 查看当前模块的文件位置
import os
print(f"当前文件位置: {__file__}")
print(f"当前工作目录: {os.getcwd()}")

# 模块的 __file__ 属性
import math
print(f"math 模块文件: {math.__file__}")

# 内置模块没有 __file__ 属性
import builtins
print(f"builtins 模块文件: {getattr(builtins, '__file__', '内置模块')}")
```

## 12.2 创建和导入模块

### 12.2.1 创建模块

```python
# 创建一个数学工具模块 math_tools.py
"""
数学工具模块
提供常用的数学计算功能
"""

def factorial(n):
    """计算阶乘"""
    if n == 0 or n == 1:
        return 1
    return n * factorial(n - 1)

def fibonacci(n):
    """计算斐波那契数列"""
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

def is_prime(n):
    """检查是否为质数"""
    if n < 2:
        return False
    for i in range(2, int(n ** 0.5) + 1):
        if n % i == 0:
            return False
    return True

def gcd(a, b):
    """计算最大公约数"""
    while b:
        a, b = b, a % b
    return a

def lcm(a, b):
    """计算最小公倍数"""
    return abs(a * b) // gcd(a, b)

# 常量
PI = 3.141592653589793
E = 2.718281828459045

if __name__ == "__main__":
    # 测试代码
    print(f"5! = {factorial(5)}")
    print(f"斐波那契数列第6项: {fibonacci(6)}")
    print(f"17是质数: {is_prime(17)}")
    print(f"12和18的最大公约数: {gcd(12, 18)}")
    print(f"12和18的最小公倍数: {lcm(12, 18)}")
```

### 12.2.2 导入模块的不同方式

```python
# 方式1: 导入整个模块
import math_tools

result = math_tools.factorial(5)
print(result)  # 120

# 方式2: 从模块导入特定函数
from math_tools import factorial, is_prime

result = factorial(4)
print(result)  # 24

print(is_prime(7))  # True

# 方式3: 导入所有内容（不推荐）
from math_tools import *

result = gcd(12, 18)
print(result)  # 6

# 方式4: 给模块或函数起别名
import math_tools as mt
from math_tools import factorial as fact

result = mt.fibonacci(5)
print(result)  # 5

result = fact(3)
print(result)  # 6
```

### 12.2.3 动态导入模块

```python
import importlib
import sys

def dynamic_import(module_name):
    """动态导入模块"""
    try:
        module = importlib.import_module(module_name)
        print(f"成功导入模块: {module_name}")
        return module
    except ImportError as e:
        print(f"导入模块失败: {e}")
        return None

# 使用示例
math_module = dynamic_import("math")
if math_module:
    print(math_module.pi)

# 根据条件导入模块
def get_json_parser():
    """根据可用性选择JSON解析器"""
    try:
        import json
        return json
    except ImportError:
        try:
            import simplejson as json
            return json
        except ImportError:
            raise ImportError("没有可用的JSON解析器")

# 延迟导入（提高启动速度）
class LazyImporter:
    def __init__(self, module_name):
        self.module_name = module_name
        self._module = None

    def __getattr__(self, name):
        if self._module is None:
            self._module = importlib.import_module(self.module_name)
        return getattr(self._module, name)

# 使用延迟导入
json_lazy = LazyImporter("json")
print(json_lazy.loads('{"key": "value"}'))  # 第一次访问时才导入
```

## 12.3 包的结构

包是包含多个模块的目录结构，用于组织大型项目的代码。

### 12.3.1 创建包的基本结构

```
my_package/
├── __init__.py
├── module1.py
├── module2.py
└── subpackage/
    ├── __init__.py
    ├── submodule1.py
    └── submodule2.py
```

```python
# my_package/__init__.py
"""
我的包 - 示例包
"""

__version__ = "1.0.0"
__author__ = "Python教程"

# 导入子模块，使其可以直接访问
from . import module1, module2
from .subpackage import submodule1

# 定义包级别的函数
def package_info():
    """获取包信息"""
    return f"包版本: {__version__}, 作者: {__author__}"

print(f"包 {__name__} 已初始化")
```

```python
# my_package/module1.py
"""
包的第一个模块
"""

def function1():
    return "这是模块1的函数1"

def function2():
    return "这是模块1的函数2"

class Class1:
    def method(self):
        return "这是类1的方法"
```

```python
# my_package/module2.py
"""
包的第二个模块
"""

def function3():
    return "这是模块2的函数3"

def function4():
    return "这是模块2的函数4"

class Class2:
    def method(self):
        return "这是类2的方法"
```

```python
# my_package/subpackage/__init__.py
"""
子包初始化
"""

from . import submodule1, submodule2

def subpackage_info():
    return "这是子包的信息"
```

```python
# my_package/subpackage/submodule1.py
"""
子包的第一个模块
"""

def sub_function1():
    return "这是子包模块1的函数"

def sub_function2():
    return "这是子包模块2的函数"
```

### 12.3.2 导入包和子模块

```python
# 方式1: 导入整个包
import my_package

print(my_package.package_info())
print(my_package.module1.function1())

# 方式2: 从包导入模块
from my_package import module1, module2

print(module1.function1())
print(module2.function3())

# 方式3: 从包的子模块导入
from my_package.subpackage import submodule1

print(submodule1.sub_function1())

# 方式4: 导入子包
from my_package import subpackage

print(subpackage.subpackage_info())
print(subpackage.submodule1.sub_function1())

# 方式5: 直接导入子模块的函数
from my_package.subpackage.submodule1 import sub_function1

print(sub_function1())
```

## 12.4 相对导入和绝对导入

### 12.4.1 绝对导入

```python
# 在 my_package/module1.py 中使用绝对导入
from my_package.module2 import function3
from my_package.subpackage.submodule1 import sub_function1

def combined_function():
    """组合使用不同模块的函数"""
    result1 = function3()
    result2 = sub_function1()
    return f"{result1} 和 {result2}"

# 在子包中使用绝对导入
# my_package/subpackage/submodule1.py
from my_package.module1 import function1
from my_package.module2 import function4

def complex_function():
    """复杂的组合函数"""
    return f"{function1()} + {function4()}"
```

### 12.4.2 相对导入

```python
# 在 my_package/module1.py 中使用相对导入
from .module2 import function3  # 从同级模块导入
from .subpackage.submodule1 import sub_function1  # 从子包导入

# 在 my_package/subpackage/submodule1.py 中使用相对导入
from ..module1 import function1  # 从父包导入
from ..module2 import function4  # 从父包导入
from .submodule2 import some_function  # 从同级模块导入

# 相对导入的层次
# .  表示当前包
# .. 表示父包
# ... 表示祖父包

def relative_import_demo():
    """演示相对导入"""
    # 从父包导入
    from .. import module1
    # 从兄弟模块导入
    from . import submodule2

    return "相对导入演示完成"
```

### 12.4.3 导入的最佳实践

```python
# 推荐：使用绝对导入
import my_package.module1
from my_package.subpackage import submodule1

# 避免：在包内部使用相对导入时可能出现问题
# 特别是在脚本直接运行时

# 好的做法：混合使用
from __future__ import absolute_import

# 这样可以确保导入行为的一致性
import sys
from os.path import join

# 在包的 __init__.py 中进行必要的导入
# 让用户可以方便地访问常用功能
```

## 12.5 __init__.py 文件

### 12.5.1 __init__.py 的作用

```python
# my_package/__init__.py
"""
包的初始化文件
控制包的导入行为和提供包级别的接口
"""

__version__ = "1.0.0"
__author__ = "Python教程"
__all__ = ["module1", "module2", "subpackage", "Calculator"]

# 导入子模块
from . import module1, module2
from . import subpackage

# 导入常用类
from .module1 import Class1
from .module2 import Class2

# 定义包级别的类
class Calculator:
    """包级别的计算器类"""

    @staticmethod
    def add(a, b):
        return a + b

    @staticmethod
    def multiply(a, b):
        return a * b

def package_function():
    """包级别的函数"""
    return "这是包级别的函数"

# 初始化代码
print(f"初始化包: {__name__}")
```

### 12.5.2 __all__ 变量的控制

```python
# 在模块中使用 __all__ 控制导入
# my_package/module1.py
__all__ = ["function1", "Class1"]  # 只允许导入这些

def function1():
    return "公开函数1"

def function2():
    return "私有函数2"

def _private_function():
    return "私有函数"

class Class1:
    pass

class _PrivateClass:
    pass

# 使用示例
from my_package.module1 import *  # 只导入 function1 和 Class1
# function2 和 _private_function 不会被导入
```

### 12.5.3 包的初始化顺序

```python
# 演示包的初始化顺序
# my_package/__init__.py
print("1. 包级别 __init__.py 开始")

# my_package/module1.py
print("2. module1 开始初始化")

# my_package/module2.py
print("3. module2 开始初始化")

# my_package/subpackage/__init__.py
print("4. 子包 __init__.py 开始")

# 导入时的执行顺序
import my_package
# 输出顺序：
# 2. module1 开始初始化
# 3. module2 开始初始化
# 4. 子包 __init__.py 开始
# 1. 包级别 __init__.py 开始
```

## 12.6 标准库模块

### 12.6.1 常用标准库模块

```python
# os 模块 - 操作系统接口
import os

print("当前工作目录:", os.getcwd())
print("目录列表:", os.listdir("."))
print("路径是否存在:", os.path.exists("my_package"))

# 创建目录
os.makedirs("test_dir", exist_ok=True)
print("创建目录成功")

# sys 模块 - 系统相关
import sys

print("Python 版本:", sys.version)
print("命令行参数:", sys.argv)
print("模块搜索路径:", sys.path[:3])

# 退出程序
# sys.exit(0)

# datetime 模块 - 日期和时间
from datetime import datetime, date, time

now = datetime.now()
print("当前时间:", now)
print("今天日期:", date.today())
print("当前时间:", now.time())

# 格式化日期
print("格式化:", now.strftime("%Y-%m-%d %H:%M:%S"))

# json 模块 - JSON 处理
import json

data = {"name": "Alice", "age": 25, "city": "Beijing"}
json_str = json.dumps(data, ensure_ascii=False)
print("JSON 字符串:", json_str)

parsed_data = json.loads(json_str)
print("解析后的数据:", parsed_data)

# collections 模块 - 高级数据结构
from collections import Counter, defaultdict, namedtuple

# Counter
words = ["apple", "banana", "apple", "cherry", "banana", "apple"]
word_count = Counter(words)
print("单词计数:", word_count)

# defaultdict
fruit_colors = defaultdict(list)
fruit_colors["apple"].append("red")
fruit_colors["banana"].append("yellow")
print("水果颜色:", dict(fruit_colors))

# namedtuple
Point = namedtuple("Point", ["x", "y"])
p = Point(1, 2)
print(f"点: ({p.x}, {p.y})")

# re 模块 - 正则表达式
import re

text = "我的邮箱是 example@email.com 和 test123@domain.org"
email_pattern = r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b'
emails = re.findall(email_pattern, text)
print("找到的邮箱:", emails)

# 替换
new_text = re.sub(r'\d+', 'NUMBER', "房间号是 101，电话是 123456")
print("替换结果:", new_text)
```

### 12.6.2 标准库模块的组织

```python
# 查看标准库的所有模块
import sys
import pkgutil

def list_stdlib_modules():
    """列出标准库模块"""
    stdlib_modules = []

    # 获取标准库路径
    for path in sys.path:
        if path and 'site-packages' not in path:
            try:
                for importer, modname, ispkg in pkgutil.iter_modules([path]):
                    if not modname.startswith('_'):
                        stdlib_modules.append(modname)
            except:
                continue

    return sorted(set(stdlib_modules))

modules = list_stdlib_modules()
print(f"标准库模块数量: {len(modules)}")
print("前20个模块:", modules[:20])
```

## 12.7 第三方模块的安装和管理

### 12.7.1 使用 pip 安装模块

```python
# 安装第三方模块的命令（在终端中运行）
# pip install requests
# pip install numpy pandas matplotlib
# pip install flask django
# pip install --upgrade package_name  # 升级包
# pip uninstall package_name  # 卸载包

# 查看已安装的包
# pip list
# pip show package_name

# 安装特定版本
# pip install requests==2.25.1
# pip install "requests>=2.20.0"

# 从 requirements.txt 安装
# pip install -r requirements.txt
```

### 12.7.2 requirements.txt 文件

```txt
# requirements.txt
# 项目依赖文件

# Web 框架
flask==2.0.1
django==3.2.5

# 数据处理
numpy==1.21.2
pandas==1.3.3
matplotlib==3.4.3

# HTTP 请求
requests==2.25.1

# 数据库
sqlalchemy==1.4.23
pymongo==3.12.0

# 测试
pytest==6.2.5
pytest-cov==2.12.1

# 代码质量
black==21.7b0
flake8==3.9.2
mypy==0.910
```

### 12.7.3 虚拟环境

```python
# 创建虚拟环境
# python -m venv myenv

# 激活虚拟环境
# Windows: myenv\Scripts\activate
# macOS/Linux: source myenv/bin/activate

# 在虚拟环境中安装包
# pip install requests numpy

# 冻结依赖
# pip freeze > requirements.txt

# 退出虚拟环境
# deactivate

# 使用示例
import sys
print("Python 路径:", sys.executable)

# 检查是否在虚拟环境中
import os
venv_path = os.environ.get('VIRTUAL_ENV')
if venv_path:
    print(f"在虚拟环境中: {venv_path}")
else:
    print("不在虚拟环境中")
```

### 12.7.4 使用第三方模块

```python
# requests 模块示例
import requests

def fetch_webpage(url):
    """获取网页内容"""
    try:
        response = requests.get(url)
        response.raise_for_status()  # 检查请求是否成功
        return response.text
    except requests.RequestException as e:
        return f"请求失败: {e}"

# 使用示例
content = fetch_webpage("https://httpbin.org/get")
print("网页内容长度:", len(content))

# numpy 示例
import numpy as np

# 创建数组
arr = np.array([1, 2, 3, 4, 5])
print("数组:", arr)
print("数组形状:", arr.shape)
print("数组均值:", np.mean(arr))

# pandas 示例
import pandas as pd

# 创建数据框
data = {
    '姓名': ['Alice', 'Bob', 'Charlie'],
    '年龄': [25, 30, 35],
    '城市': ['北京', '上海', '广州']
}

df = pd.DataFrame(data)
print("数据框:")
print(df)
print("\n年龄统计:")
print(df['年龄'].describe())
```

## 总结

本章介绍了模块和包的核心概念：
- 模块是组织代码的基本单位
- 包是包含多个模块的目录结构
- 掌握不同的导入方式和最佳实践
- 了解标准库的使用方法
- 学习第三方模块的安装和管理

通过本章的学习，你应该能够：
1. 创建和使用自定义模块
2. 组织代码为包结构
3. 正确使用相对和绝对导入
4. 利用标准库解决常见问题
5. 管理项目的依赖关系

下一章将介绍面向对象编程的基础概念。
