# 第27章：命令行工具

## 27.1 argparse 模块详解

### 27.1.1 argparse 基础

```python
# argparse 模块详解
print("argparse 模块详解:")

import argparse
import sys

# 1. 基本参数解析
print("基本参数解析:")

def basic_parser():
    """基本的参数解析器"""
    parser = argparse.ArgumentParser(description='一个简单的命令行工具')

    # 添加位置参数
    parser.add_argument('filename', help='要处理的文件名')

    # 添加可选参数
    parser.add_argument('-v', '--verbose', action='store_true',
                       help='启用详细输出')
    parser.add_argument('-o', '--output', help='输出文件名')

    # 解析参数
    args = parser.parse_args()

    print(f"输入文件: {args.filename}")
    print(f"详细输出: {args.verbose}")
    print(f"输出文件: {args.output}")

    return args

# 2. 参数类型和验证
print("\n参数类型和验证:")

def typed_parser():
    """带类型验证的参数解析器"""
    parser = argparse.ArgumentParser(description='带类型验证的参数解析器')

    # 不同类型的参数
    parser.add_argument('--count', type=int, default=1,
                       help='重复次数 (默认: 1)')
    parser.add_argument('--delay', type=float, default=0.0,
                       help='延迟时间 (秒)')
    parser.add_argument('--enabled', action='store_true',
                       help='启用功能')
    parser.add_argument('--disabled', action='store_false',
                       help='禁用功能')

    # 文件参数
    parser.add_argument('--input', type=argparse.FileType('r'),
                       help='输入文件')
    parser.add_argument('--output', type=argparse.FileType('w'),
                       help='输出文件')

    # 选择参数
    parser.add_argument('--mode', choices=['fast', 'slow', 'medium'],
                       default='medium', help='处理模式')

    args = parser.parse_args()

    print(f"计数: {args.count} (类型: {type(args.count)})")
    print(f"延迟: {args.delay} (类型: {type(args.delay)})")
    print(f"启用: {args.enabled}")
    print(f"禁用: {args.disabled}")
    print(f"模式: {args.mode}")

    return args

# 3. 参数分组
print("\n参数分组:")

def grouped_parser():
    """分组的参数解析器"""
    parser = argparse.ArgumentParser(description='分组参数示例')

    # 输入选项组
    input_group = parser.add_argument_group('输入选项')
    input_group.add_argument('--input-file', required=True,
                           help='输入文件路径')
    input_group.add_argument('--input-format', choices=['json', 'xml', 'csv'],
                           default='json', help='输入文件格式')

    # 输出选项组
    output_group = parser.add_argument_group('输出选项')
    output_group.add_argument('--output-file', help='输出文件路径')
    output_group.add_argument('--output-format', choices=['json', 'xml', 'csv'],
                            default='json', help='输出文件格式')

    # 处理选项组
    process_group = parser.add_argument_group('处理选项')
    process_group.add_argument('--compress', action='store_true',
                              help='压缩输出')
    process_group.add_argument('--validate', action='store_true',
                              help='验证输入数据')

    args = parser.parse_args()

    print("输入选项:")
    print(f"  文件: {args.input_file}")
    print(f"  格式: {args.input_format}")

    print("输出选项:")
    print(f"  文件: {args.output_file}")
    print(f"  格式: {args.output_format}")

    print("处理选项:")
    print(f"  压缩: {args.compress}")
    print(f"  验证: {args.validate}")

    return args

# 4. 子命令
print("\n子命令:")

def subcommand_parser():
    """子命令解析器"""
    parser = argparse.ArgumentParser(description='多子命令工具')
    subparsers = parser.add_subparsers(dest='command', help='可用命令')

    # 创建子命令
    create_parser = subparsers.add_parser('create', help='创建新项目')
    create_parser.add_argument('name', help='项目名称')
    create_parser.add_argument('--template', default='basic',
                              help='项目模板')

    build_parser = subparsers.add_parser('build', help='构建项目')
    build_parser.add_argument('--target', default='all',
                             help='构建目标')
    build_parser.add_argument('--clean', action='store_true',
                             help='清理构建文件')

    deploy_parser = subparsers.add_parser('deploy', help='部署项目')
    deploy_parser.add_argument('--environment', default='development',
                              choices=['development', 'staging', 'production'],
                              help='部署环境')
    deploy_parser.add_argument('--version', help='版本号')

    args = parser.parse_args()

    if args.command == 'create':
        print(f"创建项目: {args.name}")
        print(f"模板: {args.template}")
    elif args.command == 'build':
        print(f"构建目标: {args.target}")
        print(f"清理: {args.clean}")
    elif args.command == 'deploy':
        print(f"部署环境: {args.environment}")
        print(f"版本: {args.version}")
    else:
        parser.print_help()

    return args

# 5. 互斥参数
print("\n互斥参数:")

def mutually_exclusive_parser():
    """互斥参数解析器"""
    parser = argparse.ArgumentParser(description='互斥参数示例')

    # 基本参数
    parser.add_argument('filename', help='文件名')

    # 互斥组
    group = parser.add_mutually_exclusive_group()
    group.add_argument('--compress', action='store_true',
                      help='压缩文件')
    group.add_argument('--extract', action='store_true',
                      help='解压文件')
    group.add_argument('--list', action='store_true',
                      help='列出文件内容')

    args = parser.parse_args()

    print(f"文件名: {args.filename}")
    print(f"压缩: {args.compress}")
    print(f"解压: {args.extract}")
    print(f"列出: {args.list}")

    return args

# 6. 自定义动作
print("\n自定义动作:")

class UpperAction(argparse.Action):
    """自定义动作：转换为大写"""

    def __call__(self, parser, namespace, values, option_string=None):
        setattr(namespace, self.dest, values.upper())

def custom_action_parser():
    """使用自定义动作的解析器"""
    parser = argparse.ArgumentParser(description='自定义动作示例')

    parser.add_argument('--name', action=UpperAction,
                       help='名称（自动转换为大写）')
    parser.add_argument('--count', type=int, action='append',
                       help='计数（可多次指定）')

    args = parser.parse_args()

    print(f"名称: {args.name}")
    print(f"计数: {args.count}")

    return args

# 7. 帮助和版本信息
print("\n帮助和版本信息:")

def help_version_parser():
    """带帮助和版本信息的解析器"""
    parser = argparse.ArgumentParser(
        description='演示帮助和版本信息',
        epilog='使用示例: python script.py --help'
    )

    parser.add_argument('--version', action='version',
                       version='%(prog)s 1.0.0')

    parser.add_argument('files', nargs='+', help='要处理的文件')
    parser.add_argument('-v', '--verbose', action='count', default=0,
                       help='详细程度（可多次指定）')

    args = parser.parse_args()

    print(f"文件: {args.files}")
    print(f"详细程度: {args.verbose}")

    return args

# 演示函数调用（在实际使用时会解析命令行参数）
print("注意：以下函数在实际运行时会解析命令行参数")
print("这里仅展示结构，不会实际调用")
```

### 27.1.2 argparse 高级用法

```python
# argparse 高级用法
print("argparse 高级用法:")

import argparse
import os
import sys

# 1. 自定义类型转换
print("自定义类型转换:")

def file_path(path):
    """验证文件路径"""
    if not os.path.exists(path):
        raise argparse.ArgumentTypeError(f"文件不存在: {path}")
    return path

def positive_int(value):
    """验证正整数"""
    try:
        ivalue = int(value)
        if ivalue <= 0:
            raise ValueError
        return ivalue
    except ValueError:
        raise argparse.ArgumentTypeError(f"必须是正整数: {value}")

def memory_size(value):
    """解析内存大小 (支持 K, M, G 后缀)"""
    try:
        if value[-1].upper() == 'K':
            return int(value[:-1]) * 1024
        elif value[-1].upper() == 'M':
            return int(value[:-1]) * 1024 * 1024
        elif value[-1].upper() == 'G':
            return int(value[:-1]) * 1024 * 1024 * 1024
        else:
            return int(value)
    except ValueError:
        raise argparse.ArgumentTypeError(f"无效的内存大小: {value}")

def advanced_type_parser():
    """使用自定义类型的解析器"""
    parser = argparse.ArgumentParser(description='自定义类型示例')

    parser.add_argument('--input-file', type=file_path,
                       help='输入文件路径')
    parser.add_argument('--count', type=positive_int,
                       help='正整数计数')
    parser.add_argument('--memory', type=memory_size,
                       help='内存大小 (支持 K/M/G 后缀)')

    args = parser.parse_args()

    print(f"输入文件: {args.input_file}")
    print(f"计数: {args.count}")
    print(f"内存: {args.memory} bytes")

    return args

# 2. 参数默认值和配置
print("\n参数默认值和配置:")

def config_parser():
    """从配置文件读取默认值的解析器"""
    parser = argparse.ArgumentParser(description='配置示例')

    # 从环境变量读取默认值
    parser.add_argument('--host', default=os.environ.get('APP_HOST', 'localhost'),
                       help='服务器主机 (默认: localhost)')
    parser.add_argument('--port', type=int,
                       default=int(os.environ.get('APP_PORT', '8080')),
                       help='服务器端口 (默认: 8080)')

    # 数据库配置
    parser.add_argument('--db-host', default='localhost',
                       help='数据库主机')
    parser.add_argument('--db-port', type=int, default=5432,
                       help='数据库端口')
    parser.add_argument('--db-name', default='myapp',
                       help='数据库名称')

    args = parser.parse_args()

    print("服务器配置:")
    print(f"  主机: {args.host}")
    print(f"  端口: {args.port}")

    print("数据库配置:")
    print(f"  主机: {args.db_host}")
    print(f"  端口: {args.db_port}")
    print(f"  名称: {args.db_name}")

    return args

# 3. 参数验证和冲突检查
print("\n参数验证和冲突检查:")

def validate_args(args):
    """验证参数组合"""
    errors = []

    # 检查端口范围
    if hasattr(args, 'port') and not (1 <= args.port <= 65535):
        errors.append(f"端口必须在 1-65535 之间: {args.port}")

    # 检查文件存在性
    if hasattr(args, 'input_file') and args.input_file:
        if not os.path.exists(args.input_file):
            errors.append(f"输入文件不存在: {args.input_file}")

    # 检查互斥选项
    if hasattr(args, 'compress') and hasattr(args, 'extract'):
        if args.compress and args.extract:
            errors.append("不能同时指定 --compress 和 --extract")

    if errors:
        print("参数错误:")
        for error in errors:
            print(f"  - {error}")
        sys.exit(1)

def validation_parser():
    """带验证的解析器"""
    parser = argparse.ArgumentParser(description='参数验证示例')

    parser.add_argument('--input-file', help='输入文件')
    parser.add_argument('--port', type=int, help='端口号')
    parser.add_argument('--compress', action='store_true', help='压缩')
    parser.add_argument('--extract', action='store_true', help='解压')

    args = parser.parse_args()

    # 验证参数
    validate_args(args)

    print("参数验证通过!")
    return args

# 4. 动态参数
print("\n动态参数:")

def dynamic_parser():
    """动态添加参数的解析器"""
    parser = argparse.ArgumentParser(description='动态参数示例')

    # 基本参数
    parser.add_argument('--mode', choices=['simple', 'advanced'],
                       default='simple', help='模式')

    # 先解析基本参数
    args, remaining = parser.parse_known_args()

    # 根据模式添加额外参数
    if args.mode == 'advanced':
        parser.add_argument('--threads', type=int, default=4,
                           help='线程数')
        parser.add_argument('--memory', type=memory_size,
                           help='内存限制')
        parser.add_argument('--debug', action='store_true',
                           help='调试模式')

    # 重新解析所有参数
    args = parser.parse_args()

    print(f"模式: {args.mode}")
    if hasattr(args, 'threads'):
        print(f"线程数: {args.threads}")
    if hasattr(args, 'memory'):
        print(f"内存: {args.memory}")
    if hasattr(args, 'debug'):
        print(f"调试: {args.debug}")

    return args

# 5. 解析配置文件
print("\n解析配置文件:")

import configparser

def load_config(config_file):
    """加载配置文件"""
    config = configparser.ConfigParser()
    config.read(config_file)

    defaults = {}
    if 'DEFAULT' in config:
        defaults.update(dict(config['DEFAULT']))

    return defaults

def config_file_parser():
    """支持配置文件的解析器"""
    parser = argparse.ArgumentParser(description='配置文件示例')

    parser.add_argument('--config', help='配置文件路径')
    parser.add_argument('--host', help='服务器主机')
    parser.add_argument('--port', type=int, help='服务器端口')

    # 先解析配置文件参数
    args, remaining = parser.parse_known_args()

    # 加载配置文件
    if args.config and os.path.exists(args.config):
        defaults = load_config(args.config)
        parser.set_defaults(**defaults)

    # 重新解析所有参数（命令行参数会覆盖配置文件）
    args = parser.parse_args()

    print(f"主机: {args.host}")
    print(f"端口: {args.port}")
    print(f"配置文件: {args.config}")

    return args

# 6. 自定义帮助格式
print("\n自定义帮助格式:")

class CustomHelpFormatter(argparse.RawDescriptionHelpFormatter):
    """自定义帮助格式"""

    def add_usage(self, usage, actions, groups, prefix=None):
        if prefix is None:
            prefix = '用法: '
        return super().add_usage(usage, actions, groups, prefix)

    def add_argument(self, action):
        if action.help and action.help.startswith('HIDDEN'):
            return  # 隐藏参数
        return super().add_argument(action)

def custom_formatter_parser():
    """使用自定义格式器的解析器"""
    parser = argparse.ArgumentParser(
        description='自定义帮助格式示例',
        formatter_class=CustomHelpFormatter,
        epilog='''
使用示例:
  %(prog)s --input file.txt --output result.txt
  %(prog)s --config config.ini
        '''
    )

    parser.add_argument('--input', help='输入文件')
    parser.add_argument('--output', help='输出文件')
    parser.add_argument('--config', help='配置文件')
    parser.add_argument('--hidden', help='HIDDEN隐藏参数')

    args = parser.parse_args()

    print(f"输入: {args.input}")
    print(f"输出: {args.output}")
    print(f"配置: {args.config}")

    return args

# 7. 错误处理
print("\n错误处理:")

def error_handling_parser():
    """带错误处理的解析器"""
    parser = argparse.ArgumentParser(description='错误处理示例')

    try:
        parser.add_argument('--count', type=int, required=True,
                           help='计数')
        parser.add_argument('--name', required=True,
                           help='名称')

        args = parser.parse_args()

        print(f"计数: {args.count}")
        print(f"名称: {args.name}")

        return args

    except argparse.ArgumentError as e:
        print(f"参数错误: {e}")
        parser.print_help()
        sys.exit(1)
    except SystemExit:
        # 处理 -h/--help
        pass

print("注意：错误处理示例需要实际运行时测试")
```

## 27.2 Click 框架

### 27.2.1 Click 基础

```python
# Click 框架
print("Click 框架:")

# 注意：需要安装 click: pip install click
try:
    import click
except ImportError:
    print("需要安装 click: pip install click")
    click = None

if click:
    # 1. 基本命令
    print("基本命令:")

    @click.command()
    @click.option('--count', default=1, help='重复次数')
    @click.option('--name', prompt='你的名字', help='姓名')
    def hello(count, name):
        """简单的问候命令"""
        for _ in range(count):
            click.echo(f"Hello, {name}!")

    # 2. 选项和参数
    print("\n选项和参数:")

    @click.command()
    @click.argument('filename')
    @click.option('--output', '-o', help='输出文件')
    @click.option('--verbose', '-v', is_flag=True, help='详细输出')
    @click.option('--format', type=click.Choice(['json', 'xml', 'csv']),
                  default='json', help='输出格式')
    def process_file(filename, output, verbose, format):
        """处理文件命令"""
        if verbose:
            click.echo(f"处理文件: {filename}")
            click.echo(f"输出格式: {format}")

        if output:
            click.echo(f"输出到: {output}")
        else:
            click.echo("输出到标准输出")

    # 3. 命令分组
    print("\n命令分组:")

    @click.group()
    def cli():
        """命令行工具组"""
        pass

    @cli.command()
    @click.argument('name')
    def create(name):
        """创建项目"""
        click.echo(f"创建项目: {name}")

    @cli.command()
    @click.option('--target', default='all')
    def build(target):
        """构建项目"""
        click.echo(f"构建目标: {target}")

    @cli.command()
    @click.option('--env', default='development')
    def deploy(env):
        """部署项目"""
        click.echo(f"部署到环境: {env}")

    # 4. 上下文和状态
    print("\n上下文和状态:")

    class AppContext:
        """应用上下文"""

        def __init__(self):
            self.config = {}
            self.verbose = False

        def set_config(self, key, value):
            self.config[key] = value

        def get_config(self, key, default=None):
            return self.config.get(key, default)

    pass_context = click.make_pass_decorator(AppContext)

    @click.group()
    @click.option('--config', help='配置文件')
    @click.option('--verbose', is_flag=True)
    @click.pass_context
    def app(ctx, config, verbose):
        """应用命令组"""
        ctx.obj = AppContext()
        ctx.obj.verbose = verbose

        if config:
            # 加载配置文件
            ctx.obj.set_config('config_file', config)

        if ctx.obj.verbose:
            click.echo("详细模式已启用")

    @app.command()
    @pass_context
    def status(ctx):
        """显示状态"""
        click.echo("应用状态:")
        click.echo(f"  配置文件: {ctx.obj.get_config('config_file', '无')}")
        click.echo(f"  详细模式: {ctx.obj.verbose}")

    @app.command()
    @click.argument('key')
    @click.argument('value')
    @pass_context
    def set_config(ctx, key, value):
        """设置配置"""
        ctx.obj.set_config(key, value)
        click.echo(f"已设置 {key} = {value}")

    # 5. 进度条和颜色
    print("\n进度条和颜色:")

    @click.command()
    @click.option('--count', default=100)
    def progress_demo(count):
        """进度条演示"""
        with click.progressbar(range(count)) as bar:
            for item in bar:
                # 模拟工作
                pass

        click.secho("完成!", fg='green', bold=True)

    @click.command()
    def color_demo():
        """颜色演示"""
        click.secho("红色文本", fg='red')
        click.secho("绿色文本", fg='green')
        click.secho("蓝色文本", fg='blue')
        click.secho("黄色背景", bg='yellow')
        click.secho("粗体文本", bold=True)
        click.secho("下划线文本", underline=True)

    # 6. 表格输出
    print("\n表格输出:")

    @click.command()
    def table_demo():
        """表格演示"""
        from click import style

        data = [
            ["Alice", 30, "Engineer"],
            ["Bob", 25, "Designer"],
            ["Charlie", 35, "Manager"]
        ]

        # 打印表头
        click.echo(f"{'Name':<10} {'Age':<5} {'Role':<10}")
        click.echo("-" * 25)

        # 打印数据
        for name, age, role in data:
            click.echo(f"{name:<10} {age:<5} {role:<10}")

    # 7. 确认和提示
    print("\n确认和提示:")

    @click.command()
    @click.confirmation_option(prompt='确定要继续吗？')
    def dangerous_operation():
        """危险操作"""
        click.echo("执行危险操作...")

    @click.command()
    @click.option('--name', prompt='请输入你的名字')
    @click.option('--age', prompt='请输入你的年龄', type=int)
    @click.password_option()
    def user_input(name, age, password):
        """用户输入演示"""
        click.echo(f"名字: {name}")
        click.echo(f"年龄: {age}")
        click.echo(f"密码长度: {len(password)}")

    print("Click 示例代码已定义")
    print("注意：需要在安装了 click 的环境中运行")

else:
    print("Click 未安装，跳过相关示例")
```

### 27.2.2 Click 高级用法

```python
# Click 高级用法
print("Click 高级用法:")

try:
    import click
except ImportError:
    print("需要安装 click")
    click = None

if click:
    # 1. 自定义类型
    print("自定义类型:")

    class EmailType(click.ParamType):
        """邮箱类型"""

        name = "email"

        def convert(self, value, param, ctx):
            if "@" not in value:
                self.fail(f"{value} 不是有效的邮箱地址", param, ctx)
            return value

    EMAIL = EmailType()

    @click.command()
    @click.option('--email', type=EMAIL)
    def send_email(email):
        """发送邮件"""
        click.echo(f"发送邮件到: {email}")

    # 2. 回调和验证
    print("\n回调和验证:")

    def validate_port(ctx, param, value):
        """验证端口号"""
        if not (1 <= value <= 65535):
            raise click.BadParameter(f"端口必须在 1-65535 之间: {value}")
        return value

    def validate_file(ctx, param, value):
        """验证文件存在"""
        import os
        if not os.path.exists(value):
            raise click.BadParameter(f"文件不存在: {value}")
        return value

    @click.command()
    @click.option('--port', callback=validate_port, type=int)
    @click.option('--config', callback=validate_file)
    def server(port, config):
        """启动服务器"""
        click.echo(f"服务器端口: {port}")
        click.echo(f"配置文件: {config}")

    # 3. 插件系统
    print("\n插件系统:")

    class Plugin:
        """插件基类"""

        def get_commands(self):
            """返回插件提供的命令"""
            return {}

    class DatabasePlugin(Plugin):
        """数据库插件"""

        def get_commands(self):
            @click.command()
            def db_status():
                """数据库状态"""
                click.echo("数据库运行正常")

            @click.command()
            @click.option('--table')
            def db_backup(table):
                """数据库备份"""
                click.echo(f"备份表: {table}")

            return {
                'db-status': db_status,
                'db-backup': db_backup
            }

    @click.group()
    def main_cli():
        """主命令行工具"""
        pass

    # 注册插件命令
    db_plugin = DatabasePlugin()
    for name, command in db_plugin.get_commands().items():
        main_cli.add_command(command, name=name)

    # 4. 异步命令
    print("\n异步命令:")

    import asyncio

    def coro(f):
        """协程装饰器"""
        f = asyncio.coroutine(f)
        def wrapper(*args, **kwargs):
            loop = asyncio.get_event_loop()
            return loop.run_until_complete(f(*args, **kwargs))
        return wrapper

    @click.command()
    @coro
    async def async_command():
        """异步命令"""
        click.echo("开始异步操作...")
        await asyncio.sleep(1)
        click.echo("异步操作完成!")

    # 5. 自定义异常处理
    print("\n自定义异常处理:")

    class AppError(click.ClickException):
        """应用异常"""

        def __init__(self, message):
            super().__init__(message)
            self.message = message

    @click.command()
    @click.option('--fail', is_flag=True)
    def robust_command(fail):
        """健壮的命令"""
        try:
            if fail:
                raise AppError("操作失败")
            click.echo("操作成功")
        except AppError as e:
            click.echo(f"错误: {e.message}", err=True)
            raise click.Abort()

    # 6. 配置文件集成
    print("\n配置文件集成:")

    import configparser
    import os

    class Config:
        """配置类"""

        def __init__(self):
            self.data = {}

        def load_from_file(self, filename):
            """从文件加载配置"""
            if os.path.exists(filename):
                config = configparser.ConfigParser()
                config.read(filename)
                for section in config.sections():
                    for key, value in config[section].items():
                        self.data[f"{section}.{key}"] = value

        def get(self, key, default=None):
            """获取配置值"""
            return self.data.get(key, default)

    def load_config(ctx, param, value):
        """加载配置回调"""
        config = Config()
        if value:
            config.load_from_file(value)
        ctx.obj = config
        return value

    @click.group()
    @click.option('--config', callback=load_config, expose_value=False)
    @click.pass_context
    def config_cli(ctx):
        """支持配置文件的命令行工具"""
        pass

    @config_cli.command()
    @click.pass_context
    def show_config(ctx):
        """显示配置"""
        config = ctx.obj
        if config.data:
            click.echo("当前配置:")
            for key, value in config.data.items():
                click.echo(f"  {key}: {value}")
        else:
            click.echo("无配置文件")

    # 7. 国际化支持
    print("\n国际化支持:")

    import gettext

    # 设置语言环境
    try:
        lang = gettext.translation('messages', localedir='locale', languages=['zh'])
        lang.install()
        _ = lang.gettext
    except FileNotFoundError:
        _ = lambda x: x  # 如果没有翻译文件，使用原文

    @click.command()
    @click.option('--name', prompt=_('请输入你的名字'))
    def i18n_command(name):
        """国际化命令"""
        click.echo(_("你好, {}!").format(name))

    # 8. 测试支持
    print("\n测试支持:")

    from click.testing import CliRunner

    def test_command():
        """测试命令"""

        @click.command()
        @click.argument('name')
        def hello(name):
            click.echo(f"Hello, {name}!")

        runner = CliRunner()
        result = runner.invoke(hello, ['World'])

        assert result.exit_code == 0
        assert 'Hello, World!' in result.output
        print("测试通过!")

    print("Click 高级用法示例已定义")
    print("注意：需要在安装了 click 的环境中运行")

else:
    print("Click 未安装，跳过高级用法示例")
```

## 27.3 Typer 框架

### 27.3.1 Typer 基础

```python
# Typer 框架
print("Typer 框架:")

# 注意：需要安装 typer: pip install typer
try:
    import typer
except ImportError:
    print("需要安装 typer: pip install typer")
    typer = None

if typer:
    # 1. 基本应用
    print("基本应用:")

    app = typer.Typer()

    @app.command()
    def hello(name: str):
        """问候命令"""
        typer.echo(f"Hello, {name}!")

    @app.command()
    def goodbye(name: str, formal: bool = False):
        """告别命令"""
        if formal:
            typer.echo(f"Goodbye, {name}. Have a nice day!")
        else:
            typer.echo(f"Bye, {name}!")

    # 2. 参数类型
    print("\n参数类型:")

    from typing import List, Optional
    from pathlib import Path

    app2 = typer.Typer()

    @app2.command()
    def process_files(
        files: List[Path],
        output: Optional[Path] = None,
        verbose: bool = False
    ):
        """处理文件列表"""
        typer.echo(f"处理 {len(files)} 个文件")

        for file in files:
            if verbose:
                typer.echo(f"处理文件: {file}")

        if output:
            typer.echo(f"输出到: {output}")

    # 3. 选项和参数
    print("\n选项和参数:")

    app3 = typer.Typer()

    @app3.command()
    def create_user(
        username: str = typer.Argument(..., help="用户名"),
        email: str = typer.Option(..., help="邮箱地址"),
        age: int = typer.Option(18, help="年龄"),
        active: bool = typer.Option(True, help="是否激活")
    ):
        """创建用户"""
        typer.echo(f"创建用户: {username}")
        typer.echo(f"邮箱: {email}")
        typer.echo(f"年龄: {age}")
        typer.echo(f"激活: {active}")

    # 4. 子命令
    print("\n子命令:")

    users_app = typer.Typer()
    app4 = typer.Typer()
    app4.add_typer(users_app, name="users")

    @users_app.command("create")
    def users_create(name: str):
        """创建用户"""
        typer.echo(f"创建用户: {name}")

    @users_app.command("list")
    def users_list():
        """列出用户"""
        typer.echo("用户列表:")
        typer.echo("  - Alice")
        typer.echo("  - Bob")

    # 5. 回调和钩子
    print("\n回调和钩子:")

    app5 = typer.Typer()

    @app5.callback()
    def main_callback(verbose: bool = False):
        """主回调"""
        if verbose:
            typer.echo("详细模式已启用")

    @app5.command()
    def subcommand(name: str):
        """子命令"""
        typer.echo(f"执行子命令: {name}")

    # 6. 错误处理
    print("\n错误处理:")

    app6 = typer.Typer()

    @app6.command()
    def divide(a: float, b: float):
        """除法运算"""
        try:
            result = a / b
            typer.echo(f"结果: {result}")
        except ZeroDivisionError:
            typer.echo("错误: 除数不能为零", err=True)
            raise typer.Exit(1)

    # 7. 进度条
    print("\n进度条:")

    from rich.progress import Progress, SpinnerColumn, TextColumn
    import time

    app7 = typer.Typer()

    @app7.command()
    def long_task(count: int = 100):
        """长时间任务"""
        with Progress(
            SpinnerColumn(),
            TextColumn("[progress.description]{task.description}"),
            transient=True,
        ) as progress:
            task = progress.add_task("处理中...", total=count)

            for i in range(count):
                time.sleep(0.01)
                progress.update(task, advance=1)

        typer.echo("任务完成!")

    # 8. 表格输出
    print("\n表格输出:")

    from rich.table import Table
    from rich.console import Console

    app8 = typer.Typer()

    @app8.command()
    def show_table():
        """显示表格"""
        console = Console()
        table = Table("Name", "Age", "City")

        data = [
            ("Alice", 30, "Beijing"),
            ("Bob", 25, "Shanghai"),
            ("Charlie", 35, "Shenzhen")
        ]

        for name, age, city in data:
            table.add_row(name, str(age), city)

        console.print(table)

    print("Typer 示例代码已定义")
    print("注意：需要在安装了 typer 的环境中运行")

else:
    print("Typer 未安装，跳过相关示例")
```

### 27.3.2 Typer 高级用法

```python
# Typer 高级用法
print("Typer 高级用法:")

try:
    import typer
    from typing import List, Optional, Dict, Any
    from pathlib import Path
    import json
except ImportError:
    print("需要安装相关包")
    typer = None

if typer:
    # 1. 依赖注入
    print("依赖注入:")

    from dataclasses import dataclass

    @dataclass
    class Database:
        host: str
        port: int
        name: str

    @dataclass
    class Config:
        database: Database
        debug: bool

    def get_config() -> Config:
        """获取配置"""
        return Config(
            database=Database(host="localhost", port=5432, name="myapp"),
            debug=True
        )

    app = typer.Typer()

    @app.command()
    def connect(config: Config = typer.Depends(get_config)):
        """连接数据库"""
        typer.echo(f"连接到数据库: {config.database.host}:{config.database.port}")
        typer.echo(f"数据库名: {config.database.name}")
        typer.echo(f"调试模式: {config.debug}")

    # 2. 自定义类型
    print("\n自定义类型:")

    class Email(str):
        """邮箱类型"""

        @classmethod
        def __get_validators__(cls):
            yield cls.validate

        @classmethod
        def validate(cls, v):
            if "@" not in v:
                raise ValueError("无效的邮箱地址")
            return cls(v)

        def __repr__(self):
            return f"Email({super().__repr__()})"

    app2 = typer.Typer()

    @app2.command()
    def send_email(to: Email, subject: str, body: str):
        """发送邮件"""
        typer.echo(f"发送邮件到: {to}")
        typer.echo(f"主题: {subject}")
        typer.echo(f"内容: {body[:50]}...")

    # 3. 文件和目录处理
    print("\n文件和目录处理:")

    app3 = typer.Typer()

    @app3.command()
    def process_directory(
        directory: Path = typer.Argument(..., exists=True, dir_okay=True, readable=True),
        pattern: str = "*.txt",
        recursive: bool = False
    ):
        """处理目录"""
        typer.echo(f"处理目录: {directory}")
        typer.echo(f"文件模式: {pattern}")
        typer.echo(f"递归: {recursive}")

        # 模拟文件处理
        files = list(directory.glob(pattern))
        typer.echo(f"找到 {len(files)} 个文件")

    # 4. 环境变量
    print("\n环境变量:")

    import os

    app4 = typer.Typer()

    @app4.command()
    def server(
        host: str = typer.Option("localhost", envvar="APP_HOST"),
        port: int = typer.Option(8080, envvar="APP_PORT"),
        debug: bool = typer.Option(False, envvar="APP_DEBUG")
    ):
        """启动服务器"""
        typer.echo(f"服务器配置:")
        typer.echo(f"  主机: {host}")
        typer.echo(f"  端口: {port}")
        typer.echo(f"  调试: {debug}")

    # 5. 自动补全
    print("\n自动补全:")

    def complete_username(incomplete: str):
        """用户名自动补全"""
        users = ["alice", "bob", "charlie", "david"]
        return [user for user in users if user.startswith(incomplete)]

    app5 = typer.Typer()

    @app5.command()
    def greet_user(
        username: str = typer.Argument(..., autocompletion=complete_username)
    ):
        """问候用户"""
        typer.echo(f"Hello, {username}!")

    # 6. 颜色和样式
    print("\n颜色和样式:")

    app6 = typer.Typer()

    @app6.command()
    def styled_output():
        """样式化输出"""
        typer.secho("红色文本", fg=typer.colors.RED)
        typer.secho("绿色文本", fg=typer.colors.GREEN)
        typer.secho("蓝色文本", fg=typer.colors.BLUE)
        typer.secho("粗体文本", bold=True)
        typer.secho("下划线文本", underline=True)
        typer.secho("黄色背景", bg=typer.colors.YELLOW)

    # 7. 交互式输入
    print("\n交互式输入:")

    app7 = typer.Typer()

    @app7.command()
    def interactive_form():
        """交互式表单"""
        name = typer.prompt("你的名字")
        age = typer.prompt("你的年龄", type=int)
        email = typer.prompt("你的邮箱")

        typer.echo(f"\n用户信息:")
        typer.echo(f"  名字: {name}")
        typer.echo(f"  年龄: {age}")
        typer.echo(f"  邮箱: {email}")

    # 8. 配置管理
    print("\n配置管理:")

    class Settings:
        """应用设置"""

        def __init__(self):
            self.data = {}

        def load_from_file(self, path: Path):
            """从文件加载设置"""
            if path.exists():
                with open(path, 'r') as f:
                    self.data = json.load(f)

        def save_to_file(self, path: Path):
            """保存设置到文件"""
            with open(path, 'w') as f:
                json.dump(self.data, f, indent=2)

        def get(self, key: str, default: Any = None):
            """获取设置值"""
            return self.data.get(key, default)

        def set(self, key: str, value: Any):
            """设置值"""
            self.data[key] = value

    settings = Settings()

    app8 = typer.Typer()

    @app8.callback()
    def load_settings(config: Optional[Path] = None):
        """加载设置"""
        if config:
            settings.load_from_file(config)
            typer.echo(f"已加载配置: {config}")

    @app8.command()
    def get_setting(key: str):
        """获取设置"""
        value = settings.get(key, "未设置")
        typer.echo(f"{key}: {value}")

    @app8.command()
    def set_setting(key: str, value: str):
        """设置值"""
        settings.set(key, value)
        typer.echo(f"已设置 {key} = {value}")

    @app8.command()
    def save_settings(path: Path):
        """保存设置"""
        settings.save_to_file(path)
        typer.echo(f"已保存配置到: {path}")

    # 9. 异步支持
    print("\n异步支持:")

    import asyncio

    app9 = typer.Typer()

    @app9.command()
    async def async_task(duration: int = 5):
        """异步任务"""
        typer.echo(f"开始异步任务，持续 {duration} 秒...")

        for i in range(duration):
            typer.echo(f"进度: {i + 1}/{duration}")
            await asyncio.sleep(1)

        typer.echo("异步任务完成!")

    # 10. 测试支持
    print("\n测试支持:")

    from typer.testing import CliRunner

    def test_app():
        """测试应用"""

        test_app = typer.Typer()

        @test_app.command()
        def add(a: int, b: int):
            """加法"""
            typer.echo(a + b)

        runner = CliRunner()

        # 测试正常情况
        result = runner.invoke(test_app, ["add", "3", "4"])
        assert result.exit_code == 0
        assert "7" in result.output

        # 测试错误情况
        result = runner.invoke(test_app, ["add", "abc", "4"])
        assert result.exit_code != 0

        typer.echo("所有测试通过!")

    print("Typer 高级用法示例已定义")
    print("注意：需要在安装了相关包的环境中运行")

else:
    print("相关包未安装，跳过高级用法示例")
```

## 27.4 命令行工具开发最佳实践

### 27.4.1 设计原则

```python
# 命令行工具开发最佳实践
print("命令行工具开发最佳实践:")

import argparse
import sys
import os
from pathlib import Path
import logging
from typing import List, Optional

# 1. 统一的错误处理
print("统一的错误处理:")

class CLIError(Exception):
    """CLI 错误基类"""
    pass

class ValidationError(CLIError):
    """验证错误"""
    pass

class FileError(CLIError):
    """文件错误"""
    pass

def handle_error(error: Exception) -> int:
    """统一错误处理"""
    if isinstance(error, ValidationError):
        print(f"验证错误: {error}", file=sys.stderr)
        return 1
    elif isinstance(error, FileError):
        print(f"文件错误: {error}", file=sys.stderr)
        return 2
    elif isinstance(error, CLIError):
        print(f"CLI 错误: {error}", file=sys.stderr)
        return 3
    else:
        print(f"意外错误: {error}", file=sys.stderr)
        return 99

# 2. 配置管理
print("\n配置管理:")

class AppConfig:
    """应用配置"""

    def __init__(self):
        self.verbose = False
        self.log_level = logging.INFO
        self.config_file = None

    def load_from_args(self, args):
        """从命令行参数加载配置"""
        self.verbose = getattr(args, 'verbose', False)
        self.config_file = getattr(args, 'config', None)

        if self.verbose:
            self.log_level = logging.DEBUG

    def setup_logging(self):
        """设置日志"""
        logging.basicConfig(
            level=self.log_level,
            format='%(asctime)s - %(levelname)s - %(message)s'
        )

# 3. 参数验证
print("\n参数验证:")

def validate_file_exists(path: str) -> str:
    """验证文件存在"""
    if not os.path.exists(path):
        raise FileError(f"文件不存在: {path}")
    if not os.path.isfile(path):
        raise FileError(f"路径不是文件: {path}")
    return path

def validate_directory(path: str) -> str:
    """验证目录存在"""
    if not os.path.exists(path):
        raise FileError(f"目录不存在: {path}")
    if not os.path.isdir(path):
        raise FileError(f"路径不是目录: {path}")
    return path

def validate_port(port: int) -> int:
    """验证端口号"""
    if not (1 <= port <= 65535):
        raise ValidationError(f"端口号必须在 1-65535 之间: {port}")
    return port

# 4. 命令行工具基类
print("\n命令行工具基类:")

class BaseCLI:
    """命令行工具基类"""

    def __init__(self, name: str, version: str = "1.0.0"):
        self.name = name
        self.version = version
        self.config = AppConfig()

    def create_parser(self) -> argparse.ArgumentParser:
        """创建参数解析器"""
        parser = argparse.ArgumentParser(
            prog=self.name,
            description=f"{self.name} - 命令行工具"
        )

        # 全局选项
        parser.add_argument(
            '--version', action='version',
            version=f'{self.name} {self.version}'
        )
        parser.add_argument(
            '--verbose', '-v', action='store_true',
            help='启用详细输出'
        )
        parser.add_argument(
            '--config', type=str,
            help='配置文件路径'
        )

        return parser

    def setup(self, args):
        """设置应用"""
        self.config.load_from_args(args)
        self.config.setup_logging()

    def run(self, args=None) -> int:
        """运行应用"""
        if args is None:
            args = sys.argv[1:]

        try:
            parser = self.create_parser()
            parsed_args = parser.parse_args(args)

            self.setup(parsed_args)

            return self.execute(parsed_args)

        except CLIError as e:
            return handle_error(e)
        except KeyboardInterrupt:
            print("\n用户中断", file=sys.stderr)
            return 130
        except Exception as e:
            return handle_error(e)

    def execute(self, args) -> int:
        """执行具体命令（子类实现）"""
        raise NotImplementedError("子类必须实现 execute 方法")

# 5. 文件处理工具示例
print("\n文件处理工具示例:")

class FileProcessor(BaseCLI):
    """文件处理工具"""

    def create_parser(self):
        parser = super().create_parser()

        # 文件处理选项
        parser.add_argument(
            'input_file', type=validate_file_exists,
            help='输入文件路径'
        )
        parser.add_argument(
            '--output', '-o', type=str,
            help='输出文件路径'
        )
        parser.add_argument(
            '--format', choices=['json', 'xml', 'csv'],
            default='json', help='输出格式'
        )

        return parser

    def execute(self, args) -> int:
        """执行文件处理"""
        input_file = args.input_file
        output_file = args.output or f"{os.path.splitext(input_file)[0]}_processed.{args.format}"

        logging.info(f"处理文件: {input_file}")
        logging.info(f"输出文件: {output_file}")
        logging.info(f"输出格式: {args.format}")

        # 模拟文件处理
        try:
            with open(input_file, 'r', encoding='utf-8') as f:
                content = f.read()

            # 简单的处理：转换为大写
            processed_content = content.upper()

            with open(output_file, 'w', encoding='utf-8') as f:
                f.write(processed_content)

            logging.info("文件处理完成")
            return 0

        except Exception as e:
            raise FileError(f"文件处理失败: {e}")

# 6. 服务器工具示例
print("\n服务器工具示例:")

class ServerCLI(BaseCLI):
    """服务器工具"""

    def create_parser(self):
        parser = super().create_parser()

        # 服务器选项
        parser.add_argument(
            '--host', default='localhost',
            help='服务器主机 (默认: localhost)'
        )
        parser.add_argument(
            '--port', type=validate_port, default=8080,
            help='服务器端口 (默认: 8080)'
        )
        parser.add_argument(
            '--workers', type=int, default=4,
            help='工作进程数 (默认: 4)'
        )

        return parser

    def execute(self, args) -> int:
        """启动服务器"""
        logging.info(f"启动服务器: {args.host}:{args.port}")
        logging.info(f"工作进程数: {args.workers}")

        # 模拟服务器启动
        try:
            # 这里可以启动实际的服务器
            logging.info("服务器启动成功")
            logging.info("按 Ctrl+C 停止服务器")

            # 简单的服务器循环
            import time
            while True:
                time.sleep(1)

        except KeyboardInterrupt:
            logging.info("服务器已停止")
            return 0
        except Exception as e:
            logging.error(f"服务器启动失败: {e}")
            return 1

# 7. 插件系统
print("\n插件系统:")

class PluginManager:
    """插件管理器"""

    def __init__(self):
        self.plugins = {}

    def register_plugin(self, name: str, plugin_class):
        """注册插件"""
        self.plugins[name] = plugin_class

    def get_plugin(self, name: str):
        """获取插件"""
        return self.plugins.get(name)

    def list_plugins(self) -> List[str]:
        """列出所有插件"""
        return list(self.plugins.keys())

# 8. 命令行工具工厂
print("\n命令行工具工厂:")

class CLIFactory:
    """CLI 工厂"""

    def __init__(self):
        self.tools = {}

    def register_tool(self, name: str, tool_class):
        """注册工具"""
        self.tools[name] = tool_class

    def create_tool(self, name: str, **kwargs):
        """创建工具实例"""
        tool_class = self.tools.get(name)
        if tool_class:
            return tool_class(name, **kwargs)
        return None

    def list_tools(self) -> List[str]:
        """列出所有工具"""
        return list(self.tools.keys())

# 9. 完整的应用示例
print("\n完整的应用示例:")

def main():
    """主函数"""
    # 创建工具工厂
    factory = CLIFactory()

    # 注册工具
    factory.register_tool('file', FileProcessor)
    factory.register_tool('server', ServerCLI)

    # 解析全局参数
    global_parser = argparse.ArgumentParser(description='多功能命令行工具')
    global_parser.add_argument('tool', choices=factory.list_tools(),
                              help='要使用的工具')
    global_parser.add_argument('args', nargs=argparse.REMAINDER,
                              help='传递给工具的参数')

    global_args = global_parser.parse_args()

    # 创建并运行工具
    tool = factory.create_tool(global_args.tool)
    if tool:
        return tool.run(global_args.args)
    else:
        print(f"未知工具: {global_args.tool}", file=sys.stderr)
        return 1

# 演示代码
print("命令行工具开发最佳实践示例已定义")
print("这些示例展示了:")
print("1. 统一的错误处理机制")
print("2. 配置管理和日志设置")
print("3. 参数验证函数")
print("4. 可扩展的基类设计")
print("5. 具体的工具实现示例")
print("6. 插件系统架构")
print("7. 工厂模式的应用")
```

### 27.4.2 测试和分发

```python
# 测试和分发
print("测试和分发:")

import subprocess
import sys
from pathlib import Path
import argparse

# 1. 单元测试
print("单元测试:")

def test_argument_parsing():
    """测试参数解析"""
    parser = argparse.ArgumentParser()
    parser.add_argument('--count', type=int, default=1)
    parser.add_argument('--name', required=True)

    # 测试正常情况
    args = parser.parse_args(['--name', 'test', '--count', '5'])
    assert args.name == 'test'
    assert args.count == 5
    print("✓ 参数解析测试通过")

    # 测试默认值
    args = parser.parse_args(['--name', 'test'])
    assert args.name == 'test'
    assert args.count == 1
    print("✓ 默认值测试通过")

def test_validation():
    """测试验证函数"""
    from cli_best_practices import validate_port, ValidationError

    # 测试有效端口
    assert validate_port(8080) == 8080
    assert validate_port(1) == 1
    assert validate_port(65535) == 65535
    print("✓ 有效端口测试通过")

    # 测试无效端口
    try:
        validate_port(0)
        assert False, "应该抛出异常"
    except ValidationError:
        pass

    try:
        validate_port(70000)
        assert False, "应该抛出异常"
    except ValidationError:
        pass

    print("✓ 无效端口测试通过")

# 2. 集成测试
print("\n集成测试:")

def test_cli_tool():
    """测试 CLI 工具"""
    from cli_best_practices import FileProcessor

    # 创建测试文件
    test_file = Path('test_input.txt')
    test_file.write_text('Hello, World!')

    # 创建工具实例
    tool = FileProcessor('test_processor')

    # 模拟命令行参数
    test_args = ['test_input.txt', '--output', 'test_output.txt']

    try:
        result = tool.run(test_args)
        assert result == 0

        # 检查输出文件
        output_file = Path('test_output.txt')
        assert output_file.exists()
        content = output_file.read_text()
        assert content == 'HELLO, WORLD!'

        print("✓ CLI 工具集成测试通过")

    finally:
        # 清理测试文件
        test_file.unlink(missing_ok=True)
        Path('test_output.txt').unlink(missing_ok=True)

# 3. 端到端测试
print("\n端到端测试:")

def test_end_to_end():
    """端到端测试"""
    # 测试完整的命令行调用
    result = subprocess.run([
        sys.executable, '-c',
        '''
import sys
sys.path.insert(0, ".")
from cli_best_practices import main
sys.argv = ["test", "file", "test_input.txt", "--output", "test_output.txt"]
exit(main())
        '''
    ], capture_output=True, text=True, cwd='.')

    print(f"退出码: {result.returncode}")
    print(f"标准输出: {result.stdout}")
    print(f"标准错误: {result.stderr}")

    # 这里可以添加更详细的断言
    print("✓ 端到端测试完成")

# 4. 打包和分发
print("\n打包和分发:")

def create_setup_py():
    """创建 setup.py 文件"""
    setup_content = '''
from setuptools import setup, find_packages

with open("README.md", "r", encoding="utf-8") as fh:
    long_description = fh.read()

setup(
    name="my-cli-tool",
    version="1.0.0",
    author="Your Name",
    author_email="your.email@example.com",
    description="一个强大的命令行工具",
    long_description=long_description,
    long_description_content_type="text/markdown",
    url="https://github.com/yourusername/my-cli-tool",
    packages=find_packages(),
    classifiers=[
        "Development Status :: 4 - Beta",
        "Intended Audience :: Developers",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
        "Programming Language :: Python :: 3",
        "Programming Language :: Python :: 3.8",
        "Programming Language :: Python :: 3.9",
        "Programming Language :: Python :: 3.10",
        "Programming Language :: Python :: 3.11",
    ],
    python_requires=">=3.8",
    install_requires=[
        "click>=8.0.0",
        "rich>=10.0.0",
    ],
    extras_require={
        "dev": [
            "pytest>=6.0.0",
            "black>=21.0.0",
            "flake8>=3.9.0",
        ],
    },
    entry_points={
        "console_scripts": [
            "mytool=my_cli_tool.cli:main",
        ],
    },
)
'''
    with open('setup.py', 'w', encoding='utf-8') as f:
        f.write(setup_content)
    print("✓ setup.py 已创建")

def create_pyproject_toml():
    """创建 pyproject.toml 文件"""
    toml_content = '''
[build-system]
requires = ["setuptools>=45", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "my-cli-tool"
version = "1.0.0"
description = "一个强大的命令行工具"
readme = "README.md"
requires-python = ">=3.8"
dependencies = [
    "click>=8.0.0",
    "rich>=10.0.0",
]
classifiers = [
    "Development Status :: 4 - Beta",
    "Intended Audience :: Developers",
    "License :: OSI Approved :: MIT License",
    "Operating System :: OS Independent",
    "Programming Language :: Python :: 3",
]

[project.optional-dependencies]
dev = [
    "pytest>=6.0.0",
    "black>=21.0.0",
    "flake8>=3.9.0",
]

[project.scripts]
mytool = "my_cli_tool.cli:main"

[tool.black]
line-length = 88
target-version = ['py38']

[tool.isort]
profile = "black"

[tool.pytest.ini_options]
minversion = "6.0"
addopts = "-ra -q"
testpaths = ["tests"]
'''
    with open('pyproject.toml', 'w', encoding='utf-8') as f:
        f.write(toml_content)
    print("✓ pyproject.toml 已创建")

# 5. CI/CD 配置
print("\nCI/CD 配置:")

def create_github_actions():
    """创建 GitHub Actions 配置"""
    workflow_content = '''
name: CI/CD

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: [3.8, 3.9, "3.10", "3.11"]

    steps:
    - uses: actions/checkout@v2

    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v2
      with:
        python-version: ${{ matrix.python-version }}

    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -e .[dev]

    - name: Lint with flake8
      run: |
        flake8 my_cli_tool tests

    - name: Format with black
      run: |
        black --check my_cli_tool tests

    - name: Test with pytest
      run: |
        pytest tests/ --cov=my_cli_tool --cov-report=xml

    - name: Upload coverage to Codecov
      uses: codecov/codecov-action@v2
      with:
        file: ./coverage.xml

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
    - uses: actions/checkout@v2

    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: 3.9

    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install build twine

    - name: Build package
      run: python -m build

    - name: Publish to PyPI
      env:
        TWINE_USERNAME: __token__
        TWINE_PASSWORD: ${{ secrets.PYPI_API_TOKEN }}
      run: |
        twine upload dist/*
'''
    Path('.github/workflows').mkdir(parents=True, exist_ok=True)
    with open('.github/workflows/ci-cd.yml', 'w', encoding='utf-8') as f:
        f.write(workflow_content)
    print("✓ GitHub Actions 配置已创建")

# 6. 文档生成
print("\n文档生成:")

def create_readme():
    """创建 README 文件"""
    readme_content = '''
# My CLI Tool

一个强大的命令行工具，提供文件处理、服务器管理等功能。

## 安装

```bash
pip install my-cli-tool
```

## 使用

### 文件处理

```bash
# 处理单个文件
mytool file input.txt --output output.txt --format json

# 批量处理
mytool file *.txt --format xml
```

### 服务器管理

```bash
# 启动服务器
mytool server --host 0.0.0.0 --port 8080 --workers 8

# 带配置文件启动
mytool server --config config.json
```

## 开发

### 安装开发依赖

```bash
pip install -e .[dev]
```

### 运行测试

```bash
pytest tests/
```

### 代码格式化

```bash
black my_cli_tool tests
isort my_cli_tool tests
```

## 许可证

MIT License
'''
    with open('README.md', 'w', encoding='utf-8') as f:
        f.write(readme_content)
    print("✓ README.md 已创建")

# 7. 版本管理
print("\n版本管理:")

def create_version_file():
    """创建版本文件"""
    version_content = '''
# 版本信息
__version__ = "1.0.0"
__author__ = "Your Name"
__email__ = "your.email@example.com"
'''
    Path('my_cli_tool').mkdir(exist_ok=True)
    with open('my_cli_tool/__init__.py', 'w', encoding='utf-8') as f:
        f.write(version_content)
    print("✓ 版本文件已创建")

# 8. 发布脚本
print("\n发布脚本:")

def create_release_script():
    """创建发布脚本"""
    script_content = '''
#!/usr/bin/env python3
"""
发布脚本
"""
import subprocess
import sys
from pathlib import Path

def run_command(command, description):
    """运行命令"""
    print(f"执行: {description}")
    result = subprocess.run(command, shell=True)
    if result.returncode != 0:
        print(f"错误: {description} 失败")
        sys.exit(1)

def main():
    """主函数"""
    print("开始发布流程...")

    # 检查代码质量
    run_command("black --check my_cli_tool tests", "检查代码格式")
    run_command("flake8 my_cli_tool tests", "检查代码质量")
    run_command("pytest tests/ --cov=my_cli_tool", "运行测试")

    # 构建包
    run_command("python -m build", "构建包")

    # 检查构建结果
    dist_dir = Path("dist")
    if not dist_dir.exists():
        print("错误: dist 目录不存在")
        sys.exit(1)

    packages = list(dist_dir.glob("*.tar.gz")) + list(dist_dir.glob("*.whl"))
    if not packages:
        print("错误: 未找到构建的包")
        sys.exit(1)

    print(f"构建成功，生成 {len(packages)} 个包:")
    for package in packages:
        print(f"  - {package.name}")

    # 提示上传
    print("\n包已构建完成，可以使用以下命令上传到 PyPI:")
    print("  twine upload dist/*")
    print("\n或者使用测试 PyPI:")
    print("  twine upload --repository testpypi dist/*")

if __name__ == "__main__":
    main()
'''
    with open('release.py', 'w', encoding='utf-8') as f:
        f.write(script_content)
    print("✓ 发布脚本已创建")

# 运行示例
if __name__ == "__main__":
    print("测试和分发工具示例:")
    print("这些工具可以帮助你:")
    print("1. 编写和运行单元测试")
    print("2. 执行集成测试")
    print("3. 创建打包配置文件")
    print("4. 设置 CI/CD 流程")
    print("5. 生成项目文档")
    print("6. 管理版本信息")
    print("7. 自动化发布流程")

    # 创建示例文件
    try:
        create_setup_py()
        create_pyproject_toml()
        create_github_actions()
        create_readme()
        create_version_file()
        create_release_script()
        print("\n所有配置文件已创建完成!")
    except Exception as e:
        print(f"创建文件时出错: {e}")
```

## 总结

本章介绍了 Python 命令行工具的开发：
- 掌握 argparse 模块的基础和高级用法，包括参数解析、验证和子命令
- 学会使用 Click 框架简化命令行工具开发，支持选项、参数和上下文管理
- 理解 Typer 框架的现代化特性，包括类型提示集成和自动补全
- 掌握命令行工具开发的最佳实践，包括错误处理、配置管理和测试策略
- 学会打包、分发和部署命令行工具到 PyPI

通过本章的学习，你应该能够：
1. 使用 argparse 创建功能完整的命令行工具
2. 使用 Click 和 Typer 框架快速开发现代化的 CLI 应用
3. 实现参数验证、错误处理和配置管理
4. 编写可测试的命令行工具代码
5. 打包和分发工具到 PyPI
6. 设置 CI/CD 流程自动化测试和发布

下一章将介绍数据分析基础。
