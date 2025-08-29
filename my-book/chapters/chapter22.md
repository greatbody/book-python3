# 第22章：数据处理

## 22.1 collections 模块

### 22.1.1 Counter 类

```python
from collections import Counter

# Counter 类
print("Counter 类:")

# 创建计数器
print("创建计数器:")

# 从字符串创建
text = "hello world hello python world"
counter1 = Counter(text)
print(f"字符串计数: {counter1}")

# 从列表创建
words = ["hello", "world", "hello", "python", "world", "hello"]
counter2 = Counter(words)
print(f"单词计数: {counter2}")

# 从字典创建
counter3 = Counter({'a': 3, 'b': 2, 'c': 1})
print(f"字典计数: {counter3}")

# 空计数器
counter4 = Counter()
print(f"空计数器: {counter4}")

# 计数器操作
print("\n计数器操作:")

c1 = Counter(['a', 'b', 'c', 'a', 'b', 'a'])
c2 = Counter(['a', 'b', 'd', 'a'])

print(f"c1: {c1}")
print(f"c2: {c2}")

# 加法
print(f"c1 + c2: {c1 + c2}")

# 减法
print(f"c1 - c2: {c1 - c2}")

# 交集（取最小值）
print(f"c1 & c2: {c1 & c2}")

# 并集（取最大值）
print(f"c1 | c2: {c1 | c2}")

# 计数器方法
print("\n计数器方法:")

c = Counter(['a', 'b', 'c', 'a', 'b', 'a'])
print(f"原始计数器: {c}")

# most_common() - 最常见的元素
print(f"最常见的2个: {c.most_common(2)}")
print(f"最常见的全部: {c.most_common()}")

# elements() - 返回所有元素（重复出现）
elements = list(c.elements())
print(f"所有元素: {elements}")

# total() - 总计数
print(f"总计数: {c.total()}")

# subtract() - 减去另一个计数器
c.subtract(Counter(['a', 'b']))
print(f"减去后: {c}")

# update() - 更新计数
c.update(['a', 'd', 'd'])
print(f"更新后: {c}")

# 访问和修改
print("\n访问和修改:")
c = Counter(['a', 'b', 'c', 'a'])

print(f"访问 'a': {c['a']}")
print(f"访问不存在的 'd': {c['d']}")  # 返回 0

# 修改计数
c['a'] = 5
c['d'] = 2
print(f"修改后: {c}")

# 删除元素
del c['b']
print(f"删除 'b' 后: {c}")
```

### 22.1.2 defaultdict 类

```python
from collections import defaultdict

# defaultdict 类
print("defaultdict 类:")

# 传统字典的问题
print("传统字典的问题:")
normal_dict = {}
words = ['apple', 'banana', 'apple', 'cherry', 'banana', 'apple']

for word in words:
    if word not in normal_dict:
        normal_dict[word] = 0
    normal_dict[word] += 1

print(f"传统方式: {normal_dict}")

# 使用 defaultdict
print("\n使用 defaultdict:")
word_count = defaultdict(int)  # 默认值为 0

for word in words:
    word_count[word] += 1

print(f"defaultdict 方式: {dict(word_count)}")

# 不同类型的默认值
print("\n不同类型的默认值:")

# 列表默认值
list_dict = defaultdict(list)
groups = [('A', 1), ('B', 2), ('A', 3), ('B', 4), ('A', 5)]

for key, value in groups:
    list_dict[key].append(value)

print(f"列表分组: {dict(list_dict)}")

# 集合默认值
set_dict = defaultdict(set)
pairs = [('A', 1), ('B', 2), ('A', 3), ('B', 1), ('A', 1)]

for key, value in pairs:
    set_dict[key].add(value)

print(f"集合分组: {dict(set_dict)}")

# 函数作为默认值工厂
print("\n函数作为默认值工厂:")

def default_value():
    return "default"

func_dict = defaultdict(default_value)
func_dict['existing'] = 'value'

print(f"现有键: {func_dict['existing']}")
print(f"不存在的键: {func_dict['missing']}")

# lambda 表达式
lambda_dict = defaultdict(lambda: [0, 0, 0])
lambda_dict['red'][0] = 255
lambda_dict['green'][1] = 255

print(f"颜色字典: {dict(lambda_dict)}")

# 实际应用示例
print("\n实际应用示例:")

# 1. 文本分析
def analyze_text(text):
    """分析文本中的单词频率"""
    word_freq = defaultdict(int)
    punctuation = '.,!?;:'

    for word in text.lower().split():
        word = word.strip(punctuation)
        if word:
            word_freq[word] += 1

    return word_freq

text = "Hello world! Hello Python. Python is great. Hello world."
analysis = analyze_text(text)
print(f"单词频率: {dict(analysis)}")

# 2. 构建图结构
def build_graph(edges):
    """构建图的邻接表"""
    graph = defaultdict(list)

    for u, v in edges:
        graph[u].append(v)
        # 如果是无向图，还需要添加反向边
        # graph[v].append(u)

    return graph

edges = [('A', 'B'), ('A', 'C'), ('B', 'D'), ('C', 'D'), ('D', 'E')]
graph = build_graph(edges)
print(f"图结构: {dict(graph)}")

# 3. 多级分组
def multi_level_grouping(data):
    """多级分组统计"""
    result = defaultdict(lambda: defaultdict(int))

    for category, subcategory, value in data:
        result[category][subcategory] += value

    return result

data = [
    ('fruit', 'apple', 5),
    ('fruit', 'banana', 3),
    ('fruit', 'apple', 2),
    ('vegetable', 'carrot', 4),
    ('vegetable', 'broccoli', 1),
    ('fruit', 'banana', 1)
]

grouped = multi_level_grouping(data)
print("多级分组:")
for category, subcategories in grouped.items():
    print(f"  {category}: {dict(subcategories)}")
```

### 22.1.3 OrderedDict 类

```python
from collections import OrderedDict

# OrderedDict 类
print("OrderedDict 类:")

# 创建有序字典
print("创建有序字典:")

# 空有序字典
od1 = OrderedDict()
od1['c'] = 3
od1['a'] = 1
od1['b'] = 2
print(f"插入顺序: {od1}")

# 从普通字典创建（Python 3.7+ 保持插入顺序）
normal_dict = {'c': 3, 'a': 1, 'b': 2}
od2 = OrderedDict(normal_dict)
print(f"从字典创建: {od2}")

# 从键值对序列创建
pairs = [('c', 3), ('a', 1), ('b', 2)]
od3 = OrderedDict(pairs)
print(f"从序列创建: {od3}")

# 有序字典的方法
print("\n有序字典的方法:")

od = OrderedDict([('a', 1), ('b', 2), ('c', 3), ('d', 4)])

# popitem() - 移除最后一个项目
last_item = od.popitem()
print(f"移除最后一个: {last_item}, 剩余: {od}")

# popitem(last=False) - 移除第一个项目
first_item = od.popitem(last=False)
print(f"移除第一个: {first_item}, 剩余: {od}")

# move_to_end() - 移动到末尾
od.move_to_end('b')
print(f"移动 'b' 到末尾: {od}")

# move_to_end(last=False) - 移动到开头
od.move_to_end('c', last=False)
print(f"移动 'c' 到开头: {od}")

# 比较有序字典
print("\n比较有序字典:")

od1 = OrderedDict([('a', 1), ('b', 2)])
od2 = OrderedDict([('a', 1), ('b', 2)])
od3 = OrderedDict([('b', 2), ('a', 1)])

print(f"od1 == od2: {od1 == od2}")  # 值相等，顺序相同
print(f"od1 == od3: {od1 == od3}")  # 值相等，顺序不同

# 在 Python 3.7+ 中，普通字典也保持插入顺序
print("\nPython 3.7+ 普通字典顺序:")
d1 = {'a': 1, 'b': 2, 'c': 3}
d2 = {'c': 3, 'a': 1, 'b': 2}
print(f"d1: {d1}")
print(f"d2: {d2}")
print(f"d1 == d2: {d1 == d2}")

# 实际应用示例
print("\n实际应用示例:")

# 1. LRU 缓存
class LRUCache:
    """简单的 LRU 缓存实现"""

    def __init__(self, capacity):
        self.capacity = capacity
        self.cache = OrderedDict()

    def get(self, key):
        if key in self.cache:
            # 移动到末尾（最近使用）
            self.cache.move_to_end(key)
            return self.cache[key]
        return None

    def put(self, key, value):
        if key in self.cache:
            # 更新值并移动到末尾
            self.cache[key] = value
            self.cache.move_to_end(key)
        else:
            if len(self.cache) >= self.capacity:
                # 移除最久未使用的（第一个）
                self.cache.popitem(last=False)
            self.cache[key] = value

    def __str__(self):
        return str(self.cache)

# 测试 LRU 缓存
cache = LRUCache(3)
cache.put('A', 1)
cache.put('B', 2)
cache.put('C', 3)
print(f"初始缓存: {cache}")

cache.get('A')  # 访问 A，使其变为最近使用
print(f"访问 A 后: {cache}")

cache.put('D', 4)  # 添加 D，B 应该被移除
print(f"添加 D 后: {cache}")

# 2. 保持配置顺序
def load_config_with_order(config_lines):
    """保持配置项的顺序"""
    config = OrderedDict()

    for line in config_lines:
        line = line.strip()
        if line and not line.startswith('#'):
            if '=' in line:
                key, value = line.split('=', 1)
                config[key.strip()] = value.strip()

    return config

config_lines = [
    "# Database configuration",
    "db_host = localhost",
    "db_port = 5432",
    "# Application settings",
    "app_name = MyApp",
    "debug = true"
]

config = load_config_with_order(config_lines)
print(f"\n配置文件 (保持顺序):")
for key, value in config.items():
    print(f"  {key} = {value}")

# 3. 最近使用项跟踪
def track_recent_items(max_items=5):
    """跟踪最近使用的项"""
    recent = OrderedDict()

    def add_item(item):
        if item in recent:
            recent.move_to_end(item)
        else:
            if len(recent) >= max_items:
                recent.popitem(last=False)
            recent[item] = None

    def get_recent():
        return list(recent.keys())[::-1]  # 反转以获得最近的在前

    return add_item, get_recent

add_item, get_recent = track_recent_items(3)

# 添加一些项
for item in ['file1.txt', 'file2.txt', 'file3.txt', 'file1.txt', 'file4.txt']:
    add_item(item)
    print(f"添加 {item} 后，最近项: {get_recent()}")
```

### 22.1.4 deque 类

```python
from collections import deque

# deque 类
print("deque 类:")

# 创建双端队列
print("创建双端队列:")

# 空双端队列
d1 = deque()
print(f"空队列: {d1}")

# 从可迭代对象创建
d2 = deque([1, 2, 3, 4, 5])
print(f"从列表创建: {d2}")

# 指定最大长度
d3 = deque([1, 2, 3], maxlen=5)
print(f"最大长度为5: {d3}")

# 双端队列的基本操作
print("\n双端队列的基本操作:")

d = deque([1, 2, 3, 4, 5])
print(f"原始队列: {d}")

# 添加到右端
d.append(6)
print(f"右端添加6: {d}")

# 添加到左端
d.appendleft(0)
print(f"左端添加0: {d}")

# 从右端移除
right = d.pop()
print(f"右端移除: {right}, 剩余: {d}")

# 从左端移除
left = d.popleft()
print(f"左端移除: {left}, 剩余: {d}")

# 扩展操作
print("\n扩展操作:")

d = deque([1, 2, 3])
print(f"原始队列: {d}")

# 右端扩展
d.extend([4, 5, 6])
print(f"右端扩展: {d}")

# 左端扩展
d.extendleft([-1, 0])
print(f"左端扩展: {d}")

# 访问和修改
print("\n访问和修改:")

d = deque([10, 20, 30, 40, 50])
print(f"队列: {d}")

# 索引访问
print(f"d[0]: {d[0]}")  # 左端第一个
print(f"d[-1]: {d[-1]}")  # 右端第一个

# 切片
print(f"d[1:4]: {list(d)[1:4]}")  # 需要转换为列表进行切片

# 修改元素
d[0] = 100
d[-1] = 500
print(f"修改后: {d}")

# 旋转操作
print("\n旋转操作:")

d = deque([1, 2, 3, 4, 5])
print(f"原始队列: {d}")

# 向右旋转
d.rotate(2)
print(f"右旋转2: {d}")

# 向左旋转
d.rotate(-3)
print(f"左旋转3: {d}")

# 查找和计数
print("\n查找和计数:")

d = deque([1, 2, 3, 2, 1, 4, 2])
print(f"队列: {d}")

# 查找索引
try:
    index = d.index(2)
    print(f"第一个 2 的索引: {index}")
except ValueError as e:
    print(f"查找失败: {e}")

# 计数
count = d.count(2)
print(f"2 出现的次数: {count}")

# 反转
d.reverse()
print(f"反转后: {d}")

# 实际应用示例
print("\n实际应用示例:")

# 1. 实现队列
class Queue:
    """使用 deque 实现的队列"""

    def __init__(self):
        self._items = deque()

    def enqueue(self, item):
        """入队"""
        self._items.append(item)

    def dequeue(self):
        """出队"""
        if self.is_empty():
            raise IndexError("队列为空")
        return self._items.popleft()

    def peek(self):
        """查看队首元素"""
        if self.is_empty():
            raise IndexError("队列为空")
        return self._items[0]

    def is_empty(self):
        """判断是否为空"""
        return len(self._items) == 0

    def size(self):
        """返回队列大小"""
        return len(self._items)

    def __str__(self):
        return str(list(self._items))

# 测试队列
queue = Queue()
for i in range(1, 6):
    queue.enqueue(i)
    print(f"入队 {i}: {queue}")

for _ in range(3):
    item = queue.dequeue()
    print(f"出队 {item}: {queue}")

# 2. 滑动窗口最大值
def sliding_window_maximum(nums, k):
    """滑动窗口最大值"""
    if not nums or k == 0:
        return []

    result = []
    window = deque()

    for i, num in enumerate(nums):
        # 移除窗口外的元素
        while window and window[0] <= i - k:
            window.popleft()

        # 移除比当前元素小的元素
        while window and nums[window[-1]] <= num:
            window.pop()

        window.append(i)

        # 当窗口大小达到 k 时，记录最大值
        if i >= k - 1:
            result.append(nums[window[0]])

    return result

nums = [1, 3, -1, -3, 5, 3, 6, 7]
k = 3
max_values = sliding_window_maximum(nums, k)
print(f"\n滑动窗口最大值 (k={k}): {max_values}")

# 3. 最近 k 个项目的缓存
def recent_cache(max_size):
    """最近 k 个项目的缓存"""
    cache = deque(maxlen=max_size)

    def add(item):
        cache.append(item)

    def get_recent():
        return list(cache)

    return add, get_recent

add_item, get_recent = recent_cache(5)

# 添加项目
for i in range(10):
    add_item(f"item_{i}")
    print(f"添加 item_{i}, 最近项目: {get_recent()}")
```

### 22.1.5 namedtuple 类

```python
from collections import namedtuple

# namedtuple 类
print("namedtuple 类:")

# 创建命名元组
print("创建命名元组:")

# 定义 Point 命名元组
Point = namedtuple('Point', ['x', 'y'])
p1 = Point(1, 2)
p2 = Point(x=3, y=4)

print(f"Point 实例: {p1}")
print(f"带关键字参数: {p2}")

# 定义 Person 命名元组
Person = namedtuple('Person', 'name age city')
person1 = Person('Alice', 30, '北京')
person2 = Person(name='Bob', age=25, city='上海')

print(f"Person 实例: {person1}")
print(f"带关键字: {person2}")

# 从字符串创建字段名
fields = 'name age city'
Person2 = namedtuple('Person', fields)
person3 = Person2('Charlie', 35, '深圳')
print(f"从字符串创建: {person3}")

# 访问字段
print("\n访问字段:")

p = Point(10, 20)
print(f"点: {p}")
print(f"x 坐标: {p.x}")
print(f"y 坐标: {p.y}")
print(f"索引访问: p[0]={p[0]}, p[1]={p[1]}")

# 解包
x, y = p
print(f"解包: x={x}, y={y}")

# 命名元组的方法
print("\n命名元组的方法:")

p1 = Point(1, 2)
p2 = Point(3, 4)
p3 = Point(1, 2)

print(f"p1: {p1}")
print(f"p2: {p2}")
print(f"p3: {p3}")

# _replace() - 创建新实例
p1_new = p1._replace(x=100)
print(f"p1._replace(x=100): {p1_new}")
print(f"原始 p1: {p1}")  # 不可变

# _fields - 字段名
print(f"字段名: {p1._fields}")

# _field_defaults - 默认值
print(f"默认值: {p1._field_defaults}")

# _asdict() - 转换为字典
print(f"转换为字典: {p1._asdict()}")

# 比较
print(f"p1 == p3: {p1 == p3}")
print(f"p1 == p2: {p1 == p2}")

# 创建带默认值的命名元组
print("\n带默认值的命名元组:")

# 方法1: 使用 _replace
PointWithDefaults = namedtuple('PointWithDefaults', 'x y', defaults=[0, 0])
p_default = PointWithDefaults()
print(f"带默认值: {p_default}")

# 方法2: 继承并添加默认值
class PointWithDefaults2(namedtuple('PointWithDefaults2', 'x y')):
    def __new__(cls, x=0, y=0):
        return super().__new__(cls, x, y)

p_default2 = PointWithDefaults2()
print(f"继承方式默认值: {p_default2}")

# 实际应用示例
print("\n实际应用示例:")

# 1. 表示数据库记录
Employee = namedtuple('Employee', 'id name department salary')

employees = [
    Employee(1, 'Alice', 'Engineering', 75000),
    Employee(2, 'Bob', 'Marketing', 65000),
    Employee(3, 'Charlie', 'Engineering', 80000)
]

print("员工记录:")
for emp in employees:
    print(f"  {emp.name} ({emp.department}): ${emp.salary}")

# 计算平均薪资
avg_salary = sum(emp.salary for emp in employees) / len(employees)
print(f"平均薪资: ${avg_salary:.2f}")

# 2. 表示坐标和向量
Vector3D = namedtuple('Vector3D', 'x y z')

def add_vectors(v1, v2):
    """向量加法"""
    return Vector3D(v1.x + v2.x, v1.y + v2.y, v1.z + v2.z)

def dot_product(v1, v2):
    """点积"""
    return v1.x * v2.x + v1.y * v2.y + v1.z * v2.z

v1 = Vector3D(1, 2, 3)
v2 = Vector3D(4, 5, 6)

print(f"\n向量运算:")
print(f"v1: {v1}")
print(f"v2: {v2}")
print(f"v1 + v2: {add_vectors(v1, v2)}")
print(f"v1 · v2: {dot_product(v1, v2)}")

# 3. HTTP 响应
HTTPResponse = namedtuple('HTTPResponse', 'status_code headers body')

def make_response(status_code, body, content_type='text/html'):
    """创建 HTTP 响应"""
    headers = {'Content-Type': content_type}
    return HTTPResponse(status_code, headers, body)

# 模拟 HTTP 响应
response = make_response(200, '<h1>Hello World</h1>')
print(f"\nHTTP 响应:")
print(f"状态码: {response.status_code}")
print(f"头部: {response.headers}")
print(f"内容: {response.body}")

# 4. 配置管理
Config = namedtuple('Config', 'host port database timeout')

def load_database_config():
    """加载数据库配置"""
    return Config(
        host='localhost',
        port=5432,
        database='myapp',
        timeout=30
    )

config = load_database_config()
print(f"\n数据库配置:")
print(f"连接字符串: {config.host}:{config.port}/{config.database}")
print(f"超时时间: {config.timeout}秒")

# 5. 游戏中的坐标系统
Position = namedtuple('Position', 'x y z')
Velocity = namedtuple('Velocity', 'dx dy dz')

def update_position(position, velocity, delta_time):
    """更新位置"""
    return Position(
        position.x + velocity.dx * delta_time,
        position.y + velocity.dy * delta_time,
        position.z + velocity.dz * delta_time
    )

# 游戏对象
player_pos = Position(0, 0, 0)
player_vel = Velocity(1, 0, 0)  # 向右移动

# 更新位置
new_pos = update_position(player_pos, player_vel, 0.5)
print(f"\n游戏位置更新:")
print(f"初始位置: {player_pos}")
print(f"速度: {player_vel}")
print(f"0.5秒后的位置: {new_pos}")
```

## 22.2 itertools 模块

### 22.2.1 无限迭代器

```python
import itertools

# 无限迭代器
print("无限迭代器:")

# count() - 无限计数
print("count() 示例:")
counter = itertools.count(10, 2)  # 从10开始，步长为2
for i, value in enumerate(counter):
    if i >= 5:
        break
    print(f"  {i}: {value}")

# cycle() - 循环迭代
print("\ncycle() 示例:")
cycler = itertools.cycle(['A', 'B', 'C'])
for i, value in enumerate(cycler):
    if i >= 8:
        break
    print(f"  {i}: {value}")

# repeat() - 重复元素
print("\nrepeat() 示例:")
repeater = itertools.repeat('Hello', 5)  # 重复5次
for i, value in enumerate(repeater):
    print(f"  {i}: {value}")

# 不指定次数的 repeat
print("\n无限 repeat() 示例:")
infinite_repeat = itertools.repeat('World')
for i, value in enumerate(infinite_repeat):
    if i >= 3:
        break
    print(f"  {i}: {value}")

# 实际应用
print("\n实际应用:")

# 1. 生成 ID
def generate_ids(start=1):
    """生成递增的 ID"""
    return itertools.count(start)

id_generator = generate_ids(100)
for _ in range(5):
    print(f"生成 ID: {next(id_generator)}")

# 2. 轮询任务
def round_robin_scheduler(tasks):
    """轮询调度器"""
    return itertools.cycle(tasks)

tasks = ['task1', 'task2', 'task3']
scheduler = round_robin_scheduler(tasks)

print("轮询调度:")
for i in range(7):
    print(f"  时间片 {i}: {next(scheduler)}")

# 3. 重复执行函数
def repeat_function(func, times=None):
    """重复执行函数"""
    if times is None:
        return itertools.repeat(func)
    else:
        return itertools.repeat(func, times)

def say_hello():
    return "Hello!"

# 重复执行5次
for func in itertools.repeat(say_hello, 3):
    print(f"执行结果: {func()}")
```

### 22.2.2 终止于最短迭代器的迭代器

```python
# 终止于最短迭代器的迭代器
print("终止于最短迭代器的迭代器:")

# chain() - 连接多个迭代器
print("chain() 示例:")
list1 = [1, 2, 3]
list2 = ['a', 'b', 'c']
list3 = [4, 5]

chained = itertools.chain(list1, list2, list3)
print(f"连接结果: {list(chained)}")

# chain.from_iterable() - 从可迭代对象连接
print("\nchain.from_iterable() 示例:")
lists = [[1, 2], ['a', 'b'], [3, 4, 5]]
chained_from_iterable = itertools.chain.from_iterable(lists)
print(f"从可迭代对象连接: {list(chained_from_iterable)}")

# zip_longest() - 最长迭代器决定长度
print("\nzip_longest() 示例:")
list1 = [1, 2, 3]
list2 = ['a', 'b']
list3 = [10, 20, 30, 40]

zipped_longest = itertools.zip_longest(list1, list2, list3, fillvalue='N/A')
print(f"zip_longest 结果: {list(zipped_longest)}")

# 比较 zip 和 zip_longest
print("\nzip vs zip_longest:")
zipped = list(zip(list1, list2, list3))
zipped_long = list(itertools.zip_longest(list1, list2, list3, fillvalue='N/A'))
print(f"zip: {zipped}")
print(f"zip_longest: {zipped_long}")

# islice() - 切片迭代器
print("\nislice() 示例:")
data = range(10)
sliced = itertools.islice(data, 2, 8, 2)  # 从索引2开始，到8结束，步长2
print(f"islice 结果: {list(sliced)}")

# tee() - 创建多个独立的迭代器
print("\ntee() 示例:")
data = [1, 2, 3, 4, 5]
iter1, iter2, iter3 = itertools.tee(data, 3)

print(f"原始数据: {data}")
print(f"迭代器1: {list(iter1)}")
print(f"迭代器2: {list(iter2)}")
print(f"迭代器3: {list(iter3)}")

# 实际应用
print("\n实际应用:")

# 1. 合并多个文件的内容
def merge_files(file_contents):
    """合并多个文件的内容"""
    return itertools.chain.from_iterable(file_contents)

files = [
    ['line1', 'line2'],
    ['line3', 'line4', 'line5'],
    ['line6']
]

merged = merge_files(files)
print(f"合并文件内容: {list(merged)}")

# 2. 分页显示数据
def paginate(data, page_size):
    """分页显示数据"""
    return itertools.zip_longest(*[iter(data)] * page_size, fillvalue=None)

data = list(range(1, 11))
pages = list(paginate(data, 3))

print("分页显示:")
for i, page in enumerate(pages, 1):
    page_data = [item for item in page if item is not None]
    print(f"  第{i}页: {page_data}")

# 3. 读取大文件的指定行
def read_file_lines(filename, start_line, end_line):
    """读取文件的指定行范围"""
    with open(filename, 'r') as f:
        lines = itertools.islice(f, start_line - 1, end_line)
        return list(lines)

# 创建测试文件
with open('test_file.txt', 'w') as f:
    for i in range(1, 11):
        f.write(f"Line {i}\n")

# 读取第3到第7行
try:
    lines = read_file_lines('test_file.txt', 3, 7)
    print(f"读取的行: {[line.strip() for line in lines]}")
except FileNotFoundError:
    print("文件不存在")

# 清理
import os
if os.path.exists('test_file.txt'):
    os.remove('test_file.txt')
```

### 22.2.3 组合生成器

```python
# 组合生成器
print("组合生成器:")

# product() - 笛卡尔积
print("product() 示例:")
list1 = ['A', 'B']
list2 = [1, 2]
list3 = ['x', 'y']

products = itertools.product(list1, list2, list3)
print(f"笛卡尔积: {list(products)}")

# 指定重复次数
products_repeat = itertools.product(list1, repeat=3)
print(f"重复3次: {list(products_repeat)}")

# permutations() - 排列
print("\npermutations() 示例:")
data = ['A', 'B', 'C']
perms = itertools.permutations(data)
print(f"全排列: {list(perms)}")

# 指定长度
perms_len2 = itertools.permutations(data, 2)
print(f"2元素排列: {list(perms_len2)}")

# combinations() - 组合
print("\ncombinations() 示例:")
data = ['A', 'B', 'C', 'D']
combs = itertools.combinations(data, 2)
print(f"2元素组合: {list(combs)}")

# combinations_with_replacement() - 允许重复的组合
print("\ncombinations_with_replacement() 示例:")
combs_replace = itertools.combinations_with_replacement(data, 2)
print(f"允许重复的2元素组合: {list(combs_replace)}")

# 实际应用
print("\n实际应用:")

# 1. 生成所有可能的配置
def generate_configurations(options):
    """生成所有可能的配置"""
    return itertools.product(*options.values())

config_options = {
    'os': ['Windows', 'Linux', 'macOS'],
    'browser': ['Chrome', 'Firefox', 'Safari'],
    'theme': ['Light', 'Dark']
}

configurations = generate_configurations(config_options)
print("所有可能的配置:")
for i, config in enumerate(configurations, 1):
    config_dict = dict(zip(config_options.keys(), config))
    print(f"  配置{i}: {config_dict}")
    if i >= 5:  # 只显示前5个
        print("  ...")
        break

# 2. 密码生成器
def generate_passwords(charsets, length):
    """生成所有可能的密码"""
    all_chars = ''.join(charsets)
    return itertools.product(all_chars, repeat=length)

charsets = [
    'abcdefghijklmnopqrstuvwxyz',  # 小写字母
    'ABCDEFGHIJKLMNOPQRSTUVWXYZ',  # 大写字母
    '0123456789'                   # 数字
]

print("\n密码生成器示例:")
passwords = generate_passwords(charsets, 2)
for i, pwd in enumerate(passwords):
    print(f"  密码{i+1}: {''.join(pwd)}")
    if i >= 8:  # 只显示前9个
        print("  ...")
        break

# 3. 菜单组合
def menu_combinations(menu_items, max_items):
    """生成菜单组合"""
    return itertools.combinations(menu_items, max_items)

menu = ['汉堡', '薯条', '可乐', '沙拉', '甜点']
combinations = menu_combinations(menu, 2)

print("\n菜单组合:")
for combo in combinations:
    print(f"  组合: {combo}")

# 4. 团队分配
def assign_teams(members, team_size):
    """将成员分配到团队"""
    return itertools.combinations(members, team_size)

members = ['Alice', 'Bob', 'Charlie', 'Diana', 'Eve']
teams = assign_teams(members, 3)

print("\n团队分配:")
for team in teams:
    print(f"  团队: {team}")

# 5. 路径查找
def find_paths(graph, start, end, max_depth=3):
    """查找图中的路径"""
    def dfs(current, path, depth):
        if depth > max_depth:
            return
        if current == end:
            yield path + [current]
            return

        for neighbor in graph.get(current, []):
            if neighbor not in path:  # 避免环
                yield from dfs(neighbor, path + [current], depth + 1)

    return list(dfs(start, [], 0))

# 简单的图
graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': ['G'],
    'E': [],
    'F': ['G'],
    'G': []
}

paths = find_paths(graph, 'A', 'G', 4)
print(f"\n从 A 到 G 的路径:")
for path in paths:
    print(f"  路径: {' -> '.join(path)}")
```

### 22.2.4 过滤和选择

```python
# 过滤和选择
print("过滤和选择:")

# dropwhile() - 丢弃满足条件的元素，直到第一个不满足
print("dropwhile() 示例:")
data = [1, 3, 5, 2, 4, 6, 8]
result = itertools.dropwhile(lambda x: x < 5, data)
print(f"丢弃小于5的元素: {list(result)}")

# takewhile() - 保留满足条件的元素，直到第一个不满足
print("\ntakewhile() 示例:")
data = [1, 3, 5, 2, 4, 6, 8]
result = itertools.takewhile(lambda x: x < 5, data)
print(f"保留小于5的元素: {list(result)}")

# filterfalse() - 保留不满足条件的元素
print("\nfilterfalse() 示例:")
data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
result = itertools.filterfalse(lambda x: x % 2 == 0, data)
print(f"保留奇数: {list(result)}")

# compress() - 根据选择器保留元素
print("\ncompress() 示例:")
data = ['A', 'B', 'C', 'D', 'E']
selectors = [True, False, True, False, True]
result = itertools.compress(data, selectors)
print(f"根据选择器保留: {list(result)}")

# islice() - 切片（前面已介绍）
print("\nislice() 示例:")
data = range(10)
result = itertools.islice(data, 2, 8, 2)
print(f"切片 [2:8:2]: {list(result)}")

# 实际应用
print("\n实际应用:")

# 1. 数据清理
def clean_data_stream(data_stream, invalid_markers):
    """清理数据流，移除无效标记后的数据"""
    return itertools.dropwhile(lambda x: x in invalid_markers, data_stream)

data = ['START', 'INVALID', 'ERROR', 'data1', 'data2', 'data3']
cleaned = clean_data_stream(data, ['START', 'INVALID', 'ERROR'])
print(f"清理后的数据: {list(cleaned)}")

# 2. 条件聚合
def aggregate_until_condition(data, condition_func):
    """聚合数据直到满足条件"""
    return itertools.takewhile(lambda x: not condition_func(x), data)

numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
aggregated = aggregate_until_condition(numbers, lambda x: x > 5)
print(f"聚合直到大于5: {list(aggregated)}")

# 3. 过滤日志
def filter_logs(logs, exclude_patterns):
    """过滤日志，排除包含指定模式的行"""
    def should_exclude(log_line):
        return any(pattern in log_line for pattern in exclude_patterns)

    return itertools.filterfalse(should_exclude, logs)

logs = [
    "INFO: Application started",
    "DEBUG: Loading configuration",
    "ERROR: Database connection failed",
    "INFO: Processing data",
    "WARNING: Low memory",
    "DEBUG: Saving results"
]

exclude_patterns = ['DEBUG', 'WARNING']
filtered_logs = filter_logs(logs, exclude_patterns)
print("过滤后的日志:")
for log in filtered_logs:
    print(f"  {log}")

# 4. 数据采样
def sample_data(data, sample_rate):
    """按采样率采样数据"""
    selectors = itertools.cycle([True] * sample_rate + [False] * (10 - sample_rate))
    return itertools.compress(data, selectors)

data = list(range(1, 21))
sampled = sample_data(data, 3)  # 每10个中采样3个
print(f"采样数据 (3/10): {list(sampled)}")

# 5. 滑动窗口过滤
def sliding_window_filter(data, window_size, filter_func):
    """滑动窗口过滤"""
    windows = []
    for i in range(len(data) - window_size + 1):
        window = data[i:i + window_size]
        if filter_func(window):
            windows.append(window)
    return windows

data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
# 过滤出窗口内元素和大于窗口平均值的窗口
def avg_filter(window):
    avg = sum(window) / len(window)
    return all(x > avg for x in window)

filtered_windows = sliding_window_filter(data, 3, avg_filter)
print(f"滑动窗口过滤 (窗口大小3): {filtered_windows}")
```

### 22.2.5 分组和聚合

```python
# 分组和聚合
print("分组和聚合:")

# groupby() - 按键分组
print("groupby() 示例:")
data = [
    ('A', 1), ('A', 2), ('B', 3), ('B', 4), ('A', 5), ('C', 6)
]

# 按第一个元素分组
grouped = itertools.groupby(data, key=lambda x: x[0])
print("按第一个元素分组:")
for key, group in grouped:
    print(f"  {key}: {list(group)}")

# 按数字的奇偶性分组
numbers = [1, 3, 5, 2, 4, 6, 8, 7, 9]
grouped_by_parity = itertools.groupby(numbers, key=lambda x: x % 2 == 0)
print("\n按奇偶性分组:")
for key, group in grouped_by_parity:
    parity = "偶数" if key else "奇数"
    print(f"  {parity}: {list(group)}")

# 注意：groupby 需要预先排序
print("\n注意：groupby 需要预先排序")
unsorted_data = [1, 3, 2, 4, 6, 5, 8, 7, 9]
print(f"未排序数据: {unsorted_data}")

grouped_unsorted = itertools.groupby(unsorted_data, key=lambda x: x % 2 == 0)
print("未排序的分组:")
for key, group in grouped_unsorted:
    parity = "偶数" if key else "奇数"
    print(f"  {parity}: {list(group)}")

# 正确的方式：先排序再分组
sorted_data = sorted(unsorted_data, key=lambda x: x % 2 == 0)
grouped_sorted = itertools.groupby(sorted_data, key=lambda x: x % 2 == 0)
print("排序后的分组:")
for key, group in grouped_sorted:
    parity = "偶数" if key else "奇数"
    print(f"  {parity}: {list(group)}")

# accumulate() - 累积计算
print("\naccumulate() 示例:")
numbers = [1, 2, 3, 4, 5]

# 默认累积（求和）
accumulated = itertools.accumulate(numbers)
print(f"累积求和: {list(accumulated)}")

# 自定义操作
import operator
accumulated_max = itertools.accumulate(numbers, max)
print(f"累积最大值: {list(accumulated_max)}")

accumulated_mul = itertools.accumulate(numbers, operator.mul)
print(f"累积乘积: {list(accumulated_mul)}")

# 实际应用
print("\n实际应用:")

# 1. 文本分析 - 按首字母分组
def group_words_by_first_letter(words):
    """按首字母分组单词"""
    sorted_words = sorted(words, key=lambda x: x[0].lower())
    grouped = itertools.groupby(sorted_words, key=lambda x: x[0].lower())
    return {key: list(group) for key, group in grouped}

words = ['Apple', 'Banana', 'Cherry', 'Apricot', 'Blueberry', 'Carrot']
grouped_words = group_words_by_first_letter(words)
print("按首字母分组:")
for letter, word_list in grouped_words.items():
    print(f"  {letter.upper()}: {word_list}")

# 2. 股票价格分析
def analyze_stock_prices(prices):
    """分析股票价格"""
    # 计算每日收益率
    daily_returns = []
    prev_price = None
    for price in prices:
        if prev_price is not None:
            daily_return = (price - prev_price) / prev_price
            daily_returns.append(daily_return)
        prev_price = price

    # 计算累积收益率
    cumulative_returns = list(itertools.accumulate(
        daily_returns,
        lambda acc, ret: acc * (1 + ret),
        initial=1
    ))[1:]  # 移除初始值1

    return daily_returns, cumulative_returns

prices = [100, 105, 102, 108, 110, 107, 112]
daily_returns, cumulative_returns = analyze_stock_prices(prices)

print("
股票价格分析:")
print(f"价格: {prices}")
print(f"每日收益率: {[f'{r:.2%}' for r in daily_returns]}")
print(f"累积收益率: {[f'{r:.2%}' for r in cumulative_returns]}")

# 3. 用户行为分析
def analyze_user_sessions(sessions):
    """分析用户会话"""
    # 按用户ID分组
    sorted_sessions = sorted(sessions, key=lambda x: x['user_id'])
    grouped_sessions = itertools.groupby(sorted_sessions, key=lambda x: x['user_id'])

    user_stats = {}
    for user_id, user_sessions in grouped_sessions:
        session_list = list(user_sessions)
        total_duration = sum(session['duration'] for session in session_list)
        session_count = len(session_list)

        user_stats[user_id] = {
            'session_count': session_count,
            'total_duration': total_duration,
            'avg_duration': total_duration / session_count
        }

    return user_stats

sessions = [
    {'user_id': 1, 'duration': 30},
    {'user_id': 2, 'duration': 45},
    {'user_id': 1, 'duration': 25},
    {'user_id': 3, 'duration': 60},
    {'user_id': 2, 'duration': 35},
    {'user_id': 1, 'duration': 40}
]

user_stats = analyze_user_sessions(sessions)
print("
用户会话分析:")
for user_id, stats in user_stats.items():
    print(f"  用户{user_id}: {stats['session_count']}次会话, "
          f"总时长{stats['total_duration']}分钟, "
          f"平均{stats['avg_duration']:.1f}分钟")

# 4. 累积统计
def cumulative_statistics(data):
    """计算累积统计"""
    cumulative_sum = list(itertools.accumulate(data))
    cumulative_count = list(itertools.accumulate([1] * len(data)))
    cumulative_avg = [s / c for s, c in zip(cumulative_sum, cumulative_count)]

    return {
        'sum': cumulative_sum,
        'count': cumulative_count,
        'average': cumulative_avg
    }

data = [10, 15, 8, 12, 20, 5]
stats = cumulative_statistics(data)

print("
累积统计:")
print(f"数据: {data}")
print(f"累积和: {stats['sum']}")
print(f"累积平均: {[f'{avg:.2f}' for avg in stats['average']]}")
```

## 22.3 functools 模块

### 22.3.1 partial 函数

```python
from functools import partial

# partial 函数
print("partial 函数:")

# 基本用法
def multiply(x, y, z):
    return x * y * z

# 创建偏函数
double = partial(multiply, 2)  # 固定第一个参数为2
triple = partial(multiply, 3)  # 固定第一个参数为3

print(f"double(3, 4): {double(3, 4)}")  # 2 * 3 * 4 = 24
print(f"triple(2, 5): {triple(2, 5)}")  # 3 * 2 * 5 = 30

# 固定多个参数
multiply_by_10 = partial(multiply, 2, 5)  # 固定前两个参数
print(f"multiply_by_10(3): {multiply_by_10(3)}")  # 2 * 5 * 3 = 30

# 使用关键字参数
def greet(greeting, name, punctuation='!'):
    return f"{greeting}, {name}{punctuation}"

# 创建偏函数
say_hello = partial(greet, 'Hello')
say_hi = partial(greet, 'Hi', punctuation='.')

print(f"say_hello('Alice'): {say_hello('Alice')}")
print(f"say_hi('Bob'): {say_hi('Bob')}")

# 检查偏函数
print(f"\n偏函数信息:")
print(f"double.func: {double.func}")
print(f"double.args: {double.args}")
print(f"double.keywords: {double.keywords}")

# 实际应用
print("\n实际应用:")

# 1. HTTP 请求函数
def make_request(method, url, headers=None, data=None, timeout=30):
    """模拟 HTTP 请求"""
    return {
        'method': method,
        'url': url,
        'headers': headers or {},
        'data': data,
        'timeout': timeout
    }

# 创建专用请求函数
get_request = partial(make_request, 'GET')
post_request = partial(make_request, 'POST', headers={'Content-Type': 'application/json'})

print("HTTP 请求示例:")
get_req = get_request('https://api.example.com/users')
post_req = post_request('https://api.example.com/users', data={'name': 'Alice'})

print(f"GET 请求: {get_req}")
print(f"POST 请求: {post_req}")

# 2. 数据库查询
def query_database(table, columns=None, where=None, limit=None):
    """模拟数据库查询"""
    query = f"SELECT {', '.join(columns or ['*'])} FROM {table}"
    if where:
        query += f" WHERE {where}"
    if limit:
        query += f" LIMIT {limit}"
    return query

# 创建专用查询函数
query_users = partial(query_database, 'users')
query_active_users = partial(query_database, 'users', where='active=1')

print("\n数据库查询示例:")
print(f"查询所有用户: {query_users()}")
print(f"查询活跃用户: {query_active_users(limit=10)}")
print(f"查询特定列: {query_users(columns=['id', 'name'], limit=5)}")

# 3. 文件处理
import os

def process_file(filename, mode='r', encoding='utf-8', action=None):
    """处理文件"""
    try:
        with open(filename, mode, encoding=encoding) as f:
            if action:
                return action(f)
            else:
                return f.read()
    except IOError as e:
        return f"Error: {e}"

# 创建专用文件处理函数
read_text_file = partial(process_file, mode='r', encoding='utf-8')
read_binary_file = partial(process_file, mode='rb')
write_text_file = partial(process_file, mode='w', encoding='utf-8')

# 创建测试文件
with open('test.txt', 'w') as f:
    f.write('Hello, World!')

# 使用偏函数
content = read_text_file('test.txt')
print(f"\n文件内容: {content}")

# 自定义处理动作
def count_lines(file_obj):
    return len(file_obj.readlines())

line_count = process_file('test.txt', action=count_lines)
print(f"行数: {line_count}")

# 清理
if os.path.exists('test.txt'):
    os.remove('test.txt')

# 4. 数学函数
import math

# 创建专用数学函数
sqrt = partial(math.pow, exp=0.5)  # 平方根
cube = partial(math.pow, exp=3)    # 立方
reciprocal = partial(math.pow, exp=-1)  # 倒数

print("\n数学函数示例:")
print(f"sqrt(16): {sqrt(16)}")
print(f"cube(3): {cube(3)}")
print(f"reciprocal(4): {reciprocal(4)}")

# 5. 排序函数
def sort_data(data, key=None, reverse=False):
    """排序数据"""
    return sorted(data, key=key, reverse=reverse)

# 创建专用排序函数
sort_by_name = partial(sort_data, key=lambda x: x['name'])
sort_by_age_desc = partial(sort_data, key=lambda x: x['age'], reverse=True)

data = [
    {'name': 'Alice', 'age': 30},
    {'name': 'Bob', 'age': 25},
    {'name': 'Charlie', 'age': 35}
]

print("\n排序示例:")
print(f"按姓名排序: {sort_by_name(data)}")
print(f"按年龄降序: {sort_by_age_desc(data)}")
```

### 22.3.2 reduce 函数

```python
from functools import reduce

# reduce 函数
print("reduce 函数:")

# 基本用法
numbers = [1, 2, 3, 4, 5]

# 求和
sum_result = reduce(lambda x, y: x + y, numbers)
print(f"求和: {sum_result}")

# 求积
product_result = reduce(lambda x, y: x * y, numbers)
print(f"求积: {product_result}")

# 最大值
max_result = reduce(lambda x, y: x if x > y else y, numbers)
print(f"最大值: {max_result}")

# 字符串连接
words = ['Hello', ' ', 'World', '!']
concat_result = reduce(lambda x, y: x + y, words)
print(f"字符串连接: '{concat_result}'")

# 使用初始值
print("\n使用初始值:")
numbers = [1, 2, 3, 4, 5]

sum_with_init = reduce(lambda x, y: x + y, numbers, 10)
print(f"求和 (初始值10): {sum_with_init}")

product_with_init = reduce(lambda x, y: x * y, numbers, 2)
print(f"求积 (初始值2): {product_with_init}")

# 空序列的情况
empty_list = []
try:
    result = reduce(lambda x, y: x + y, empty_list)
    print(f"空列表结果: {result}")
except TypeError as e:
    print(f"空列表错误: {e}")

# 带初始值的空序列
result_with_init = reduce(lambda x, y: x + y, empty_list, 0)
print(f"空列表 (初始值0): {result_with_init}")

# 实际应用
print("\n实际应用:")

# 1. 计算阶乘
def factorial(n):
    """计算阶乘"""
    if n <= 1:
        return 1
    return reduce(lambda x, y: x * y, range(1, n + 1))

print(f"5! = {factorial(5)}")
print(f"10! = {factorial(10)}")

# 2. 列表展平
def flatten_list(nested_list):
    """展平嵌套列表"""
    return reduce(lambda x, y: x + y, nested_list, [])

nested = [[1, 2], [3, 4], [5, 6]]
flattened = flatten_list(nested)
print(f"展平列表: {flattened}")

# 3. 字典合并
def merge_dicts(*dicts):
    """合并多个字典"""
    def merge_two_dicts(d1, d2):
        result = d1.copy()
        result.update(d2)
        return result

    return reduce(merge_two_dicts, dicts, {})

dict1 = {'a': 1, 'b': 2}
dict2 = {'b': 3, 'c': 4}
dict3 = {'c': 5, 'd': 6}

merged = merge_dicts(dict1, dict2, dict3)
print(f"合并字典: {merged}")

# 4. 运行长度编码
def run_length_encode(data):
    """运行长度编码"""
    if not data:
        return []

    def encode_pair(acc, item):
        if acc and acc[-1][0] == item:
            # 增加计数
            acc[-1] = (item, acc[-1][1] + 1)
        else:
            # 新元素
            acc.append((item, 1))
        return acc

    return reduce(encode_pair, data, [])

data = ['a', 'a', 'b', 'b', 'b', 'c', 'a', 'a']
encoded = run_length_encode(data)
print(f"运行长度编码: {encoded}")

# 5. 投票统计
def count_votes(votes):
    """统计投票"""
    def add_vote(counts, vote):
        counts[vote] = counts.get(vote, 0) + 1
        return counts

    return reduce(add_vote, votes, {})

votes = ['A', 'B', 'A', 'C', 'A', 'B', 'A']
vote_counts = count_votes(votes)
print(f"投票统计: {vote_counts}")

# 6. 矩阵乘法
def matrix_multiply(A, B):
    """矩阵乘法"""
    if len(A[0]) != len(B):
        raise ValueError("矩阵维度不匹配")

    def multiply_row(row, matrix):
        return [sum(a * b for a, b in zip(row, col)) for col in zip(*matrix)]

    return [multiply_row(row, B) for row in A]

# 使用 reduce 实现
def matrix_multiply_reduce(A, B):
    """使用 reduce 实现矩阵乘法"""
    if len(A[0]) != len(B):
        raise ValueError("矩阵维度不匹配")

    def multiply_elements(elements):
        return reduce(lambda x, y: x + y[0] * y[1], elements, 0)

    result = []
    for i in range(len(A)):
        row = []
        for j in range(len(B[0])):
            # 获取第i行和第j列的元素
            elements = list(zip(A[i], [B[k][j] for k in range(len(B))]))
            row.append(multiply_elements(elements))
        result.append(row)

    return result

A = [[1, 2], [3, 4]]
B = [[5, 6], [7, 8]]

result1 = matrix_multiply(A, B)
result2 = matrix_multiply_reduce(A, B)

print(f"\n矩阵乘法 A * B:")
print(f"结果: {result1}")
print(f"reduce实现: {result2}")
print(f"结果相同: {result1 == result2}")
```

### 22.3.3 lru_cache 装饰器

```python
from functools import lru_cache
import time

# lru_cache 装饰器
print("lru_cache 装饰器:")

# 基本用法
@lru_cache(maxsize=128)
def fibonacci(n):
    """计算斐波那契数"""
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

print("斐波那契数列:")
start_time = time.time()
result = fibonacci(30)
end_time = time.time()

print(f"fibonacci(30) = {result}")
print(f"首次计算时间: {end_time - start_time:.6f}秒")

# 再次计算（使用缓存）
start_time = time.time()
result = fibonacci(30)
end_time = time.time()

print(f"再次计算时间: {end_time - start_time:.6f}秒")

# 查看缓存信息
print(f"缓存信息: {fibonacci.cache_info()}")

# 清除缓存
fibonacci.cache_clear()
print(f"清除缓存后: {fibonacci.cache_info()}")

# 实际应用
print("\n实际应用:")

# 1. Web API 缓存
@lru_cache(maxsize=100)
def fetch_user_data(user_id):
    """模拟获取用户数据"""
    print(f"从数据库获取用户 {user_id} 的数据")
    # 模拟数据库查询延迟
    time.sleep(0.1)
    return {
        'id': user_id,
        'name': f'User_{user_id}',
        'email': f'user{user_id}@example.com'
    }

print("Web API 缓存示例:")
# 首次请求
user1 = fetch_user_data(1)
print(f"用户1数据: {user1}")

# 再次请求相同用户（使用缓存）
user1_cached = fetch_user_data(1)
print(f"缓存的用户1数据: {user1_cached}")

# 请求不同用户
user2 = fetch_user_data(2)
print(f"用户2数据: {user2}")

print(f"缓存信息: {fetch_user_data.cache_info()}")

# 2. 数据库查询缓存
@lru_cache(maxsize=50)
def query_database(query, params=None):
    """模拟数据库查询"""
    print(f"执行查询: {query}")
    time.sleep(0.05)  # 模拟查询延迟
    return f"查询结果: {hash(query + str(params))}"

print("\n数据库查询缓存:")
queries = [
    ("SELECT * FROM users WHERE id = ?", (1,)),
    ("SELECT * FROM users WHERE id = ?", (2,)),
    ("SELECT * FROM users WHERE id = ?", (1,)),  # 重复查询
    ("SELECT * FROM products WHERE category = ?", ("electronics",)),
    ("SELECT * FROM users WHERE id = ?", (1,)),  # 再次重复
]

for query, params in queries:
    result = query_database(query, params)
    print(f"结果: {result}")

print(f"缓存信息: {query_database.cache_info()}")

# 3. 计算密集型函数缓存
@lru_cache(maxsize=64)
def expensive_computation(x, y, z):
    """模拟计算密集型函数"""
    print(f"计算 expensive_computation({x}, {y}, {z})")
    time.sleep(0.2)  # 模拟计算时间
    return x**2 + y**2 + z**2

print("\n计算密集型函数缓存:")
computations = [
    (1, 2, 3),
    (2, 3, 4),
    (1, 2, 3),  # 重复计算
    (3, 4, 5),
    (1, 2, 3),  # 再次重复
]

for x, y, z in computations:
    result = expensive_computation(x, y, z)
    print(f"结果: {result}")

print(f"缓存信息: {expensive_computation.cache_info()}")

# 4. HTTP 请求缓存
@lru_cache(maxsize=32)
def http_get(url, headers=None):
    """模拟 HTTP GET 请求"""
    print(f"发送 HTTP 请求: {url}")
    time.sleep(0.1)  # 模拟网络延迟
    return f"响应内容: {hash(url + str(headers))}"

print("\nHTTP 请求缓存:")
urls = [
    "https://api.example.com/users",
    "https://api.example.com/products",
    "https://api.example.com/users",  # 重复请求
    "https://api.example.com/orders",
    "https://api.example.com/users",  # 再次重复
]

for url in urls:
    response = http_get(url)
    print(f"响应: {response}")

print(f"缓存信息: {http_get.cache_info()}")

# 5. 递归函数优化
@lru_cache(maxsize=None)  # 无限制缓存
def binomial_coefficient(n, k):
    """计算二项式系数 C(n, k)"""
    if k == 0 or k == n:
        return 1
    if k > n:
        return 0
    return binomial_coefficient(n-1, k-1) + binomial_coefficient(n-1, k)

print("\n二项式系数计算:")
print(f"C(10, 5) = {binomial_coefficient(10, 5)}")
print(f"C(20, 10) = {binomial_coefficient(20, 10)}")

# 查看缓存命中
print(f"缓存信息: {binomial_coefficient.cache_info()}")

# 6. 自定义缓存键
def make_cache_key(func_name, args, kwargs):
    """自定义缓存键函数"""
    # 将参数转换为可哈希的形式
    key_parts = [func_name]
    key_parts.extend(str(arg) for arg in args)
    key_parts.extend(f"{k}={v}" for k, v in sorted(kwargs.items()))
    return tuple(key_parts)

@lru_cache(maxsize=16)
def complex_function(a, b, c=None, d=None):
    """复杂函数，带有可选参数"""
    print(f"计算 complex_function(a={a}, b={b}, c={c}, d={d})")
    time.sleep(0.1)
    return a + b + (c or 0) + (d or 0)

print("\n自定义缓存键示例:")
calls = [
    (1, 2, 3, 4),
    (1, 2, 3, 4),  # 重复调用
    (1, 2, None, 4),  # 不同参数
    (1, 2, 3, 4),  # 再次重复
]

for a, b, c, d in calls:
    result = complex_function(a, b, c=c, d=d)
    print(f"结果: {result}")

print(f"缓存信息: {complex_function.cache_info()}")

# 7. 缓存统计和监控
def create_monitored_cache(maxsize=128):
    """创建带监控的缓存函数"""

    def decorator(func):
        cached_func = lru_cache(maxsize=maxsize)(func)

        def wrapper(*args, **kwargs):
            result = cached_func(*args, **kwargs)
            cache_info = cached_func.cache_info()

            print(f"函数: {func.__name__}")
            print(f"  缓存命中: {cache_info.hits}")
            print(f"  缓存未命中: {cache_info.misses}")
            print(f"  缓存大小: {cache_info.currsize}/{cache_info.maxsize}")
            print(f"  结果: {result}")
            print()

            return result

        # 保留原始函数的元数据
        wrapper.__name__ = func.__name__
        wrapper.__doc__ = func.__doc__
        wrapper.cache_info = cached_func.cache_info
        wrapper.cache_clear = cached_func.cache_clear

        return wrapper

    return decorator

@create_monitored_cache(maxsize=4)
def monitored_function(x):
    """被监控的函数"""
    time.sleep(0.05)
    return x * x

print("缓存监控示例:")
for i in [1, 2, 1, 3, 2, 4, 1, 5]:
    monitored_function(i)
```

### 22.3.4 其他工具函数

```python
from functools import wraps, singledispatch, total_ordering
import time

# 其他工具函数
print("其他工具函数:")

# 1. wraps 装饰器
print("wraps 装饰器:")

def timing_decorator(func):
    """计时装饰器"""
    @wraps(func)  # 保留原始函数的元数据
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"{func.__name__} 执行时间: {end_time - start_time:.6f}秒")
        return result
    return wrapper

@timing_decorator
def slow_function(n):
    """一个慢函数"""
    time.sleep(n * 0.1)
    return f"处理了 {n} 项"

print(f"函数名: {slow_function.__name__}")
print(f"函数文档: {slow_function.__doc__}")
result = slow_function(3)
print(f"结果: {result}")

# 2. singledispatch 装饰器
print("\nsingledispatch 装饰器:")

@singledispatch
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

@process_data.register
def _(data: dict):
    return f"字典键数: {len(data)}"

print("单分派示例:")
test_data = [
    "hello",
    42,
    [1, 2, 3, 4],
    {"a": 1, "b": 2},
    3.14  # 未注册的类型
]

for data in test_data:
    result = process_data(data)
    print(f"  {data} -> {result}")

# 3. total_ordering 装饰器
print("\ntotal_ordering 装饰器:")

@total_ordering
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __eq__(self, other):
        if not isinstance(other, Person):
            return NotImplemented
        return (self.name, self.age) == (other.name, other.age)

    def __lt__(self, other):
        if not isinstance(other, Person):
            return NotImplemented
        return (self.name, self.age) < (other.name, other.age)

    def __repr__(self):
        return f"Person('{self.name}', {self.age})"

people = [
    Person("Alice", 30),
    Person("Bob", 25),
    Person("Charlie", 35),
    Person("Alice", 28)
]

print("排序前:", people)
people.sort()
print("排序后:", people)

# 测试所有比较操作
p1 = Person("Alice", 30)
p2 = Person("Bob", 25)

print(f"p1 == p2: {p1 == p2}")
print(f"p1 != p2: {p1 != p2}")
print(f"p1 < p2: {p1 < p2}")
print(f"p1 <= p2: {p1 <= p2}")
print(f"p1 > p2: {p1 > p2}")
print(f"p1 >= p2: {p1 >= p2}")

# 实际应用示例
print("\n实际应用示例:")

# 1. 装饰器工厂
def retry(max_attempts=3, delay=1):
    """重试装饰器工厂"""
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
            return None
        return wrapper
    return decorator

@retry(max_attempts=3, delay=0.5)
def unreliable_function():
    """不可靠的函数"""
    if time.time() % 2 > 1:  # 随机失败
        raise ValueError("随机错误")
    return "成功"

print("重试装饰器示例:")
result = unreliable_function()
print(f"最终结果: {result}")

# 2. 类型分派的实际应用
@singledispatch
def serialize(data):
    """序列化不同类型的数据"""
    return str(data)

@serialize.register
def _(data: str):
    return f"string:{data}"

@serialize.register
def _(data: int):
    return f"int:{data:04d}"

@serialize.register
def _(data: float):
    return f"float:{data:.2f}"

@serialize.register
def _(data: list):
    return f"list:{','.join(serialize(item) for item in data)}"

@serialize.register
def _(data: dict):
    items = [f"{k}={serialize(v)}" for k, v in data.items()]
    return f"dict:{{{','.join(items)}}}"

print("\n序列化示例:")
test_objects = [
    "hello",
    42,
    3.14159,
    [1, "two", 3.0],
    {"name": "Alice", "age": 30, "scores": [85, 92, 88]}
]

for obj in test_objects:
    serialized = serialize(obj)
    print(f"  {obj} -> {serialized}")

# 3. 完整的比较类
@total_ordering
class Version:
    """版本号类"""

    def __init__(self, version_string):
        self.parts = [int(x) for x in version_string.split('.')]

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

versions = [
    Version("1.0.0"),
    Version("1.10.0"),
    Version("1.2.0"),
    Version("2.0.0"),
    Version("1.1.5")
]

print("\n版本排序:")
print("排序前:", versions)
versions.sort()
print("排序后:", versions)

# 测试版本比较
v1 = Version("1.2.3")
v2 = Version("1.2.4")
v3 = Version("1.2.3")

print(f"v1 == v3: {v1 == v3}")
print(f"v1 < v2: {v1 < v2}")
print(f"v2 > v1: {v2 > v1}")
print(f"v1 <= v3: {v1 <= v3}")
print(f"v2 >= v1: {v2 >= v1}")
```

## 总结

本章介绍了 Python 的数据处理模块：
- 掌握 collections 模块的高级数据结构，包括 Counter、defaultdict、OrderedDict 和 deque
- 学会使用 itertools 模块的各种迭代器工具，从无限迭代器到组合生成器
- 熟练运用 functools 模块的函数式编程工具，特别是 partial、reduce 和 lru_cache

通过本章的学习，你应该能够：
1. 使用 Counter 进行高效的计数和统计操作
2. 利用 defaultdict 简化字典操作和分组任务
3. 应用 OrderedDict 维护插入顺序和实现 LRU 缓存
4. 运用 deque 实现高效的双端队列操作
5. 使用 itertools 的各种迭代器进行数据处理和组合生成
6. 应用 functools 的工具函数进行函数式编程和性能优化
7. 实现缓存、偏函数和类型分派等高级功能

下一章将介绍装饰器和生成器的相关内容。
