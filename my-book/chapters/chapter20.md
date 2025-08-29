# 第20章：文件系统操作

## 20.1 os 模块

### 20.1.1 系统信息和环境变量

```python
import os
import platform

# 系统信息
print("系统信息:")
print(f"操作系统: {os.name}")
print(f"平台: {platform.system()}")
print(f"平台详情: {platform.platform()}")
print(f"处理器架构: {platform.machine()}")
print(f"Python 版本: {platform.python_version()}")

# 当前工作目录
print(f"\n当前工作目录: {os.getcwd()}")

# 环境变量
print("\n环境变量:")
print(f"PATH: {os.environ.get('PATH', 'Not found')[:100]}...")
print(f"HOME: {os.environ.get('HOME', 'Not found')}")
print(f"USER: {os.environ.get('USER', 'Not found')}")

# 获取特定环境变量
pythonpath = os.environ.get('PYTHONPATH', '')
if pythonpath:
    print(f"PYTHONPATH: {pythonpath}")
else:
    print("PYTHONPATH 未设置")

# 设置环境变量
os.environ['MY_VAR'] = 'Hello World'
print(f"自定义变量: {os.environ.get('MY_VAR')}")

# 列出所有环境变量
print("\n前10个环境变量:")
for i, (key, value) in enumerate(os.environ.items()):
    if i >= 10:
        break
    print(f"  {key}: {value[:50]}...")
```

### 20.1.2 目录操作

```python
# 创建目录
print("创建目录:")

# 创建单个目录
os.mkdir("test_dir")
print("创建目录: test_dir")

# 创建嵌套目录
os.makedirs("parent/child/grandchild", exist_ok=True)
print("创建嵌套目录: parent/child/grandchild")

# 列出目录内容
print("\n列出目录内容:")
contents = os.listdir(".")
print(f"当前目录内容: {contents}")

# 递归列出目录内容
def list_directory(path, level=0):
    """递归列出目录内容"""
    indent = "  " * level
    try:
        items = os.listdir(path)
        for item in sorted(items):
            item_path = os.path.join(path, item)
            if os.path.isdir(item_path):
                print(f"{indent}[DIR] {item}")
                list_directory(item_path, level + 1)
            else:
                print(f"{indent}[FILE] {item}")
    except PermissionError:
        print(f"{indent}[ACCESS DENIED] {path}")

print("\n递归列出目录结构:")
list_directory("parent")

# 改变工作目录
print(f"\n当前工作目录: {os.getcwd()}")
os.chdir("parent")
print(f"改变到 parent: {os.getcwd()}")
os.chdir("..")
print(f"回到上级目录: {os.getcwd()}")

# 删除目录
print("\n删除目录:")
os.rmdir("test_dir")
print("删除目录: test_dir")

# 删除嵌套目录
import shutil
shutil.rmtree("parent")
print("删除嵌套目录: parent")
```

### 20.1.3 文件操作

```python
# 文件操作
print("文件操作:")

# 创建测试文件
with open("test_file.txt", "w") as f:
    f.write("这是一个测试文件")

# 文件属性
file_path = "test_file.txt"
print(f"\n文件: {file_path}")
print(f"存在: {os.path.exists(file_path)}")
print(f"是文件: {os.path.isfile(file_path)}")
print(f"是目录: {os.path.isdir(file_path)}")
print(f"绝对路径: {os.path.abspath(file_path)}")

# 文件大小和时间
stat_info = os.stat(file_path)
print(f"文件大小: {stat_info.st_size} 字节")
print(f"修改时间: {stat_info.st_mtime}")
print(f"访问时间: {stat_info.st_atime}")
print(f"创建时间: {stat_info.st_ctime}")

# 格式化时间显示
import time
print(f"修改时间(格式化): {time.ctime(stat_info.st_mtime)}")

# 文件权限
print(f"文件权限: {oct(stat_info.st_mode)}")
print(f"可读: {os.access(file_path, os.R_OK)}")
print(f"可写: {os.access(file_path, os.W_OK)}")
print(f"可执行: {os.access(file_path, os.X_OK)}")

# 重命名文件
os.rename("test_file.txt", "renamed_file.txt")
print(f"\n重命名文件: test_file.txt -> renamed_file.txt")

# 复制文件
shutil.copy("renamed_file.txt", "copied_file.txt")
print("复制文件: renamed_file.txt -> copied_file.txt")

# 移动文件
os.rename("copied_file.txt", "subdir/copied_file.txt")
print("移动文件到子目录")

# 删除文件
os.remove("renamed_file.txt")
print("删除文件: renamed_file.txt")

# 清理
if os.path.exists("subdir"):
    shutil.rmtree("subdir")
```

### 20.1.4 路径操作

```python
# 路径操作
print("路径操作:")

# 路径拼接
path1 = os.path.join("home", "user", "documents")
path2 = os.path.join(path1, "file.txt")
print(f"路径拼接: {path2}")

# 路径分隔
full_path = "/home/user/documents/file.txt"
print(f"\n完整路径: {full_path}")
print(f"目录部分: {os.path.dirname(full_path)}")
print(f"文件名部分: {os.path.basename(full_path)}")
print(f"文件名: {os.path.splitext(os.path.basename(full_path))[0]}")
print(f"扩展名: {os.path.splitext(full_path)[1]}")

# 路径判断
test_paths = [
    "/absolute/path",
    "relative/path",
    "existing_file.txt",
    "nonexistent.txt"
]

# 创建测试文件
with open("existing_file.txt", "w") as f:
    f.write("test")

print("\n路径判断:")
for path in test_paths:
    print(f"路径: {path}")
    print(f"  存在: {os.path.exists(path)}")
    print(f"  是文件: {os.path.isfile(path)}")
    print(f"  是目录: {os.path.isdir(path)}")
    print(f"  是绝对路径: {os.path.isabs(path)}")
    print(f"  是链接: {os.path.islink(path)}")

# 路径转换
print("\n路径转换:")
relative_path = "data/file.txt"
absolute_path = os.path.abspath(relative_path)
print(f"相对路径: {relative_path}")
print(f"绝对路径: {absolute_path}")

back_to_relative = os.path.relpath(absolute_path)
print(f"转回相对路径: {back_to_relative}")

# 路径规范化
messy_path = "home//user/./documents/../documents/file.txt"
normalized = os.path.normpath(messy_path)
print(f"\n规范化路径:")
print(f"原始: {messy_path}")
print(f"规范化: {normalized}")

# 路径比较
path_a = "/home/user/file.txt"
path_b = "/home/user/documents/../user/file.txt"
print(f"\n路径比较:")
print(f"路径A: {path_a}")
print(f"路径B: {path_b}")
print(f"相同: {os.path.samefile(path_a, path_b) if os.path.exists(path_a) else '无法比较'}")

# 清理
os.remove("existing_file.txt")
```

## 20.2 pathlib 模块

### 20.2.1 Path 对象的创建和基本操作

```python
from pathlib import Path
import os

# 创建 Path 对象
print("创建 Path 对象:")

# 从字符串创建
p1 = Path("home/user/documents/file.txt")
print(f"从字符串: {p1}")

# 从当前目录创建
p2 = Path.cwd() / "data" / "file.txt"
print(f"从当前目录: {p2}")

# 从环境变量创建
home_path = Path.home()
print(f"用户主目录: {home_path}")

# Path 对象属性
print(f"\nPath 对象属性:")
print(f"路径: {p1}")
print(f"父目录: {p1.parent}")
print(f"文件名: {p1.name}")
print(f"扩展名: {p1.suffix}")
print(f"无扩展名文件名: {p1.stem}")
print(f"parts: {p1.parts}")
print(f"drive: {p1.drive}")
print(f"root: {p1.root}")
print(f"anchor: {p1.anchor}")

# 路径判断
print(f"\n路径判断:")
print(f"存在: {p1.exists()}")
print(f"是文件: {p1.is_file()}")
print(f"是目录: {p1.is_dir()}")
print(f"是绝对路径: {p1.is_absolute()}")
print(f"是相对路径: {p1.is_relative_to(Path.cwd())}")
```

### 20.2.2 路径操作和计算

```python
# 路径操作
print("路径操作:")

base_path = Path("/home/user")
file_path = Path("documents/file.txt")

# 路径拼接
combined = base_path / file_path
print(f"路径拼接: {combined}")

# 路径计算
print(f"\n路径计算:")
p = Path("/home/user/documents/file.txt")
print(f"原始路径: {p}")

# 相对路径
relative = p.relative_to(Path("/home/user"))
print(f"相对路径: {relative}")

# 解析路径（解析符号链接和相对路径）
complex_path = Path("~/documents/../downloads/./file.txt")
resolved = complex_path.expanduser().resolve()
print(f"解析路径: {resolved}")

# 路径匹配
print(f"\n路径匹配:")
files = [
    Path("data/file.txt"),
    Path("data/image.jpg"),
    Path("src/main.py"),
    Path("src/utils.py")
]

# 匹配模式
patterns = ["*.txt", "*.py", "data/*"]
for pattern in patterns:
    print(f"匹配 {pattern}:")
    for file in files:
        if file.match(pattern):
            print(f"  {file}")

# 路径替换
print(f"\n路径替换:")
original = Path("/home/user/file.txt")
print(f"原始: {original}")

# 替换扩展名
new_ext = original.with_suffix(".bak")
print(f"替换扩展名: {new_ext}")

# 替换文件名
new_name = original.with_name("new_file.txt")
print(f"替换文件名: {new_name}")

# 替换父目录
new_parent = original.with_stem("backup")
print(f"替换文件名部分: {new_parent}")
```

### 20.2.3 文件和目录操作

```python
# 创建测试目录结构
test_dir = Path("pathlib_test")
test_dir.mkdir(exist_ok=True)

# 创建子目录
(test_dir / "src").mkdir(exist_ok=True)
(test_dir / "docs").mkdir(exist_ok=True)
(test_dir / "data").mkdir(exist_ok=True)

# 创建测试文件
test_files = [
    test_dir / "src" / "main.py",
    test_dir / "src" / "utils.py",
    test_dir / "docs" / "readme.md",
    test_dir / "docs" / "guide.txt",
    test_dir / "data" / "config.json",
    test_dir / "data" / "data.txt"
]

for file_path in test_files:
    file_path.write_text(f"# {file_path.name}\n这是 {file_path.name} 文件的内容\n")

print("创建的目录结构:")
def print_tree(path, prefix=""):
    if path.is_dir():
        print(f"{prefix}[DIR] {path.name}")
        for item in sorted(path.iterdir()):
            print_tree(item, prefix + "  ")
    else:
        print(f"{prefix}[FILE] {path.name}")

print_tree(test_dir)

# 文件操作
print(f"\n文件操作:")

# 读取文件
readme_path = test_dir / "docs" / "readme.md"
if readme_path.exists():
    content = readme_path.read_text(encoding='utf-8')
    print(f"读取文件内容:\n{content}")

# 写入文件
new_file = test_dir / "new_file.txt"
new_file.write_text("这是新创建的文件\n包含一些内容\n", encoding='utf-8')
print(f"创建新文件: {new_file}")

# 追加内容
new_file.write_text(new_file.read_text() + "追加的内容\n", encoding='utf-8')
print("追加文件内容")

# 复制文件
copied_file = test_dir / "copied_file.txt"
import shutil
shutil.copy(new_file, copied_file)
print(f"复制文件: {copied_file}")

# 移动文件
moved_file = test_dir / "moved_file.txt"
new_file.rename(moved_file)
print(f"移动文件: {moved_file}")

# 删除文件
copied_file.unlink()
print("删除文件: copied_file.txt")

# 目录操作
print(f"\n目录操作:")

# 列出目录内容
print("docs 目录内容:")
for item in (test_dir / "docs").iterdir():
    print(f"  {item.name}")

# 递归查找文件
print("\n递归查找所有 .py 文件:")
for py_file in test_dir.rglob("*.py"):
    print(f"  {py_file}")

# 查找所有文件
print("\n查找所有文件:")
for file_path in test_dir.rglob("*"):
    if file_path.is_file():
        size = file_path.stat().st_size
        print(f"  {file_path.relative_to(test_dir)} ({size} 字节)")

# 清理
import shutil
shutil.rmtree(test_dir)
print(f"\n清理测试目录: {test_dir}")
```

### 20.2.4 高级路径处理

```python
# 高级路径处理
print("高级路径处理:")

# 1. 路径比较和排序
paths = [
    Path("/home/user/documents/file.txt"),
    Path("/home/user/pictures/image.jpg"),
    Path("/home/user/music/song.mp3"),
    Path("/home/user/documents/readme.md")
]

print("路径排序:")
for path in sorted(paths):
    print(f"  {path}")

# 2. 路径分组
from collections import defaultdict

files_by_dir = defaultdict(list)
for path in paths:
    files_by_dir[path.parent].append(path.name)

print("\n按目录分组:")
for dir_path, files in files_by_dir.items():
    print(f"  {dir_path}: {files}")

# 3. 批量路径操作
print("\n批量路径操作:")

# 创建多个文件
base_dir = Path("batch_test")
base_dir.mkdir(exist_ok=True)

# 批量创建文件
file_names = [f"file_{i:02d}.txt" for i in range(10)]
for name in file_names:
    file_path = base_dir / name
    file_path.write_text(f"这是 {name}\n内容: {name}\n")

print(f"创建了 {len(file_names)} 个文件")

# 批量读取文件
total_content = ""
for file_path in base_dir.glob("*.txt"):
    content = file_path.read_text()
    total_content += content + "---\n"

print(f"总内容长度: {len(total_content)}")

# 批量重命名
print("批量重命名:")
for file_path in base_dir.glob("*.txt"):
    new_name = f"renamed_{file_path.name}"
    new_path = file_path.with_name(new_name)
    file_path.rename(new_path)
    print(f"  {file_path.name} -> {new_name}")

# 4. 路径统计
print("\n路径统计:")
all_files = list(base_dir.rglob("*"))
files_count = sum(1 for p in all_files if p.is_file())
dirs_count = sum(1 for p in all_files if p.is_dir())

print(f"文件总数: {files_count}")
print(f"目录总数: {dirs_count}")

# 文件大小统计
total_size = sum(p.stat().st_size for p in all_files if p.is_file())
print(f"总文件大小: {total_size} 字节")

# 按扩展名统计
from collections import Counter
extensions = Counter(p.suffix for p in all_files if p.is_file())
print("按扩展名统计:")
for ext, count in extensions.items():
    print(f"  {ext or '无扩展名'}: {count} 个文件")

# 清理
shutil.rmtree(base_dir)
```

## 20.3 文件和目录操作

### 20.3.1 高级文件操作

```python
import os
import shutil
import tempfile
from pathlib import Path

# 1. 临时文件和目录
print("临时文件和目录:")

# 创建临时文件
with tempfile.NamedTemporaryFile(mode='w', suffix='.txt', delete=False) as temp_file:
    temp_file.write("这是临时文件的内容\n")
    temp_path = temp_file.name

print(f"创建临时文件: {temp_path}")

# 读取临时文件
with open(temp_path, 'r') as f:
    content = f.read()
    print(f"临时文件内容: {content}")

# 删除临时文件
os.unlink(temp_path)
print("删除临时文件")

# 创建临时目录
with tempfile.TemporaryDirectory() as temp_dir:
    print(f"创建临时目录: {temp_dir}")

    # 在临时目录中创建文件
    test_file = Path(temp_dir) / "test.txt"
    test_file.write_text("临时目录中的文件")

    # 列出临时目录内容
    contents = list(Path(temp_dir).iterdir())
    print(f"临时目录内容: {[p.name for p in contents]}")

print("临时目录自动删除")

# 2. 文件备份和恢复
print("\n文件备份和恢复:")

# 创建原始文件
original_file = Path("original.txt")
original_file.write_text("这是原始文件的内容\n重要数据\n")

# 创建备份
backup_file = original_file.with_suffix('.bak')
shutil.copy2(original_file, backup_file)
print(f"创建备份: {backup_file}")

# 修改原始文件
original_file.write_text(original_file.read_text() + "新增内容\n")
print("修改原始文件")

# 从备份恢复
shutil.copy2(backup_file, original_file)
print("从备份恢复文件")

# 验证恢复结果
content = original_file.read_text()
print(f"恢复后的内容:\n{content}")

# 3. 文件同步
print("\n文件同步:")

def sync_files(source_dir, target_dir):
    """同步两个目录"""
    source_path = Path(source_dir)
    target_path = Path(target_dir)

    if not source_path.exists():
        print(f"源目录不存在: {source_dir}")
        return

    target_path.mkdir(parents=True, exist_ok=True)

    synced_files = 0
    for source_file in source_path.rglob("*"):
        if source_file.is_file():
            # 计算相对路径
            relative_path = source_file.relative_to(source_path)
            target_file = target_path / relative_path

            # 确保目标目录存在
            target_file.parent.mkdir(parents=True, exist_ok=True)

            # 检查是否需要复制
            if not target_file.exists() or source_file.stat().st_mtime > target_file.stat().st_mtime:
                shutil.copy2(source_file, target_file)
                synced_files += 1

    print(f"同步完成，共复制 {synced_files} 个文件")

# 创建测试目录
source_dir = Path("source")
target_dir = Path("target")

source_dir.mkdir(exist_ok=True)
(source_dir / "file1.txt").write_text("文件1内容")
(source_dir / "subdir").mkdir(exist_ok=True)
(source_dir / "subdir" / "file2.txt").write_text("文件2内容")

# 同步文件
sync_files(source_dir, target_dir)

print("同步后的目标目录结构:")
def print_tree(path, prefix=""):
    if path.is_dir():
        print(f"{prefix}[DIR] {path.name}")
        for item in sorted(path.iterdir()):
            print_tree(item, prefix + "  ")
    else:
        print(f"{prefix}[FILE] {path.name}")

print_tree(target_dir)

# 清理
shutil.rmtree(source_dir)
shutil.rmtree(target_dir)
original_file.unlink()
backup_file.unlink()
```

### 20.3.2 目录遍历和搜索

```python
# 目录遍历和搜索
print("目录遍历和搜索:")

# 创建复杂的目录结构
project_root = Path("sample_project")
project_root.mkdir(exist_ok=True)

# 创建目录结构
dirs = [
    "src",
    "src/utils",
    "src/core",
    "tests",
    "tests/unit",
    "tests/integration",
    "docs",
    "docs/api",
    "data",
    "data/input",
    "data/output"
]

for dir_path in dirs:
    (project_root / dir_path).mkdir(parents=True, exist_ok=True)

# 创建各种类型的文件
files = [
    ("src/main.py", "print('Hello, World!')"),
    ("src/utils/helpers.py", "def helper():\n    pass"),
    ("src/core/engine.py", "class Engine:\n    pass"),
    ("tests/unit/test_main.py", "def test_main():\n    pass"),
    ("tests/integration/test_api.py", "def test_api():\n    pass"),
    ("docs/readme.md", "# 项目文档"),
    ("docs/api/index.html", "<html><body>API文档</body></html>"),
    ("data/input/config.json", '{"setting": "value"}'),
    ("data/output/result.txt", "处理结果"),
    (".gitignore", "*.pyc\n__pycache__/"),
    ("requirements.txt", "requests==2.25.1\nflask==2.0.1")
]

for file_path, content in files:
    (project_root / file_path).write_text(content)

print("创建的项目结构:")
print_tree(project_root)

# 1. 使用 os.walk 遍历目录
print("\n使用 os.walk 遍历:")
for root, dirs, files in os.walk(project_root):
    level = root.replace(str(project_root), "").count(os.sep)
    indent = "  " * level
    print(f"{indent}{Path(root).name}/")
    subindent = "  " * (level + 1)
    for file in files:
        print(f"{subindent}{file}")

# 2. 使用 pathlib 递归查找
print("\n使用 pathlib 查找:")

# 查找所有 Python 文件
python_files = list(project_root.rglob("*.py"))
print(f"Python 文件 ({len(python_files)} 个):")
for file in python_files:
    print(f"  {file.relative_to(project_root)}")

# 查找所有测试文件
test_files = list(project_root.rglob("test_*.py"))
print(f"\n测试文件 ({len(test_files)} 个):")
for file in test_files:
    print(f"  {file.relative_to(project_root)}")

# 查找配置文件
config_files = list(project_root.rglob("*.json")) + list(project_root.rglob("*.txt"))
print(f"\n配置文件 ({len(config_files)} 个):")
for file in config_files:
    print(f"  {file.relative_to(project_root)}")

# 3. 高级搜索功能
print("\n高级搜索功能:")

def find_files_by_content(root_path, search_text):
    """根据内容搜索文件"""
    matches = []
    for file_path in Path(root_path).rglob("*"):
        if file_path.is_file():
            try:
                content = file_path.read_text(encoding='utf-8')
                if search_text in content:
                    matches.append(file_path)
            except (UnicodeDecodeError, OSError):
                continue
    return matches

# 搜索包含特定内容的文件
search_results = find_files_by_content(project_root, "print")
print(f"包含 'print' 的文件 ({len(search_results)} 个):")
for file in search_results:
    print(f"  {file.relative_to(project_root)}")

# 4. 文件统计
print("\n文件统计:")

all_files = list(project_root.rglob("*"))
total_files = sum(1 for p in all_files if p.is_file())
total_dirs = sum(1 for p in all_files if p.is_dir())
total_size = sum(p.stat().st_size for p in all_files if p.is_file())

print(f"总文件数: {total_files}")
print(f"总目录数: {total_dirs}")
print(f"总大小: {total_size} 字节")

# 按扩展名统计
from collections import Counter
extensions = Counter(p.suffix for p in all_files if p.is_file())
print("按扩展名统计:")
for ext, count in sorted(extensions.items()):
    print(f"  {ext or '无扩展名'}: {count} 个")

# 查找大文件
large_files = [p for p in all_files if p.is_file() and p.stat().st_size > 50]
if large_files:
    print(f"\n大文件 (>50字节):")
    for file in large_files:
        size = file.stat().st_size
        print(f"  {file.relative_to(project_root)} ({size} 字节)")

# 清理
shutil.rmtree(project_root)
```

### 20.3.3 文件权限和安全

```python
# 文件权限和安全
print("文件权限和安全:")

# 创建测试文件
test_file = Path("security_test.txt")
test_file.write_text("这是测试文件\n包含敏感信息\n")

# 1. 查看文件权限
print("文件权限信息:")
stat_info = test_file.stat()
permissions = stat_info.st_mode

print(f"文件: {test_file}")
print(f"权限 (八进制): {oct(permissions)}")
print(f"权限 (符号): {stat.filemode(permissions)}")

# 详细权限分析
import stat

print("权限详情:")
print(f"  所有者可读: {bool(permissions & stat.S_IRUSR)}")
print(f"  所有者可写: {bool(permissions & stat.S_IWUSR)}")
print(f"  所有者可执行: {bool(permissions & stat.S_IXUSR)}")
print(f"  组可读: {bool(permissions & stat.S_IRGRP)}")
print(f"  组可写: {bool(permissions & stat.S_IWGRP)}")
print(f"  组可执行: {bool(permissions & stat.S_IXGRP)}")
print(f"  其他用户可读: {bool(permissions & stat.S_IROTH)}")
print(f"  其他用户可写: {bool(permissions & stat.S_IWOTH)}")
print(f"  其他用户可执行: {bool(permissions & stat.S_IXOTH)}")

# 2. 修改文件权限
print("\n修改文件权限:")

# 使用 os.chmod 修改权限
# 所有者可读写，组和其他用户只读
new_permissions = stat.S_IRUSR | stat.S_IWUSR | stat.S_IRGRP | stat.S_IROTH
os.chmod(test_file, new_permissions)

print(f"修改后权限: {oct(test_file.stat().st_mode)}")

# 使用 pathlib 修改权限
test_file.chmod(0o644)  # rw-r--r--
print(f"pathlib 修改后权限: {oct(test_file.stat().st_mode)}")

# 3. 文件所有者
print("\n文件所有者信息:")
import pwd
import grp

stat_info = test_file.stat()
try:
    owner_name = pwd.getpwuid(stat_info.st_uid).pw_name
    group_name = grp.getgrgid(stat_info.st_gid).gr_name
    print(f"所有者: {owner_name} (UID: {stat_info.st_uid})")
    print(f"组: {group_name} (GID: {stat_info.st_gid})")
except KeyError:
    print("无法获取所有者信息")

# 4. 安全文件操作
print("\n安全文件操作:")

def safe_write_file(file_path, content, permissions=0o600):
    """安全地写入文件"""
    # 创建临时文件
    temp_fd, temp_path = tempfile.mkstemp()
    try:
        # 写入临时文件
        with os.fdopen(temp_fd, 'w') as temp_file:
            temp_file.write(content)

        # 设置临时文件权限
        os.chmod(temp_path, permissions)

        # 原子性替换
        os.rename(temp_path, file_path)

        print(f"安全写入文件: {file_path}")

    except Exception as e:
        # 清理临时文件
        if os.path.exists(temp_path):
            os.unlink(temp_path)
        raise e

# 测试安全写入
safe_write_file("secure_file.txt", "这是安全写入的内容\n", 0o600)

# 验证权限
secure_file = Path("secure_file.txt")
print(f"安全文件权限: {oct(secure_file.stat().st_mode)}")

# 5. 文件完整性检查
print("\n文件完整性检查:")

import hashlib

def calculate_file_hash(file_path, algorithm='sha256'):
    """计算文件哈希值"""
    hash_obj = hashlib.new(algorithm)
    with open(file_path, 'rb') as f:
        while True:
            data = f.read(65536)  # 64KB
            if not data:
                break
            hash_obj.update(data)
    return hash_obj.hexdigest()

# 计算文件哈希
file_hash = calculate_file_hash("secure_file.txt")
print(f"文件哈希 (SHA256): {file_hash}")

# 验证文件完整性
def verify_file_integrity(file_path, expected_hash):
    """验证文件完整性"""
    actual_hash = calculate_file_hash(file_path)
    return actual_hash == expected_hash

is_integrity_ok = verify_file_integrity("secure_file.txt", file_hash)
print(f"完整性检查: {'通过' if is_integrity_ok else '失败'}")

# 6. 清理
print("\n清理测试文件:")
files_to_clean = ["security_test.txt", "secure_file.txt"]
for file_path in files_to_clean:
    if os.path.exists(file_path):
        os.remove(file_path)
        print(f"删除: {file_path}")
```

## 总结

本章介绍了文件系统操作的核心概念：
- 掌握 os 模块的系统信息和环境变量操作
- 熟练使用 os 模块进行目录和文件操作
- 理解 pathlib 模块的现代路径处理方式
- 学会高级的文件和目录操作技巧
- 掌握文件权限和安全处理方法

通过本章的学习，你应该能够：
1. 使用 os 模块进行系统级文件操作
2. 使用 pathlib 模块进行现代化的路径处理
3. 实现安全的文件操作和权限管理
4. 进行复杂的目录遍历和文件搜索
5. 处理文件完整性和安全性问题

下一章将介绍常用模块和数据处理。
