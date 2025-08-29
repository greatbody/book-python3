# 第26章：类型提示

## 26.1 类型提示基础

### 26.1.1 类型提示简介

```python
# 类型提示基础
print("类型提示基础:")

# 1. 变量类型提示
from typing import List, Dict, Tuple, Set, Optional, Union

# 基本类型提示
name: str = "Alice"
age: int = 30
height: float = 1.75
is_student: bool = True

print(f"基本类型提示:")
print(f"  name: {name} (类型: {type(name).__name__})")
print(f"  age: {age} (类型: {type(age).__name__})")
print(f"  height: {height} (类型: {type(height).__name__})")
print(f"  is_student: {is_student} (类型: {type(is_student).__name__})")

# 2. 集合类型提示
numbers: List[int] = [1, 2, 3, 4, 5]
names: List[str] = ["Alice", "Bob", "Charlie"]
person: Dict[str, Union[str, int]] = {"name": "Alice", "age": 30}
coordinates: Tuple[float, float] = (10.5, 20.3)
unique_numbers: Set[int] = {1, 2, 3, 4, 5}

print(f"\n集合类型提示:")
print(f"  numbers: {numbers}")
print(f"  names: {names}")
print(f"  person: {person}")
print(f"  coordinates: {coordinates}")
print(f"  unique_numbers: {unique_numbers}")

# 3. 可选类型
def greet(name: Optional[str] = None) -> str:
    """带可选参数的函数"""
    if name is None:
        return "Hello, World!"
    return f"Hello, {name}!"

print(f"\n可选类型:")
print(f"  greet(): {greet()}")
print(f"  greet('Alice'): {greet('Alice')}")

# 4. 联合类型
def process_value(value: Union[int, str, float]) -> str:
    """处理多种类型的参数"""
    if isinstance(value, int):
        return f"整数: {value * 2}"
    elif isinstance(value, str):
        return f"字符串: {value.upper()}"
    elif isinstance(value, float):
        return f"浮点数: {value:.2f}"
    else:
        return f"未知类型: {value}"

print(f"\n联合类型:")
print(f"  process_value(42): {process_value(42)}")
print(f"  process_value('hello'): {process_value('hello')}")
print(f"  process_value(3.14): {process_value(3.14)}")

# 5. 函数类型提示
def add_numbers(a: int, b: int) -> int:
    """两个整数相加"""
    return a + b

def calculate_average(numbers: List[float]) -> float:
    """计算平均值"""
    if not numbers:
        return 0.0
    return sum(numbers) / len(numbers)

def find_max_value(data: Dict[str, int]) -> Optional[str]:
    """查找最大值对应的键"""
    if not data:
        return None

    max_key = max(data, key=data.get)
    return max_key

print(f"\n函数类型提示:")
print(f"  add_numbers(5, 3): {add_numbers(5, 3)}")
print(f"  calculate_average([1.0, 2.0, 3.0, 4.0]): {calculate_average([1.0, 2.0, 3.0, 4.0])}")
print(f"  find_max_value({{'a': 10, 'b': 20, 'c': 15}}): {find_max_value({'a': 10, 'b': 20, 'c': 15})}")

# 6. 类型别名
Vector2D = Tuple[float, float]
Matrix2x2 = List[List[float]]
PersonInfo = Dict[str, Union[str, int, float]]

def create_vector(x: float, y: float) -> Vector2D:
    """创建二维向量"""
    return (x, y)

def create_matrix() -> Matrix2x2:
    """创建2x2矩阵"""
    return [[1.0, 0.0], [0.0, 1.0]]

def create_person(name: str, age: int, height: float) -> PersonInfo:
    """创建人员信息"""
    return {
        "name": name,
        "age": age,
        "height": height
    }

print(f"\n类型别名:")
vector = create_vector(3.0, 4.0)
matrix = create_matrix()
person = create_person("Alice", 30, 1.75)

print(f"  vector: {vector}")
print(f"  matrix: {matrix}")
print(f"  person: {person}")

# 7. Any 类型
from typing import Any

def process_anything(data: Any) -> Any:
    """处理任意类型的数据"""
    if isinstance(data, (int, float)):
        return data * 2
    elif isinstance(data, str):
        return data.upper()
    elif isinstance(data, list):
        return len(data)
    else:
        return str(data)

print(f"\nAny 类型:")
print(f"  process_anything(42): {process_anything(42)}")
print(f"  process_anything('hello'): {process_anything('hello')}")
print(f"  process_anything([1, 2, 3]): {process_anything([1, 2, 3])}")
print(f"  process_anything({{'key': 'value'}}): {process_anything({'key': 'value'})}")

# 8. NoReturn 类型
from typing import NoReturn

def raise_error(message: str) -> NoReturn:
    """抛出异常的函数"""
    raise ValueError(message)

def infinite_loop() -> NoReturn:
    """无限循环的函数"""
    while True:
        pass

print(f"\nNoReturn 类型:")
try:
    raise_error("这是一个错误")
except ValueError as e:
    print(f"  捕获到异常: {e}")
```

### 26.1.2 类型提示的实际应用

```python
# 类型提示的实际应用
print("类型提示的实际应用:")

from typing import List, Dict, Tuple, Optional, Union
import json

# 1. 数据验证函数
def validate_user_data(user_data: Dict[str, Union[str, int]]) -> bool:
    """验证用户数据"""
    required_fields = ['name', 'age', 'email']

    # 检查必需字段
    for field in required_fields:
        if field not in user_data:
            return False

    # 验证数据类型
    if not isinstance(user_data['name'], str):
        return False
    if not isinstance(user_data['age'], int) or user_data['age'] < 0:
        return False
    if not isinstance(user_data['email'], str) or '@' not in user_data['email']:
        return False

    return True

print("数据验证函数:")
valid_user = {'name': 'Alice', 'age': 30, 'email': 'alice@example.com'}
invalid_user = {'name': 'Bob', 'age': 'thirty', 'email': 'bob@example.com'}

print(f"  有效用户: {validate_user_data(valid_user)}")
print(f"  无效用户: {validate_user_data(invalid_user)}")

# 2. 数据转换器
def convert_data_format(data: Union[str, bytes, dict]) -> str:
    """转换数据格式"""
    if isinstance(data, str):
        return data
    elif isinstance(data, bytes):
        return data.decode('utf-8')
    elif isinstance(data, dict):
        return json.dumps(data, ensure_ascii=False)
    else:
        return str(data)

print(f"\n数据转换器:")
print(f"  字符串: {convert_data_format('hello')}")
print(f"  字节串: {convert_data_format(b'hello')}")
print(f"  字典: {convert_data_format({'key': 'value'})}")
print(f"  数字: {convert_data_format(42)}")

# 3. 配置管理器
class ConfigurationManager:
    """配置管理器"""

    def __init__(self):
        self.config: Dict[str, Union[str, int, float, bool]] = {}

    def set_config(self, key: str, value: Union[str, int, float, bool]) -> None:
        """设置配置项"""
        self.config[key] = value

    def get_config(self, key: str, default: Optional[Union[str, int, float, bool]] = None) -> Optional[Union[str, int, float, bool]]:
        """获取配置项"""
        return self.config.get(key, default)

    def get_all_config(self) -> Dict[str, Union[str, int, float, bool]]:
        """获取所有配置"""
        return self.config.copy()

    def save_to_file(self, filename: str) -> bool:
        """保存配置到文件"""
        try:
            with open(filename, 'w', encoding='utf-8') as f:
                json.dump(self.config, f, ensure_ascii=False, indent=2)
            return True
        except Exception:
            return False

    def load_from_file(self, filename: str) -> bool:
        """从文件加载配置"""
        try:
            with open(filename, 'r', encoding='utf-8') as f:
                self.config = json.load(f)
            return True
        except Exception:
            return False

print(f"\n配置管理器:")
config_manager = ConfigurationManager()
config_manager.set_config('app_name', 'MyApp')
config_manager.set_config('version', '1.0.0')
config_manager.set_config('debug', True)
config_manager.set_config('port', 8080)

print(f"  配置: {config_manager.get_all_config()}")
print(f"  app_name: {config_manager.get_config('app_name')}")
print(f"  timeout: {config_manager.get_config('timeout', 30)}")

# 4. 数据处理器
class DataProcessor:
    """数据处理器"""

    def __init__(self):
        self.processed_data: List[Dict[str, Union[str, int, float]]] = []

    def process_numbers(self, numbers: List[Union[int, float]]) -> List[float]:
        """处理数字列表"""
        return [float(n) for n in numbers]

    def process_text(self, text: str, operations: List[str]) -> str:
        """处理文本"""
        result = text
        for op in operations:
            if op == 'upper':
                result = result.upper()
            elif op == 'lower':
                result = result.lower()
            elif op == 'strip':
                result = result.strip()
            elif op == 'title':
                result = result.title()
        return result

    def process_records(self, records: List[Dict[str, Union[str, int]]]) -> List[Dict[str, Union[str, int, float]]]:
        """处理记录列表"""
        processed = []
        for record in records:
            processed_record = record.copy()

            # 添加处理时间戳
            processed_record['processed_at'] = time.time()

            # 计算年龄（如果存在）
            if 'birth_year' in record:
                current_year = 2024  # 假设当前年份
                processed_record['age'] = current_year - record['birth_year']

            processed.append(processed_record)

        self.processed_data.extend(processed)
        return processed

print(f"\n数据处理器:")
processor = DataProcessor()

# 处理数字
numbers = [1, 2, 3, 4, 5]
processed_numbers = processor.process_numbers(numbers)
print(f"  处理数字: {processed_numbers}")

# 处理文本
text = "  hello world  "
operations = ['strip', 'title']
processed_text = processor.process_text(text, operations)
print(f"  处理文本: '{processed_text}'")

# 处理记录
records = [
    {'name': 'Alice', 'birth_year': 1994},
    {'name': 'Bob', 'birth_year': 1985},
    {'name': 'Charlie', 'age': 35}
]
processed_records = processor.process_records(records)
print(f"  处理记录: {processed_records}")

# 5. API 客户端
class APIClient:
    """API 客户端"""

    def __init__(self, base_url: str, timeout: int = 30):
        self.base_url = base_url
        self.timeout = timeout
        self.session_headers: Dict[str, str] = {}

    def set_header(self, key: str, value: str) -> None:
        """设置请求头"""
        self.session_headers[key] = value

    def get(self, endpoint: str, params: Optional[Dict[str, str]] = None) -> Optional[Dict[str, Union[str, int]]]:
        """GET 请求"""
        # 模拟 API 调用
        print(f"GET {self.base_url}{endpoint}")
        if params:
            print(f"  参数: {params}")

        # 模拟响应
        if endpoint == '/users/1':
            return {'id': 1, 'name': 'Alice', 'email': 'alice@example.com'}
        elif endpoint == '/users':
            return {'users': [{'id': 1, 'name': 'Alice'}, {'id': 2, 'name': 'Bob'}]}
        else:
            return None

    def post(self, endpoint: str, data: Dict[str, Union[str, int]]) -> Optional[Dict[str, Union[str, int]]]:
        """POST 请求"""
        # 模拟 API 调用
        print(f"POST {self.base_url}{endpoint}")
        print(f"  数据: {data}")

        # 模拟响应
        if endpoint == '/users':
            return {'id': 3, 'name': data.get('name'), 'email': data.get('email')}
        else:
            return None

print(f"\nAPI 客户端:")
client = APIClient('https://api.example.com')

# GET 请求
user = client.get('/users/1')
print(f"  获取用户: {user}")

users = client.get('/users', {'limit': '10'})
print(f"  获取用户列表: {users}")

# POST 请求
new_user = client.post('/users', {'name': 'Charlie', 'email': 'charlie@example.com'})
print(f"  创建用户: {new_user}")

# 6. 文件处理器
class FileProcessor:
    """文件处理器"""

    def read_text_file(self, filename: str, encoding: str = 'utf-8') -> Optional[str]:
        """读取文本文件"""
        try:
            with open(filename, 'r', encoding=encoding) as f:
                return f.read()
        except FileNotFoundError:
            return None
        except UnicodeDecodeError:
            return None

    def write_text_file(self, filename: str, content: str, encoding: str = 'utf-8') -> bool:
        """写入文本文件"""
        try:
            with open(filename, 'w', encoding=encoding) as f:
                f.write(content)
            return True
        except Exception:
            return False

    def read_json_file(self, filename: str) -> Optional[Dict[str, Union[str, int, float, bool]]]:
        """读取 JSON 文件"""
        try:
            with open(filename, 'r', encoding='utf-8') as f:
                return json.load(f)
        except (FileNotFoundError, json.JSONDecodeError):
            return None

    def write_json_file(self, filename: str, data: Dict[str, Union[str, int, float, bool]]) -> bool:
        """写入 JSON 文件"""
        try:
            with open(filename, 'w', encoding='utf-8') as f:
                json.dump(data, f, ensure_ascii=False, indent=2)
            return True
        except Exception:
            return False

    def process_csv_data(self, csv_content: str) -> List[Dict[str, str]]:
        """处理 CSV 数据"""
        lines = csv_content.strip().split('\n')
        if not lines:
            return []

        headers = [h.strip() for h in lines[0].split(',')]
        result = []

        for line in lines[1:]:
            if not line.strip():
                continue

            values = [v.strip() for v in line.split(',')]
            if len(values) == len(headers):
                record = dict(zip(headers, values))
                result.append(record)

        return result

print(f"\n文件处理器:")
file_processor = FileProcessor()

# 创建测试文件
test_content = "Hello, World!\nThis is a test file."
file_processor.write_text_file('test.txt', test_content)

# 读取文件
read_content = file_processor.read_text_file('test.txt')
print(f"  读取文本文件: {read_content}")

# JSON 文件操作
test_data = {'name': 'Alice', 'age': 30, 'active': True}
file_processor.write_json_file('test.json', test_data)

read_data = file_processor.read_json_file('test.json')
print(f"  读取 JSON 文件: {read_data}")

# CSV 数据处理
csv_data = """name,age,city
Alice,30,Beijing
Bob,25,Shanghai
Charlie,35,Shenzhen"""

processed_csv = file_processor.process_csv_data(csv_data)
print(f"  处理 CSV 数据: {processed_csv}")

# 清理文件
import os
for f in ['test.txt', 'test.json']:
    if os.path.exists(f):
        os.remove(f)
```

## 26.2 泛型类型

### 26.2.1 泛型基础

```python
# 泛型类型
print("泛型类型:")

from typing import TypeVar, Generic, List, Dict, Tuple, Optional
import time

# 1. TypeVar 基础
T = TypeVar('T')  # 单个类型变量
K = TypeVar('K')  # 键类型
V = TypeVar('V')  # 值类型

def first_item(items: List[T]) -> Optional[T]:
    """获取列表的第一个元素"""
    return items[0] if items else None

def swap_items(items: List[T], i: int, j: int) -> List[T]:
    """交换列表中的两个元素"""
    if 0 <= i < len(items) and 0 <= j < len(items):
        items[i], items[j] = items[j], items[i]
    return items

def create_pair(first: K, second: V) -> Tuple[K, V]:
    """创建键值对"""
    return (first, second)

print("TypeVar 基础:")
numbers = [1, 2, 3, 4, 5]
first_num = first_item(numbers)
print(f"  第一个数字: {first_num}")

swapped = swap_items(numbers.copy(), 0, 4)
print(f"  交换后: {swapped}")

pair = create_pair("name", "Alice")
print(f"  键值对: {pair}")

# 2. 约束类型变量
from typing import Union

NumberType = TypeVar('NumberType', int, float, complex)

def add_numbers(a: NumberType, b: NumberType) -> NumberType:
    """数字相加"""
    return a + b

StringOrBytes = TypeVar('StringOrBytes', str, bytes)

def get_length(data: StringOrBytes) -> int:
    """获取长度"""
    return len(data)

print(f"\n约束类型变量:")
print(f"  add_numbers(3, 4): {add_numbers(3, 4)}")
print(f"  add_numbers(3.5, 2.1): {add_numbers(3.5, 2.1)}")
print(f"  get_length('hello'): {get_length('hello')}")
print(f"  get_length(b'hello'): {get_length(b'hello')}")

# 3. 泛型类
class Stack(Generic[T]):
    """泛型栈"""

    def __init__(self):
        self.items: List[T] = []

    def push(self, item: T) -> None:
        """入栈"""
        self.items.append(item)

    def pop(self) -> Optional[T]:
        """出栈"""
        return self.items.pop() if self.items else None

    def peek(self) -> Optional[T]:
        """查看栈顶"""
        return self.items[-1] if self.items else None

    def is_empty(self) -> bool:
        """是否为空"""
        return len(self.items) == 0

    def size(self) -> int:
        """栈大小"""
        return len(self.items)

print(f"\n泛型类 - 栈:")
int_stack = Stack[int]()
int_stack.push(1)
int_stack.push(2)
int_stack.push(3)

print(f"  栈大小: {int_stack.size()}")
print(f"  栈顶元素: {int_stack.peek()}")
print(f"  出栈: {int_stack.pop()}")
print(f"  出栈: {int_stack.pop()}")

str_stack = Stack[str]()
str_stack.push("hello")
str_stack.push("world")
print(f"  字符串栈: {str_stack.pop()}")

# 4. 泛型字典
class TypedDict(Generic[K, V]):
    """类型化的字典"""

    def __init__(self):
        self.data: Dict[K, V] = {}

    def set_item(self, key: K, value: V) -> None:
        """设置项目"""
        self.data[key] = value

    def get_item(self, key: K) -> Optional[V]:
        """获取项目"""
        return self.data.get(key)

    def get_all_keys(self) -> List[K]:
        """获取所有键"""
        return list(self.data.keys())

    def get_all_values(self) -> List[V]:
        """获取所有值"""
        return list(self.data.values())

print(f"\n泛型字典:")
str_int_dict = TypedDict[str, int]()
str_int_dict.set_item("Alice", 30)
str_int_dict.set_item("Bob", 25)

print(f"  Alice 的年龄: {str_int_dict.get_item('Alice')}")
print(f"  所有键: {str_int_dict.get_all_keys()}")
print(f"  所有值: {str_int_dict.get_all_values()}")

# 5. 多个类型变量
class Pair(Generic[T, U]):
    """成对数据"""

    def __init__(self, first: T, second: U):
        self.first = first
        self.second = second

    def get_first(self) -> T:
        return self.first

    def get_second(self) -> U:
        return self.second

    def swap(self) -> 'Pair[U, T]':
        """交换类型"""
        return Pair(self.second, self.first)

print(f"\n多个类型变量:")
name_age_pair = Pair("Alice", 30)
print(f"  姓名: {name_age_pair.get_first()}")
print(f"  年龄: {name_age_pair.get_second()}")

swapped_pair = name_age_pair.swap()
print(f"  交换后 - 第一个: {swapped_pair.get_first()}")
print(f"  交换后 - 第二个: {swapped_pair.get_second()}")

# 6. 泛型函数的高级用法
def filter_by_type(items: List[T], target_type: type) -> List[T]:
    """按类型过滤"""
    return [item for item in items if isinstance(item, target_type)]

def group_by_type(items: List[T]) -> Dict[type, List[T]]:
    """按类型分组"""
    groups: Dict[type, List[T]] = {}
    for item in items:
        item_type = type(item)
        if item_type not in groups:
            groups[item_type] = []
        groups[item_type].append(item)
    return groups

def transform_items(items: List[T], transformer) -> List[T]:
    """转换项目"""
    return [transformer(item) for item in items]

print(f"\n泛型函数的高级用法:")
mixed_list = [1, "hello", 2.5, "world", 3, True]

# 过滤字符串
strings = filter_by_type(mixed_list, str)
print(f"  字符串: {strings}")

# 按类型分组
grouped = group_by_type(mixed_list)
print(f"  分组: {grouped}")

# 转换所有项目为字符串
transformed = transform_items(mixed_list, str)
print(f"  转换为字符串: {transformed}")

# 7. 协变和逆变
from typing import Sequence

def process_sequence(seq: Sequence[T]) -> List[T]:
    """处理序列（协变）"""
    return list(seq)

def store_in_dict(key: T, value: T, storage: Dict[T, T]) -> None:
    """存储到字典（不变）"""
    storage[key] = value

print(f"\n协变和逆变:")
numbers_seq: Sequence[int] = [1, 2, 3, 4, 5]
processed = process_sequence(numbers_seq)  # Sequence[int] 可以传递给 Sequence[T]
print(f"  处理序列: {processed}")

storage: Dict[str, str] = {}
store_in_dict("key1", "value1", storage)  # 类型必须完全匹配
print(f"  存储结果: {storage}")
```

### 26.2.2 泛型的实际应用

```python
# 泛型的实际应用
print("泛型的实际应用:")

from typing import TypeVar, Generic, List, Dict, Optional, Union
import json
import time

# 1. 数据容器
T = TypeVar('T')

class DataContainer(Generic[T]):
    """数据容器"""

    def __init__(self, data: Optional[List[T]] = None):
        self.data: List[T] = data or []
        self.metadata: Dict[str, Union[str, int, float]] = {
            'created_at': time.time(),
            'version': '1.0'
        }

    def add_item(self, item: T) -> None:
        """添加项目"""
        self.data.append(item)
        self.metadata['last_modified'] = time.time()

    def get_item(self, index: int) -> Optional[T]:
        """获取项目"""
        if 0 <= index < len(self.data):
            return self.data[index]
        return None

    def get_all_items(self) -> List[T]:
        """获取所有项目"""
        return self.data.copy()

    def filter_items(self, predicate) -> List[T]:
        """过滤项目"""
        return [item for item in self.data if predicate(item)]

    def transform_items(self, transformer) -> List[T]:
        """转换项目"""
        return [transformer(item) for item in self.data]

    def get_metadata(self) -> Dict[str, Union[str, int, float]]:
        """获取元数据"""
        return self.metadata.copy()

print("数据容器:")
# 整数容器
int_container = DataContainer[int]()
int_container.add_item(10)
int_container.add_item(20)
int_container.add_item(30)

print(f"  整数容器: {int_container.get_all_items()}")
print(f"  过滤偶数: {int_container.filter_items(lambda x: x % 2 == 0)}")
print(f"  转换为字符串: {int_container.transform_items(str)}")

# 字符串容器
str_container = DataContainer[str]()
str_container.add_item("Alice")
str_container.add_item("Bob")
str_container.add_item("Charlie")

print(f"  字符串容器: {str_container.get_all_items()}")
print(f"  过滤长名称: {str_container.filter_items(lambda x: len(x) > 4)}")

# 2. 缓存系统
class Cache(Generic[T]):
    """泛型缓存"""

    def __init__(self, max_size: int = 100):
        self.max_size = max_size
        self.cache: Dict[str, Tuple[T, float]] = {}  # (value, timestamp)

    def set(self, key: str, value: T, ttl: Optional[float] = None) -> None:
        """设置缓存"""
        if len(self.cache) >= self.max_size:
            # 简单的 LRU 策略：删除最旧的
            oldest_key = min(self.cache.keys(), key=lambda k: self.cache[k][1])
            del self.cache[oldest_key]

        self.cache[key] = (value, time.time() + (ttl or 0))

    def get(self, key: str) -> Optional[T]:
        """获取缓存"""
        if key in self.cache:
            value, expiry = self.cache[key]
            if expiry == 0 or time.time() < expiry:
                return value
            else:
                # 过期删除
                del self.cache[key]
        return None

    def delete(self, key: str) -> bool:
        """删除缓存"""
        if key in self.cache:
            del self.cache[key]
            return True
        return False

    def clear(self) -> None:
        """清空缓存"""
        self.cache.clear()

    def size(self) -> int:
        """缓存大小"""
        return len(self.cache)

print(f"\n缓存系统:")
# 用户信息缓存
user_cache = Cache[Dict[str, Union[str, int]]]()

user1 = {'name': 'Alice', 'age': 30, 'email': 'alice@example.com'}
user2 = {'name': 'Bob', 'age': 25, 'email': 'bob@example.com'}

user_cache.set('user_1', user1, ttl=300)  # 5分钟TTL
user_cache.set('user_2', user2)

print(f"  缓存大小: {user_cache.size()}")
print(f"  获取用户1: {user_cache.get('user_1')}")
print(f"  获取用户2: {user_cache.get('user_2')}")
print(f"  获取不存在的用户: {user_cache.get('user_3')}")

# 数值缓存
number_cache = Cache[int]()
number_cache.set('answer', 42)
print(f"  数值缓存: {number_cache.get('answer')}")

# 3. 事件系统
class Event(Generic[T]):
    """事件类"""

    def __init__(self, event_type: str, data: T):
        self.event_type = event_type
        self.data = data
        self.timestamp = time.time()

    def get_event_type(self) -> str:
        return self.event_type

    def get_data(self) -> T:
        return self.data

    def get_timestamp(self) -> float:
        return self.timestamp

class EventHandler(Generic[T]):
    """事件处理器"""

    def __init__(self):
        self.handlers: Dict[str, List[callable]] = {}

    def register_handler(self, event_type: str, handler) -> None:
        """注册处理器"""
        if event_type not in self.handlers:
            self.handlers[event_type] = []
        self.handlers[event_type].append(handler)

    def unregister_handler(self, event_type: str, handler) -> bool:
        """注销处理器"""
        if event_type in self.handlers and handler in self.handlers[event_type]:
            self.handlers[event_type].remove(handler)
            return True
        return False

    def handle_event(self, event: Event[T]) -> List[T]:
        """处理事件"""
        results = []
        event_type = event.get_event_type()

        if event_type in self.handlers:
            for handler in self.handlers[event_type]:
                try:
                    result = handler(event.get_data())
                    results.append(result)
                except Exception as e:
                    print(f"处理器错误: {e}")

        return results

print(f"\n事件系统:")
# 创建事件处理器
event_handler = EventHandler[Dict[str, Union[str, int]]]()

# 注册处理器
def user_login_handler(user_data):
    return f"用户 {user_data['name']} 已登录"

def user_logout_handler(user_data):
    return f"用户 {user_data['name']} 已登出"

event_handler.register_handler('user_login', user_login_handler)
event_handler.register_handler('user_logout', user_logout_handler)

# 创建和处理事件
login_event = Event('user_login', {'name': 'Alice', 'user_id': 1})
logout_event = Event('user_logout', {'name': 'Alice', 'user_id': 1})

login_results = event_handler.handle_event(login_event)
logout_results = event_handler.handle_event(logout_event)

print(f"  登录事件结果: {login_results}")
print(f"  登出事件结果: {logout_results}")

# 4. 数据管道
class Pipeline(Generic[T, U]):
    """数据管道"""

    def __init__(self):
        self.stages: List[callable] = []

    def add_stage(self, stage_func) -> None:
        """添加处理阶段"""
        self.stages.append(stage_func)

    def process(self, data: T) -> U:
        """处理数据"""
        result = data
        for stage in self.stages:
            result = stage(result)
        return result

    def process_batch(self, data_list: List[T]) -> List[U]:
        """批量处理"""
        return [self.process(data) for data in data_list]

print(f"\n数据管道:")
# 创建数据管道
pipeline = Pipeline[str, Dict[str, Union[str, int]]]()

# 添加处理阶段
def parse_line(line: str) -> List[str]:
    """解析行"""
    return line.strip().split(',')

def create_record(fields: List[str]) -> Dict[str, Union[str, int]]:
    """创建记录"""
    if len(fields) >= 3:
        return {
            'name': fields[0],
            'age': int(fields[1]) if fields[1].isdigit() else 0,
            'city': fields[2]
        }
    return {'name': '', 'age': 0, 'city': ''}

pipeline.add_stage(parse_line)
pipeline.add_stage(create_record)

# 处理数据
csv_lines = [
    "Alice,30,Beijing",
    "Bob,25,Shanghai",
    "Charlie,35,Shenzhen"
]

processed_data = pipeline.process_batch(csv_lines)
print(f"  处理结果: {processed_data}")

# 5. 配置管理器
class ConfigurationManager(Generic[T]):
    """配置管理器"""

    def __init__(self):
        self.config: Dict[str, T] = {}
        self.validators: Dict[str, callable] = {}

    def set_config(self, key: str, value: T) -> bool:
        """设置配置"""
        # 验证值
        if key in self.validators:
            if not self.validators[key](value):
                return False

        self.config[key] = value
        return True

    def get_config(self, key: str, default: Optional[T] = None) -> Optional[T]:
        """获取配置"""
        return self.config.get(key, default)

    def add_validator(self, key: str, validator) -> None:
        """添加验证器"""
        self.validators[key] = validator

    def get_all_config(self) -> Dict[str, T]:
        """获取所有配置"""
        return self.config.copy()

    def save_config(self, filename: str) -> bool:
        """保存配置"""
        try:
            with open(filename, 'w', encoding='utf-8') as f:
                json.dump(self.config, f, ensure_ascii=False, indent=2)
            return True
        except Exception:
            return False

    def load_config(self, filename: str) -> bool:
        """加载配置"""
        try:
            with open(filename, 'r', encoding='utf-8') as f:
                self.config = json.load(f)
            return True
        except Exception:
            return False

print(f"\n配置管理器:")
# 字符串配置管理器
str_config = ConfigurationManager[str]()

# 添加验证器
def email_validator(email: str) -> bool:
    return '@' in email and '.' in email

str_config.add_validator('email', email_validator)

# 设置配置
str_config.set_config('app_name', 'MyApp')
str_config.set_config('email', 'admin@example.com')
str_config.set_config('invalid_email', 'invalid-email')  # 应该失败

print(f"  字符串配置: {str_config.get_all_config()}")

# 数值配置管理器
int_config = ConfigurationManager[int]()

def port_validator(port: int) -> bool:
    return 1 <= port <= 65535

int_config.add_validator('port', port_validator)

int_config.set_config('port', 8080)
int_config.set_config('invalid_port', 99999)  # 应该失败

print(f"  数值配置: {int_config.get_all_config()}")

# 6. 数据库连接池
class ConnectionPool(Generic[T]):
    """数据库连接池"""

    def __init__(self, connection_factory, max_connections: int = 10):
        self.connection_factory = connection_factory
        self.max_connections = max_connections
        self.available_connections: List[T] = []
        self.used_connections: List[T] = []

    def get_connection(self) -> Optional[T]:
        """获取连接"""
        if self.available_connections:
            connection = self.available_connections.pop()
            self.used_connections.append(connection)
            return connection

        if len(self.used_connections) < self.max_connections:
            try:
                connection = self.connection_factory()
                self.used_connections.append(connection)
                return connection
            except Exception:
                return None

        return None

    def release_connection(self, connection: T) -> bool:
        """释放连接"""
        if connection in self.used_connections:
            self.used_connections.remove(connection)
            self.available_connections.append(connection)
            return True
        return False

    def close_all(self) -> None:
        """关闭所有连接"""
        for connection in self.available_connections + self.used_connections:
            if hasattr(connection, 'close'):
                connection.close()

        self.available_connections.clear()
        self.used_connections.clear()

print(f"\n数据库连接池:")
# 模拟数据库连接
class MockConnection:
    def __init__(self, connection_id: int):
        self.connection_id = connection_id
        self.is_open = True

    def close(self):
        self.is_open = False

    def __repr__(self):
        return f"Connection-{self.connection_id}"

def create_connection():
    # 模拟连接创建
    connection_id = int(time.time() * 1000) % 1000
    return MockConnection(connection_id)

# 创建连接池
pool = ConnectionPool[MockConnection](create_connection, max_connections=3)

# 获取连接
conn1 = pool.get_connection()
conn2 = pool.get_connection()
conn3 = pool.get_connection()
conn4 = pool.get_connection()  # 应该返回 None

print(f"  获取连接1: {conn1}")
print(f"  获取连接2: {conn2}")
print(f"  获取连接3: {conn3}")
print(f"  获取连接4: {conn4}")

# 释放连接
pool.release_connection(conn1)
print(f"  释放连接1后再次获取: {pool.get_connection()}")

# 关闭所有连接
pool.close_all()
print("  已关闭所有连接")
```

## 26.3 类型检查和工具

### 26.3.1 mypy 基础

```python
# 类型检查和工具
print("类型检查和工具:")

# 注意：以下代码需要在安装了 mypy 的环境中运行
# 可以通过命令行运行: mypy filename.py

# 1. 基本类型检查示例
def add_numbers(a: int, b: int) -> int:
    """两个整数相加"""
    return a + b

def greet(name: str) -> str:
    """问候函数"""
    return f"Hello, {name}!"

# 这些调用在 mypy 中会通过类型检查
result1 = add_numbers(5, 3)  # 正确
greeting = greet("Alice")    # 正确

# 这些调用在 mypy 中会产生类型错误
# result2 = add_numbers("5", "3")  # 类型错误：期望 int，得到 str
# greeting2 = greet(123)          # 类型错误：期望 str，得到 int

print("基本类型检查示例:")
print(f"  add_numbers(5, 3): {result1}")
print(f"  greet('Alice'): {greeting}")

# 2. 类型忽略注释
from typing import Any

def process_data(data: Any) -> Any:
    """处理任意类型的数据"""
    # 使用类型忽略注释来抑制 mypy 警告
    result = data.upper()  # type: ignore
    return result

# 3. 类型断言
def safe_cast(value: Any, target_type: type) -> Any:
    """安全的类型转换"""
    if isinstance(value, target_type):
        return value
    else:
        raise TypeError(f"Cannot cast {type(value)} to {target_type}")

# 使用类型断言
def get_user_name(user_data: dict) -> str:
    """获取用户名"""
    name = user_data.get('name')
    if isinstance(name, str):
        return name
    else:
        return "Unknown"

print(f"\n类型断言:")
user = {'name': 'Alice', 'age': 30}
print(f"  用户名: {get_user_name(user)}")

invalid_user = {'name': 123, 'age': 30}
print(f"  无效用户名: {get_user_name(invalid_user)}")

# 4. 类型保护函数
def is_string(value: Any) -> bool:
    """类型保护：检查是否为字符串"""
    return isinstance(value, str)

def process_string_safe(value: Any) -> str:
    """安全处理字符串"""
    if is_string(value):
        # 在这个分支中，mypy 知道 value 是 str 类型
        return value.upper()
    else:
        return str(value)

print(f"\n类型保护:")
print(f"  process_string_safe('hello'): {process_string_safe('hello')}")
print(f"  process_string_safe(123): {process_string_safe(123)}")

# 5. 渐进式类型检查
# 使用 # type: ignore 来忽略特定行的类型检查
def legacy_function(param):  # type: ignore
    """遗留函数，没有类型提示"""
    return param * 2

# 部分类型化的函数
def partially_typed(a, b: int) -> int:  # type: ignore
    """部分类型化的函数"""
    return a + b

print(f"\n渐进式类型检查:")
print(f"  legacy_function(5): {legacy_function(5)}")
print(f"  partially_typed(3, 4): {partially_typed(3, 4)}")

# 6. 类型存根文件
# 类型存根文件（.pyi）可以为没有类型提示的模块提供类型信息
# 示例存根文件内容：
"""
# example.pyi
def legacy_function(param: Any) -> Any: ...
def another_function(x: int, y: str) -> bool: ...
"""

# 7. 配置 mypy
# mypy 可以通过配置文件进行配置
# 示例 mypy.ini 内容：
"""
[mypy]
python_version = 3.11
warn_return_any = True
warn_unused_configs = True
disallow_untyped_defs = True
disallow_incomplete_defs = True
check_untyped_defs = True
disallow_untyped_decorators = True
no_implicit_optional = True
warn_redundant_casts = True
warn_unused_ignores = True
warn_no_return = True
warn_unreachable = True
strict_equality = True
"""

print(f"\n类型检查配置:")
print("  mypy 可以通过以下方式配置：")
print("  1. 命令行参数: mypy --strict filename.py")
print("  2. 配置文件: mypy.ini 或 setup.cfg")
print("  3. pyproject.toml 文件")
```

### 26.3.2 类型检查的最佳实践

```python
# 类型检查的最佳实践
print("类型检查的最佳实践:")

from typing import List, Dict, Optional, Union, Any, cast
import time

# 1. 使用显式类型而不是 Any
# 错误的方式
def process_bad(data: Any) -> Any:
    """不好的做法：使用 Any"""
    return data

# 正确的方式
def process_good(data: Union[str, int, float]) -> str:
    """好的做法：使用具体的联合类型"""
    if isinstance(data, str):
        return data.upper()
    elif isinstance(data, (int, float)):
        return f"Number: {data}"
    else:
        return "Unknown"

print("使用显式类型:")
print(f"  process_good('hello'): {process_good('hello')}")
print(f"  process_good(42): {process_good(42)}")

# 2. 使用 Optional 而不是 Union[T, None]
# 错误的方式
def find_item_bad(items: List[str], target: str) -> Union[str, None]:
    """不好的做法"""
    for item in items:
        if item == target:
            return item
    return None

# 正确的方式
def find_item_good(items: List[str], target: str) -> Optional[str]:
    """好的做法"""
    for item in items:
        if item == target:
            return item
    return None

print(f"\n使用 Optional:")
items = ['apple', 'banana', 'cherry']
print(f"  find_item_good: {find_item_good(items, 'banana')}")
print(f"  find_item_good: {find_item_good(items, 'grape')}")

# 3. 使用类型别名简化复杂类型
# 类型别名
UserId = int
UserName = str
UserEmail = str
UserData = Dict[str, Union[UserName, UserEmail, int]]

def create_user(user_id: UserId, name: UserName, email: UserEmail) -> UserData:
    """创建用户"""
    return {
        'id': user_id,
        'name': name,
        'email': email,
        'created_at': int(time.time())
    }

print(f"\n类型别名:")
user = create_user(1, "Alice", "alice@example.com")
print(f"  创建用户: {user}")

# 4. 使用 cast() 进行类型断言
def get_config_value(config: Dict[str, Any], key: str) -> Optional[str]:
    """获取配置值"""
    value = config.get(key)
    if value is not None and isinstance(value, str):
        return value
    return None

def get_config_value_cast(config: Dict[str, Any], key: str) -> Optional[str]:
    """使用 cast 的版本"""
    value = config.get(key)
    if value is not None:
        # 使用 cast 告诉 mypy 这是一个字符串
        return cast(str, value)
    return None

print(f"\n类型断言:")
config = {'app_name': 'MyApp', 'version': 1.0}
print(f"  get_config_value: {get_config_value(config, 'app_name')}")
print(f"  get_config_value_cast: {get_config_value_cast(config, 'app_name')}")

# 5. 避免过度使用类型忽略
# 好的做法：修复类型问题
def calculate_average_good(numbers: List[Union[int, float]]) -> float:
    """计算平均值 - 好的做法"""
    if not numbers:
        return 0.0
    return sum(numbers) / len(numbers)

# 不好的做法：使用类型忽略
def calculate_average_bad(numbers):  # type: ignore
    """计算平均值 - 不好的做法"""
    if not numbers:
        return 0.0
    return sum(numbers) / len(numbers)

print(f"\n避免过度使用类型忽略:")
numbers = [1, 2, 3, 4, 5]
print(f"  calculate_average_good: {calculate_average_good(numbers)}")
print(f"  calculate_average_bad: {calculate_average_bad(numbers)}")

# 6. 使用协议（Protocol）定义接口
from typing import Protocol

class Drawable(Protocol):
    """可绘制对象的协议"""

    def draw(self) -> None:
        """绘制方法"""
        ...

    def get_area(self) -> float:
        """获取面积"""
        ...

class Circle:
    """圆形"""

    def __init__(self, radius: float):
        self.radius = radius

    def draw(self) -> None:
        print(f"绘制圆形，半径: {self.radius}")

    def get_area(self) -> float:
        return 3.14159 * self.radius * self.radius

class Rectangle:
    """矩形"""

    def __init__(self, width: float, height: float):
        self.width = width
        self.height = height

    def draw(self) -> None:
        print(f"绘制矩形，宽: {self.width}, 高: {self.height}")

    def get_area(self) -> float:
        return self.width * self.height

def render_shape(shape: Drawable) -> None:
    """渲染形状"""
    shape.draw()
    print(f"面积: {shape.get_area()}")

print(f"\n使用协议定义接口:")
circle = Circle(5.0)
rectangle = Rectangle(4.0, 3.0)

render_shape(circle)
render_shape(rectangle)

# 7. 泛型约束
from typing import TypeVar

T = TypeVar('T')
NumberT = TypeVar('NumberT', int, float, complex)

def add_generic(a: T, b: T) -> T:
    """泛型加法（类型不安全）"""
    return a + b  # type: ignore

def add_numbers(a: NumberT, b: NumberT) -> NumberT:
    """数值加法（类型安全）"""
    return a + b

print(f"\n泛型约束:")
print(f"  add_generic('a', 'b'): {add_generic('a', 'b')}")
print(f"  add_numbers(3, 4): {add_numbers(3, 4)}")
print(f"  add_numbers(3.5, 2.1): {add_numbers(3.5, 2.1)}")

# 8. 文档字符串和类型提示结合
def process_user_data(
    user_data: Dict[str, Any],
    validate_email: bool = True,
    normalize_names: bool = True
) -> Dict[str, Any]:
    """
    处理用户数据。

    Args:
        user_data: 用户数据字典
        validate_email: 是否验证邮箱格式
        normalize_names: 是否规范化姓名

    Returns:
        处理后的用户数据字典

    Raises:
        ValueError: 当数据无效时抛出

    Example:
        >>> data = {'name': 'alice', 'email': 'alice@example.com'}
        >>> result = process_user_data(data)
        >>> print(result['name'])
        Alice
    """
    result = user_data.copy()

    # 规范化姓名
    if normalize_names and 'name' in result:
        result['name'] = result['name'].title()

    # 验证邮箱
    if validate_email and 'email' in result:
        email = result['email']
        if not isinstance(email, str) or '@' not in email:
            raise ValueError("无效的邮箱格式")

    return result

print(f"\n文档字符串和类型提示结合:")
user_data = {'name': 'alice', 'email': 'alice@example.com', 'age': 30}
processed = process_user_data(user_data)
print(f"  处理结果: {processed}")

# 9. 类型检查的性能考虑
def expensive_operation(n: int) -> int:
    """耗时的操作"""
    time.sleep(0.001)  # 模拟耗时
    return n * n

def process_with_type_check(items: List[int]) -> List[int]:
    """带类型检查的处理"""
    result = []
    for item in items:
        if isinstance(item, int):  # 运行时类型检查
            result.append(expensive_operation(item))
    return result

def process_without_type_check(items: List[int]) -> List[int]:
    """不带类型检查的处理（假设输入正确）"""
    return [expensive_operation(item) for item in items]

print(f"\n类型检查的性能考虑:")
test_data = list(range(100))

start_time = time.time()
result1 = process_with_type_check(test_data)
time1 = time.time() - start_time

start_time = time.time()
result2 = process_without_type_check(test_data)
time2 = time.time() - start_time

print(f"  带类型检查: {time1:.4f}秒")
print(f"  不带类型检查: {time2:.4f}秒")
print(f"  结果是否相同: {result1 == result2}")

# 10. 渐进式采用类型提示
# 在现有代码库中逐步添加类型提示

# 第一阶段：添加基本类型提示
def legacy_function_1(param):  # type: ignore
    """第一阶段：保持原有函数不变"""
    return param * 2

# 第二阶段：为新函数添加类型提示
def new_function(param: int) -> int:
    """第二阶段：新函数使用类型提示"""
    return param * 2

# 第三阶段：重构现有函数
def refactored_function(param: int) -> int:
    """第三阶段：重构后的函数"""
    if not isinstance(param, int):
        raise TypeError("param must be int")
    return param * 2

print(f"\n渐进式采用类型提示:")
print(f"  legacy_function_1(5): {legacy_function_1(5)}")
print(f"  new_function(5): {new_function(5)}")
print(f"  refactored_function(5): {refactored_function(5)}")
```

## 总结

本章介绍了 Python 的类型提示系统：
- 掌握类型提示的基础语法，包括变量、函数和集合类型的类型提示
- 学会使用泛型类型和类型变量创建可重用的类型化代码
- 理解类型检查工具 mypy 的使用和配置
- 掌握类型提示在实际项目中的最佳实践

通过本章的学习，你应该能够：
1. 为 Python 代码添加适当的类型提示，提高代码可读性和可维护性
2. 使用泛型创建类型安全的可重用组件
3. 配置和使用 mypy 进行静态类型检查
4. 在现有代码库中渐进式采用类型提示
5. 编写类型安全的高质量 Python 代码

下一章将介绍命令行工具的开发。
