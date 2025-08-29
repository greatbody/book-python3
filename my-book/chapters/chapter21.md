# 第21章：常用模块

## 21.1 datetime 模块

### 21.1.1 日期和时间对象

```python
from datetime import datetime, date, time, timedelta
import time as time_module

# 创建日期和时间对象
print("创建日期和时间对象:")

# 当前日期和时间
now = datetime.now()
print(f"当前日期时间: {now}")
print(f"当前日期: {now.date()}")
print(f"当前时间: {now.time()}")

# 今天日期
today = date.today()
print(f"今天日期: {today}")

# 创建特定日期
specific_date = date(2024, 12, 25)
print(f"特定日期: {specific_date}")

# 创建特定时间
specific_time = time(14, 30, 45)
print(f"特定时间: {specific_time}")

# 创建特定日期时间
specific_datetime = datetime(2024, 12, 25, 14, 30, 45)
print(f"特定日期时间: {specific_datetime}")

# 从时间戳创建
timestamp = 1700000000
from_timestamp = datetime.fromtimestamp(timestamp)
print(f"从时间戳创建: {from_timestamp}")

# 日期时间属性
print(f"\n日期时间属性:")
print(f"年: {now.year}")
print(f"月: {now.month}")
print(f"日: {now.day}")
print(f"时: {now.hour}")
print(f"分: {now.minute}")
print(f"秒: {now.second}")
print(f"微秒: {now.microsecond}")
print(f"星期几 (0=周一): {now.weekday()}")
print(f"星期几 (1=周一): {now.isoweekday()}")
print(f"ISO 日历: {now.isocalendar()}")
```

### 21.1.2 日期时间格式化

```python
# 日期时间格式化
print("日期时间格式化:")

now = datetime.now()

# strftime - 格式化为字符串
print("strftime 格式化:")
formats = [
    "%Y-%m-%d",           # 2024-01-15
    "%H:%M:%S",           # 14:30:45
    "%Y-%m-%d %H:%M:%S",  # 2024-01-15 14:30:45
    "%A, %B %d, %Y",      # Monday, January 15, 2024
    "%d/%m/%Y",           # 15/01/2024
    "%Y%m%d_%H%M%S",      # 20240115_143045
    "%I:%M %p",           # 02:30 PM
    "%j",                 # 一年中的第几天
    "%U",                 # 一年中的第几周 (周日为第一天)
    "%W"                  # 一年中的第几周 (周一为第一天)
]

for fmt in formats:
    print(f"  {fmt:15}: {now.strftime(fmt)}")

# strptime - 从字符串解析
print("\nstrptime 解析:")
date_strings = [
    "2024-01-15",
    "2024/01/15 14:30:45",
    "January 15, 2024",
    "15-Jan-2024"
]

parse_formats = [
    "%Y-%m-%d",
    "%Y/%m/%d %H:%M:%S",
    "%B %d, %Y",
    "%d-%b-%Y"
]

for date_str, fmt in zip(date_strings, parse_formats):
    try:
        parsed = datetime.strptime(date_str, fmt)
        print(f"  {date_str:15} -> {parsed}")
    except ValueError as e:
        print(f"  {date_str:15} -> 解析失败: {e}")

# ISO 格式
print("\nISO 格式:")
iso_string = "2024-01-15T14:30:45"
parsed_iso = datetime.fromisoformat(iso_string)
print(f"ISO 字符串: {iso_string}")
print(f"解析结果: {parsed_iso}")
print(f"ISO 格式: {parsed_iso.isoformat()}")
```

### 21.1.3 日期时间运算

```python
# 日期时间运算
print("日期时间运算:")

# 创建基准时间
base_time = datetime(2024, 1, 15, 14, 30, 45)
print(f"基准时间: {base_time}")

# 时间间隔
print("\n时间间隔运算:")

# 加法运算
future_time = base_time + timedelta(days=7, hours=2, minutes=30)
print(f"7天2小时30分钟后: {future_time}")

# 减法运算
past_time = base_time - timedelta(days=3, hours=1)
print(f"3天1小时前: {past_time}")

# 时间间隔计算
time_diff = future_time - past_time
print(f"时间差: {time_diff}")
print(f"总秒数: {time_diff.total_seconds()}")

# 日期运算
print("\n日期运算:")
base_date = date(2024, 1, 15)
print(f"基准日期: {base_date}")

# 日期加法
next_week = base_date + timedelta(days=7)
print(f"一周后: {next_week}")

next_month = base_date.replace(month=base_date.month + 1)
print(f"一个月后: {next_month}")

# 日期比较
print("\n日期比较:")
date1 = date(2024, 1, 15)
date2 = date(2024, 2, 15)
date3 = date(2024, 1, 15)

print(f"date1 == date2: {date1 == date2}")
print(f"date1 == date3: {date1 == date3}")
print(f"date1 < date2: {date1 < date2}")
print(f"date1 <= date3: {date1 <= date3}")

# 计算两个日期之间的天数
days_between = (date2 - date1).days
print(f"date1 到 date2 的天数: {days_between}")

# 工作日计算
print("\n工作日计算:")
def is_weekend(date_obj):
    """判断是否为周末"""
    return date_obj.weekday() >= 5  # 5=周六, 6=周日

def add_business_days(start_date, days):
    """添加工作日"""
    current_date = start_date
    while days > 0:
        current_date += timedelta(days=1)
        if not is_weekend(current_date):
            days -= 1
    return current_date

start_date = date(2024, 1, 15)  # 假设是周一
business_days_later = add_business_days(start_date, 5)
print(f"5个工作日后: {business_days_later}")
```

### 21.1.4 时区处理

```python
from datetime import timezone, tzinfo
import pytz

# 时区处理
print("时区处理:")

# UTC 时间
utc_now = datetime.now(timezone.utc)
print(f"UTC 时间: {utc_now}")

# 本地时间
local_now = datetime.now()
print(f"本地时间: {local_now}")

# 创建带时区的日期时间
print("\n创建带时区的日期时间:")

# 东八区 (北京时间)
beijing_tz = timezone(timedelta(hours=8))
beijing_time = datetime(2024, 1, 15, 14, 30, 45, tzinfo=beijing_tz)
print(f"北京时间: {beijing_time}")

# 纽约时间 (东五区)
ny_tz = timezone(timedelta(hours=-5))
ny_time = beijing_time.astimezone(ny_tz)
print(f"纽约时间: {ny_time}")

# 伦敦时间 (UTC)
london_time = beijing_time.astimezone(timezone.utc)
print(f"伦敦时间: {london_time}")

# 使用 pytz 处理复杂时区
print("\n使用 pytz 处理复杂时区:")
try:
    # 创建时区对象
    beijing = pytz.timezone('Asia/Shanghai')
    new_york = pytz.timezone('America/New_York')
    london = pytz.timezone('Europe/London')

    # 创建本地化时间
    naive_time = datetime(2024, 1, 15, 14, 30, 45)
    beijing_localized = beijing.localize(naive_time)
    print(f"北京本地化时间: {beijing_localized}")

    # 转换到其他时区
    ny_converted = beijing_localized.astimezone(new_york)
    london_converted = beijing_localized.astimezone(london)

    print(f"转换为纽约时间: {ny_converted}")
    print(f"转换为伦敦时间: {london_converted}")

    # 处理夏令时
    print("\n夏令时处理:")
    # 2024年3月 (夏令时开始)
    dst_start = datetime(2024, 3, 10, 2, 0, 0)
    dst_beijing = beijing.localize(dst_start)
    dst_ny = dst_beijing.astimezone(new_york)
    print(f"北京时间 {dst_beijing} 对应纽约时间: {dst_ny}")

    # 2024年11月 (夏令时结束)
    dst_end = datetime(2024, 11, 3, 1, 0, 0)
    dst_end_beijing = beijing.localize(dst_end)
    dst_end_ny = dst_end_beijing.astimezone(new_york)
    print(f"北京时间 {dst_end_beijing} 对应纽约时间: {dst_end_ny}")

except ImportError:
    print("pytz 模块未安装，请使用: pip install pytz")

# 时区转换函数
def convert_timezone(dt, from_tz, to_tz):
    """在时区之间转换时间"""
    if dt.tzinfo is None:
        # 如果是 naive 时间，假设为 from_tz
        dt = from_tz.localize(dt)

    return dt.astimezone(to_tz)

print("\n时区转换函数示例:")
try:
    # 创建时区对象
    utc = pytz.timezone('UTC')
    tokyo = pytz.timezone('Asia/Tokyo')

    # 转换示例
    utc_time = datetime(2024, 1, 15, 12, 0, 0, tzinfo=utc)
    tokyo_time = convert_timezone(utc_time, utc, tokyo)
    print(f"UTC {utc_time} -> 东京 {tokyo_time}")

except NameError:
    print("需要安装 pytz 才能运行时区转换示例")
```

### 21.1.5 实际应用示例

```python
# 实际应用示例
print("实际应用示例:")

# 1. 计算年龄
def calculate_age(birth_date):
    """计算年龄"""
    today = date.today()
    age = today.year - birth_date.year

    # 检查是否已经过生日
    if (today.month, today.day) < (birth_date.month, birth_date.day):
        age -= 1

    return age

birth_date = date(1990, 5, 15)
age = calculate_age(birth_date)
print(f"出生日期: {birth_date}, 年龄: {age}")

# 2. 会议安排
def schedule_meeting(start_time, duration_hours, participants_tz):
    """安排会议时间"""
    try:
        from pytz import timezone

        # 将开始时间转换为 UTC
        local_tz = timezone('Asia/Shanghai')  # 假设本地时区
        if start_time.tzinfo is None:
            start_time = local_tz.localize(start_time)

        utc_start = start_time.astimezone(timezone('UTC'))
        utc_end = utc_start + timedelta(hours=duration_hours)

        print(f"会议时间 (UTC): {utc_start} - {utc_end}")

        # 显示每个参与者的本地时间
        for name, tz_name in participants_tz.items():
            participant_tz = timezone(tz_name)
            local_start = utc_start.astimezone(participant_tz)
            local_end = utc_end.astimezone(participant_tz)
            print(f"{name} 的本地时间: {local_start} - {local_end}")

    except ImportError:
        print("需要安装 pytz 才能运行会议安排示例")

# 3. 日志时间戳
def log_with_timestamp(message, level="INFO"):
    """带时间戳的日志记录"""
    timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    print(f"[{timestamp}] {level}: {message}")

log_with_timestamp("应用程序启动")
time_module.sleep(1)
log_with_timestamp("处理用户请求", "DEBUG")
time_module.sleep(1)
log_with_timestamp("请求处理完成", "INFO")

# 4. 倒计时功能
def countdown_timer(target_datetime):
    """倒计时功能"""
    while True:
        now = datetime.now()
        if now >= target_datetime:
            print("时间到！")
            break

        remaining = target_datetime - now
        days = remaining.days
        hours, remainder = divmod(remaining.seconds, 3600)
        minutes, seconds = divmod(remainder, 60)

        print(f"剩余时间: {days}天 {hours:02d}:{minutes:02d}:{seconds:02d}", end='\r')
        time_module.sleep(1)

print("\n倒计时示例 (5秒):")
target = datetime.now() + timedelta(seconds=5)
countdown_timer(target)

# 5. 工作时间计算
def calculate_work_hours(start_time, end_time, break_minutes=60):
    """计算工作时间"""
    work_duration = end_time - start_time
    break_duration = timedelta(minutes=break_minutes)

    actual_work = work_duration - break_duration

    print(f"开始时间: {start_time}")
    print(f"结束时间: {end_time}")
    print(f"总时长: {work_duration}")
    print(f"休息时间: {break_duration}")
    print(f"实际工作时间: {actual_work}")

    return actual_work

# 示例
start = datetime(2024, 1, 15, 9, 0, 0)
end = datetime(2024, 1, 15, 17, 30, 0)
calculate_work_hours(start, end)
```

## 21.2 json 模块

### 21.2.1 JSON 基础

```python
import json

# JSON 基础
print("JSON 基础:")

# Python 对象到 JSON 字符串
data = {
    "name": "Alice",
    "age": 30,
    "city": "北京",
    "skills": ["Python", "JavaScript", "SQL"],
    "active": True,
    "profile": {
        "department": "工程部",
        "level": "高级工程师"
    }
}

# 序列化
json_string = json.dumps(data, ensure_ascii=False, indent=2)
print("Python 对象序列化为 JSON:")
print(json_string)

# 反序列化
parsed_data = json.loads(json_string)
print(f"\nJSON 反序列化为 Python 对象:")
print(f"类型: {type(parsed_data)}")
print(f"姓名: {parsed_data['name']}")
print(f"技能: {parsed_data['skills']}")

# 支持的数据类型
print(f"\nJSON 支持的数据类型:")
print(f"字符串: {'Hello'} -> {json.dumps('Hello')}")
print(f"数字: {42} -> {json.dumps(42)}")
print(f"浮点数: {3.14} -> {json.dumps(3.14)}")
print(f"布尔值: {True} -> {json.dumps(True)}")
print(f"空值: {None} -> {json.dumps(None)}")
print(f"列表: {[1, 2, 3]} -> {json.dumps([1, 2, 3])}")
print(f"字典: {{'a': 1}} -> {json.dumps({'a': 1})}")
```

### 21.2.2 文件读写

```python
# JSON 文件读写
print("JSON 文件读写:")

# 准备测试数据
user_data = {
    "users": [
        {
            "id": 1,
            "name": "Alice",
            "email": "alice@example.com",
            "profile": {
                "age": 30,
                "city": "北京",
                "interests": ["编程", "阅读", "旅行"]
            }
        },
        {
            "id": 2,
            "name": "Bob",
            "email": "bob@example.com",
            "profile": {
                "age": 25,
                "city": "上海",
                "interests": ["音乐", "运动", "摄影"]
            }
        }
    ],
    "metadata": {
        "version": "1.0",
        "created": "2024-01-15",
        "total_users": 2
    }
}

# 写入 JSON 文件
print("写入 JSON 文件:")
with open('users.json', 'w', encoding='utf-8') as f:
    json.dump(user_data, f, ensure_ascii=False, indent=2)

print("用户数据已保存到 users.json")

# 读取 JSON 文件
print("\n读取 JSON 文件:")
with open('users.json', 'r', encoding='utf-8') as f:
    loaded_data = json.load(f)

print(f"加载的用户数量: {loaded_data['metadata']['total_users']}")
print("用户列表:")
for user in loaded_data['users']:
    print(f"  - {user['name']} ({user['email']})")

# 验证数据完整性
print(f"\n数据完整性检查:")
print(f"原始数据用户数: {len(user_data['users'])}")
print(f"加载数据用户数: {len(loaded_data['users'])}")
print(f"数据是否相同: {user_data == loaded_data}")
```

### 21.2.3 自定义编码和解码

```python
# 自定义编码和解码
print("自定义编码和解码:")

# 自定义对象
class Person:
    def __init__(self, name, age, city):
        self.name = name
        self.age = age
        self.city = city

    def __repr__(self):
        return f"Person(name='{self.name}', age={self.age}, city='{self.city}')"

# 自定义编码器
class PersonEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, Person):
            return {
                "__type__": "Person",
                "name": obj.name,
                "age": obj.age,
                "city": obj.city
            }
        return super().default(obj)

# 自定义解码器
def person_decoder(obj):
    if "__type__" in obj and obj["__type__"] == "Person":
        return Person(obj["name"], obj["age"], obj["city"])
    return obj

# 测试自定义编码解码
person = Person("Charlie", 35, "深圳")

# 编码
encoded = json.dumps(person, cls=PersonEncoder, ensure_ascii=False, indent=2)
print("自定义对象编码:")
print(encoded)

# 解码
decoded = json.loads(encoded, object_hook=person_decoder)
print(f"\n解码结果: {decoded}")
print(f"类型: {type(decoded)}")

# 处理日期时间
print("\n处理日期时间:")

from datetime import datetime

class DateTimeEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, datetime):
            return {
                "__type__": "datetime",
                "value": obj.isoformat()
            }
        return super().default(obj)

def datetime_decoder(obj):
    if "__type__" in obj and obj["__type__"] == "datetime":
        return datetime.fromisoformat(obj["value"])
    return obj

# 测试日期时间编码解码
now = datetime.now()
data_with_datetime = {
    "event": "系统启动",
    "timestamp": now,
    "details": {
        "version": "1.0",
        "status": "running"
    }
}

# 编码
encoded_datetime = json.dumps(data_with_datetime, cls=DateTimeEncoder, ensure_ascii=False, indent=2)
print("包含日期时间的对象编码:")
print(encoded_datetime)

# 解码
decoded_datetime = json.loads(encoded_datetime, object_hook=datetime_decoder)
print(f"\n解码结果:")
print(f"事件: {decoded_datetime['event']}")
print(f"时间戳: {decoded_datetime['timestamp']}")
print(f"时间戳类型: {type(decoded_datetime['timestamp'])}")
```

### 21.2.4 JSON 模式验证

```python
# JSON 模式验证
print("JSON 模式验证:")

# 简单的模式验证函数
def validate_user_data(data):
    """验证用户数据"""
    required_fields = ['name', 'email', 'age']
    errors = []

    # 检查必需字段
    for field in required_fields:
        if field not in data:
            errors.append(f"缺少必需字段: {field}")

    # 验证字段类型和值
    if 'name' in data and not isinstance(data['name'], str):
        errors.append("name 必须是字符串")

    if 'email' in data and not isinstance(data['email'], str):
        errors.append("email 必须是字符串")

    if 'age' in data:
        if not isinstance(data['age'], int):
            errors.append("age 必须是整数")
        elif data['age'] < 0 or data['age'] > 150:
            errors.append("age 必须在 0-150 之间")

    return errors

# 测试数据
test_users = [
    {"name": "Alice", "email": "alice@example.com", "age": 30},
    {"name": "Bob", "email": "bob@example.com"},  # 缺少 age
    {"name": "Charlie", "email": "charlie@example.com", "age": "25"},  # age 不是整数
    {"email": "diana@example.com", "age": 28},  # 缺少 name
]

print("用户数据验证:")
for i, user in enumerate(test_users, 1):
    errors = validate_user_data(user)
    print(f"\n用户 {i}: {user}")
    if errors:
        print(f"  验证错误: {errors}")
    else:
        print("  验证通过")

# 使用 jsonschema 进行高级验证
try:
    import jsonschema

    # 定义 JSON 模式
    user_schema = {
        "type": "object",
        "properties": {
            "name": {"type": "string", "minLength": 1},
            "email": {"type": "string", "format": "email"},
            "age": {"type": "integer", "minimum": 0, "maximum": 150},
            "active": {"type": "boolean"}
        },
        "required": ["name", "email"],
        "additionalProperties": False
    }

    # 测试模式验证
    test_user = {
        "name": "Eve",
        "email": "eve@example.com",
        "age": 28,
        "active": True
    }

    try:
        jsonschema.validate(test_user, user_schema)
        print(f"\n高级模式验证通过: {test_user}")
    except jsonschema.ValidationError as e:
        print(f"\n高级模式验证失败: {e.message}")

except ImportError:
    print("\n需要安装 jsonschema: pip install jsonschema")
```

### 21.2.5 实际应用示例

```python
# 实际应用示例
print("实际应用示例:")

# 1. 配置文件管理
def load_config(config_file='config.json'):
    """加载配置文件"""
    try:
        with open(config_file, 'r', encoding='utf-8') as f:
            config = json.load(f)
        return config
    except FileNotFoundError:
        # 返回默认配置
        return {
            "database": {
                "host": "localhost",
                "port": 5432,
                "name": "myapp"
            },
            "logging": {
                "level": "INFO",
                "file": "app.log"
            }
        }

def save_config(config, config_file='config.json'):
    """保存配置文件"""
    with open(config_file, 'w', encoding='utf-8') as f:
        json.dump(config, f, ensure_ascii=False, indent=2)

# 测试配置文件管理
config = load_config()
print("加载配置:")
print(json.dumps(config, ensure_ascii=False, indent=2))

# 修改配置
config['database']['port'] = 3306
config['logging']['level'] = 'DEBUG'

save_config(config)
print("\n配置已保存")

# 2. API 数据处理
def process_api_response(response_data):
    """处理 API 响应数据"""
    try:
        # 假设 response_data 是 JSON 字符串
        data = json.loads(response_data)

        # 提取需要的信息
        result = {
            'status': data.get('status', 'unknown'),
            'total': data.get('total', 0),
            'items': []
        }

        for item in data.get('items', []):
            processed_item = {
                'id': item.get('id'),
                'name': item.get('name', 'Unknown'),
                'active': item.get('active', False)
            }
            result['items'].append(processed_item)

        return result

    except json.JSONDecodeError as e:
        print(f"JSON 解析错误: {e}")
        return None

# 测试 API 数据处理
api_response = '''{
    "status": "success",
    "total": 2,
    "items": [
        {"id": 1, "name": "Item 1", "active": true},
        {"id": 2, "name": "Item 2", "active": false}
    ]
}'''

processed = process_api_response(api_response)
if processed:
    print("\n处理后的 API 数据:")
    print(json.dumps(processed, ensure_ascii=False, indent=2))

# 3. 数据缓存
import hashlib
import os

class JSONCache:
    """简单的 JSON 数据缓存"""

    def __init__(self, cache_dir='.cache'):
        self.cache_dir = cache_dir
        os.makedirs(cache_dir, exist_ok=True)

    def _get_cache_key(self, key):
        """生成缓存键"""
        return hashlib.md5(key.encode()).hexdigest() + '.json'

    def get(self, key):
        """获取缓存数据"""
        cache_file = os.path.join(self.cache_dir, self._get_cache_key(key))
        if os.path.exists(cache_file):
            try:
                with open(cache_file, 'r', encoding='utf-8') as f:
                    return json.load(f)
            except (json.JSONDecodeError, IOError):
                return None
        return None

    def set(self, key, data, ttl=None):
        """设置缓存数据"""
        cache_file = os.path.join(self.cache_dir, self._get_cache_key(key))
        try:
            with open(cache_file, 'w', encoding='utf-8') as f:
                json.dump(data, f, ensure_ascii=False, indent=2)
            return True
        except IOError:
            return False

    def delete(self, key):
        """删除缓存数据"""
        cache_file = os.path.join(self.cache_dir, self._get_cache_key(key))
        if os.path.exists(cache_file):
            os.remove(cache_file)
            return True
        return False

# 测试缓存
cache = JSONCache()

# 存储数据
test_data = {"message": "Hello, Cache!", "timestamp": str(datetime.now())}
cache.set("greeting", test_data)

# 检索数据
cached_data = cache.get("greeting")
if cached_data:
    print(f"\n缓存数据: {cached_data}")

# 清理
cache.delete("greeting")
os.remove('users.json')
os.remove('config.json')
```

## 21.3 re 模块（正则表达式）

### 21.3.1 正则表达式基础

```python
import re

# 正则表达式基础
print("正则表达式基础:")

# 基本模式匹配
text = "Hello, my email is user@example.com and phone is 123-456-7890"

# 匹配邮箱
email_pattern = r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b'
emails = re.findall(email_pattern, text)
print(f"找到的邮箱: {emails}")

# 匹配电话号码
phone_pattern = r'\d{3}-\d{3}-\d{4}'
phones = re.findall(phone_pattern, text)
print(f"找到的电话: {phones}")

# 常用正则表达式模式
patterns = {
    "邮箱": r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b',
    "URL": r'https?://(?:[-\w.])+(?:[:\d]+)?(?:/(?:[\w/_.])*(?:\?(?:[\w&=%.])*)?(?:#(?:\w*))*)?',
    "IP地址": r'\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b',
    "日期(YYYY-MM-DD)": r'\b\d{4}-\d{2}-\d{2}\b',
    "时间(HH:MM)": r'\b\d{2}:\d{2}\b',
    "中国手机号": r'1[3-9]\d{9}',
    "身份证号": r'\d{17}[\dXx]'
}

test_text = """
网址: https://www.example.com/path?param=value#section
IP: 192.168.1.1
日期: 2024-01-15
时间: 14:30
手机号: 13800138000
身份证: 110101199001011234
邮箱: test@example.com
"""

print("\n模式匹配测试:")
for name, pattern in patterns.items():
    matches = re.findall(pattern, test_text)
    if matches:
        print(f"{name}: {matches}")
```

### 21.3.2 re 模块主要函数

```python
# re 模块主要函数
print("re 模块主要函数:")

text = "Python is a programming language. Python is easy to learn."

# 1. re.match() - 从字符串开头匹配
print("1. re.match():")
match = re.match(r'Python', text)
if match:
    print(f"匹配成功: '{match.group()}' at position {match.start()}-{match.end()}")

# 2. re.search() - 在字符串中搜索第一个匹配
print("\n2. re.search():")
search = re.search(r'language', text)
if search:
    print(f"搜索到: '{search.group()}' at position {search.start()}-{search.end()}")

# 3. re.findall() - 查找所有匹配
print("\n3. re.findall():")
all_matches = re.findall(r'Python', text)
print(f"所有匹配: {all_matches}")

# 4. re.finditer() - 返回匹配对象的迭代器
print("\n4. re.finditer():")
for match in re.finditer(r'Python', text):
    print(f"匹配: '{match.group()}' at {match.start()}-{match.end()}")

# 5. re.sub() - 替换匹配
print("\n5. re.sub():")
replaced = re.sub(r'Python', 'Java', text)
print(f"替换后: {replaced}")

# 替换次数限制
replaced_limited = re.sub(r'Python', 'Java', text, count=1)
print(f"替换一次: {replaced_limited}")

# 6. re.split() - 按模式分割字符串
print("\n6. re.split():")
split_result = re.split(r'\s+', text)  # 按空白字符分割
print(f"分割结果: {split_result}")

# 保留分隔符
split_with_sep = re.split(r'(\s+)', text)
print(f"保留分隔符: {split_with_sep}")
```

### 21.3.3 编译正则表达式

```python
# 编译正则表达式
print("编译正则表达式:")

# 编译模式以提高性能
pattern = re.compile(r'\b\d{4}-\d{2}-\d{2}\b')  # 日期模式

# 测试文本
dates_text = "重要日期: 2024-01-15, 2023-12-25, 2022-06-30, 无效日期: 2024-13-45"

# 使用编译后的模式
matches = pattern.findall(dates_text)
print(f"找到的日期: {matches}")

# 获取匹配对象
for match in pattern.finditer(dates_text):
    print(f"日期: {match.group()} 位置: {match.start()}-{match.end()}")

# 模式标志
print("\n模式标志:")

text_with_case = "Python PYTHON python PyThOn"

# 不区分大小写
case_insensitive = re.compile(r'python', re.IGNORECASE)
matches_ci = case_insensitive.findall(text_with_case)
print(f"不区分大小写: {matches_ci}")

# 多行模式
multiline_text = """Line 1: Hello
Line 2: World
Line 3: Python"""

# ^ 和 $ 在多行模式下的行为
single_line = re.findall(r'^Line \d+', multiline_text)
multiline = re.findall(r'^Line \d+', multiline_text, re.MULTILINE)

print(f"单行模式: {single_line}")
print(f"多行模式: {multiline}")

# DOTALL 模式
dot_text = "Hello\nWorld\nPython"
normal_dot = re.findall(r'Hello.*Python', dot_text)
dotall = re.findall(r'Hello.*Python', dot_text, re.DOTALL)

print(f"普通模式: {normal_dot}")
print(f"DOTALL模式: {dotall}")

# 详细模式 (VERBOSE)
verbose_pattern = re.compile(r"""
    \b              # 单词边界
    \d{4}           # 四位数字 (年)
    -               # 连字符
    \d{2}           # 二位数字 (月)
    -               # 连字符
    \d{2}           # 二位数字 (日)
    \b              # 单词边界
""", re.VERBOSE)

verbose_matches = verbose_pattern.findall("日期: 2024-01-15 和 2023-12-25")
print(f"详细模式匹配: {verbose_matches}")
```

### 21.3.4 分组和捕获

```python
# 分组和捕获
print("分组和捕获:")

# 基本分组
text = "联系电话: 138-0013-8000, 工作电话: 010-12345678"

# 捕获组
phone_pattern = r'(\d{3})-(\d{4})-(\d{4})'
matches = re.findall(phone_pattern, text)
print(f"捕获的电话号码组: {matches}")

# 访问匹配对象
match = re.search(phone_pattern, text)
if match:
    print(f"完整匹配: {match.group(0)}")
    print(f"第一组: {match.group(1)}")
    print(f"第二组: {match.group(2)}")
    print(f"第三组: {match.group(3)}")
    print(f"所有组: {match.groups()}")

# 命名组
named_pattern = r'(?P<area>\d{3})-(?P<prefix>\d{4})-(?P<number>\d{4})'
named_match = re.search(named_pattern, text)
if named_match:
    print(f"\n命名组匹配:")
    print(f"区域码: {named_match.group('area')}")
    print(f"前缀: {named_match.group('prefix')}")
    print(f"号码: {named_match.group('number')}")
    print(f"组字典: {named_match.groupdict()}")

# 非捕获组
non_capturing = r'(?:\d{3}-)\d{4}-\d{4}'  # 第一组不捕获
nc_matches = re.findall(non_capturing, text)
print(f"\n非捕获组匹配: {nc_matches}")

# 后向引用
duplicate_pattern = r'(\b\w+)\s+\1\b'  # 匹配重复单词
duplicate_text = "This is is a test test sentence"
duplicates = re.findall(duplicate_pattern, duplicate_text)
print(f"重复单词: {duplicates}")

# 替换中使用组
replacement_text = "日期: 2024-01-15, 时间: 14:30:45"
# 将日期格式从 YYYY-MM-DD 改为 MM/DD/YYYY
reformatted = re.sub(r'(\d{4})-(\d{2})-(\d{2})', r'\2/\3/\1', replacement_text)
print(f"重新格式化日期: {reformatted}")
```

### 21.3.5 实际应用示例

```python
# 实际应用示例
print("实际应用示例:")

# 1. 邮箱验证和提取
def extract_emails(text):
    """提取文本中的所有邮箱地址"""
    email_pattern = r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b'
    return re.findall(email_pattern, text)

# 2. URL 提取
def extract_urls(text):
    """提取文本中的 URL"""
    url_pattern = r'https?://(?:[-\w.])+(?:[:\d]+)?(?:/(?:[\w/_.])*(?:\?(?:[\w&=%.])*)?(?:#(?:\w*))*)?'
    return re.findall(url_pattern, text)

# 3. 日志解析
def parse_log_line(log_line):
    """解析日志行"""
    # 匹配格式: [2024-01-15 14:30:45] INFO: User login successful
    pattern = r'\[(\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2})\]\s+(\w+):\s+(.+)'
    match = re.search(pattern, log_line)
    if match:
        timestamp, level, message = match.groups()
        return {
            'timestamp': timestamp,
            'level': level,
            'message': message
        }
    return None

# 4. HTML 标签清理
def clean_html_tags(text):
    """清理 HTML 标签"""
    # 移除所有 HTML 标签
    clean_pattern = r'<[^>]+>'
    return re.sub(clean_pattern, '', text)

# 5. 密码强度检查
def check_password_strength(password):
    """检查密码强度"""
    # 至少8个字符，包含大写、小写、数字、特殊字符
    patterns = {
        'length': len(password) >= 8,
        'uppercase': bool(re.search(r'[A-Z]', password)),
        'lowercase': bool(re.search(r'[a-z]', password)),
        'digit': bool(re.search(r'\d', password)),
        'special': bool(re.search(r'[!@#$%^&*(),.?":{}|<>]', password))
    }

    score = sum(patterns.values())
    strength = ['Very Weak', 'Weak', 'Fair', 'Good', 'Strong'][score]

    return strength, patterns

# 测试应用示例
test_text = """
请联系我们：
邮箱: support@example.com 或 info@company.com
网站: https://www.example.com 和 http://test.com/page?param=value
"""

print("邮箱提取:", extract_emails(test_text))
print("URL提取:", extract_urls(test_text))

# 测试日志解析
log_lines = [
    "[2024-01-15 14:30:45] INFO: User login successful",
    "[2024-01-15 14:31:20] ERROR: Database connection failed",
    "[2024-01-15 14:32:10] WARNING: Disk space low"
]

print("\n日志解析:")
for log_line in log_lines:
    parsed = parse_log_line(log_line)
    if parsed:
        print(f"  {parsed}")

# 测试 HTML 清理
html_text = "<p>这是一个<strong>重要的</strong>消息。</p>"
clean_text = clean_html_tags(html_text)
print(f"\nHTML清理: '{html_text}' -> '{clean_text}'")

# 测试密码强度
passwords = ["123", "password", "Password123", "Password123!", "P@ssw0rd123!"]
print("\n密码强度检查:")
for pwd in passwords:
    strength, checks = check_password_strength(pwd)
    print(f"  '{pwd}': {strength} (长度:{checks['length']}, 大写:{checks['uppercase']}, 小写:{checks['lowercase']}, 数字:{checks['digit']}, 特殊:{checks['special']})")

# 6. 文本预处理
def preprocess_text(text):
    """文本预处理"""
    # 移除多余空白
    text = re.sub(r'\s+', ' ', text.strip())
    # 移除标点符号
    text = re.sub(r'[^\w\s]', '', text)
    # 转换为小写
    text = text.lower()
    return text

original_text = "Hello, World!!!   This is a TEST."
processed_text = preprocess_text(original_text)
print(f"\n文本预处理: '{original_text}' -> '{processed_text}'")

# 7. CSV 数据解析
def parse_csv_line(line):
    """解析 CSV 行（简单版本）"""
    # 匹配 CSV 字段，考虑引号
    pattern = r'(?:^|,)("(?:[^"]*(?:\\.[^"]*)*)"|[^,]*)'
    matches = re.findall(pattern, line)
    return [match.strip('"') for match in matches]

csv_line = 'John Doe,"New York, NY",30,"Software Engineer"'
parsed_csv = parse_csv_line(csv_line)
print(f"\nCSV解析: {parsed_csv}")
```

## 21.4 random 模块

### 21.4.1 基本随机函数

```python
import random
import string

# 基本随机函数
print("基本随机函数:")

# 随机浮点数
print(f"random.random(): {random.random()}")  # [0.0, 1.0)
print(f"random.uniform(1, 10): {random.uniform(1, 10)}")  # [1, 10] 之间的均匀分布

# 随机整数
print(f"random.randint(1, 10): {random.randint(1, 10)}")  # [1, 10] 之间的整数
print(f"random.randrange(0, 10, 2): {random.randrange(0, 10, 2)}")  # [0, 2, 4, 6, 8]

# 序列随机选择
colors = ['red', 'blue', 'green', 'yellow', 'purple']
print(f"random.choice(): {random.choice(colors)}")
print(f"random.choices() 3个: {random.choices(colors, k=3)}")  # 有放回抽样
print(f"random.sample() 2个: {random.sample(colors, 2)}")  # 无放回抽样

# 序列随机排列
numbers = [1, 2, 3, 4, 5]
random.shuffle(numbers)
print(f"shuffle 后的列表: {numbers}")

# 生成随机字符串
def generate_random_string(length=8):
    """生成随机字符串"""
    characters = string.ascii_letters + string.digits
    return ''.join(random.choices(characters, k=length))

print(f"随机字符串: {generate_random_string()}")
print(f"随机密码: {generate_random_string(12)}")
```

### 21.4.2 种子和可重现性

```python
# 种子和可重现性
print("种子和可重现性:")

# 设置种子
random.seed(42)

print("第一次运行 (种子=42):")
print(f"  random.random(): {random.random()}")
print(f"  random.randint(1, 10): {random.randint(1, 10)}")
print(f"  random.choice(['A', 'B', 'C']): {random.choice(['A', 'B', 'C'])}")

# 重置种子
random.seed(42)

print("第二次运行 (种子=42):")
print(f"  random.random(): {random.random()}")
print(f"  random.randint(1, 10): {random.randint(1, 10)}")
print(f"  random.choice(['A', 'B', 'C']): {random.choice(['A', 'B', 'C'])}")

# 使用当前时间作为种子
random.seed()

print("使用时间种子:")
print(f"  random.random(): {random.random()}")

# 获取随机状态
state = random.getstate()
print(f"随机状态长度: {len(state[1])}")  # Mersenne Twister 有 625 个状态

# 设置随机状态
random.setstate(state)
print(f"恢复状态后 random.random(): {random.random()}")
```

### 21.4.3 概率分布

```python
# 概率分布
print("概率分布:")

# 高斯分布（正态分布）
print("高斯分布 (μ=0, σ=1):")
for _ in range(5):
    print(f"  {random.gauss(0, 1):.3f}")

# 指数分布
print("\n指数分布 (λ=1):")
for _ in range(5):
    print(f"  {random.expovariate(1):.3f}")

# Beta 分布
print("\nBeta 分布 (α=2, β=5):")
for _ in range(5):
    print(f"  {random.betavariate(2, 5):.3f}")

# Gamma 分布
print("\nGamma 分布 (α=2, β=1):")
for _ in range(5):
    print(f"  {random.gammavariate(2, 1):.3f}")

# 自定义离散分布
def custom_distribution():
    """自定义离散分布"""
    x = random.random()
    if x < 0.5:
        return 'A'  # 50% 概率
    elif x < 0.8:
        return 'B'  # 30% 概率
    else:
        return 'C'  # 20% 概率

print("\n自定义离散分布 (1000 次采样):")
results = [custom_distribution() for _ in range(1000)]
from collections import Counter
counts = Counter(results)
for item, count in sorted(counts.items()):
    percentage = count / 10  # 百分比
    print(f"  {item}: {percentage:.1f}%")
```

### 21.4.4 实际应用示例

```python
# 实际应用示例
print("实际应用示例:")

# 1. 生成测试数据
def generate_test_data(num_records=10):
    """生成测试用户数据"""
    first_names = ['Alice', 'Bob', 'Charlie', 'Diana', 'Eve', 'Frank', 'Grace', 'Henry']
    last_names = ['Smith', 'Johnson', 'Williams', 'Brown', 'Jones', 'Garcia', 'Miller', 'Davis']
    cities = ['北京', '上海', '深圳', '广州', '杭州', '南京', '苏州', '武汉']

    test_data = []
    for _ in range(num_records):
        first_name = random.choice(first_names)
        last_name = random.choice(last_names)
        age = random.randint(18, 80)
        city = random.choice(cities)
        email = f"{first_name.lower()}.{last_name.lower()}@example.com"

        test_data.append({
            'name': f"{first_name} {last_name}",
            'age': age,
            'city': city,
            'email': email
        })

    return test_data

# 生成测试数据
test_users = generate_test_data(5)
print("生成的测试用户数据:")
for user in test_users:
    print(f"  {user}")

# 2. 随机抽样
def random_sampling(data, sample_size):
    """随机抽样"""
    return random.sample(data, min(sample_size, len(data)))

population = list(range(1, 101))  # 1-100 的数字
sample = random_sampling(population, 10)
print(f"\n从 1-100 中随机抽样 10 个: {sorted(sample)}")

# 3. 洗牌算法
def shuffle_deck():
    """洗扑克牌"""
    suits = ['♠', '♥', '♦', '♣']
    ranks = ['A', '2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K']

    deck = [f"{rank}{suit}" for suit in suits for rank in ranks]
    random.shuffle(deck)
    return deck

print("\n洗牌后的扑克牌 (前10张):")
deck = shuffle_deck()
for i in range(10):
    print(f"  {i+1:2d}: {deck[i]}")

# 4. 密码生成器
def generate_password(length=12, use_uppercase=True, use_digits=True, use_special=True):
    """生成随机密码"""
    characters = string.ascii_lowercase

    if use_uppercase:
        characters += string.ascii_uppercase
    if use_digits:
        characters += string.digits
    if use_special:
        characters += "!@#$%^&*()_+-=[]{}|;:,.<>?"

    # 确保至少包含所需类型的字符
    password = []

    if use_uppercase:
        password.append(random.choice(string.ascii_uppercase))
    if use_digits:
        password.append(random.choice(string.digits))
    if use_special:
        password.append(random.choice("!@#$%^&*()_+-=[]{}|;:,.<>?"))

    # 填充剩余字符
    remaining_length = length - len(password)
    password.extend(random.choices(characters, k=remaining_length))

    # 打乱顺序
    random.shuffle(password)

    return ''.join(password)

print("\n生成的随机密码:")
passwords = [
    generate_password(8),
    generate_password(12, use_special=False),
    generate_password(16, use_uppercase=False, use_special=False)
]

for i, pwd in enumerate(passwords, 1):
    print(f"  密码{i}: {pwd}")

# 5. 模拟实验
def coin_flip_experiment(num_flips=1000):
    """模拟抛硬币实验"""
    heads = sum(1 for _ in range(num_flips) if random.choice([True, False]))
    tails = num_flips - heads

    print(f"抛硬币 {num_flips} 次:")
    print(f"  正面: {heads} ({heads/num_flips*100:.1f}%)")
    print(f"  反面: {tails} ({tails/num_flips*100:.1f}%)")

def dice_roll_experiment(num_rolls=1000):
    """模拟掷骰子实验"""
    results = [random.randint(1, 6) for _ in range(num_rolls)]
    from collections import Counter
    counts = Counter(results)

    print(f"掷骰子 {num_rolls} 次:")
    for face in range(1, 7):
        count = counts.get(face, 0)
        percentage = count / num_rolls * 100
        print(f"  {face}点: {count} ({percentage:.1f}%)")

print("\n模拟实验:")
coin_flip_experiment(1000)
print()
dice_roll_experiment(1000)

# 6. 随机数生成器类
class SecureRandomGenerator:
    """安全的随机数生成器"""

    def __init__(self, seed=None):
        if seed is not None:
            random.seed(seed)
        self._state = random.getstate()

    def generate_token(self, length=32):
        """生成随机令牌"""
        characters = string.ascii_letters + string.digits
        return ''.join(random.choices(characters, k=length))

    def generate_uuid_like(self):
        """生成类似 UUID 的字符串"""
        parts = []
        for length in [8, 4, 4, 4, 12]:
            part = ''.join(random.choices(string.ascii_letters + string.digits, k=length))
            parts.append(part.lower())
        return '-'.join(parts)

    def shuffle_list(self, data):
        """打乱列表"""
        shuffled = data.copy()
        random.shuffle(shuffled)
        return shuffled

# 测试安全随机数生成器
generator = SecureRandomGenerator()

print("\n安全随机数生成器:")
print(f"随机令牌: {generator.generate_token()}")
print(f"UUID 风格: {generator.generate_uuid_like()}")

original_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
shuffled_list = generator.shuffle_list(original_list)
print(f"原始列表: {original_list}")
print(f"打乱后: {shuffled_list}")
```

## 总结

本章介绍了 Python 的常用模块：
- 掌握 datetime 模块处理日期和时间的各种操作
- 学会使用 json 模块进行数据序列化和反序列化
- 熟练运用 re 模块进行正则表达式匹配和处理
- 理解 random 模块的各种随机数生成方法

通过本章的学习，你应该能够：
1. 处理各种日期时间计算和格式化
2. 进行 JSON 数据的读写和处理
3. 使用正则表达式进行文本匹配和处理
4. 生成各种类型的随机数和随机数据
5. 编写健壮的文本处理和数据验证代码

下一章将介绍数据处理相关的模块。
