# 第19章：文件读写

## 19.1 打开和关闭文件

### 19.1.1 open() 函数详解

```python
# open() 函数的完整语法
# open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)

# 基本用法
file = open('example.txt', 'r', encoding='utf-8')

# 常用参数
print("open() 函数参数:")
print("- file: 文件路径")
print("- mode: 打开模式")
print("- encoding: 文本编码")
print("- buffering: 缓冲区大小")
print("- errors: 编码错误处理")
print("- newline: 换行符处理")

# 不同打开模式
modes = {
    'r': '只读模式',
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

for mode, desc in modes.items():
    print(f"  {mode:4}: {desc}")
```

### 19.1.2 文件对象的属性和方法

```python
# 创建测试文件
with open('test_file.txt', 'w', encoding='utf-8') as f:
    f.write('Hello, World!\n')
    f.write('第二行内容\n')
    f.write('第三行内容\n')

# 文件对象属性
with open('test_file.txt', 'r', encoding='utf-8') as file:
    print("文件对象属性:")
    print(f"  name: {file.name}")
    print(f"  mode: {file.mode}")
    print(f"  encoding: {file.encoding}")
    print(f"  closed: {file.closed}")
    print(f"  readable(): {file.readable()}")
    print(f"  writable(): {file.writable()}")
    print(f"  seekable(): {file.seekable()}")

    # 文件对象方法
    print("\n文件对象方法:")
    print(f"  tell(): {file.tell()}")  # 当前位置

    content = file.read()
    print(f"  read(): 读取了 {len(content)} 个字符")
    print(f"  tell() after read: {file.tell()}")

# 文件关闭检查
print(f"\n文件是否关闭: {file.closed}")

# 清理文件
import os
os.remove('test_file.txt')
```

### 19.1.3 安全的文件关闭

```python
# 不安全的做法（可能导致资源泄漏）
def unsafe_file_operation():
    file = open('example.txt', 'w')
    file.write('Hello')
    # 如果这里发生异常，文件不会被关闭
    # file.close()  # 这行可能不会执行

# 安全的做法1: try-finally
def safe_file_operation_v1():
    file = None
    try:
        file = open('example.txt', 'w', encoding='utf-8')
        file.write('Hello, World!')
        # 模拟可能发生的异常
        # raise ValueError("测试异常")
    finally:
        if file:
            file.close()
            print("文件已关闭")

# 安全的做法2: with 语句（推荐）
def safe_file_operation_v2():
    with open('example.txt', 'w', encoding='utf-8') as file:
        file.write('Hello, World!')
        # 即使发生异常，文件也会自动关闭
    print("文件已自动关闭")

# 测试异常情况
def test_exception_handling():
    try:
        with open('example.txt', 'w', encoding='utf-8') as file:
            file.write('开始写入\n')
            raise ValueError("模拟异常")
            file.write('这行不会执行')
    except ValueError as e:
        print(f"捕获到异常: {e}")

    # 检查文件是否已关闭
    print(f"文件是否关闭: {file.closed}")

# 运行测试
print("测试安全文件操作:")
safe_file_operation_v1()
safe_file_operation_v2()
test_exception_handling()

# 清理文件
if os.path.exists('example.txt'):
    os.remove('example.txt')
```

## 19.2 文本文件操作

### 19.2.1 读取文本文件

```python
# 创建示例文本文件
sample_text = """Python 编程语言
Python 是一种高级编程语言，具有简洁的语法和强大的功能。

主要特点：
1. 简单易学
2. 功能强大
3. 生态丰富
4. 社区活跃

应用领域：
- Web 开发
- 数据科学
- 人工智能
- 系统运维
"""

with open('sample.txt', 'w', encoding='utf-8') as f:
    f.write(sample_text)

# 方法1: read() - 读取整个文件
print("方法1: read() - 读取整个文件")
with open('sample.txt', 'r', encoding='utf-8') as file:
    content = file.read()
    print(f"文件内容:\n{content}")
    print(f"字符数: {len(content)}")

# 方法2: read(size) - 读取指定字符数
print("\n方法2: read(size) - 读取指定字符数")
with open('sample.txt', 'r', encoding='utf-8') as file:
    chunk1 = file.read(50)  # 读取前50个字符
    print(f"前50个字符: {repr(chunk1)}")

    chunk2 = file.read(50)  # 继续读取50个字符
    print(f"接下来50个字符: {repr(chunk2)}")

# 方法3: readline() - 逐行读取
print("\n方法3: readline() - 逐行读取")
with open('sample.txt', 'r', encoding='utf-8') as file:
    line_number = 1
    while True:
        line = file.readline()
        if not line:  # 空字符串表示文件结束
            break
        print(f"第{line_number}行: {line.rstrip()}")
        line_number += 1

# 方法4: readlines() - 读取所有行
print("\n方法4: readlines() - 读取所有行")
with open('sample.txt', 'r', encoding='utf-8') as file:
    lines = file.readlines()
    print(f"总行数: {len(lines)}")
    for i, line in enumerate(lines, 1):
        print(f"第{i}行: {line.rstrip()}")

# 方法5: 直接迭代文件对象
print("\n方法5: 直接迭代文件对象")
with open('sample.txt', 'r', encoding='utf-8') as file:
    for line_number, line in enumerate(file, 1):
        print(f"第{line_number}行: {line.rstrip()}")

# 清理文件
os.remove('sample.txt')
```

### 19.2.2 写入文本文件

```python
# 方法1: write() - 写入字符串
print("方法1: write() - 写入字符串")
with open('output1.txt', 'w', encoding='utf-8') as file:
    file.write("第一行内容\n")
    file.write("第二行内容\n")
    file.write("第三行内容\n")

# 读取验证
with open('output1.txt', 'r', encoding='utf-8') as file:
    print("写入的内容:")
    print(repr(file.read()))

# 方法2: writelines() - 写入字符串列表
print("\n方法2: writelines() - 写入字符串列表")
lines_to_write = [
    "Python 是一门优秀的编程语言\n",
    "它具有简洁的语法\n",
    "强大的功能\n",
    "丰富的生态系统\n"
]

with open('output2.txt', 'w', encoding='utf-8') as file:
    file.writelines(lines_to_write)

# 读取验证
with open('output2.txt', 'r', encoding='utf-8') as file:
    print("写入的内容:")
    print(repr(file.read()))

# 方法3: print() 函数写入文件
print("\n方法3: print() 函数写入文件")
with open('output3.txt', 'w', encoding='utf-8') as file:
    print("使用 print() 写入", file=file)
    print("自动添加换行符", file=file)
    print("很方便的方法", file=file)

# 读取验证
with open('output3.txt', 'r', encoding='utf-8') as file:
    print("写入的内容:")
    print(repr(file.read()))

# 清理文件
for filename in ['output1.txt', 'output2.txt', 'output3.txt']:
    if os.path.exists(filename):
        os.remove(filename)
```

### 19.2.3 文件位置和缓冲

```python
# 文件位置操作
with open('position_test.txt', 'w', encoding='utf-8') as f:
    f.write('0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ')

with open('position_test.txt', 'r', encoding='utf-8') as file:
    print("文件位置操作:")

    # 查看文件大小
    file.seek(0, 2)  # 移动到文件末尾
    file_size = file.tell()
    print(f"文件大小: {file_size} 字符")

    # 回到开头
    file.seek(0)
    print(f"当前位置: {file.tell()}")

    # 读取前10个字符
    content = file.read(10)
    print(f"读取内容: {repr(content)}")
    print(f"当前位置: {file.tell()}")

    # 移动到第20个字符
    file.seek(20)
    content = file.read(5)
    print(f"从位置20读取5个字符: {repr(content)}")

# 缓冲区演示
print("\n缓冲区演示:")
import time

# 无缓冲写入
start_time = time.time()
with open('no_flush.txt', 'w', encoding='utf-8') as f:
    for i in range(100):
        f.write(f"行 {i}\n")
        f.flush()  # 强制刷新缓冲区
no_flush_time = time.time() - start_time

# 缓冲写入
start_time = time.time()
with open('buffered.txt', 'w', encoding='utf-8') as f:
    for i in range(100):
        f.write(f"行 {i}\n")
    # 自动刷新缓冲区
buffered_time = time.time() - start_time

print(f"无缓冲写入时间: {no_flush_time:.4f} 秒")
print(f"缓冲写入时间: {buffered_time:.4f} 秒")

# 清理文件
for filename in ['position_test.txt', 'no_flush.txt', 'buffered.txt']:
    if os.path.exists(filename):
        os.remove(filename)
```

## 19.3 二进制文件操作

### 19.3.1 读取二进制文件

```python
# 创建二进制数据
binary_data = bytes([
    0x48, 0x65, 0x6C, 0x6C, 0x6F,  # "Hello"
    0x20, 0x57, 0x6F, 0x72, 0x6C, 0x64,  # " World"
    0x21, 0x0A,  # "!\n"
    0x50, 0x79, 0x74, 0x68, 0x6F, 0x6E,  # "Python"
])

# 写入二进制文件
with open('binary_file.bin', 'wb') as file:
    file.write(binary_data)
    # 写入更多数据
    file.write(b"\n")
    file.write(bytes([80, 121, 116, 104, 111, 110]))  # "Python"

# 读取二进制文件
print("读取二进制文件:")
with open('binary_file.bin', 'rb') as file:
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
with open('binary_file.bin', 'rb') as file:
    chunk_size = 5
    while True:
        chunk = file.read(chunk_size)
        if not chunk:
            break
        print(f"块: {chunk} (长度: {len(chunk)})")

# 清理文件
os.remove('binary_file.bin')
```

### 19.3.2 写入二进制文件

```python
# 创建不同类型的二进制数据
import struct

# 1. 写入整数
print("写入整数:")
with open('integers.bin', 'wb') as file:
    # 使用 struct 打包数据
    data = struct.pack('iii', 42, 100, 255)  # 3个整数
    file.write(data)

# 读取验证
with open('integers.bin', 'rb') as file:
    data = file.read()
    numbers = struct.unpack('iii', data)
    print(f"读取的整数: {numbers}")

# 2. 写入浮点数
print("\n写入浮点数:")
with open('floats.bin', 'wb') as file:
    data = struct.pack('ff', 3.14159, 2.71828)  # 2个浮点数
    file.write(data)

# 读取验证
with open('floats.bin', 'rb') as file:
    data = file.read()
    floats = struct.unpack('ff', data)
    print(f"读取的浮点数: {floats}")

# 3. 写入字符串（UTF-8编码）
print("\n写入字符串:")
text = "Hello, 世界!"
encoded_text = text.encode('utf-8')

with open('text.bin', 'wb') as file:
    # 先写入字符串长度
    length_data = struct.pack('I', len(encoded_text))
    file.write(length_data)
    # 再写入字符串数据
    file.write(encoded_text)

# 读取验证
with open('text.bin', 'rb') as file:
    length_data = file.read(4)  # 读取4字节的长度
    length = struct.unpack('I', length_data)[0]
    text_data = file.read(length)
    decoded_text = text_data.decode('utf-8')
    print(f"读取的字符串: {decoded_text}")

# 4. 写入混合数据
print("\n写入混合数据:")
with open('mixed.bin', 'wb') as file:
    # 写入一个整数
    file.write(struct.pack('i', 12345))
    # 写入一个浮点数
    file.write(struct.pack('f', 3.14))
    # 写入一个字符串
    text = "混合数据"
    encoded = text.encode('utf-8')
    file.write(struct.pack('I', len(encoded)))
    file.write(encoded)

# 读取验证
with open('mixed.bin', 'rb') as file:
    # 读取整数
    int_data = file.read(4)
    number = struct.unpack('i', int_data)[0]
    print(f"整数: {number}")

    # 读取浮点数
    float_data = file.read(4)
    float_num = struct.unpack('f', float_data)[0]
    print(f"浮点数: {float_num}")

    # 读取字符串
    length_data = file.read(4)
    length = struct.unpack('I', length_data)[0]
    text_data = file.read(length)
    text = text_data.decode('utf-8')
    print(f"字符串: {text}")

# 清理文件
for filename in ['integers.bin', 'floats.bin', 'text.bin', 'mixed.bin']:
    if os.path.exists(filename):
        os.remove(filename)
```

### 19.3.3 图像和媒体文件处理

```python
# 创建简单的BMP图像数据
def create_simple_bmp(width, height):
    """创建简单的BMP图像"""

    # BMP文件头 (14字节)
    file_header = b'BM'  # 文件类型
    file_header += (14 + 40 + width * height * 3).to_bytes(4, 'little')  # 文件大小
    file_header += (0).to_bytes(4, 'little')  # 保留
    file_header += (54).to_bytes(4, 'little')  # 数据偏移

    # BMP信息头 (40字节)
    info_header = (40).to_bytes(4, 'little')  # 信息头大小
    info_header += width.to_bytes(4, 'little')  # 宽度
    info_header += height.to_bytes(4, 'little')  # 高度
    info_header += (1).to_bytes(2, 'little')  # 颜色平面数
    info_header += (24).to_bytes(2, 'little')  # 位深度
    info_header += (0).to_bytes(4, 'little')  # 压缩方式
    info_header += (width * height * 3).to_bytes(4, 'little')  # 图像大小
    info_header += (0).to_bytes(4, 'little')  # X像素每米
    info_header += (0).to_bytes(4, 'little')  # Y像素每米
    info_header += (0).to_bytes(4, 'little')  # 使用的颜色数
    info_header += (0).to_bytes(4, 'little')  # 重要颜色数

    # 创建简单的渐变图像数据 (BGR格式)
    image_data = b''
    for y in range(height):
        for x in range(width):
            # 简单的红色渐变
            red = int(255 * x / width)
            green = int(255 * y / height)
            blue = 128
            image_data += bytes([blue, green, red])  # BGR格式

    return file_header + info_header + image_data

# 创建并保存BMP图像
print("创建BMP图像:")
bmp_data = create_simple_bmp(100, 100)

with open('gradient.bmp', 'wb') as file:
    file.write(bmp_data)

print(f"图像文件大小: {len(bmp_data)} 字节")

# 读取并分析BMP文件
print("\n分析BMP文件:")
with open('gradient.bmp', 'rb') as file:
    # 读取文件头
    file_header = file.read(14)
    print(f"文件头: {file_header.hex()}")

    # 读取信息头
    info_header = file.read(40)
    print(f"信息头长度: {len(info_header)} 字节")

    # 解析信息头
    width = int.from_bytes(info_header[4:8], 'little')
    height = int.from_bytes(info_header[8:12], 'little')
    bit_depth = int.from_bytes(info_header[14:16], 'little')

    print(f"图像尺寸: {width} x {height}")
    print(f"位深度: {bit_depth}")

    # 计算数据部分大小
    data_size = width * height * 3
    print(f"图像数据大小: {data_size} 字节")

# 清理文件
os.remove('gradient.bmp')
```

## 19.4 文件路径处理

### 19.4.1 路径操作基础

```python
import os
from pathlib import Path

# 当前工作目录
print("当前工作目录:")
current_dir = os.getcwd()
print(f"os.getcwd(): {current_dir}")

# 改变工作目录
print("\n改变工作目录:")
print(f"当前目录: {os.getcwd()}")

# 创建测试目录
test_dir = "test_directory"
if not os.path.exists(test_dir):
    os.mkdir(test_dir)

os.chdir(test_dir)
print(f"改变后目录: {os.getcwd()}")

# 回到原目录
os.chdir("..")
print(f"回到原目录: {os.getcwd()}")

# 清理
os.rmdir(test_dir)

# 路径拼接
print("\n路径拼接:")
path1 = os.path.join("home", "user", "documents", "file.txt")
print(f"os.path.join(): {path1}")

path2 = os.path.join(os.getcwd(), "data", "file.txt")
print(f"完整路径: {path2}")

# 路径分隔
print("\n路径分隔:")
full_path = "/home/user/documents/file.txt"
print(f"完整路径: {full_path}")
print(f"目录部分: {os.path.dirname(full_path)}")
print(f"文件名部分: {os.path.basename(full_path)}")
print(f"文件名: {os.path.splitext(os.path.basename(full_path))[0]}")
print(f"扩展名: {os.path.splitext(full_path)[1]}")

# 路径规范化
print("\n路径规范化:")
messy_path = "home//user/./documents/../documents/file.txt"
normalized = os.path.normpath(messy_path)
print(f"原始路径: {messy_path}")
print(f"规范化: {normalized}")

# 使用 pathlib
print("\n使用 pathlib:")
p = Path("home/user/documents/file.txt")
print(f"Path对象: {p}")
print(f"父目录: {p.parent}")
print(f"文件名: {p.name}")
print(f"扩展名: {p.suffix}")
print(f"无扩展名文件名: {p.stem}")
print(f"是否存在: {p.exists()}")

# pathlib 路径操作
p1 = Path("data/input.txt")
p2 = Path("data/output.txt")
print(f"p1: {p1}")
print(f"p2: {p2}")
print(f"p1.parent: {p1.parent}")
print(f"p1.relative_to(p1.parent): {p1.relative_to(p1.parent)}")
```

### 19.4.2 路径判断和转换

```python
# 路径判断
test_paths = [
    "existing_file.txt",
    "nonexistent_file.txt",
    "test_directory",
    "/absolute/path",
    "relative/path"
]

# 创建测试文件和目录
with open("existing_file.txt", "w") as f:
    f.write("test")

os.mkdir("test_directory")

print("路径判断:")
for path in test_paths:
    print(f"\n路径: {path}")
    print(f"  存在: {os.path.exists(path)}")
    print(f"  是文件: {os.path.isfile(path)}")
    print(f"  是目录: {os.path.isdir(path)}")
    print(f"  是绝对路径: {os.path.isabs(path)}")
    print(f"  是链接: {os.path.islink(path)}")

# 路径转换
print("\n路径转换:")
relative_path = "data/file.txt"
absolute_path = "/home/user/documents/file.txt"

print(f"相对路径: {relative_path}")
print(f"转换为绝对路径: {os.path.abspath(relative_path)}")

print(f"绝对路径: {absolute_path}")
print(f"转换为相对路径: {os.path.relpath(absolute_path)}")

# 获取路径的各个部分
print("\n路径分解:")
test_path = "/home/user/documents/file.txt"
print(f"完整路径: {test_path}")

parts = []
current = test_path
while current != os.path.dirname(current):
    parts.append(os.path.basename(current))
    current = os.path.dirname(current)

parts.reverse()
print(f"路径部分: {parts}")

# 使用 pathlib 进行路径操作
print("\npathlib 路径操作:")
p = Path("/home/user/documents/file.txt")

print(f"Path: {p}")
print(f"parts: {p.parts}")
print(f"drive: {p.drive}")
print(f"root: {p.root}")
print(f"anchor: {p.anchor}")

# 路径计算
print("\n路径计算:")
base_path = Path("/home/user")
file_path = Path("documents/file.txt")

# 拼接路径
combined = base_path / file_path
print(f"拼接路径: {combined}")

# 计算相对路径
print(f"相对路径: {combined.relative_to(base_path)}")

# 解析路径
complex_path = Path("~/documents/../downloads/./file.txt")
resolved = complex_path.expanduser().resolve()
print(f"解析路径: {resolved}")

# 清理
os.remove("existing_file.txt")
os.rmdir("test_directory")
```

### 19.4.3 批量路径处理

```python
import glob

# 创建测试文件结构
os.makedirs("project/src", exist_ok=True)
os.makedirs("project/docs", exist_ok=True)
os.makedirs("project/tests", exist_ok=True)

# 创建测试文件
test_files = [
    "project/src/main.py",
    "project/src/utils.py",
    "project/src/config.py",
    "project/docs/readme.md",
    "project/docs/guide.txt",
    "project/tests/test_main.py",
    "project/tests/test_utils.py",
    "project/data.json",
    "project/config.ini"
]

for file_path in test_files:
    with open(file_path, "w") as f:
        f.write(f"# {os.path.basename(file_path)}\n")

print("创建的文件结构:")
for root, dirs, files in os.walk("project"):
    level = root.replace("project", "").count(os.sep)
    indent = " " * 2 * level
    print(f"{indent}{os.path.basename(root)}/")
    subindent = " " * 2 * (level + 1)
    for file in files:
        print(f"{subindent}{file}")

# 使用 glob 查找文件
print("\n使用 glob 查找文件:")

# 查找所有 Python 文件
python_files = glob.glob("project/**/*.py", recursive=True)
print(f"Python 文件: {python_files}")

# 查找所有文档文件
doc_files = glob.glob("project/**/*.md", recursive=True) + glob.glob("project/**/*.txt", recursive=True)
print(f"文档文件: {doc_files}")

# 查找 src 目录下的所有文件
src_files = glob.glob("project/src/*")
print(f"src 目录文件: {src_files}")

# 查找所有配置文件
config_files = glob.glob("project/**/*.ini", recursive=True) + glob.glob("project/**/*.json", recursive=True)
print(f"配置文件: {config_files}")

# 使用 pathlib 进行更复杂的查找
print("\n使用 pathlib 查找文件:")

project_path = Path("project")

# 查找所有 Python 文件
python_files_pathlib = list(project_path.rglob("*.py"))
print(f"Python 文件: {[str(p) for p in python_files_pathlib]}")

# 查找大文件（超过100字节）
large_files = [p for p in project_path.rglob("*") if p.is_file() and p.stat().st_size > 100]
print(f"大文件: {[str(p) for p in large_files]}")

# 按扩展名分组文件
from collections import defaultdict

files_by_ext = defaultdict(list)
for file_path in project_path.rglob("*"):
    if file_path.is_file():
        ext = file_path.suffix
        files_by_ext[ext].append(str(file_path))

print("\n按扩展名分组的文件:")
for ext, files in sorted(files_by_ext.items()):
    print(f"{ext or '无扩展名'}: {files}")

# 清理测试文件
import shutil
shutil.rmtree("project")
```

## 总结

本章介绍了文件读写的核心概念：
- 掌握 open() 函数的各种参数和模式
- 理解文件对象的属性和方法
- 学会安全的文件关闭方法
- 熟练进行文本文件的读写操作
- 掌握二进制文件的处理技巧
- 理解文件路径的处理方法

通过本章的学习，你应该能够：
1. 正确打开和关闭各种类型的文件
2. 进行文本和二进制文件的读写操作
3. 处理文件位置和缓冲
4. 进行路径操作和文件查找
5. 编写安全和高效的文件处理代码

下一章将介绍文件系统操作的高级内容。
