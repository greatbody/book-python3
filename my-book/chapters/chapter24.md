# 第24章：生成器和迭代器

## 24.1 生成器函数

### 24.1.1 生成器基础

```python
# 生成器基础
print("生成器基础:")

# 1. 生成器函数 vs 普通函数
def normal_function():
    """普通函数"""
    return [1, 2, 3, 4, 5]

def generator_function():
    """生成器函数"""
    yield 1
    yield 2
    yield 3
    yield 4
    yield 5

print("普通函数 vs 生成器函数:")

# 普通函数
result = normal_function()
print(f"普通函数返回: {result}, 类型: {type(result)}")

# 生成器函数
generator = generator_function()
print(f"生成器对象: {generator}, 类型: {type(generator)}")

# 获取生成器的值
values = list(generator)
print(f"生成器值: {values}")

# 2. 生成器的惰性求值
def lazy_generator():
    """演示惰性求值"""
    print("开始生成第一个值")
    yield 1
    print("开始生成第二个值")
    yield 2
    print("开始生成第三个值")
    yield 3
    print("生成器结束")

print("\n生成器的惰性求值:")
gen = lazy_generator()

print("创建生成器后，还没有执行任何代码")

# 第一次调用 next()
value1 = next(gen)
print(f"第一次获取: {value1}")

# 第二次调用 next()
value2 = next(gen)
print(f"第二次获取: {value2}")

# 第三次调用 next()
value3 = next(gen)
print(f"第三次获取: {value3}")

# 第四次调用 next() 会抛出 StopIteration
try:
    value4 = next(gen)
except StopIteration:
    print("生成器已耗尽")

# 3. 使用 for 循环遍历生成器
def countdown(n):
    """倒计时生成器"""
    while n > 0:
        yield n
        n -= 1
    yield "发射！"

print("\n使用 for 循环遍历生成器:")
for item in countdown(5):
    print(f"倒计时: {item}")

# 4. 生成器的内存效率
def memory_comparison():
    """内存效率比较"""

    # 普通方式：创建大列表
    print("创建大列表...")
    big_list = [x**2 for x in range(1000000)]
    print(f"大列表长度: {len(big_list)}")
    print(f"大列表占用内存: {big_list.__sizeof__()} bytes")

    # 生成器方式
    print("\n创建生成器...")
    big_generator = (x**2 for x in range(1000000))
    print(f"生成器对象: {big_generator}")
    print(f"生成器占用内存: {big_generator.__sizeof__()} bytes")

    # 只获取前10个值
    print("\n获取前10个值:")
    for i, value in enumerate(big_generator):
        if i >= 10:
            break
        print(f"  {i}: {value}")

print("\n内存效率比较:")
memory_comparison()
```

### 24.1.2 生成器的高级用法

```python
# 生成器的高级用法
print("生成器的高级用法:")

# 1. 生成器与 return 语句
def generator_with_return():
    """带有 return 的生成器"""
    yield 1
    yield 2
    return "生成器结束"
    yield 3  # 这行不会执行

print("生成器与 return 语句:")
gen = generator_with_return()

try:
    while True:
        value = next(gen)
        print(f"值: {value}")
except StopIteration as e:
    print(f"生成器返回值: {e.value}")

# 2. 生成器的 send() 方法
def echo_generator():
    """回声生成器"""
    while True:
        received = yield
        print(f"收到: {received}")

print("\n生成器的 send() 方法:")
echo_gen = echo_generator()

# 启动生成器
next(echo_gen)  # 或者 echo_gen.send(None)

# 发送值
echo_gen.send("Hello")
echo_gen.send("World")
echo_gen.send("Python")

# 3. 生成器的 throw() 方法
def robust_generator():
    """健壮的生成器"""
    try:
        while True:
            value = yield
            print(f"处理值: {value}")
    except ValueError as e:
        print(f"捕获到异常: {e}")
        yield "异常已处理"
    except GeneratorExit:
        print("生成器被关闭")
        return

print("\n生成器的 throw() 方法:")
robust_gen = robust_generator()
next(robust_gen)

robust_gen.send("正常值")
robust_gen.throw(ValueError("测试异常"))
robust_gen.send("异常后的值")

# 关闭生成器
robust_gen.close()

# 4. 生成器的 close() 方法
def closable_generator():
    """可关闭的生成器"""
    try:
        yield 1
        yield 2
        yield 3
    finally:
        print("生成器被关闭，执行清理工作")

print("\n生成器的 close() 方法:")
close_gen = closable_generator()

print(f"值1: {next(close_gen)}")
print(f"值2: {next(close_gen)}")

close_gen.close()  # 关闭生成器

# 5. 子生成器和委托生成器
def sub_generator():
    """子生成器"""
    yield "子生成器1"
    yield "子生成器2"
    return "子生成器返回值"

def delegating_generator():
    """委托生成器"""
    result = yield from sub_generator()
    print(f"子生成器返回值: {result}")
    yield "委托生成器1"
    yield "委托生成器2"

print("\n子生成器和委托生成器:")
del_gen = delegating_generator()

for value in del_gen:
    print(f"值: {value}")

# 6. 生成器的状态
def stateful_generator():
    """有状态的生成器"""
    total = 0
    count = 0

    while True:
        value = yield total
        if value is not None:
            total += value
            count += 1
            print(f"累加: {value}, 当前总数: {total}, 计数: {count}")

print("\n生成器的状态:")
state_gen = stateful_generator()
next(state_gen)  # 启动生成器

state_gen.send(10)
state_gen.send(20)
state_gen.send(5)

print(f"当前总数: {next(state_gen)}")
```

### 24.1.3 生成器的实际应用

```python
# 生成器的实际应用
print("生成器的实际应用:")

# 1. 读取大文件
def read_large_file(filename, chunk_size=1024):
    """逐块读取大文件"""
    try:
        with open(filename, 'r', encoding='utf-8') as file:
            while True:
                chunk = file.read(chunk_size)
                if not chunk:
                    break
                yield chunk
    except FileNotFoundError:
        yield f"文件 {filename} 不存在"

print("读取大文件:")
# 创建测试文件
with open('large_test.txt', 'w', encoding='utf-8') as f:
    for i in range(100):
        f.write(f"这是第{i+1}行数据\n")

# 使用生成器读取
for chunk in read_large_file('large_test.txt', chunk_size=50):
    print(f"读取块: {repr(chunk)}")
    break  # 只读取第一块

# 清理文件
import os
if os.path.exists('large_test.txt'):
    os.remove('large_test.txt')

# 2. 斐波那契数列生成器
def fibonacci_generator(limit=None):
    """斐波那契数列生成器"""
    a, b = 0, 1
    count = 0

    while limit is None or count < limit:
        yield a
        a, b = b, a + b
        count += 1

print("\n斐波那契数列生成器:")
fib_gen = fibonacci_generator(10)
for num in fib_gen:
    print(f"斐波那契数: {num}")

# 无限斐波那契数列
print("\n无限斐波那契数列 (前15个):")
fib_infinite = fibonacci_generator()
for i, num in enumerate(fib_infinite):
    if i >= 15:
        break
    print(f"F({i}) = {num}")

# 3. 素数生成器
def prime_generator():
    """素数生成器"""
    def is_prime(n):
        if n < 2:
            return False
        for i in range(2, int(n**0.5) + 1):
            if n % i == 0:
                return False
        return True

    num = 2
    while True:
        if is_prime(num):
            yield num
        num += 1

print("\n素数生成器:")
primes = prime_generator()
for i, prime in enumerate(primes):
    if i >= 10:
        break
    print(f"第{i+1}个素数: {prime}")

# 4. 数据管道
def data_pipeline(data_source):
    """数据处理管道"""

    # 步骤1: 过滤空值
    def filter_empty(data):
        for item in data:
            if item.strip():
                yield item

    # 步骤2: 转换为大写
    def to_upper(data):
        for item in data:
            yield item.upper()

    # 步骤3: 添加前缀
    def add_prefix(data, prefix):
        for item in data:
            yield f"{prefix}: {item}"

    # 组合管道
    filtered = filter_empty(data_source)
    uppered = to_upper(filtered)
    prefixed = add_prefix(uppered, "处理后")

    return prefixed

print("\n数据管道:")
raw_data = ["  hello  ", "", "world", "  ", "python", ""]
processed_data = data_pipeline(raw_data)

for item in processed_data:
    print(f"  {item}")

# 5. 模拟数据流
def data_stream_generator():
    """模拟实时数据流"""
    import time
    import random

    data_types = ["温度", "湿度", "压力", "流量"]
    sensors = ["传感器A", "传感器B", "传感器C"]

    count = 0
    while count < 20:  # 限制输出数量
        sensor = random.choice(sensors)
        data_type = random.choice(data_types)
        value = random.uniform(0, 100)

        yield {
            'timestamp': time.time(),
            'sensor': sensor,
            'type': data_type,
            'value': round(value, 2)
        }

        time.sleep(0.1)  # 模拟数据间隔
        count += 1

print("\n模拟数据流:")
for data_point in data_stream_generator():
    print(f"数据点: {data_point}")

# 6. 分页数据生成器
def paginated_data_generator(data, page_size):
    """分页数据生成器"""
    for i in range(0, len(data), page_size):
        page = data[i:i + page_size]
        yield {
            'page_number': i // page_size + 1,
            'page_size': len(page),
            'total_items': len(data),
            'data': page
        }

print("\n分页数据生成器:")
sample_data = list(range(1, 25))  # 1-24 的数字
pages = paginated_data_generator(sample_data, 5)

for page in pages:
    print(f"第{page['page_number']}页: {page['data']}")

# 7. 递归生成器
def tree_traversal_generator(root):
    """树遍历生成器"""
    yield root

    # 模拟子节点
    if root < 10:  # 限制深度
        for child in [root * 2, root * 2 + 1]:
            yield from tree_traversal_generator(child)

print("\n递归生成器 (树遍历):")
tree_gen = tree_traversal_generator(1)
for i, node in enumerate(tree_gen):
    if i >= 15:  # 限制输出
        break
    print(f"节点: {node}")

# 8. 协程风格的生成器
def coroutine_generator():
    """协程风格的生成器"""
    print("生成器启动")
    running_sum = 0

    try:
        while True:
            value = yield running_sum
            if value is not None:
                running_sum += value
                print(f"累加 {value}, 当前和: {running_sum}")
    except GeneratorExit:
        print(f"最终结果: {running_sum}")

print("\n协程风格的生成器:")
coroutine = coroutine_generator()
next(coroutine)  # 启动

coroutine.send(10)
coroutine.send(20)
coroutine.send(5)

coroutine.close()
```

## 24.2 生成器表达式

### 24.2.1 生成器表达式基础

```python
# 生成器表达式基础
print("生成器表达式基础:")

# 1. 列表推导式 vs 生成器表达式
print("列表推导式 vs 生成器表达式:")

# 列表推导式
list_comprehension = [x**2 for x in range(10)]
print(f"列表推导式: {list_comprehension}, 类型: {type(list_comprehension)}")

# 生成器表达式
generator_expression = (x**2 for x in range(10))
print(f"生成器表达式: {generator_expression}, 类型: {type(generator_expression)}")

# 获取值
gen_values = list(generator_expression)
print(f"生成器值: {gen_values}")

# 2. 内存效率比较
import sys

print("\n内存效率比较:")

# 大列表
big_list = [x for x in range(10000)]
print(f"大列表内存: {sys.getsizeof(big_list)} bytes")

# 生成器表达式
big_generator = (x for x in range(10000))
print(f"生成器内存: {sys.getsizeof(big_generator)} bytes")

# 3. 条件过滤
print("\n条件过滤:")

# 基本过滤
even_squares = (x**2 for x in range(20) if x % 2 == 0)
print(f"偶数平方: {list(even_squares)}")

# 多重条件
complex_filter = (x for x in range(50) if x % 3 == 0 and x % 5 == 0)
print(f"3和5的倍数: {list(complex_filter)}")

# 4. 嵌套生成器表达式
print("\n嵌套生成器表达式:")

# 矩阵转置
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

transposed = [[row[i] for row in matrix] for i in range(len(matrix[0]))]
print(f"列表推导式转置: {transposed}")

# 使用生成器表达式
transposed_gen = ([row[i] for row in matrix] for i in range(len(matrix[0])))
print(f"生成器表达式转置: {list(transposed_gen)}")

# 5. 多重循环
print("\n多重循环:")

# 笛卡尔积
colors = ['red', 'blue', 'green']
sizes = ['S', 'M', 'L']

combinations = (f"{color}-{size}" for color in colors for size in sizes)
print(f"颜色尺寸组合: {list(combinations)}")

# 条件多重循环
valid_combinations = (f"{color}-{size}" for color in colors
                     for size in sizes if not (color == 'red' and size == 'S'))
print(f"有效组合: {list(valid_combinations)}")

# 6. 函数调用中的生成器表达式
print("\n函数调用中的生成器表达式:")

def sum_squares(numbers):
    """计算平方和"""
    return sum(x**2 for x in numbers)

def find_max_filtered(data, condition):
    """查找满足条件的最大值"""
    return max((x for x in data if condition(x)), default=None)

numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

print(f"平方和: {sum_squares(numbers)}")
print(f"偶数最大值: {find_max_filtered(numbers, lambda x: x % 2 == 0)}")
print(f"奇数最大值: {find_max_filtered(numbers, lambda x: x % 2 == 1)}")

# 7. 生成器表达式的链式操作
print("\n生成器表达式的链式操作:")

data = range(1, 21)

# 链式处理
result = (x for x in data if x % 2 == 0)  # 过滤偶数
result = (x**2 for x in result)           # 平方
result = (x for x in result if x > 50)    # 过滤大于50的
result = (x // 10 for x in result)        # 除以10

print(f"链式处理结果: {list(result)}")

# 8. 生成器表达式的惰性求值
print("\n生成器表达式的惰性求值:")

def expensive_operation(x):
    """模拟耗时操作"""
    print(f"处理 {x}")
    return x * 2

# 列表推导式会立即执行
print("列表推导式:")
list_result = [expensive_operation(x) for x in range(5)]
print(f"结果: {list_result}")

# 生成器表达式延迟执行
print("\n生成器表达式:")
gen_result = (expensive_operation(x) for x in range(5))

print("生成器创建完成，还没有执行 expensive_operation")

# 只有在需要时才执行
print("开始遍历生成器:")
for value in gen_result:
    print(f"获取值: {value}")
```

### 24.2.2 生成器表达式的实际应用

```python
# 生成器表达式的实际应用
print("生成器表达式的实际应用:")

# 1. 文件处理
def process_large_file(filename):
    """处理大文件"""
    try:
        with open(filename, 'r', encoding='utf-8') as file:
            # 只处理非空行
            lines = (line.strip() for line in file if line.strip())
            # 转换为大写
            upper_lines = (line.upper() for line in lines)
            # 添加行号
            numbered_lines = (f"{i+1}: {line}" for i, line in enumerate(upper_lines))

            return numbered_lines
    except FileNotFoundError:
        return (f"文件 {filename} 不存在",)

print("文件处理:")
# 创建测试文件
with open('test_file.txt', 'w', encoding='utf-8') as f:
    f.write("Hello World\n")
    f.write("\n")
    f.write("Python Programming\n")
    f.write("   \n")
    f.write("Generator Expressions\n")

# 处理文件
processed_lines = process_large_file('test_file.txt')
for line in processed_lines:
    print(f"  {line}")

# 清理
if os.path.exists('test_file.txt'):
    os.remove('test_file.txt')

# 2. 数据转换管道
def data_transformation_pipeline(data):
    """数据转换管道"""

    # 步骤1: 清理数据
    cleaned = (item.strip() for item in data if item.strip())

    # 步骤2: 类型转换
    def try_convert(value):
        try:
            return int(value)
        except ValueError:
            try:
                return float(value)
            except ValueError:
                return value

    converted = (try_convert(item) for item in cleaned)

    # 步骤3: 过滤和转换
    processed = (x * 2 if isinstance(x, (int, float)) else len(x) for x in converted)

    return processed

print("\n数据转换管道:")
raw_data = ["  123  ", "", "45.67", "hello", "  ", "89"]
transformed = data_transformation_pipeline(raw_data)
print(f"原始数据: {raw_data}")
print(f"转换结果: {list(transformed)}")

# 3. 数据库查询模拟
def simulate_database_query(table, conditions=None):
    """模拟数据库查询"""

    # 模拟数据
    mock_data = [
        {"id": 1, "name": "Alice", "age": 30, "city": "北京"},
        {"id": 2, "name": "Bob", "age": 25, "city": "上海"},
        {"id": 3, "name": "Charlie", "age": 35, "city": "北京"},
        {"id": 4, "name": "Diana", "age": 28, "city": "深圳"},
    ]

    # 应用条件过滤
    if conditions:
        filtered = (row for row in mock_data if all(
            row.get(key) == value for key, value in conditions.items()
        ))
    else:
        filtered = (row for row in mock_data)

    return filtered

print("\n数据库查询模拟:")
# 查询所有北京用户
beijing_users = simulate_database_query("users", {"city": "北京"})
print("北京用户:")
for user in beijing_users:
    print(f"  {user}")

# 查询年龄大于30的用户
older_users = simulate_database_query("users", {"age": lambda x: x > 30})
print("\n年龄大于30的用户:")
for user in older_users:
    print(f"  {user}")

# 4. 实时数据过滤
def real_time_filter(data_stream, filters):
    """实时数据过滤"""

    # 应用所有过滤器
    filtered = data_stream

    for filter_func in filters:
        filtered = (item for item in filtered if filter_func(item))

    return filtered

print("\n实时数据过滤:")
# 模拟数据流
data_stream = (x for x in range(1, 101))

# 定义过滤器
filters = [
    lambda x: x % 2 == 0,      # 偶数
    lambda x: x % 3 == 0,      # 3的倍数
    lambda x: x > 20,          # 大于20
    lambda x: x < 80,          # 小于80
]

filtered_stream = real_time_filter(data_stream, filters)
print(f"过滤结果: {list(filtered_stream)}")

# 5. 无限序列处理
def infinite_sequence_processor():
    """无限序列处理器"""

    # 生成无限序列
    numbers = (x for x in range(1, float('inf')))

    # 应用处理
    processed = (x**2 for x in numbers if x % 2 == 0 and x % 3 == 0)

    return processed

print("\n无限序列处理:")
processor = infinite_sequence_processor()

print("前10个满足条件的值:")
for i, value in enumerate(processor):
    if i >= 10:
        break
    print(f"  {i+1}: {value}")

# 6. 内存高效的文本搜索
def search_in_files(file_patterns, search_term):
    """在文件中搜索文本"""

    import glob

    # 查找匹配的文件
    files = (f for pattern in file_patterns for f in glob.glob(pattern))

    # 读取文件内容
    def read_file_lines(filename):
        try:
            with open(filename, 'r', encoding='utf-8') as f:
                return ((filename, line_num, line.strip())
                       for line_num, line in enumerate(f, 1))
        except (FileNotFoundError, UnicodeDecodeError):
            return []

    # 搜索匹配的行
    matching_lines = (file_info for filename in files
                     for file_info in read_file_lines(filename)
                     if search_term.lower() in file_info[2].lower())

    return matching_lines

print("\n内存高效的文本搜索:")
# 创建测试文件
with open('search_test1.txt', 'w', encoding='utf-8') as f:
    f.write("Hello World\n")
    f.write("Python is great\n")
    f.write("Search functionality\n")

with open('search_test2.txt', 'w', encoding='utf-8') as f:
    f.write("Another file\n")
    f.write("More content here\n")
    f.write("Python programming\n")

# 搜索
results = search_in_files(['search_test*.txt'], 'python')
print("搜索结果:")
for filename, line_num, line in results:
    print(f"  {filename}:{line_num} - {line}")

# 清理
for f in ['search_test1.txt', 'search_test2.txt']:
    if os.path.exists(f):
        os.remove(f)

# 7. 流式数据聚合
def streaming_aggregator(data_stream, window_size):
    """流式数据聚合"""

    # 创建滑动窗口
    def sliding_window(iterable, size):
        iterators = [iter(iterable) for _ in range(size)]
        for i in range(size):
            for j in range(i):
                next(iterators[j], None)

        while True:
            window = []
            for it in iterators:
                value = next(it, None)
                if value is None:
                    return
                window.append(value)
            yield window

    # 计算窗口统计
    windows = sliding_window(data_stream, window_size)
    stats = ((min(window), max(window), sum(window) / len(window))
             for window in windows)

    return stats

print("\n流式数据聚合:")
data = (x for x in range(1, 21))
aggregator = streaming_aggregator(data, 5)

print("滑动窗口统计 (最小值, 最大值, 平均值):")
for i, (min_val, max_val, avg_val) in enumerate(aggregator):
    print(f"  窗口{i+1}: 最小={min_val}, 最大={max_val}, 平均={avg_val:.2f}")

# 8. 惰性配置文件解析
def lazy_config_parser(config_lines):
    """惰性配置文件解析器"""

    # 解析配置行
    def parse_line(line):
        line = line.strip()
        if not line or line.startswith('#'):
            return None
        if '=' in line:
            key, value = line.split('=', 1)
            return ('config', key.strip(), value.strip())
        return ('comment', line)

    # 过滤有效配置
    parsed_lines = (parse_line(line) for line in config_lines)
    valid_configs = (item for item in parsed_lines if item and item[0] == 'config')

    # 转换为字典
    config_dict = {key: value for _, key, value in valid_configs}

    return config_dict

print("\n惰性配置文件解析:")
config_lines = [
    "# Database configuration",
    "db_host = localhost",
    "db_port = 5432",
    "",
    "# Application settings",
    "app_name = MyApp",
    "debug = true",
    "# This is a comment"
]

config = lazy_config_parser(config_lines)
print(f"解析后的配置: {config}")
```

## 24.3 迭代器协议

### 24.3.1 迭代器协议基础

```python
# 迭代器协议基础
print("迭代器协议基础:")

# 1. 可迭代对象 vs 迭代器
print("可迭代对象 vs 迭代器:")

# 列表是可迭代对象
my_list = [1, 2, 3, 4, 5]
print(f"列表: {my_list}")
print(f"列表有 __iter__: {hasattr(my_list, '__iter__')}")
print(f"列表有 __next__: {hasattr(my_list, '__next__')}")

# 获取列表的迭代器
list_iterator = iter(my_list)
print(f"列表迭代器: {list_iterator}")
print(f"迭代器有 __iter__: {hasattr(list_iterator, '__iter__')}")
print(f"迭代器有 __next__: {hasattr(list_iterator, '__next__')}")

# 2. 手动实现迭代器协议
class SimpleIterator:
    """简单的迭代器实现"""

    def __init__(self, data):
        self.data = data
        self.index = 0

    def __iter__(self):
        """返回迭代器本身"""
        return self

    def __next__(self):
        """返回下一个元素"""
        if self.index >= len(self.data):
            raise StopIteration

        value = self.data[self.index]
        self.index += 1
        return value

print("\n手动实现迭代器协议:")
simple_iter = SimpleIterator([10, 20, 30, 40, 50])

print("遍历迭代器:")
for value in simple_iter:
    print(f"  {value}")

# 再次遍历会失败，因为迭代器已耗尽
print("再次遍历:")
try:
    for value in simple_iter:
        print(f"  {value}")
except:
    print("  迭代器已耗尽")

# 3. 可迭代对象（非迭代器）
class SimpleIterable:
    """简单的可迭代对象"""

    def __init__(self, data):
        self.data = data

    def __iter__(self):
        """返回新的迭代器"""
        return SimpleIterator(self.data)

print("\n可迭代对象:")
iterable = SimpleIterable([100, 200, 300, 400, 500])

print("第一次遍历:")
for value in iterable:
    print(f"  {value}")

print("第二次遍历:")
for value in iterable:
    print(f"  {value}")

# 4. 使用 iter() 和 next()
print("\n使用 iter() 和 next():")
data = [1, 2, 3, 4, 5]
iterator = iter(data)

try:
    while True:
        value = next(iterator)
        print(f"值: {value}")
except StopIteration:
    print("迭代完成")

# 5. 内置可迭代对象
print("\n内置可迭代对象:")

# 字符串
string_iter = iter("Hello")
print(f"字符串迭代器: {list(string_iter)}")

# 字典
dict_iter = iter({"a": 1, "b": 2, "c": 3})
print(f"字典键迭代器: {list(dict_iter)}")

# 集合
set_iter = iter({1, 2, 3, 4, 5})
print(f"集合迭代器: {list(set_iter)}")

# 文件对象
with open('temp_iter_test.txt', 'w') as f:
    f.write("Line 1\nLine 2\nLine 3\n")

with open('temp_iter_test.txt', 'r') as f:
    file_iter = iter(f)
    print(f"文件迭代器: {[line.strip() for line in file_iter]}")

if os.path.exists('temp_iter_test.txt'):
    os.remove('temp_iter_test.txt')

# 6. 迭代器的特性
print("\n迭代器的特性:")

# 迭代器是一次性的
numbers = [1, 2, 3, 4, 5]
iter1 = iter(numbers)
iter2 = iter(numbers)

print(f"iter1 第一次: {list(iter1)}")
print(f"iter2 第一次: {list(iter2)}")
print(f"iter1 第二次: {list(iter1)}")  # 空列表
print(f"iter2 第二次: {list(iter2)}")  # 空列表

# 可迭代对象可以多次迭代
print(f"原始列表第一次: {list(numbers)}")
print(f"原始列表第二次: {list(numbers)}")
```

### 24.3.2 自定义迭代器

```python
# 自定义迭代器
print("自定义迭代器:")

# 1. 斐波那契迭代器
class FibonacciIterator:
    """斐波那契数列迭代器"""

    def __init__(self, max_value=None):
        self.max_value = max_value
        self.a, self.b = 0, 1
        self.count = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.max_value is not None and self.a > self.max_value:
            raise StopIteration

        value = self.a
        self.a, self.b = self.b, self.a + self.b
        self.count += 1

        return value

print("斐波那契迭代器:")
fib_iter = FibonacciIterator(max_value=100)
for num in fib_iter:
    print(f"  {num}")

# 2. 反向迭代器
class ReverseIterator:
    """反向迭代器"""

    def __init__(self, data):
        self.data = data
        self.index = len(data)

    def __iter__(self):
        return self

    def __next__(self):
        if self.index <= 0:
            raise StopIteration

        self.index -= 1
        return self.data[self.index]

print("\n反向迭代器:")
original = [1, 2, 3, 4, 5]
reverse_iter = ReverseIterator(original)
print(f"原始: {original}")
print(f"反向: {list(reverse_iter)}")

# 3. 步进迭代器
class StepIterator:
    """步进迭代器"""

    def __init__(self, start, end, step=1):
        self.current = start
        self.end = end
        self.step = step

    def __iter__(self):
        return self

    def __next__(self):
        if (self.step > 0 and self.current >= self.end) or \
           (self.step < 0 and self.current <= self.end):
            raise StopIteration

        value = self.current
        self.current += self.step
        return value

print("\n步进迭代器:")
step_iter = StepIterator(0, 20, 3)
print(f"步进3: {list(step_iter)}")

negative_step = StepIterator(10, 0, -2)
print(f"负步进: {list(negative_step)}")

# 4. 循环迭代器
class CycleIterator:
    """循环迭代器"""

    def __init__(self, data, max_cycles=None):
        self.data = data
        self.max_cycles = max_cycles
        self.index = 0
        self.cycle_count = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.max_cycles is not None and self.cycle_count >= self.max_cycles:
            raise StopIteration

        if self.index >= len(self.data):
            self.index = 0
            self.cycle_count += 1

        if self.max_cycles is not None and self.cycle_count >= self.max_cycles:
            raise StopIteration

        value = self.data[self.index]
        self.index += 1
        return value

print("\n循环迭代器:")
cycle_iter = CycleIterator(['A', 'B', 'C'], max_cycles=2)
print(f"循环2次: {list(cycle_iter)}")

# 5. 过滤迭代器
class FilterIterator:
    """过滤迭代器"""

    def __init__(self, data, predicate):
        self.data = data
        self.predicate = predicate
        self.index = 0

    def __iter__(self):
        return self

    def __next__(self):
        while self.index < len(self.data):
            value = self.data[self.index]
            self.index += 1
            if self.predicate(value):
                return value
        raise StopIteration

print("\n过滤迭代器:")
numbers = list(range(1, 21))
even_filter = FilterIterator(numbers, lambda x: x % 2 == 0)
print(f"偶数: {list(even_filter)}")

large_filter = FilterIterator(numbers, lambda x: x > 15)
print(f"大数: {list(large_filter)}")

# 6. 链式迭代器
class ChainIterator:
    """链式迭代器"""

    def __init__(self, *iterables):
        self.iterables = iterables
        self.current_iter = None
        self.iterable_index = 0

    def __iter__(self):
        return self

    def __next__(self):
        # 如果当前迭代器为空或耗尽，获取下一个
        while self.current_iter is None or self.current_iter is StopIteration:
            if self.iterable_index >= len(self.iterables):
                raise StopIteration

            self.current_iter = iter(self.iterables[self.iterable_index])
            self.iterable_index += 1

        # 获取下一个值
        try:
            return next(self.current_iter)
        except StopIteration:
            self.current_iter = StopIteration
            return self.__next__()

print("\n链式迭代器:")
list1 = [1, 2, 3]
list2 = ['a', 'b', 'c']
list3 = [4, 5, 6]

chain_iter = ChainIterator(list1, list2, list3)
print(f"链式: {list(chain_iter)}")

# 7. 窗口迭代器
class WindowIterator:
    """窗口迭代器"""

    def __init__(self, data, window_size):
        self.data = data
        self.window_size = window_size
        self.index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.index + self.window_size > len(self.data):
            raise StopIteration

        window = self.data[self.index:self.index + self.window_size]
        self.index += 1
        return window

print("\n窗口迭代器:")
data = list(range(1, 11))
window_iter = WindowIterator(data, 3)
print("滑动窗口:")
for window in window_iter:
    print(f"  {window}")

# 8. 记忆化迭代器
class MemoizedIterator:
    """记忆化迭代器"""

    def __init__(self, iterable):
        self.iterable = iterable
        self.cache = []
        self.iterator = iter(iterable)
        self.exhausted = False

    def __iter__(self):
        return self

    def __next__(self):
        if not self.exhausted:
            try:
                value = next(self.iterator)
                self.cache.append(value)
                return value
            except StopIteration:
                self.exhausted = True

        # 如果已经耗尽，返回缓存中的值
        if self.cache:
            # 这里简化实现，实际应该支持索引访问
            raise StopIteration

        raise StopIteration

print("\n记忆化迭代器:")
memo_iter = MemoizedIterator(range(1, 6))
print(f"第一次遍历: {list(memo_iter)}")

# 重新创建迭代器来模拟第二次遍历
memo_iter2 = MemoizedIterator(range(1, 6))
print(f"第二次遍历: {list(memo_iter2)}")
```

### 24.3.3 迭代器协议的实际应用

```python
# 迭代器协议的实际应用
print("迭代器协议的实际应用:")

# 1. 数据库结果集迭代器
class DatabaseResultIterator:
    """数据库结果集迭代器"""

    def __init__(self, query, batch_size=100):
        self.query = query
        self.batch_size = batch_size
        self.offset = 0
        self.current_batch = []
        self.batch_index = 0

    def __iter__(self):
        return self

    def __next__(self):
        # 如果当前批次已耗尽，获取下一批次
        if self.batch_index >= len(self.current_batch):
            self._fetch_next_batch()
            self.batch_index = 0

        if self.batch_index >= len(self.current_batch):
            raise StopIteration

        result = self.current_batch[self.batch_index]
        self.batch_index += 1
        return result

    def _fetch_next_batch(self):
        """模拟获取下一批数据"""
        # 模拟数据库查询
        start = self.offset
        end = start + self.batch_size

        # 模拟数据
        if start >= 1000:  # 总共1000条记录
            self.current_batch = []
        else:
            self.current_batch = [
                {"id": i, "name": f"User_{i}", "email": f"user{i}@example.com"}
                for i in range(start, min(end, 1000))
            ]

        self.offset = end

print("数据库结果集迭代器:")
db_iter = DatabaseResultIterator("SELECT * FROM users", batch_size=5)

print("获取前15条记录:")
for i, record in enumerate(db_iter):
    if i >= 15:
        break
    print(f"  记录{i+1}: {record}")

# 2. 文件行迭代器
class FileLineIterator:
    """文件行迭代器"""

    def __init__(self, filename, encoding='utf-8'):
        self.filename = filename
        self.encoding = encoding
        self.file = None
        self.line_number = 0

    def __iter__(self):
        self.file = open(self.filename, 'r', encoding=self.encoding)
        self.line_number = 0
        return self

    def __next__(self):
        if self.file is None:
            raise StopIteration

        line = self.file.readline()
        if not line:
            self.file.close()
            self.file = None
            raise StopIteration

        self.line_number += 1
        return {
            'line_number': self.line_number,
            'content': line.rstrip('\n\r'),
            'filename': self.filename
        }

    def __del__(self):
        if self.file:
            self.file.close()

print("\n文件行迭代器:")
# 创建测试文件
with open('test_lines.txt', 'w', encoding='utf-8') as f:
    for i in range(10):
        f.write(f"这是第{i+1}行\n")

file_iter = FileLineIterator('test_lines.txt')
for line_info in file_iter:
    print(f"  {line_info}")

if os.path.exists('test_lines.txt'):
    os.remove('test_lines.txt')

# 3. 树形结构迭代器
class TreeIterator:
    """树形结构迭代器"""

    def __init__(self, root, traversal='preorder'):
        self.root = root
        self.traversal = traversal
        self.stack = []

    def __iter__(self):
        if self.traversal == 'preorder':
            self.stack = [self.root]
        return self

    def __next__(self):
        if not self.stack:
            raise StopIteration

        node = self.stack.pop()

        # 模拟树节点
        if hasattr(node, 'children'):
            # 后进先出，所以反转添加
            for child in reversed(node.children):
                self.stack.append(child)

        return node

print("\n树形结构迭代器:")
# 模拟树节点
class TreeNode:
    def __init__(self, value, children=None):
        self.value = value
        self.children = children or []

    def __repr__(self):
        return f"TreeNode({self.value})"

# 构建树
root = TreeNode(1, [
    TreeNode(2, [TreeNode(4), TreeNode(5)]),
    TreeNode(3, [TreeNode(6), TreeNode(7)])
])

tree_iter = TreeIterator(root)
print("前序遍历:")
for node in tree_iter:
    print(f"  {node}")

# 4. 异步数据流迭代器
class AsyncDataStreamIterator:
    """异步数据流迭代器"""

    def __init__(self, data_source, chunk_size=10):
        self.data_source = data_source
        self.chunk_size = chunk_size
        self.index = 0

    def __iter__(self):
        return self

    def __next__(self):
        # 模拟异步数据获取
        import time
        time.sleep(0.01)  # 模拟网络延迟

        start = self.index
        end = start + self.chunk_size

        if start >= len(self.data_source):
            raise StopIteration

        chunk = self.data_source[start:end]
        self.index = end

        return chunk

print("\n异步数据流迭代器:")
large_data = list(range(1, 101))
stream_iter = AsyncDataStreamIterator(large_data, chunk_size=10)

print("分块获取数据:")
for i, chunk in enumerate(stream_iter):
    print(f"  块{i+1}: {chunk}")
    if i >= 4:  # 只显示前5块
        print("  ...")
        break

# 5. 配置解析迭代器
class ConfigParserIterator:
    """配置解析迭代器"""

    def __init__(self, config_lines):
        self.config_lines = config_lines
        self.index = 0

    def __iter__(self):
        return self

    def __next__(self):
        while self.index < len(self.config_lines):
            line = self.config_lines[self.index]
            self.index += 1

            line = line.strip()

            # 跳过空行和注释
            if not line or line.startswith('#'):
                continue

            # 解析键值对
            if '=' in line:
                key, value = line.split('=', 1)
                return {
                    'type': 'config',
                    'key': key.strip(),
                    'value': value.strip(),
                    'line_number': self.index
                }
            else:
                return {
                    'type': 'unknown',
                    'content': line,
                    'line_number': self.index
                }

        raise StopIteration

print("\n配置解析迭代器:")
config_content = [
    "# Application Configuration",
    "app_name = MyApp",
    "",
    "version = 1.0.0",
    "debug = true",
    "# Database settings",
    "db_host = localhost",
    "db_port = 5432"
]

config_iter = ConfigParserIterator(config_content)
print("解析配置:")
for item in config_iter:
    if item['type'] == 'config':
        print(f"  配置: {item['key']} = {item['value']}")
    else:
        print(f"  其他: {item['content']}")

# 6. 事件流迭代器
class EventStreamIterator:
    """事件流迭代器"""

    def __init__(self, event_generator):
        self.event_generator = event_generator
        self.events = []

    def __iter__(self):
        return self

    def __next__(self):
        # 模拟等待新事件
        import time
        import random

        if random.random() < 0.3:  # 30% 概率没有新事件
            raise StopIteration

        # 生成随机事件
        event_types = ['user_login', 'user_logout', 'data_update', 'error']
        event_type = random.choice(event_types)

        event = {
            'type': event_type,
            'timestamp': time.time(),
            'user_id': random.randint(1, 100),
            'data': f"Event data for {event_type}"
        }

        return event

print("\n事件流迭代器:")
event_iter = EventStreamIterator(None)

print("监听事件流:")
for i, event in enumerate(event_iter):
    print(f"  事件{i+1}: {event['type']} - 用户{event['user_id']}")
    if i >= 5:  # 只监听前6个事件
        break

# 7. 缓存迭代器
class CachedIterator:
    """缓存迭代器"""

    def __init__(self, iterable):
        self.iterable = iterable
        self.cache = []
        self.iterator = iter(iterable)
        self.exhausted = False

    def __iter__(self):
        return self

    def __next__(self):
        if self.exhausted:
            raise StopIteration

        try:
            value = next(self.iterator)
            self.cache.append(value)
            return value
        except StopIteration:
            self.exhausted = True
            raise

    def get_cache(self):
        """获取缓存的数据"""
        return self.cache.copy()

    def rewind(self):
        """重置到开始位置"""
        self.iterator = iter(self.iterable)
        self.exhausted = False

print("\n缓存迭代器:")
data = range(1, 11)
cached_iter = CachedIterator(data)

print("第一次遍历:")
for value in cached_iter:
    print(f"  {value}")

print(f"缓存内容: {cached_iter.get_cache()}")

# 重置并再次遍历
cached_iter.rewind()
print("第二次遍历:")
for value in cached_iter:
    print(f"  {value}")

# 8. 组合迭代器
class CompositeIterator:
    """组合迭代器"""

    def __init__(self, *iterators):
        self.iterators = iterators
        self.current_index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.current_index >= len(self.iterators):
            raise StopIteration

        try:
            value = next(self.iterators[self.current_index])
            return value
        except StopIteration:
            self.current_index += 1
            return self.__next__()

print("\n组合迭代器:")
iter1 = iter([1, 2, 3])
iter2 = iter(['a', 'b', 'c'])
iter3 = iter([10, 20, 30])

composite = CompositeIterator(iter1, iter2, iter3)
print(f"组合结果: {list(composite)}")
```

## 总结

本章介绍了 Python 的生成器和迭代器：
- 掌握生成器函数的创建、使用和高级特性，包括 yield、send、throw、close
- 学会生成器表达式的语法和应用，理解其内存效率优势
- 深入理解迭代器协议，实现自定义的可迭代对象和迭代器
- 掌握惰性求值、内存优化和流式处理等核心概念

通过本章的学习，你应该能够：
1. 编写高效的生成器函数来处理大数据集
2. 使用生成器表达式简化代码并提高性能
3. 实现自定义迭代器来支持特殊的数据访问模式
4. 理解并应用迭代器协议的核心概念
5. 在实际项目中运用生成器和迭代器进行数据处理和优化

下一章将介绍并发编程的基础知识。
