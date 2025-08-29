# 第18章：文件操作基础

## 18.1 文件的基本操作

### 18.1.1 文件的概念

文件是存储在持久化存储介质（如硬盘、SSD）上的数据集合。在 Python 中，文件操作是最常见的 I/O 操作之一。

```python
# 文件的基本概念
print("文件的基本属性:")
print("- 文件名: 标识文件的名称")
print("- 路径: 文件在文件系统中的位置")
print("- 扩展名: 文件类型的标识")
print("- 大小: 文件包含的数据量")
print("- 权限: 文件的访问权限")

# 文件路径示例
import os

# 绝对路径
absolute_path = "/Users/username/Documents/file.txt"
print(f"绝对路径: {absolute_path}")

# 相对路径
relative_path = "data/file.txt"
print(f"相对路径: {relative_path}")

# 当前工作目录
current_dir = os.getcwd()
print(f"当前工作目录: {current_dir}")

# 路径操作
file_path = "/Users/username/Documents/data.txt"

print(f"目录部分: {os.path.dirname(file_path)}")
print(f"文件名部分: {os.path.basename(file_path)}")
print(f"文件扩展名: {os.path.splitext(file_path)[1]}")
print(f"是否存在: {os.path.exists(file_path)}")
```

### 18.1.2 文件模式

```python
# 文件打开模式
modes = {
    'r': '只读模式（默认）',
    'w': '写入模式（覆盖）',
    'a': '追加模式',
    'x': '独占创建模式',
    'r+': '读写模式',
    'w+': '读写模式（覆盖）',
    'a+': '读写追加模式',
    'rb': '二进制只读',
    'wb': '二进制写入',
    'ab': '二进制追加',
    'r+b': '二进制读写',
    'w+b': '二进制读写（覆盖）',
    'a+b': '二进制读写追加'
}

print("文件打开模式:")
for mode, description in modes.items():
    print(f"  {mode:3}: {description}")

# 文本模式 vs 二进制模式
print("\n文本模式 vs 二进制模式:")
print("- 文本模式: 处理字符串，自动处理编码")
print("- 二进制模式: 处理字节数据，不进行编码转换")
print("- 推荐: 文本文件用文本模式，二进制文件用二进制模式")
```

### 18.1.3 文件对象的属性和方法

```python
# 创建一个示例文件来演示
with open("example.txt", "w", encoding="utf-8") as f:
    f.write("Hello, World!\n")
    f.write("这是第二行\n")
    f.write("第三行内容")

# 打开文件并查看属性
with open("example.txt", "r", encoding="utf-8") as file:
    print("文件对象属性:")
    print(f"  文件名: {file.name}")
    print(f"  模式: {file.mode}")
    print(f"  编码: {file.encoding}")
    print(f"  是否可读: {file.readable()}")
    print(f"  是否可写: {file.writable()}")
    print(f"  是否已关闭: {file.closed}")

    # 读取内容
    content = file.read()
    print(f"  文件内容:\n{content}")

print(f"文件关闭后状态: {file.closed}")

# 清理示例文件
import os
os.remove("example.txt")
```

## 18.2 文本文件读写

### 18.2.1 读取文本文件

```python
# 创建示例文本文件
sample_text = """Python 编程语言
Python 是一种高级编程语言
它具有简洁、易读的语法
广泛应用于数据科学、Web开发等领域

特点：
- 简单易学
- 功能强大
- 生态丰富
- 社区活跃
"""

with open("sample.txt", "w", encoding="utf-8") as f:
    f.write(sample_text)

# 方法1: read() - 读取整个文件
print("方法1: read() - 读取整个文件")
with open("sample.txt", "r", encoding="utf-8") as file:
    content = file.read()
    print(f"文件内容:\n{content}")
    print(f"字符数: {len(content)}")

# 方法2: readline() - 逐行读取
print("\n方法2: readline() - 逐行读取")
with open("sample.txt", "r", encoding="utf-8") as file:
    line_number = 1
    while True:
        line = file.readline()
        if not line:  # 空字符串表示文件结束
            break
        print(f"第{line_number}行: {line.rstrip()}")
        line_number += 1

# 方法3: readlines() - 读取所有行到列表
print("\n方法3: readlines() - 读取所有行到列表")
with open("sample.txt", "r", encoding="utf-8") as file:
    lines = file.readlines()
    print(f"总行数: {len(lines)}")
    for i, line in enumerate(lines, 1):
        print(f"第{i}行: {line.rstrip()}")

# 方法4: 直接迭代文件对象
print("\n方法4: 直接迭代文件对象")
with open("sample.txt", "r", encoding="utf-8") as file:
    for line_number, line in enumerate(file, 1):
        print(f"第{line_number}行: {line.rstrip()}")

# 清理文件
import os
os.remove("sample.txt")
```

### 18.2.2 写入文本文件

```python
# 方法1: write() - 写入字符串
print("方法1: write() - 写入字符串")
with open("output1.txt", "w", encoding="utf-8") as file:
    file.write("第一行内容\n")
    file.write("第二行内容\n")
    file.write("第三行内容\n")

# 读取验证
with open("output1.txt", "r", encoding="utf-8") as file:
    print("写入的内容:")
    print(file.read())

# 方法2: writelines() - 写入字符串列表
print("\n方法2: writelines() - 写入字符串列表")
lines_to_write = [
    "Python 是一门优秀的编程语言\n",
    "它具有简洁的语法\n",
    "强大的功能\n",
    "丰富的生态系统\n"
]

with open("output2.txt", "w", encoding="utf-8") as file:
    file.writelines(lines_to_write)

# 读取验证
with open("output2.txt", "r", encoding="utf-8") as file:
    print("写入的内容:")
    print(file.read())

# 方法3: print() 函数写入文件
print("\n方法3: print() 函数写入文件")
with open("output3.txt", "w", encoding="utf-8") as file:
    print("使用 print() 写入", file=file)
    print("自动添加换行符", file=file)
    print("很方便的方法", file=file)

# 读取验证
with open("output3.txt", "r", encoding="utf-8") as file:
    print("写入的内容:")
    print(repr(file.read()))  # 显示原始字符串

# 清理文件
for filename in ["output1.txt", "output2.txt", "output3.txt"]:
    if os.path.exists(filename):
        os.remove(filename)
```

### 18.2.3 追加写入

```python
# 创建初始文件
with open("log.txt", "w", encoding="utf-8") as file:
    file.write("程序启动\n")
    file.write("初始化完成\n")

# 追加内容
with open("log.txt", "a", encoding="utf-8") as file:
    file.write("用户登录\n")
    file.write("数据处理中...\n")
    file.write("处理完成\n")

# 读取完整日志
with open("log.txt", "r", encoding="utf-8") as file:
    print("完整日志内容:")
    print(file.read())

# 模拟日志记录函数
def log_message(message, level="INFO"):
    """记录日志消息"""
    import datetime
    timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    log_entry = f"[{timestamp}] {level}: {message}\n"

    with open("app.log", "a", encoding="utf-8") as log_file:
        log_file.write(log_entry)

# 测试日志记录
log_message("应用程序启动")
log_message("用户认证成功", "INFO")
log_message("数据库连接建立", "INFO")
log_message("处理用户请求", "DEBUG")
log_message("请求处理完成", "INFO")

# 读取日志
with open("app.log", "r", encoding="utf-8") as file:
    print("\n应用程序日志:")
    print(file.read())

# 清理文件
for filename in ["log.txt", "app.log"]:
    if os.path.exists(filename):
        os.remove(filename)
```

## 18.3 二进制文件操作

### 18.3.1 读取二进制文件

```python
# 创建二进制数据
binary_data = bytes([72, 101, 108, 108, 111, 32, 87, 111, 114, 108, 100, 33])  # "Hello World!"

# 写入二进制文件
with open("binary_file.bin", "wb") as file:
    file.write(binary_data)
    # 写入更多数据
    file.write(b"\n")
    file.write(bytes([80, 121, 116, 104, 111, 110]))  # "Python"

# 读取二进制文件
print("读取二进制文件:")
with open("binary_file.bin", "rb") as file:
    # 读取整个文件
    all_data = file.read()
    print(f"原始字节: {all_data}")
    print(f"十六进制: {all_data.hex()}")
    print(f"长度: {len(all_data)} 字节")

    # 尝试解码为字符串
    try:
        text = all_data.decode('utf-8')
        print(f"解码为文本: {text}")
    except UnicodeDecodeError as e:
        print(f"解码失败: {e}")

# 分块读取
print("\n分块读取:")
with open("binary_file.bin", "rb") as file:
    chunk_size = 5
    while True:
        chunk = file.read(chunk_size)
        if not chunk:
            break
        print(f"块: {chunk} (长度: {len(chunk)})")

# 清理文件
os.remove("binary_file.bin")
```

### 18.3.2 处理图像文件

```python
# 创建一个简单的二进制文件模拟图像数据
def create_mock_image():
    """创建模拟的图像数据"""
    # BMP 文件头 (简化版)
    bmp_header = bytes([
        0x42, 0x4D,  # "BM"
        0x36, 0x00, 0x00, 0x00,  # 文件大小
        0x00, 0x00, 0x00, 0x00,  # 保留
        0x36, 0x00, 0x00, 0x00,  # 数据偏移
    ])

    # 模拟像素数据 (RGB 值)
    pixel_data = b""
    for i in range(10):
        # 红色渐变
        red = i * 25
        pixel_data += bytes([red, 0, 0])  # RGB

    return bmp_header + pixel_data

# 保存和读取图像数据
image_data = create_mock_image()

with open("mock_image.bmp", "wb") as file:
    file.write(image_data)

# 读取图像文件
with open("mock_image.bmp", "rb") as file:
    # 读取文件头
    header = file.read(14)
    print(f"文件头: {header.hex()}")

    # 读取像素数据
    pixels = file.read()
    print(f"像素数据长度: {len(pixels)}")
    print(f"前10个像素: {pixels[:30].hex()}")

# 清理文件
os.remove("mock_image.bmp")
```

### 18.3.3 序列化对象

```python
import pickle

# 要序列化的数据
data = {
    "name": "Alice",
    "age": 30,
    "scores": [95, 87, 92],
    "active": True,
    "metadata": {
        "created": "2024-01-01",
        "version": "1.0"
    }
}

# 序列化到文件
print("序列化对象到文件:")
with open("data.pkl", "wb") as file:
    pickle.dump(data, file)

# 查看文件大小
file_size = os.path.getsize("data.pkl")
print(f"序列化文件大小: {file_size} 字节")

# 反序列化
print("\n反序列化对象:")
with open("data.pkl", "rb") as file:
    loaded_data = pickle.load(file)

print(f"加载的数据: {loaded_data}")
print(f"数据是否相同: {data == loaded_data}")

# 序列化多个对象
print("\n序列化多个对象:")
objects = [
    "字符串数据",
    [1, 2, 3, 4, 5],
    {"key": "value"},
    42
]

with open("multiple_objects.pkl", "wb") as file:
    for obj in objects:
        pickle.dump(obj, file)

# 反序列化多个对象
print("反序列化多个对象:")
with open("multiple_objects.pkl", "rb") as file:
    loaded_objects = []
    while True:
        try:
            obj = pickle.load(file)
            loaded_objects.append(obj)
        except EOFError:
            break

print(f"加载的对象: {loaded_objects}")

# 清理文件
for filename in ["data.pkl", "multiple_objects.pkl"]:
    if os.path.exists(filename):
        os.remove(filename)

# 注意事项
print("\nPickle 注意事项:")
print("- 只在可信任的数据源使用 pickle")
print("- pickle 不保证跨 Python 版本兼容")
print("- 对于简单数据，考虑使用 JSON")
print("- pickle 文件不是人类可读的")
```

## 18.4 文件指针和定位

### 18.4.1 文件指针的概念

```python
# 创建测试文件
with open("test_pointer.txt", "w", encoding="utf-8") as f:
    f.write("第一行：Hello World\n")
    f.write("第二行：Python 编程\n")
    f.write("第三行：文件操作\n")

# 文件指针操作
with open("test_pointer.txt", "r", encoding="utf-8") as file:
    print("文件指针操作演示:")

    # 查看初始位置
    print(f"初始位置: {file.tell()}")

    # 读取一行
    line1 = file.readline()
    print(f"读取: {line1.rstrip()}")
    print(f"当前位置: {file.tell()}")

    # 读取另一行
    line2 = file.readline()
    print(f"读取: {line2.rstrip()}")
    print(f"当前位置: {file.tell()}")

    # 移动到文件开头
    file.seek(0)
    print(f"移动到开头: {file.tell()}")

    # 读取第一行
    line1_again = file.readline()
    print(f"再次读取第一行: {line1_again.rstrip()}")

    # 移动到特定位置
    file.seek(15)  # 移动到第15个字符
    print(f"移动到位置15: {file.tell()}")

    # 从当前位置读取
    partial_content = file.read(10)
    print(f"读取10个字符: {partial_content}")

# 清理文件
os.remove("test_pointer.txt")
```

### 18.4.2 seek() 和 tell() 方法

```python
# seek() 方法详解
print("seek() 方法详解:")

# 创建测试文件
content = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ"
with open("seek_test.txt", "w", encoding="utf-8") as f:
    f.write(content)

with open("seek_test.txt", "r", encoding="utf-8") as file:
    print(f"文件内容: {content}")
    print(f"文件长度: {len(content)}")

    # seek(offset, whence)
    # whence=0: 从文件开头 (默认)
    # whence=1: 从当前位置
    # whence=2: 从文件末尾

    # 从开头移动
    file.seek(5)  # 移动到第5个字符
    print(f"seek(5): 位置={file.tell()}, 读取='{file.read(1)}'")

    # 从当前位置移动
    file.seek(2, 1)  # 从当前位置向前移动2个字符
    print(f"seek(2, 1): 位置={file.tell()}, 读取='{file.read(1)}'")

    # 从文件末尾移动
    file.seek(-3, 2)  # 从末尾向前3个字符
    print(f"seek(-3, 2): 位置={file.tell()}, 读取='{file.read(1)}'")

# 二进制文件的定位
print("\n二进制文件的定位:")
binary_content = bytes(range(20))  # 0-19 的字节

with open("binary_seek.bin", "wb") as f:
    f.write(binary_content)

with open("binary_seek.bin", "rb") as file:
    print(f"二进制内容: {binary_content}")

    # 移动到第10个字节
    file.seek(10)
    byte = file.read(1)
    print(f"第10个字节: {byte[0]} (位置: {file.tell()})")

    # 从当前位置移动
    file.seek(3, 1)
    byte = file.read(1)
    print(f"从当前位置+3: {byte[0]} (位置: {file.tell()})")

# 清理文件
for filename in ["seek_test.txt", "binary_seek.bin"]:
    if os.path.exists(filename):
        os.remove(filename)
```

### 18.4.3 随机访问文件

```python
# 实现简单的数据库文件
class SimpleDatabase:
    """简单的基于文件的数据库"""

    def __init__(self, filename):
        self.filename = filename
        self.record_size = 50  # 每条记录50字节

    def write_record(self, record_id, data):
        """写入记录"""
        with open(self.filename, "r+b" if os.path.exists(self.filename) else "wb") as file:
            # 计算位置
            position = record_id * self.record_size

            # 移动到指定位置
            file.seek(position)

            # 准备数据 (填充到固定长度)
            data_bytes = data.encode('utf-8')
            if len(data_bytes) > self.record_size:
                data_bytes = data_bytes[:self.record_size]
            else:
                data_bytes = data_bytes.ljust(self.record_size, b'\x00')

            # 写入数据
            file.write(data_bytes)

    def read_record(self, record_id):
        """读取记录"""
        if not os.path.exists(self.filename):
            return None

        with open(self.filename, "rb") as file:
            position = record_id * self.record_size
            file.seek(position)

            data = file.read(self.record_size)
            if not data:
                return None

            # 去除填充的空字节
            data = data.rstrip(b'\x00')
            return data.decode('utf-8')

# 测试简单数据库
db = SimpleDatabase("simple_db.dat")

# 写入记录
db.write_record(0, "Alice Johnson")
db.write_record(1, "Bob Smith")
db.write_record(2, "Charlie Brown")
db.write_record(5, "Diana Prince")  # 跳过一些记录

# 读取记录
for i in range(8):
    record = db.read_record(i)
    if record:
        print(f"记录 {i}: {record}")
    else:
        print(f"记录 {i}: (空)")

# 查看文件大小
file_size = os.path.getsize("simple_db.dat")
print(f"\n数据库文件大小: {file_size} 字节")
print(f"预期大小: {8 * 50} 字节")

# 清理文件
os.remove("simple_db.dat")
```

## 18.5 文件编码处理

### 18.5.1 字符编码基础

```python
# 字符编码的概念
print("字符编码基础:")
print("- ASCII: 7位编码，只支持英文")
print("- UTF-8: 可变长度编码，支持所有Unicode字符")
print("- UTF-16: 16位编码")
print("- GBK/GB2312: 中文编码")
print("- Latin-1: 西欧字符编码")

# Python 中的编码处理
text = "Hello, 世界! 🌍"

print(f"原始文本: {text}")
print(f"UTF-8 编码: {text.encode('utf-8')}")
print(f"UTF-8 字节长度: {len(text.encode('utf-8'))}")

# 不同编码的比较
encodings = ['utf-8', 'utf-16', 'gbk', 'ascii']

for encoding in encodings:
    try:
        encoded = text.encode(encoding)
        print(f"{encoding:8}: {len(encoded):2} 字节 - {encoded}")
    except UnicodeEncodeError as e:
        print(f"{encoding:8}: 编码失败 - {e}")
```

### 18.5.2 编码错误处理

```python
# 编码错误处理策略
text_with_emoji = "Hello, 世界! 🌍 🚀"

print("编码错误处理策略:")

# 1. strict - 默认，遇到错误抛出异常
try:
    encoded = text_with_emoji.encode('ascii')
except UnicodeEncodeError as e:
    print(f"strict 模式错误: {e}")

# 2. ignore - 忽略无法编码的字符
encoded_ignore = text_with_emoji.encode('ascii', errors='ignore')
print(f"ignore 模式: {encoded_ignore}")

# 3. replace - 用 ? 替换无法编码的字符
encoded_replace = text_with_emoji.encode('ascii', errors='replace')
print(f"replace 模式: {encoded_replace}")

# 4. xmlcharrefreplace - 用 XML 字符引用替换
encoded_xml = text_with_emoji.encode('ascii', errors='xmlcharrefreplace')
print(f"xmlcharrefreplace 模式: {encoded_xml}")

# 解码错误处理
print("\n解码错误处理:")
malformed_bytes = b"Hello, \xff\xfe World!"

try:
    decoded = malformed_bytes.decode('utf-8')
except UnicodeDecodeError as e:
    print(f"strict 模式错误: {e}")

# 使用不同错误处理策略
decoded_ignore = malformed_bytes.decode('utf-8', errors='ignore')
decoded_replace = malformed_bytes.decode('utf-8', errors='replace')

print(f"ignore 模式: {decoded_ignore}")
print(f"replace 模式: {decoded_replace}")
```

### 18.5.3 实际文件编码处理

```python
# 创建不同编码的文件
texts = {
    'utf8_file.txt': ('UTF-8', 'Hello, 世界! 🌍'),
    'gbk_file.txt': ('GBK', '你好，世界！'),
    'latin1_file.txt': ('Latin-1', 'Hello, World!')
}

# 写入不同编码的文件
for filename, (encoding, content) in texts.items():
    with open(filename, 'w', encoding=encoding) as file:
        file.write(content)
    print(f"创建 {filename} ({encoding})")

# 读取文件时指定编码
print("\n读取文件:")
for filename, (expected_encoding, _) in texts.items():
    print(f"\n读取 {filename}:")

    # 1. 使用正确编码读取
    try:
        with open(filename, 'r', encoding=expected_encoding) as file:
            content = file.read()
        print(f"  正确编码: {content}")
    except UnicodeDecodeError as e:
        print(f"  正确编码失败: {e}")

    # 2. 使用错误编码读取
    try:
        with open(filename, 'r', encoding='ascii') as file:
            content = file.read()
        print(f"  ASCII编码: {content}")
    except UnicodeDecodeError as e:
        print(f"  ASCII编码失败: {e}")

    # 3. 自动检测编码 (chardet 库)
    try:
        import chardet
        with open(filename, 'rb') as file:
            raw_data = file.read()
        detected = chardet.detect(raw_data)
        print(f"  检测到编码: {detected['encoding']} (置信度: {detected['confidence']:.2f})")

        # 使用检测到的编码读取
        with open(filename, 'r', encoding=detected['encoding']) as file:
            content = file.read()
        print(f"  检测编码读取: {content}")
    except ImportError:
        print("  chardet 库未安装，跳过编码检测")
    except Exception as e:
        print(f"  编码检测失败: {e}")

# 清理文件
for filename in texts.keys():
    if os.path.exists(filename):
        os.remove(filename)
```

## 18.6 上下文管理器

### 18.6.1 with 语句的基本用法

```python
# with 语句的基本用法
print("with 语句的基本用法:")

# 1. 自动关闭文件
with open("temp_file.txt", "w", encoding="utf-8") as file:
    file.write("Hello, World!")
    print(f"文件是否打开: {not file.closed}")

# 文件自动关闭
print(f"退出with块后文件是否关闭: {file.closed}")

# 2. 多个上下文管理器
with open("file1.txt", "w") as f1, open("file2.txt", "w") as f2:
    f1.write("文件1的内容")
    f2.write("文件2的内容")
    print("两个文件都已打开")

print("两个文件都已自动关闭")

# 清理文件
for filename in ["temp_file.txt", "file1.txt", "file2.txt"]:
    if os.path.exists(filename):
        os.remove(filename)
```

### 18.6.2 自定义上下文管理器

```python
# 自定义上下文管理器类
class FileManager:
    """自定义文件管理器"""

    def __init__(self, filename, mode='r', encoding=None):
        self.filename = filename
        self.mode = mode
        self.encoding = encoding
        self.file = None

    def __enter__(self):
        """进入上下文时调用"""
        print(f"打开文件: {self.filename}")
        if 'b' in self.mode:
            self.file = open(self.filename, self.mode)
        else:
            self.file = open(self.filename, self.mode, encoding=self.encoding)
        return self.file

    def __exit__(self, exc_type, exc_val, exc_tb):
        """退出上下文时调用"""
        if self.file:
            print(f"关闭文件: {self.filename}")
            self.file.close()

        # 如果有异常，可以在这里处理
        if exc_type:
            print(f"发生异常: {exc_type.__name__}: {exc_val}")
            return False  # 重新抛出异常

        return True

# 使用自定义上下文管理器
print("使用自定义上下文管理器:")

# 写入文件
with FileManager("test.txt", "w", encoding="utf-8") as file:
    file.write("这是测试内容\n")
    file.write("第二行内容\n")

# 读取文件
with FileManager("test.txt", "r", encoding="utf-8") as file:
    content = file.read()
    print(f"读取的内容:\n{content}")

# 测试异常处理
try:
    with FileManager("test.txt", "r", encoding="utf-8") as file:
        # 故意引发异常
        raise ValueError("测试异常")
except ValueError as e:
    print(f"捕获到异常: {e}")

# 清理文件
os.remove("test.txt")
```

### 18.6.3 contextlib 模块

```python
import contextlib
import time

# 1. @contextmanager 装饰器
@contextlib.contextmanager
def timer(description):
    """计时器上下文管理器"""
    start = time.time()
    print(f"开始: {description}")
    try:
        yield
    finally:
        end = time.time()
        print(f"结束: {description}, 耗时: {end - start:.3f} 秒")

# 使用计时器
with timer("文件操作"):
    with open("temp.txt", "w") as f:
        for i in range(1000):
            f.write(f"行 {i}\n")
    time.sleep(0.1)  # 模拟其他操作

# 2. contextlib.suppress - 抑制异常
print("\n使用 suppress 抑制异常:")
with contextlib.suppress(FileNotFoundError):
    with open("nonexistent.txt", "r") as f:
        content = f.read()
print("文件不存在，但没有抛出异常")

# 3. contextlib.redirect_stdout - 重定向输出
print("\n重定向输出:")
import io

output_buffer = io.StringIO()
with contextlib.redirect_stdout(output_buffer):
    print("这条消息被重定向了")
    print("不会出现在控制台")

captured_output = output_buffer.getvalue()
print(f"捕获的输出: {repr(captured_output)}")

# 4. 嵌套上下文管理器
@contextlib.contextmanager
def database_connection(db_name):
    """模拟数据库连接"""
    print(f"连接到数据库: {db_name}")
    try:
        yield f"连接对象_{db_name}"
    finally:
        print(f"断开数据库连接: {db_name}")

# 使用嵌套上下文管理器
with database_connection("users") as conn1:
    print(f"使用连接: {conn1}")
    with database_connection("orders") as conn2:
        print(f"使用连接: {conn2}")
        print("执行数据库操作...")

# 5. contextlib.ExitStack - 动态管理上下文
print("\n使用 ExitStack:")
files_to_open = ["file1.txt", "file2.txt", "file3.txt"]

# 创建这些文件
for filename in files_to_open:
    with open(filename, "w") as f:
        f.write(f"内容 of {filename}\n")

# 使用 ExitStack 动态打开文件
with contextlib.ExitStack() as stack:
    opened_files = []
    for filename in files_to_open:
        file = stack.enter_context(open(filename, "r"))
        opened_files.append(file)

    # 现在所有文件都已打开
    for file in opened_files:
        print(f"读取 {file.name}: {file.read().strip()}")

# 所有文件自动关闭
print("所有文件已自动关闭")

# 清理文件
for filename in files_to_open:
    if os.path.exists(filename):
        os.remove(filename)

if os.path.exists("temp.txt"):
    os.remove("temp.txt")
```

## 18.7 文件操作最佳实践

### 18.7.1 安全的文件操作

```python
import os
import tempfile
import shutil

# 1. 使用临时文件避免数据丢失
def safe_file_update(filename, new_content):
    """安全地更新文件内容"""

    # 创建临时文件
    temp_fd, temp_path = tempfile.mkstemp()

    try:
        # 写入新内容到临时文件
        with os.fdopen(temp_fd, 'w', encoding='utf-8') as temp_file:
            temp_file.write(new_content)

        # 原子性替换原文件
        os.replace(temp_path, filename)
        print(f"文件 {filename} 更新成功")

    except Exception as e:
        # 如果出错，删除临时文件
        if os.path.exists(temp_path):
            os.unlink(temp_path)
        print(f"文件更新失败: {e}")
        raise

# 测试安全文件更新
safe_file_update("safe_test.txt", "这是新内容\n第二行\n")

# 验证内容
with open("safe_test.txt", "r", encoding="utf-8") as f:
    print("更新后的内容:")
    print(f.read())

# 2. 文件权限检查
def check_file_permissions(filename):
    """检查文件权限"""
    if not os.path.exists(filename):
        print(f"文件 {filename} 不存在")
        return

    stat_info = os.stat(filename)

    print(f"文件: {filename}")
    print(f"大小: {stat_info.st_size} 字节")
    print(f"权限: {oct(stat_info.st_mode)[-3:]}")  # 最后3位

    # 检查可读性
    print(f"可读: {os.access(filename, os.R_OK)}")
    print(f"可写: {os.access(filename, os.W_OK)}")
    print(f"可执行: {os.access(filename, os.X_OK)}")

check_file_permissions("safe_test.txt")

# 3. 备份重要文件
def backup_file(filename):
    """备份文件"""
    if not os.path.exists(filename):
        print(f"文件 {filename} 不存在")
        return None

    backup_name = filename + ".backup"
    shutil.copy2(filename, backup_name)  # 保留元数据
    print(f"备份创建: {backup_name}")
    return backup_name

backup_file("safe_test.txt")

# 清理文件
for filename in ["safe_test.txt", "safe_test.txt.backup"]:
    if os.path.exists(filename):
        os.remove(filename)
```

### 18.7.2 性能优化

```python
import time

# 1. 大文件处理 - 分块读取
def process_large_file(filename, chunk_size=8192):
    """高效处理大文件"""

    total_lines = 0
    total_chars = 0

    with open(filename, "r", encoding="utf-8") as file:
        while True:
            chunk = file.read(chunk_size)
            if not chunk:
                break

            lines = chunk.count('\n')
            total_lines += lines
            total_chars += len(chunk)

            # 处理每一行
            for line in chunk.split('\n'):
                if line.strip():  # 跳过空行
                    # 这里可以添加具体的处理逻辑
                    pass

    return total_lines, total_chars

# 创建大文件进行测试
print("创建大文件进行测试...")
with open("large_file.txt", "w", encoding="utf-8") as f:
    for i in range(10000):
        f.write(f"这是第 {i+1} 行数据，包含一些内容用于测试\n")

# 测试处理性能
start_time = time.time()
lines, chars = process_large_file("large_file.txt")
end_time = time.time()

print(f"处理结果: {lines} 行, {chars} 个字符")
print(f"处理时间: {end_time - start_time:.3f} 秒")

# 2. 批量文件操作
def batch_file_operations():
    """批量文件操作"""

    # 创建多个文件
    files = []
    for i in range(5):
        filename = f"batch_file_{i}.txt"
        with open(filename, "w", encoding="utf-8") as f:
            f.write(f"文件 {i} 的内容\n" * 100)
        files.append(filename)

    # 批量读取
    start_time = time.time()
    total_content = ""

    for filename in files:
        with open(filename, "r", encoding="utf-8") as f:
            total_content += f.read()

    end_time = time.time()
    print(f"批量读取 {len(files)} 个文件，耗时: {end_time - start_time:.3f} 秒")
    print(f"总内容长度: {len(total_content)}")

    # 清理文件
    for filename in files:
        os.remove(filename)

batch_file_operations()

# 3. 使用缓冲区优化写入
def buffered_write_test():
    """缓冲区写入测试"""

    # 不使用缓冲区
    start_time = time.time()
    with open("no_buffer.txt", "w", encoding="utf-8") as f:
        for i in range(1000):
            f.write(f"行 {i}\n")
            f.flush()  # 强制写入
    no_buffer_time = time.time() - start_time

    # 使用缓冲区
    start_time = time.time()
    with open("buffered.txt", "w", encoding="utf-8") as f:
        for i in range(1000):
            f.write(f"行 {i}\n")
        # 自动刷新缓冲区
    buffered_time = time.time() - start_time

    print(f"无缓冲区写入时间: {no_buffer_time:.3f} 秒")
    print(f"缓冲区写入时间: {buffered_time:.3f} 秒")
    print(f"性能提升: {no_buffer_time/buffered_time:.1f} 倍")

    # 清理文件
    for filename in ["no_buffer.txt", "buffered.txt", "large_file.txt"]:
        if os.path.exists(filename):
            os.remove(filename)

buffered_write_test()
```

### 18.7.3 错误处理和日志记录

```python
import logging
import os

# 配置日志
logging.basicConfig(
    filename='file_operations.log',
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s',
    encoding='utf-8'
)

class SafeFileHandler:
    """安全的文件处理器"""

    def __init__(self, filename):
        self.filename = filename
        self.logger = logging.getLogger(__name__)

    def safe_read(self, encoding='utf-8'):
        """安全读取文件"""

        try:
            if not os.path.exists(self.filename):
                self.logger.warning(f"文件不存在: {self.filename}")
                return None

            with open(self.filename, 'r', encoding=encoding) as file:
                content = file.read()
                self.logger.info(f"成功读取文件: {self.filename} ({len(content)} 字符)")
                return content

        except UnicodeDecodeError as e:
            self.logger.error(f"编码错误: {self.filename} - {e}")
            return None
        except PermissionError as e:
            self.logger.error(f"权限错误: {self.filename} - {e}")
            return None
        except Exception as e:
            self.logger.error(f"读取文件时发生未知错误: {self.filename} - {e}")
            return None

    def safe_write(self, content, encoding='utf-8', backup=True):
        """安全写入文件"""

        try:
            # 创建备份
            if backup and os.path.exists(self.filename):
                backup_name = self.filename + '.backup'
                os.replace(self.filename, backup_name)
                self.logger.info(f"创建备份: {backup_name}")

            # 写入新内容
            with open(self.filename, 'w', encoding=encoding) as file:
                file.write(content)

            self.logger.info(f"成功写入文件: {self.filename} ({len(content)} 字符)")
            return True

        except PermissionError as e:
            self.logger.error(f"权限错误: {self.filename} - {e}")
            return False
        except Exception as e:
            self.logger.error(f"写入文件时发生未知错误: {self.filename} - {e}")
            return False

# 测试安全文件处理器
handler = SafeFileHandler("test_safe.txt")

# 写入内容
success = handler.safe_write("这是测试内容\n包含多行\n数据")
print(f"写入结果: {success}")

# 读取内容
content = handler.safe_read()
if content:
    print(f"读取的内容:\n{content}")

# 测试错误情况
error_handler = SafeFileHandler("/root/forbidden.txt")
error_handler.safe_write("这应该会失败")

# 查看日志
print("\n操作日志:")
with open("file_operations.log", "r", encoding="utf-8") as log_file:
    print(log_file.read())

# 清理文件
for filename in ["test_safe.txt", "test_safe.txt.backup", "file_operations.log"]:
    if os.path.exists(filename):
        os.remove(filename)
```

## 总结

本章介绍了文件操作的基础知识：
- 理解文件的基本概念和操作模式
- 掌握文本文件和二进制文件的读写方法
- 学会使用文件指针进行随机访问
- 理解字符编码及其处理方法
- 掌握上下文管理器的使用
- 学习文件操作的最佳实践

通过本章的学习，你应该能够：
1. 熟练进行文件的基本读写操作
2. 处理不同类型的文件和编码
3. 使用上下文管理器确保资源正确释放
4. 实现安全和高效的文件操作
5. 处理文件操作中的各种异常情况

下一章将介绍文件和目录操作的高级内容。
