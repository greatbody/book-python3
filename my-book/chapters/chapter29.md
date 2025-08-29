# 第29章：Web 应用基础

## 29.1 HTTP 协议基础

### 29.1.1 HTTP 协议概述

```python
# HTTP 协议基础
print("HTTP 协议基础:")

import requests
import json
from urllib.parse import urlparse, urlencode, parse_qs
import http.server
import socketserver
from http import HTTPStatus

# 1. HTTP 请求方法
print("HTTP 请求方法:")

# GET 请求 - 获取资源
def get_request_example():
    """GET 请求示例"""
    url = "https://httpbin.org/get"
    params = {"key": "value", "foo": "bar"}

    try:
        response = requests.get(url, params=params)
        print(f"GET 请求状态码: {response.status_code}")
        print(f"GET 请求 URL: {response.url}")
        print(f"GET 请求响应头: {dict(response.headers)}")
        print(f"GET 请求响应内容: {response.json()}")
    except requests.RequestException as e:
        print(f"GET 请求失败: {e}")

# POST 请求 - 创建资源
def post_request_example():
    """POST 请求示例"""
    url = "https://httpbin.org/post"
    data = {"username": "john", "password": "secret"}

    try:
        response = requests.post(url, json=data)
        print(f"POST 请求状态码: {response.status_code}")
        print(f"POST 请求响应: {response.json()}")
    except requests.RequestException as e:
        print(f"POST 请求失败: {e}")

# PUT 请求 - 更新资源
def put_request_example():
    """PUT 请求示例"""
    url = "https://httpbin.org/put"
    data = {"name": "John Doe", "email": "john@example.com"}

    try:
        response = requests.put(url, json=data)
        print(f"PUT 请求状态码: {response.status_code}")
        print(f"PUT 请求响应: {response.json()}")
    except requests.RequestException as e:
        print(f"PUT 请求失败: {e}")

# DELETE 请求 - 删除资源
def delete_request_example():
    """DELETE 请求示例"""
    url = "https://httpbin.org/delete"

    try:
        response = requests.delete(url)
        print(f"DELETE 请求状态码: {response.status_code}")
        print(f"DELETE 请求响应: {response.json()}")
    except requests.RequestException as e:
        print(f"DELETE 请求失败: {e}")

# 调用示例
print("=== GET 请求示例 ===")
get_request_example()

print("\n=== POST 请求示例 ===")
post_request_example()

print("\n=== PUT 请求示例 ===")
put_request_example()

print("\n=== DELETE 请求示例 ===")
delete_request_example()

# 2. HTTP 状态码
print("\nHTTP 状态码:")

def status_code_examples():
    """HTTP 状态码示例"""

    # 1xx - 信息响应
    print("1xx - 信息响应:")
    print("100 Continue - 继续")
    print("101 Switching Protocols - 切换协议")

    # 2xx - 成功响应
    print("\n2xx - 成功响应:")
    print("200 OK - 请求成功")
    print("201 Created - 已创建")
    print("204 No Content - 无内容")

    # 3xx - 重定向
    print("\n3xx - 重定向:")
    print("301 Moved Permanently - 永久移动")
    print("302 Found - 临时移动")
    print("304 Not Modified - 未修改")

    # 4xx - 客户端错误
    print("\n4xx - 客户端错误:")
    print("400 Bad Request - 错误请求")
    print("401 Unauthorized - 未授权")
    print("403 Forbidden - 禁止访问")
    print("404 Not Found - 未找到")
    print("422 Unprocessable Entity - 无法处理的实体")

    # 5xx - 服务器错误
    print("\n5xx - 服务器错误:")
    print("500 Internal Server Error - 内部服务器错误")
    print("502 Bad Gateway - 错误网关")
    print("503 Service Unavailable - 服务不可用")

status_code_examples()

# 3. HTTP 头信息
print("\nHTTP 头信息:")

def headers_example():
    """HTTP 头信息示例"""

    # 请求头
    request_headers = {
        "User-Agent": "Python HTTP Client",
        "Accept": "application/json",
        "Accept-Language": "zh-CN,zh;q=0.9,en;q=0.8",
        "Content-Type": "application/json",
        "Authorization": "Bearer token123"
    }

    # 发送请求时设置头信息
    url = "https://httpbin.org/headers"

    try:
        response = requests.get(url, headers=request_headers)
        print("请求头信息:")
        for key, value in request_headers.items():
            print(f"  {key}: {value}")

        print("\n服务器响应头信息:")
        for key, value in response.headers.items():
            print(f"  {key}: {value}")

    except requests.RequestException as e:
        print(f"请求失败: {e}")

headers_example()

# 4. URL 解析和构建
print("\nURL 解析和构建:")

def url_parsing_example():
    """URL 解析示例"""

    # 解析 URL
    url = "https://www.example.com:8080/path/to/resource?param1=value1&param2=value2#section"

    parsed = urlparse(url)
    print(f"原始 URL: {url}")
    print(f"协议: {parsed.scheme}")
    print(f"域名: {parsed.netloc}")
    print(f"主机名: {parsed.hostname}")
    print(f"端口: {parsed.port}")
    print(f"路径: {parsed.path}")
    print(f"查询参数: {parsed.query}")
    print(f"片段: {parsed.fragment}")

    # 解析查询参数
    query_params = parse_qs(parsed.query)
    print(f"解析后的查询参数: {query_params}")

    # 构建 URL
    print("\n构建 URL:")
    base_url = "https://api.example.com"
    endpoint = "/users"
    params = {"page": "1", "limit": "10", "sort": "name"}

    # 方法1: 手动构建
    query_string = urlencode(params)
    full_url = f"{base_url}{endpoint}?{query_string}"
    print(f"手动构建: {full_url}")

    # 方法2: 使用 requests 自动处理
    try:
        response = requests.get(base_url + endpoint, params=params)
        print(f"自动构建: {response.url}")
    except requests.RequestException:
        print("构建 URL 示例")

url_parsing_example()

# 5. Cookie 和 Session
print("\nCookie 和 Session:")

def cookie_example():
    """Cookie 示例"""

    # 创建会话
    session = requests.Session()

    # 设置 Cookie
    session.cookies.set('session_id', 'abc123', domain='httpbin.org')
    session.cookies.set('user_id', 'user456', domain='httpbin.org')

    try:
        # 发送请求，包含 Cookie
        response = session.get('https://httpbin.org/cookies')
        print("发送的 Cookie:")
        for cookie in session.cookies:
            print(f"  {cookie.name}: {cookie.value}")

        print("\n服务器返回的 Cookie:")
        print(response.json())

    except requests.RequestException as e:
        print(f"Cookie 示例失败: {e}")

    # 手动处理 Cookie
    print("\n手动处理 Cookie:")
    jar = requests.cookies.RequestsCookieJar()
    jar.set('custom_cookie', 'custom_value', domain='httpbin.org')

    try:
        response = requests.get('https://httpbin.org/cookies', cookies=jar)
        print("自定义 Cookie 响应:")
        print(response.json())
    except requests.RequestException as e:
        print(f"手动 Cookie 示例失败: {e}")

cookie_example()

# 6. 文件上传
print("\n文件上传:")

def file_upload_example():
    """文件上传示例"""

    # 创建测试文件
    test_content = "这是一个测试文件的内容\n包含多行文本"
    with open('test_upload.txt', 'w', encoding='utf-8') as f:
        f.write(test_content)

    try:
        # 上传文件
        url = "https://httpbin.org/post"
        files = {
            'file': ('test_upload.txt', open('test_upload.txt', 'rb'), 'text/plain')
        }

        response = requests.post(url, files=files)
        print("文件上传响应:")
        print(json.dumps(response.json(), indent=2, ensure_ascii=False))

    except requests.RequestException as e:
        print(f"文件上传失败: {e}")
    except FileNotFoundError:
        print("测试文件不存在")
    finally:
        # 清理文件
        import os
        if os.path.exists('test_upload.txt'):
            os.remove('test_upload.txt')

file_upload_example()

# 7. 超时和重试
print("\n超时和重试:")

def timeout_retry_example():
    """超时和重试示例"""

    import time

    # 设置超时
    print("超时示例:")
    try:
        # 2秒超时
        response = requests.get('https://httpbin.org/delay/1', timeout=2)
        print(f"请求成功，状态码: {response.status_code}")
    except requests.Timeout:
        print("请求超时")
    except requests.RequestException as e:
        print(f"请求失败: {e}")

    # 重试机制
    print("\n重试机制示例:")
    def retry_request(url, max_retries=3, delay=1):
        """带重试的请求函数"""
        for attempt in range(max_retries):
            try:
                response = requests.get(url, timeout=5)
                return response
            except (requests.Timeout, requests.ConnectionError) as e:
                if attempt < max_retries - 1:
                    print(f"第{attempt + 1}次尝试失败，{delay}秒后重试...")
                    time.sleep(delay)
                    delay *= 2  # 指数退避
                else:
                    print(f"所有重试都失败: {e}")
                    raise

    try:
        response = retry_request('https://httpbin.org/delay/3')
        print(f"重试后请求成功: {response.status_code}")
    except Exception as e:
        print(f"重试请求失败: {e}")

timeout_retry_example()

print("HTTP 协议基础示例已定义")
```

### 29.1.2 简单 HTTP 服务器

```python
# 简单 HTTP 服务器
print("简单 HTTP 服务器:")

import http.server
import socketserver
import json
import threading
import time
from urllib.parse import urlparse, parse_qs

# 1. 基础 HTTP 服务器
print("基础 HTTP 服务器:")

class SimpleHTTPRequestHandler(http.server.BaseHTTPRequestHandler):
    """简单的 HTTP 请求处理器"""

    def do_GET(self):
        """处理 GET 请求"""
        parsed_path = urlparse(self.path)

        if parsed_path.path == '/':
            # 首页
            self.send_response(200)
            self.send_header('Content-type', 'text/html; charset=utf-8')
            self.end_headers()

            html = """
            <html>
            <head><title>简单 HTTP 服务器</title></head>
            <body>
                <h1>欢迎访问简单 HTTP 服务器</h1>
                <p>当前时间: <span id="time"></span></p>
                <p>请求路径: /</p>
                <p>请求方法: GET</p>
                <ul>
                    <li><a href="/api/data">API 数据</a></li>
                    <li><a href="/api/time">当前时间</a></li>
                    <li><a href="/form">表单页面</a></li>
                </ul>
                <script>
                    document.getElementById('time').textContent = new Date().toLocaleString();
                </script>
            </body>
            </html>
            """
            self.wfile.write(html.encode('utf-8'))

        elif parsed_path.path == '/api/data':
            # API 数据
            self.send_response(200)
            self.send_header('Content-type', 'application/json')
            self.end_headers()

            data = {
                'message': 'Hello from API',
                'timestamp': time.time(),
                'server': 'Simple Python Server'
            }
            self.wfile.write(json.dumps(data, ensure_ascii=False).encode('utf-8'))

        elif parsed_path.path == '/api/time':
            # 当前时间
            self.send_response(200)
            self.send_header('Content-type', 'application/json')
            self.end_headers()

            data = {
                'current_time': time.strftime('%Y-%m-%d %H:%M:%S'),
                'timestamp': time.time()
            }
            self.wfile.write(json.dumps(data, ensure_ascii=False).encode('utf-8'))

        elif parsed_path.path == '/form':
            # 表单页面
            self.send_response(200)
            self.send_header('Content-type', 'text/html; charset=utf-8')
            self.end_headers()

            html = """
            <html>
            <head><title>表单页面</title></head>
            <body>
                <h1>用户表单</h1>
                <form method="POST" action="/submit">
                    <label for="name">姓名:</label>
                    <input type="text" id="name" name="name" required><br><br>

                    <label for="email">邮箱:</label>
                    <input type="email" id="email" name="email" required><br><br>

                    <label for="message">消息:</label><br>
                    <textarea id="message" name="message" rows="4" cols="50"></textarea><br><br>

                    <input type="submit" value="提交">
                </form>
            </body>
            </html>
            """
            self.wfile.write(html.encode('utf-8'))

        else:
            # 404 错误
            self.send_response(404)
            self.send_header('Content-type', 'text/html; charset=utf-8')
            self.end_headers()

            html = """
            <html>
            <head><title>404 - 未找到</title></head>
            <body>
                <h1>404 - 页面未找到</h1>
                <p>抱歉，您请求的页面不存在。</p>
                <a href="/">返回首页</a>
            </body>
            </html>
            """
            self.wfile.write(html.encode('utf-8'))

    def do_POST(self):
        """处理 POST 请求"""
        if self.path == '/submit':
            # 处理表单提交
            content_length = int(self.headers['Content-Length'])
            post_data = self.rfile.read(content_length).decode('utf-8')

            # 解析表单数据
            parsed_data = parse_qs(post_data)

            self.send_response(200)
            self.send_header('Content-type', 'text/html; charset=utf-8')
            self.end_headers()

            html = f"""
            <html>
            <head><title>表单提交成功</title></head>
            <body>
                <h1>表单提交成功</h1>
                <p>感谢您的提交！</p>
                <h2>提交的数据:</h2>
                <ul>
                    <li>姓名: {parsed_data.get('name', [''])[0]}</li>
                    <li>邮箱: {parsed_data.get('email', [''])[0]}</li>
                    <li>消息: {parsed_data.get('message', [''])[0]}</li>
                </ul>
                <a href="/">返回首页</a>
            </body>
            </html>
            """
            self.wfile.write(html.encode('utf-8'))
        else:
            self.send_response(404)
            self.end_headers()

    def log_message(self, format, *args):
        """自定义日志格式"""
        print(f"[HTTP Server] {time.strftime('%Y-%m-%d %H:%M:%S')} - {format % args}")

# 2. 运行服务器
print("\n运行服务器:")

def run_simple_server(port=8000):
    """运行简单 HTTP 服务器"""

    with socketserver.TCPServer(("", port), SimpleHTTPRequestHandler) as httpd:
        print(f"服务器启动在端口 {port}")
        print(f"访问地址: http://localhost:{port}")
        print("按 Ctrl+C 停止服务器")

        try:
            httpd.serve_forever()
        except KeyboardInterrupt:
            print("\n服务器停止")
            httpd.shutdown()

# 3. 高级 HTTP 服务器
print("\n高级 HTTP 服务器:")

class AdvancedHTTPRequestHandler(http.server.BaseHTTPRequestHandler):
    """高级 HTTP 请求处理器"""

    def __init__(self, *args, **kwargs):
        # 模拟数据库
        self.users = {
            '1': {'name': 'Alice', 'email': 'alice@example.com'},
            '2': {'name': 'Bob', 'email': 'bob@example.com'}
        }
        super().__init__(*args, **kwargs)

    def do_GET(self):
        """处理 GET 请求"""
        parsed_path = urlparse(self.path)

        if parsed_path.path.startswith('/api/users'):
            self.handle_users_api(parsed_path)
        elif parsed_path.path == '/health':
            self.handle_health_check()
        else:
            self.send_error(404, "Endpoint not found")

    def do_POST(self):
        """处理 POST 请求"""
        if self.path == '/api/users':
            self.handle_create_user()
        else:
            self.send_error(404, "Endpoint not found")

    def do_PUT(self):
        """处理 PUT 请求"""
        if self.path.startswith('/api/users/'):
            user_id = self.path.split('/')[-1]
            self.handle_update_user(user_id)
        else:
            self.send_error(404, "Endpoint not found")

    def do_DELETE(self):
        """处理 DELETE 请求"""
        if self.path.startswith('/api/users/'):
            user_id = self.path.split('/')[-1]
            self.handle_delete_user(user_id)
        else:
            self.send_error(404, "Endpoint not found")

    def handle_users_api(self, parsed_path):
        """处理用户 API"""
        path_parts = parsed_path.path.strip('/').split('/')

        if len(path_parts) == 2:  # /api/users
            # 获取所有用户
            self.send_response(200)
            self.send_header('Content-type', 'application/json')
            self.end_headers()

            response = {
                'users': list(self.users.values()),
                'count': len(self.users)
            }
            self.wfile.write(json.dumps(response, ensure_ascii=False).encode('utf-8'))

        elif len(path_parts) == 3:  # /api/users/{id}
            user_id = path_parts[2]
            if user_id in self.users:
                self.send_response(200)
                self.send_header('Content-type', 'application/json')
                self.end_headers()
                self.wfile.write(json.dumps(self.users[user_id], ensure_ascii=False).encode('utf-8'))
            else:
                self.send_error(404, "User not found")

    def handle_create_user(self):
        """创建用户"""
        try:
            content_length = int(self.headers['Content-Length'])
            post_data = self.rfile.read(content_length).decode('utf-8')
            user_data = json.loads(post_data)

            # 生成新用户 ID
            user_id = str(max(int(k) for k in self.users.keys()) + 1)
            self.users[user_id] = user_data

            self.send_response(201)
            self.send_header('Content-type', 'application/json')
            self.end_headers()

            response = {'id': user_id, 'message': 'User created successfully'}
            self.wfile.write(json.dumps(response, ensure_ascii=False).encode('utf-8'))

        except Exception as e:
            self.send_error(400, f"Invalid request: {str(e)}")

    def handle_update_user(self, user_id):
        """更新用户"""
        if user_id not in self.users:
            self.send_error(404, "User not found")
            return

        try:
            content_length = int(self.headers['Content-Length'])
            put_data = self.rfile.read(content_length).decode('utf-8')
            user_data = json.loads(put_data)

            self.users[user_id].update(user_data)

            self.send_response(200)
            self.send_header('Content-type', 'application/json')
            self.end_headers()

            response = {'message': 'User updated successfully'}
            self.wfile.write(json.dumps(response, ensure_ascii=False).encode('utf-8'))

        except Exception as e:
            self.send_error(400, f"Invalid request: {str(e)}")

    def handle_delete_user(self, user_id):
        """删除用户"""
        if user_id not in self.users:
            self.send_error(404, "User not found")
            return

        del self.users[user_id]

        self.send_response(200)
        self.send_header('Content-type', 'application/json')
        self.end_headers()

        response = {'message': 'User deleted successfully'}
        self.wfile.write(json.dumps(response, ensure_ascii=False).encode('utf-8')

    def handle_health_check(self):
        """健康检查"""
        self.send_response(200)
        self.send_header('Content-type', 'application/json')
        self.end_headers()

        health_data = {
            'status': 'healthy',
            'timestamp': time.time(),
            'server': 'Advanced Python HTTP Server'
        }
        self.wfile.write(json.dumps(health_data, ensure_ascii=False).encode('utf-8'))

# 4. 服务器配置和运行
print("\n服务器配置和运行:")

def run_advanced_server(port=8080):
    """运行高级 HTTP 服务器"""

    with socketserver.TCPServer(("", port), AdvancedHTTPRequestHandler) as httpd:
        print(f"高级服务器启动在端口 {port}")
        print(f"访问地址: http://localhost:{port}")
        print("可用端点:")
        print("  GET  /api/users      - 获取所有用户")
        print("  GET  /api/users/{id} - 获取特定用户")
        print("  POST /api/users      - 创建用户")
        print("  PUT  /api/users/{id} - 更新用户")
        print("  DELETE /api/users/{id} - 删除用户")
        print("  GET  /health         - 健康检查")
        print("按 Ctrl+C 停止服务器")

        try:
            httpd.serve_forever()
        except KeyboardInterrupt:
            print("\n服务器停止")
            httpd.shutdown()

# 5. 客户端测试
print("\n客户端测试:")

def test_http_server():
    """测试 HTTP 服务器"""

    import subprocess
    import signal
    import os

    # 启动服务器进程
    server_process = subprocess.Popen([
        'python3', '-c', '''
import time
from simple_http_server import run_simple_server
run_simple_server(9000)
'''
    ])

    time.sleep(2)  # 等待服务器启动

    try:
        # 测试各种端点
        base_url = "http://localhost:9000"

        # 测试首页
        response = requests.get(base_url)
        print(f"首页测试: {response.status_code}")

        # 测试 API
        response = requests.get(f"{base_url}/api/data")
        if response.status_code == 200:
            print(f"API 数据测试: {response.json()}")

        # 测试不存在的端点
        response = requests.get(f"{base_url}/notfound")
        print(f"404 测试: {response.status_code}")

    except Exception as e:
        print(f"测试失败: {e}")
    finally:
        # 停止服务器
        server_process.terminate()
        server_process.wait()

# 演示代码（不实际运行服务器）
print("HTTP 服务器示例已定义")
print("要运行服务器，请使用以下代码:")
print("run_simple_server(8000)    # 简单服务器")
print("run_advanced_server(8080)  # 高级服务器")
print("test_http_server()         # 测试服务器")
```

## 29.2 Flask Web 框架

### 29.2.1 Flask 基础

```python
# Flask Web 框架基础
print("Flask Web 框架基础:")

# 注意：需要安装 flask: pip install flask
try:
    from flask import Flask, request, jsonify, render_template_string, redirect, url_for, session, flash
    import json
    import os
except ImportError:
    print("需要安装 flask: pip install flask")
    Flask = None

if Flask:
    # 1. 基础 Flask 应用
    print("基础 Flask 应用:")

    app = Flask(__name__)
    app.secret_key = 'your-secret-key-here'  # 用于 session

    # 路由装饰器
    @app.route('/')
    def home():
        """首页路由"""
        return '''
        <h1>欢迎访问 Flask 应用</h1>
        <p>这是一个简单的 Flask Web 应用示例</p>
        <ul>
            <li><a href="/hello/World">问候页面</a></li>
            <li><a href="/api/data">API 数据</a></li>
            <li><a href="/form">表单页面</a></li>
            <li><a href="/template">模板示例</a></li>
        </ul>
        '''

    @app.route('/hello/<name>')
    def hello(name):
        """动态路由示例"""
        return f'<h1>Hello, {name}!</h1><p>欢迎访问我们的网站</p>'

    @app.route('/api/data')
    def api_data():
        """API 端点"""
        data = {
            'message': 'Hello from Flask API',
            'timestamp': '2025-01-30',
            'version': '1.0'
        }
        return jsonify(data)

    # 2. 请求处理
    print("\n请求处理:")

    @app.route('/user/<int:user_id>')
    def get_user(user_id):
        """带类型转换的路由"""
        return f'<h1>用户 ID: {user_id}</h1><p>用户类型: {type(user_id)}</p>'

    @app.route('/search')
    def search():
        """查询参数处理"""
        query = request.args.get('q', '默认搜索')
        page = request.args.get('page', 1, type=int)

        return f'''
        <h1>搜索结果</h1>
        <p>搜索关键词: {query}</p>
        <p>页码: {page}</p>
        <p>所有参数: {dict(request.args)}</p>
        '''

    @app.route('/method', methods=['GET', 'POST', 'PUT', 'DELETE'])
    def handle_method():
        """多方法路由"""
        if request.method == 'GET':
            return '<h1>GET 请求</h1><p>这是一个 GET 请求</p>'
        elif request.method == 'POST':
            return '<h1>POST 请求</h1><p>这是一个 POST 请求</p>'
        elif request.method == 'PUT':
            return '<h1>PUT 请求</h1><p>这是一个 PUT 请求</p>'
        elif request.method == 'DELETE':
            return '<h1>DELETE 请求</h1><p>这是一个 DELETE 请求</p>'

    # 3. 表单处理
    print("\n表单处理:")

    @app.route('/form', methods=['GET', 'POST'])
    def form_example():
        """表单处理示例"""
        if request.method == 'POST':
            # 处理表单提交
            name = request.form.get('name')
            email = request.form.get('email')
            message = request.form.get('message')

            # 这里可以保存到数据库
            flash(f'感谢 {name} 的提交！', 'success')

            return redirect(url_for('form_example'))

        # GET 请求显示表单
        return '''
        <h1>联系表单</h1>
        <form method="POST">
            <label for="name">姓名:</label><br>
            <input type="text" id="name" name="name" required><br><br>

            <label for="email">邮箱:</label><br>
            <input type="email" id="email" name="email" required><br><br>

            <label for="message">消息:</label><br>
            <textarea id="message" name="message" rows="4" cols="50"></textarea><br><br>

            <input type="submit" value="提交">
        </form>
        {% with messages = get_flashed_messages(with_categories=true) %}
            {% if messages %}
                <ul>
                {% for category, message in messages %}
                    <li class="{{ category }}">{{ message }}</li>
                {% endfor %}
                </ul>
            {% endif %}
        {% endwith %}
        '''

    # 4. JSON API
    print("\nJSON API:")

    # 模拟数据库
    users_db = {
        '1': {'name': 'Alice', 'email': 'alice@example.com', 'age': 25},
        '2': {'name': 'Bob', 'email': 'bob@example.com', 'age': 30}
    }

    @app.route('/api/users', methods=['GET'])
    def get_users():
        """获取所有用户"""
        return jsonify({
            'users': list(users_db.values()),
            'count': len(users_db)
        })

    @app.route('/api/users/<user_id>', methods=['GET'])
    def get_user_by_id(user_id):
        """获取特定用户"""
        if user_id in users_db:
            return jsonify(users_db[user_id])
        else:
            return jsonify({'error': 'User not found'}), 404

    @app.route('/api/users', methods=['POST'])
    def create_user():
        """创建用户"""
        if not request.is_json:
            return jsonify({'error': 'Content-Type must be application/json'}), 400

        data = request.get_json()

        # 验证必需字段
        if not all(key in data for key in ['name', 'email']):
            return jsonify({'error': 'Missing required fields: name, email'}), 400

        # 生成新用户 ID
        user_id = str(max(int(k) for k in users_db.keys()) + 1)
        users_db[user_id] = {
            'name': data['name'],
            'email': data['email'],
            'age': data.get('age', 18)
        }

        return jsonify({
            'message': 'User created successfully',
            'user_id': user_id,
            'user': users_db[user_id]
        }), 201

    @app.route('/api/users/<user_id>', methods=['PUT'])
    def update_user(user_id):
        """更新用户"""
        if user_id not in users_db:
            return jsonify({'error': 'User not found'}), 404

        if not request.is_json:
            return jsonify({'error': 'Content-Type must be application/json'}), 400

        data = request.get_json()
        users_db[user_id].update(data)

        return jsonify({
            'message': 'User updated successfully',
            'user': users_db[user_id]
        })

    @app.route('/api/users/<user_id>', methods=['DELETE'])
    def delete_user(user_id):
        """删除用户"""
        if user_id not in users_db:
            return jsonify({'error': 'User not found'}), 404

        deleted_user = users_db.pop(user_id)

        return jsonify({
            'message': 'User deleted successfully',
            'deleted_user': deleted_user
        })

    # 5. 模板渲染
    print("\n模板渲染:")

    @app.route('/template')
    def template_example():
        """模板渲染示例"""
        users = [
            {'name': 'Alice', 'age': 25, 'city': 'Beijing'},
            {'name': 'Bob', 'age': 30, 'city': 'Shanghai'},
            {'name': 'Charlie', 'age': 35, 'city': 'Shenzhen'}
        ]

        current_time = '2025-01-30 12:00:00'

        template = '''
        <!DOCTYPE html>
        <html>
        <head>
            <title>用户列表</title>
            <style>
                body { font-family: Arial, sans-serif; margin: 20px; }
                table { border-collapse: collapse; width: 100%; }
                th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
                th { background-color: #f2f2f2; }
                .header { color: #333; }
            </style>
        </head>
        <body>
            <h1 class="header">用户列表</h1>
            <p>当前时间: {{ current_time }}</p>
            <p>用户总数: {{ users|length }}</p>

            <table>
                <thead>
                    <tr>
                        <th>姓名</th>
                        <th>年龄</th>
                        <th>城市</th>
                    </tr>
                </thead>
                <tbody>
                    {% for user in users %}
                    <tr>
                        <td>{{ user.name }}</td>
                        <td>{{ user.age }}</td>
                        <td>{{ user.city }}</td>
                    </tr>
                    {% endfor %}
                </tbody>
            </table>

            <h2>条件渲染</h2>
            {% if users %}
                <p>有用户数据</p>
            {% else %}
                <p>没有用户数据</p>
            {% endif %}

            <h2>循环和条件结合</h2>
            <ul>
                {% for user in users %}
                    <li>
                        {{ user.name }}
                        {% if user.age > 30 %}
                            <span style="color: red;">(年龄较大)</span>
                        {% endif %}
                    </li>
                {% endfor %}
            </ul>
        </body>
        </html>
        '''

        return render_template_string(template, users=users, current_time=current_time)

    # 6. Session 和 Cookie
    print("\nSession 和 Cookie:")

    @app.route('/login', methods=['GET', 'POST'])
    def login():
        """登录示例"""
        if request.method == 'POST':
            username = request.form.get('username')
            password = request.form.get('password')

            # 简单的验证（实际应用中应该使用安全的验证）
            if username == 'admin' and password == 'password':
                session['username'] = username
                session['logged_in'] = True
                flash('登录成功！', 'success')
                return redirect(url_for('dashboard'))
            else:
                flash('用户名或密码错误', 'error')
                return redirect(url_for('login'))

        return '''
        <h1>用户登录</h1>
        <form method="POST">
            <label for="username">用户名:</label><br>
            <input type="text" id="username" name="username" required><br><br>

            <label for="password">密码:</label><br>
            <input type="password" id="password" name="password" required><br><br>

            <input type="submit" value="登录">
        </form>
        {% with messages = get_flashed_messages(with_categories=true) %}
            {% if messages %}
                <ul>
                {% for category, message in messages %}
                    <li class="{{ category }}">{{ message }}</li>
                {% endfor %}
                </ul>
            {% endif %}
        {% endwith %}
        '''

    @app.route('/dashboard')
    def dashboard():
        """仪表板（需要登录）"""
        if not session.get('logged_in'):
            flash('请先登录', 'error')
            return redirect(url_for('login'))

        username = session.get('username', 'Unknown')

        return f'''
        <h1>欢迎, {username}!</h1>
        <p>这是您的仪表板</p>
        <p><a href="/logout">登出</a></p>
        <p>Session 信息: {dict(session)}</p>
        '''

    @app.route('/logout')
    def logout():
        """登出"""
        session.clear()
        flash('已成功登出', 'info')
        return redirect(url_for('home'))

    # 7. 错误处理
    print("\n错误处理:")

    @app.errorhandler(404)
    def page_not_found(error):
        """404 错误处理"""
        return '''
        <h1>404 - 页面未找到</h1>
        <p>抱歉，您请求的页面不存在。</p>
        <p><a href="/">返回首页</a></p>
        ''', 404

    @app.errorhandler(500)
    def internal_error(error):
        """500 错误处理"""
        return '''
        <h1>500 - 服务器内部错误</h1>
        <p>抱歉，服务器遇到了一个错误。</p>
        <p><a href="/">返回首页</a></p>
        ''', 500

    @app.route('/error')
    def trigger_error():
        """触发错误（用于测试）"""
        raise Exception("这是一个测试错误")

    # 8. 中间件和钩子
    print("\n中间件和钩子:")

    @app.before_request
    def before_request_func():
        """请求前钩子"""
        print(f"收到请求: {request.method} {request.path}")

    @app.after_request
    def after_request_func(response):
        """请求后钩子"""
        print(f"响应状态码: {response.status_code}")
        return response

    @app.teardown_request
    def teardown_request_func(error=None):
        """请求结束钩子"""
        print("请求处理完成")

    # 9. 静态文件服务
    print("\n静态文件服务:")

    @app.route('/static-example')
    def static_example():
        """静态文件示例"""
        return '''
        <h1>静态文件示例</h1>
        <p>Flask 可以自动服务静态文件</p>
        <p>通常放在 static/ 目录下</p>
        <p>访问路径: /static/filename</p>

        <h2>常用静态文件类型:</h2>
        <ul>
            <li>CSS 样式文件</li>
            <li>JavaScript 文件</li>
            <li>图片文件 (PNG, JPG, GIF, SVG)</li>
            <li>字体文件</li>
            <li>其他资源文件</li>
        </ul>
        '''

    print("Flask 基础应用已定义")
    print("要运行应用，请使用以下代码:")
    print("if __name__ == '__main__':")
    print("    app.run(debug=True)")
else:
    print("Flask 未安装，跳过相关示例")
```

### 29.2.2 Flask 高级用法

```python
# Flask 高级用法
print("Flask 高级用法:")

try:
    from flask import Flask, Blueprint, request, jsonify, current_app, g
    from flask_sqlalchemy import SQLAlchemy
    from flask_migrate import Migrate
    from flask_wtf import FlaskForm
    from wtforms import StringField, PasswordField, SubmitField, validators
    from flask_login import LoginManager, UserMixin, login_user, logout_user, login_required, current_user
    from werkzeug.security import generate_password_hash, check_password_hash
    import os
    import click
except ImportError:
    print("需要安装相关包:")
    print("pip install flask flask-sqlalchemy flask-migrate flask-wtf flask-login")
    Flask = None

if Flask:
    # 1. 应用工厂模式
    print("应用工厂模式:")

    def create_app(config_name='development'):
        """应用工厂函数"""
        app = Flask(__name__)

        # 配置
        if config_name == 'development':
            app.config.update(
                SECRET_KEY='dev-secret-key',
                SQLALCHEMY_DATABASE_URI='sqlite:///dev.db',
                SQLALCHEMY_TRACK_MODIFICATIONS=False,
                DEBUG=True
            )
        elif config_name == 'production':
            app.config.update(
                SECRET_KEY=os.environ.get('SECRET_KEY', 'prod-secret-key'),
                SQLALCHEMY_DATABASE_URI=os.environ.get('DATABASE_URL', 'sqlite:///prod.db'),
                SQLALCHEMY_TRACK_MODIFICATIONS=False,
                DEBUG=False
            )

        # 初始化扩展
        db.init_app(app)
        migrate.init_app(app, db)
        login_manager.init_app(app)

        # 注册蓝图
        app.register_blueprint(auth_bp)
        app.register_blueprint(api_bp)

        # 注册命令
        app.cli.add_command(init_db_command)

        return app

    # 2. 蓝图 (Blueprint)
    print("\n蓝图 (Blueprint):")

    auth_bp = Blueprint('auth', __name__, url_prefix='/auth')
    api_bp = Blueprint('api', __name__, url_prefix='/api')

    @auth_bp.route('/login', methods=['GET', 'POST'])
    def login():
        """认证蓝图的登录路由"""
        return '<h1>认证 - 登录</h1>'

    @auth_bp.route('/register')
    def register():
        """认证蓝图的注册路由"""
        return '<h1>认证 - 注册</h1>'

    @api_bp.route('/users')
    def users():
        """API 蓝图的用户路由"""
        return jsonify({'message': 'API - 用户列表'})

    @api_bp.route('/posts')
    def posts():
        """API 蓝图的文章路由"""
        return jsonify({'message': 'API - 文章列表'})

    # 3. SQLAlchemy 数据库集成
    print("\nSQLAlchemy 数据库集成:")

    db = SQLAlchemy()
    migrate = Migrate()

    class User(db.Model, UserMixin):
        """用户模型"""
        id = db.Column(db.Integer, primary_key=True)
        username = db.Column(db.String(80), unique=True, nullable=False)
        email = db.Column(db.String(120), unique=True, nullable=False)
        password_hash = db.Column(db.String(128))
        posts = db.relationship('Post', backref='author', lazy=True)

        def set_password(self, password):
            self.password_hash = generate_password_hash(password)

        def check_password(self, password):
            return check_password_hash(self.password_hash, password)

        def __repr__(self):
            return f'<User {self.username}>'

    class Post(db.Model):
        """文章模型"""
        id = db.Column(db.Integer, primary_key=True)
        title = db.Column(db.String(100), nullable=False)
        content = db.Column(db.Text, nullable=False)
        created_at = db.Column(db.DateTime, default=db.func.now())
        user_id = db.Column(db.Integer, db.ForeignKey('user.id'), nullable=False)

        def __repr__(self):
            return f'<Post {self.title}>'

    # 4. Flask-WTF 表单处理
    print("\nFlask-WTF 表单处理:")

    class LoginForm(FlaskForm):
        """登录表单"""
        username = StringField('用户名', validators=[validators.DataRequired()])
        password = PasswordField('密码', validators=[validators.DataRequired()])
        submit = SubmitField('登录')

    class RegistrationForm(FlaskForm):
        """注册表单"""
        username = StringField('用户名', validators=[
            validators.DataRequired(),
            validators.Length(min=3, max=80)
        ])
        email = StringField('邮箱', validators=[
            validators.DataRequired(),
            validators.Email()
        ])
        password = PasswordField('密码', validators=[
            validators.DataRequired(),
            validators.Length(min=6)
        ])
        confirm_password = PasswordField('确认密码', validators=[
            validators.DataRequired(),
            validators.EqualTo('password', message='密码不匹配')
        ])
        submit = SubmitField('注册')

    # 5. Flask-Login 用户认证
    print("\nFlask-Login 用户认证:")

    login_manager = LoginManager()
    login_manager.login_view = 'auth.login'

    @login_manager.user_loader
    def load_user(user_id):
        """加载用户"""
        return User.query.get(int(user_id))

    # 6. 完整的认证蓝图
    print("\n完整的认证蓝图:")

    @auth_bp.route('/login', methods=['GET', 'POST'])
    def login():
        """用户登录"""
        if current_user.is_authenticated:
            return redirect(url_for('index'))

        form = LoginForm()
        if form.validate_on_submit():
            user = User.query.filter_by(username=form.username.data).first()
            if user and user.check_password(form.password.data):
                login_user(user)
                next_page = request.args.get('next')
                return redirect(next_page or url_for('index'))
            else:
                flash('用户名或密码错误', 'error')

        return render_template_string('''
        <h1>用户登录</h1>
        <form method="POST">
            {{ form.hidden_tag() }}
            <p>{{ form.username.label }}: {{ form.username() }}</p>
            <p>{{ form.password.label }}: {{ form.password() }}</p>
            <p>{{ form.submit() }}</p>
        </form>
        {% with messages = get_flashed_messages(with_categories=true) %}
            {% if messages %}
                {% for category, message in messages %}
                    <p class="{{ category }}">{{ message }}</p>
                {% endfor %}
            {% endif %}
        {% endwith %}
        ''', form=form)

    @auth_bp.route('/register', methods=['GET', 'POST'])
    def register():
        """用户注册"""
        if current_user.is_authenticated:
            return redirect(url_for('index'))

        form = RegistrationForm()
        if form.validate_on_submit():
            user = User.query.filter_by(username=form.username.data).first()
            if user:
                flash('用户名已存在', 'error')
                return redirect(url_for('auth.register'))

            user = User.query.filter_by(email=form.email.data).first()
            if user:
                flash('邮箱已被注册', 'error')
                return redirect(url_for('auth.register'))

            user = User(
                username=form.username.data,
                email=form.email.data
            )
            user.set_password(form.password.data)

            db.session.add(user)
            db.session.commit()

            flash('注册成功，请登录', 'success')
            return redirect(url_for('auth.login'))

        return render_template_string('''
        <h1>用户注册</h1>
        <form method="POST">
            {{ form.hidden_tag() }}
            <p>{{ form.username.label }}: {{ form.username() }}</p>
            <p>{{ form.email.label }}: {{ form.email() }}</p>
            <p>{{ form.password.label }}: {{ form.password() }}</p>
            <p>{{ form.confirm_password.label }}: {{ form.confirm_password() }}</p>
            <p>{{ form.submit() }}</p>
        </form>
        {% with messages = get_flashed_messages(with_categories=true) %}
            {% if messages %}
                {% for category, message in messages %}
                    <p class="{{ category }}">{{ message }}</p>
                {% endfor %}
            {% endif %}
        {% endwith %}
        ''', form=form)

    @auth_bp.route('/logout')
    @login_required
    def logout():
        """用户登出"""
        logout_user()
        return redirect(url_for('index'))

    # 7. REST API 蓝图
    print("\nREST API 蓝图:")

    @api_bp.route('/users', methods=['GET'])
    def get_users():
        """获取所有用户"""
        users = User.query.all()
        return jsonify({
            'users': [{
                'id': user.id,
                'username': user.username,
                'email': user.email
            } for user in users]
        })

    @api_bp.route('/users/<int:user_id>', methods=['GET'])
    def get_user(user_id):
        """获取特定用户"""
        user = User.query.get_or_404(user_id)
        return jsonify({
            'id': user.id,
            'username': user.username,
            'email': user.email
        })

    @api_bp.route('/users', methods=['POST'])
    def create_user():
        """创建用户"""
        data = request.get_json()

        if not data or not all(key in data for key in ['username', 'email', 'password']):
            return jsonify({'error': 'Missing required fields'}), 400

        if User.query.filter_by(username=data['username']).first():
            return jsonify({'error': 'Username already exists'}), 400

        if User.query.filter_by(email=data['email']).first():
            return jsonify({'error': 'Email already exists'}), 400

        user = User(
            username=data['username'],
            email=data['email']
        )
        user.set_password(data['password'])

        db.session.add(user)
        db.session.commit()

        return jsonify({
            'message': 'User created successfully',
            'user': {
                'id': user.id,
                'username': user.username,
                'email': user.email
            }
        }), 201

    @api_bp.route('/users/<int:user_id>', methods=['PUT'])
    def update_user(user_id):
        """更新用户"""
        user = User.query.get_or_404(user_id)
        data = request.get_json()

        if 'username' in data:
            existing_user = User.query.filter_by(username=data['username']).first()
            if existing_user and existing_user.id != user_id:
                return jsonify({'error': 'Username already exists'}), 400
            user.username = data['username']

        if 'email' in data:
            existing_user = User.query.filter_by(email=data['email']).first()
            if existing_user and existing_user.id != user_id:
                return jsonify({'error': 'Email already exists'}), 400
            user.email = data['email']

        if 'password' in data:
            user.set_password(data['password'])

        db.session.commit()

        return jsonify({
            'message': 'User updated successfully',
            'user': {
                'id': user.id,
                'username': user.username,
                'email': user.email
            }
        })

    @api_bp.route('/users/<int:user_id>', methods=['DELETE'])
    def delete_user(user_id):
        """删除用户"""
        user = User.query.get_or_404(user_id)

        db.session.delete(user)
        db.session.commit()

        return jsonify({'message': 'User deleted successfully'})

    # 8. 命令行接口
    print("\n命令行接口:")

    @click.command('init-db')
    @click.option('--drop', is_flag=True, help='Drop existing tables')
    def init_db_command(drop):
        """初始化数据库"""
        if drop:
            db.drop_all()
            click.echo('Dropped all tables')

        db.create_all()
        click.echo('Initialized the database')

    # 9. 应用上下文和全局变量
    print("\n应用上下文和全局变量:")

    def get_db():
        """获取数据库连接"""
        if 'db' not in g:
            # 这里可以是实际的数据库连接
            g.db = {'connection': 'mock_connection'}
        return g.db

    @api_bp.before_request
    def before_api_request():
        """API 请求前处理"""
        g.start_time = time.time()
        g.request_id = 'req_' + str(int(time.time() * 1000))

    @api_bp.after_request
    def after_api_request(response):
        """API 请求后处理"""
        if hasattr(g, 'start_time'):
            duration = time.time() - g.start_time
            print(f"Request {g.request_id} took {duration:.2f}s")
        return response

    # 10. 完整的应用示例
    print("\n完整的应用示例:")

    def create_complete_app():
        """创建完整的 Flask 应用"""
        app = create_app('development')

        @app.route('/')
        def index():
            """首页"""
            return '''
            <h1>Flask 完整应用</h1>
            <ul>
                <li><a href="/auth/login">登录</a></li>
                <li><a href="/auth/register">注册</a></li>
                <li><a href="/api/users">API - 用户列表</a></li>
                <li><a href="/dashboard">仪表板</a> (需要登录)</li>
            </ul>
            '''

        @app.route('/dashboard')
        @login_required
        def dashboard():
            """仪表板"""
            posts = Post.query.filter_by(user_id=current_user.id).all()
            return f'''
            <h1>仪表板 - 欢迎, {current_user.username}!</h1>
            <h2>您的文章</h2>
            <ul>
                {"".join(f"<li>{post.title}</li>" for post in posts)}
            </ul>
            <p><a href="/auth/logout">登出</a></p>
            '''

        return app

    print("Flask 高级用法示例已定义")
    print("要运行完整应用，请使用以下代码:")
    print("app = create_complete_app()")
    print("app.run(debug=True)")
else:
    print("相关包未安装，跳过高级用法示例")
```

## 29.3 Django Web 框架

### 29.3.1 Django 基础

```python
# Django Web 框架基础
print("Django Web 框架基础:")

# 注意：Django 通常需要项目结构，这里展示核心概念
# 实际使用时需要运行: django-admin startproject myproject

try:
    import os
    import sys
    import django
    from django.conf import settings
    from django.urls import path
    from django.http import HttpResponse, JsonResponse
    from django.shortcuts import render, redirect
    from django.contrib.auth.models import User
    from django.contrib.auth import authenticate, login, logout
    from django.contrib.auth.decorators import login_required
    from django.db import models
    from django.core.management import execute_from_command_line
except ImportError:
    print("需要安装 Django: pip install django")
    django = None

if django:
    # 1. Django 设置配置
    print("Django 设置配置:")

    # 配置 Django 设置
    if not settings.configured:
        settings.configure(
            DEBUG=True,
            SECRET_KEY='your-secret-key-here',
            INSTALLED_APPS=[
                'django.contrib.auth',
                'django.contrib.contenttypes',
                'django.contrib.sessions',
                'django.contrib.messages',
                'myapp',  # 我们的应用
            ],
            DATABASES={
                'default': {
                    'ENGINE': 'django.db.backends.sqlite3',
                    'NAME': ':memory:',  # 使用内存数据库
                }
            },
            MIDDLEWARE=[
                'django.middleware.security.SecurityMiddleware',
                'django.contrib.sessions.middleware.SessionMiddleware',
                'django.middleware.common.CommonMiddleware',
                'django.middleware.csrf.CsrfViewMiddleware',
                'django.contrib.auth.middleware.AuthenticationMiddleware',
                'django.contrib.messages.middleware.MessageMiddleware',
            ],
            ROOT_URLCONF='myproject.urls',
            TEMPLATES=[
                {
                    'BACKEND': 'django.template.backends.django.DjangoTemplates',
                    'DIRS': [],
                    'APP_DIRS': True,
                    'OPTIONS': {
                        'context_processors': [
                            'django.template.context_processors.debug',
                            'django.template.context_processors.request',
                            'django.contrib.auth.context_processors.auth',
                            'django.contrib.messages.context_processors.messages',
                        ],
                    },
                },
            ],
            USE_TZ=True,
        )

    django.setup()

    # 2. Django 模型
    print("\nDjango 模型:")

    class Article(models.Model):
        """文章模型"""
        title = models.CharField(max_length=200)
        content = models.TextField()
        author = models.ForeignKey(User, on_delete=models.CASCADE)
        created_at = models.DateTimeField(auto_now_add=True)
        updated_at = models.DateTimeField(auto_now=True)
        published = models.BooleanField(default=False)

        class Meta:
            ordering = ['-created_at']

        def __str__(self):
            return self.title

    class Comment(models.Model):
        """评论模型"""
        article = models.ForeignKey(Article, on_delete=models.CASCADE, related_name='comments')
        author = models.ForeignKey(User, on_delete=models.CASCADE)
        content = models.TextField()
        created_at = models.DateTimeField(auto_now_add=True)

        def __str__(self):
            return f'Comment by {self.author.username} on {self.article.title}'

    # 3. Django 视图
    print("\nDjango 视图:")

    def home_view(request):
        """首页视图"""
        return HttpResponse('''
        <h1>欢迎访问 Django 应用</h1>
        <p>这是一个简单的 Django Web 应用示例</p>
        <ul>
            <li><a href="/articles/">文章列表</a></li>
            <li><a href="/api/articles/">API - 文章列表</a></li>
            <li><a href="/login/">登录</a></li>
            <li><a href="/register/">注册</a></li>
        </ul>
        ''')

    def articles_list(request):
        """文章列表视图"""
        articles = Article.objects.filter(published=True)
        html = '<h1>文章列表</h1>'
        if articles:
            html += '<ul>'
            for article in articles:
                html += f'<li><a href="/articles/{article.id}/">{article.title}</a> - {article.author.username}</li>'
            html += '</ul>'
        else:
            html += '<p>暂无文章</p>'
        return HttpResponse(html)

    def article_detail(request, article_id):
        """文章详情视图"""
        try:
            article = Article.objects.get(id=article_id, published=True)
            html = f'''
            <h1>{article.title}</h1>
            <p>作者: {article.author.username}</p>
            <p>发布时间: {article.created_at}</p>
            <div>{article.content}</div>
            <h2>评论</h2>
            '''
            comments = article.comments.all()
            if comments:
                html += '<ul>'
                for comment in comments:
                    html += f'<li>{comment.author.username}: {comment.content}</li>'
                html += '</ul>'
            else:
                html += '<p>暂无评论</p>'
            html += '<p><a href="/articles/">返回列表</a></p>'
            return HttpResponse(html)
        except Article.DoesNotExist:
            return HttpResponse('<h1>文章不存在</h1>', status=404)

    # 4. Django 表单
    print("\nDjango 表单:")

    from django import forms

    class ArticleForm(forms.ModelForm):
        """文章表单"""
        class Meta:
            model = Article
            fields = ['title', 'content', 'published']
            widgets = {
                'content': forms.Textarea(attrs={'rows': 10, 'cols': 80}),
            }

    class CommentForm(forms.ModelForm):
        """评论表单"""
        class Meta:
            model = Comment
            fields = ['content']
            widgets = {
                'content': forms.Textarea(attrs={'rows': 3, 'cols': 60}),
            }

    def add_article(request):
        """添加文章视图"""
        if request.method == 'POST':
            form = ArticleForm(request.POST)
            if form.is_valid():
                article = form.save(commit=False)
                article.author = request.user
                article.save()
                return redirect('articles_list')
        else:
            form = ArticleForm()

        html = f'''
        <h1>添加文章</h1>
        <form method="post">
            {% csrf_token %}
            {form.as_p()}
            <button type="submit">保存</button>
        </form>
        <p><a href="/articles/">返回列表</a></p>
        '''
        return HttpResponse(html)

    # 5. Django API 视图
    print("\nDjango API 视图:")

    def api_articles_list(request):
        """API - 文章列表"""
        articles = Article.objects.filter(published=True).values(
            'id', 'title', 'author__username', 'created_at'
        )
        return JsonResponse({
            'articles': list(articles),
            'count': len(articles)
        })

    def api_article_detail(request, article_id):
        """API - 文章详情"""
        try:
            article = Article.objects.get(id=article_id, published=True)
            data = {
                'id': article.id,
                'title': article.title,
                'content': article.content,
                'author': article.author.username,
                'created_at': article.created_at.isoformat(),
                'comments': [
                    {
                        'id': comment.id,
                        'author': comment.author.username,
                        'content': comment.content,
                        'created_at': comment.created_at.isoformat()
                    }
                    for comment in article.comments.all()
                ]
            }
            return JsonResponse(data)
        except Article.DoesNotExist:
            return JsonResponse({'error': 'Article not found'}, status=404)

    def api_create_article(request):
        """API - 创建文章"""
        if request.method != 'POST':
            return JsonResponse({'error': 'Method not allowed'}, status=405)

        try:
            import json
            data = json.loads(request.body)
            article = Article.objects.create(
                title=data['title'],
                content=data['content'],
                author=request.user,
                published=data.get('published', False)
            )
            return JsonResponse({
                'id': article.id,
                'message': 'Article created successfully'
            }, status=201)
        except Exception as e:
            return JsonResponse({'error': str(e)}, status=400)

    # 6. Django 用户认证
    print("\nDjango 用户认证:")

    def login_view(request):
        """登录视图"""
        if request.method == 'POST':
            username = request.POST.get('username')
            password = request.POST.get('password')

            user = authenticate(request, username=username, password=password)
            if user is not None:
                login(request, user)
                return redirect('home')
            else:
                return HttpResponse('<h1>登录失败</h1><p><a href="/login/">重试</a></p>')

        return HttpResponse('''
        <h1>用户登录</h1>
        <form method="post">
            <label for="username">用户名:</label>
            <input type="text" id="username" name="username" required><br><br>

            <label for="password">密码:</label>
            <input type="password" id="password" name="password" required><br><br>

            <button type="submit">登录</button>
        </form>
        <p><a href="/register/">注册</a></p>
        ''')

    def register_view(request):
        """注册视图"""
        if request.method == 'POST':
            username = request.POST.get('username')
            email = request.POST.get('email')
            password = request.POST.get('password')

            if User.objects.filter(username=username).exists():
                return HttpResponse('<h1>用户名已存在</h1><p><a href="/register/">重试</a></p>')

            user = User.objects.create_user(username=username, email=email, password=password)
            login(request, user)
            return redirect('home')

        return HttpResponse('''
        <h1>用户注册</h1>
        <form method="post">
            <label for="username">用户名:</label>
            <input type="text" id="username" name="username" required><br><br>

            <label for="email">邮箱:</label>
            <input type="email" id="email" name="email" required><br><br>

            <label for="password">密码:</label>
            <input type="password" id="password" name="password" required><br><br>

            <button type="submit">注册</button>
        </form>
        <p><a href="/login/">登录</a></p>
        ''')

    def logout_view(request):
        """登出视图"""
        logout(request)
        return redirect('home')

    @login_required
    def dashboard_view(request):
        """仪表板视图"""
        articles = Article.objects.filter(author=request.user)
        html = f'''
        <h1>仪表板 - 欢迎, {request.user.username}!</h1>
        <h2>您的文章</h2>
        <ul>
        '''
        for article in articles:
            html += f'<li>{article.title} ({article.created_at})</li>'
        html += '''
        </ul>
        <p><a href="/articles/add/">添加文章</a></p>
        <p><a href="/logout/">登出</a></p>
        '''
        return HttpResponse(html)

    # 7. Django URL 配置
    print("\nDjango URL 配置:")

    from django.urls import include

    # 主 URL 配置
    urlpatterns = [
        path('', home_view, name='home'),
        path('articles/', articles_list, name='articles_list'),
        path('articles/<int:article_id>/', article_detail, name='article_detail'),
        path('articles/add/', add_article, name='add_article'),
        path('api/articles/', api_articles_list, name='api_articles_list'),
        path('api/articles/<int:article_id>/', api_article_detail, name='api_article_detail'),
        path('api/articles/create/', api_create_article, name='api_create_article'),
        path('login/', login_view, name='login'),
        path('register/', register_view, name='register'),
        path('logout/', logout_view, name='logout'),
        path('dashboard/', dashboard_view, name='dashboard'),
    ]

    # 8. Django 管理界面
    print("\nDjango 管理界面:")

    # 在 settings.py 中添加:
    # INSTALLED_APPS = [
    #     'django.contrib.admin',
    #     'django.contrib.auth',
    #     'django.contrib.contenttypes',
    #     ...
    # ]

    # 在 urls.py 中添加:
    # from django.contrib import admin
    # urlpatterns = [
    #     path('admin/', admin.site.urls),
    #     ...
    # ]

    # 注册模型到管理界面
    from django.contrib import admin

    @admin.register(Article)
    class ArticleAdmin(admin.ModelAdmin):
        list_display = ['title', 'author', 'created_at', 'published']
        list_filter = ['published', 'created_at']
        search_fields = ['title', 'content']
        ordering = ['-created_at']

    @admin.register(Comment)
    class CommentAdmin(admin.ModelAdmin):
        list_display = ['article', 'author', 'created_at']
        list_filter = ['created_at']
        search_fields = ['content']

    # 9. Django 模板
    print("\nDjango 模板:")

    def template_example(request):
        """模板示例视图"""
        articles = Article.objects.filter(published=True)[:5]
        context = {
            'articles': articles,
            'current_time': '2025-01-30',
            'user': request.user if hasattr(request, 'user') else None,
        }

        # 模板内容（通常放在 templates/ 目录中）
        template_content = '''
        <!DOCTYPE html>
        <html>
        <head>
            <title>文章列表</title>
            <style>
                body { font-family: Arial, sans-serif; margin: 20px; }
                .article { border: 1px solid #ddd; padding: 10px; margin: 10px 0; }
                .header { color: #333; }
            </style>
        </head>
        <body>
            <h1 class="header">文章列表</h1>
            <p>当前时间: {{ current_time }}</p>
            {% if user.is_authenticated %}
                <p>欢迎, {{ user.username }}!</p>
            {% else %}
                <p><a href="/login/">请登录</a></p>
            {% endif %}

            <h2>最新文章</h2>
            {% for article in articles %}
                <div class="article">
                    <h3>{{ article.title }}</h3>
                    <p>作者: {{ article.author.username }}</p>
                    <p>发布时间: {{ article.created_at|date:"Y-m-d H:i" }}</p>
                    <p>{{ article.content|truncatechars:100 }}</p>
                    <a href="/articles/{{ article.id }}/">阅读全文</a>
                </div>
            {% empty %}
                <p>暂无文章</p>
            {% endfor %}

            <h2>统计信息</h2>
            <p>文章总数: {{ articles|length }}</p>
        </body>
        </html>
        '''

        # 渲染模板
        from django.template import Template, Context
        template = Template(template_content)
        html = template.render(Context(context))

        return HttpResponse(html)

    # 10. Django 静态文件
    print("\nDjango 静态文件:")

    # 在 settings.py 中配置:
    # STATIC_URL = '/static/'
    # STATICFILES_DIRS = [
    #     os.path.join(BASE_DIR, 'static'),
    # ]

    def static_example(request):
        """静态文件示例"""
        html = '''
        <!DOCTYPE html>
        <html>
        <head>
            <title>静态文件示例</title>
            <link rel="stylesheet" type="text/css" href="/static/css/style.css">
        </head>
        <body>
            <h1>静态文件示例</h1>
            <p>Django 可以自动服务静态文件</p>
            <img src="/static/images/logo.png" alt="Logo">

            <h2>常用静态文件类型:</h2>
            <ul>
                <li>CSS 样式文件 (/static/css/)</li>
                <li>JavaScript 文件 (/static/js/)</li>
                <li>图片文件 (/static/images/)</li>
                <li>字体文件 (/static/fonts/)</li>
            </ul>

            <script src="/static/js/script.js"></script>
        </body>
        </html>
        '''
        return HttpResponse(html)

    print("Django 基础示例已定义")
    print("要创建完整 Django 项目，请运行:")
    print("django-admin startproject myproject")
    print("cd myproject")
    print("python manage.py startapp myapp")
else:
    print("Django 未安装，跳过相关示例")
```

## 29.4 REST API 设计

### 29.4.1 REST API 基础

```python
# REST API 设计基础
print("REST API 设计基础:")

import json
from typing import Dict, List, Optional, Any
from dataclasses import dataclass, asdict
from datetime import datetime
import uuid

# 1. REST API 原则
print("REST API 原则:")

def rest_principles():
    """REST API 设计原则"""

    principles = {
        "客户端-服务器": {
            "描述": "分离客户端和服务器关注点",
            "优势": "可移植性、可扩展性",
            "示例": "Web 浏览器作为客户端，Web 服务器作为服务器"
        },
        "无状态": {
            "描述": "每个请求包含所有必要信息",
            "优势": "可靠性、可扩展性",
            "示例": "不依赖服务器端会话，使用 JWT 或 API Key"
        },
        "缓存": {
            "描述": "响应可以被缓存",
            "优势": "性能提升、减少服务器负载",
            "示例": "使用 Cache-Control 和 ETag 头"
        },
        "统一接口": {
            "描述": "标准化的接口设计",
            "优势": "简化系统架构",
            "示例": "使用 HTTP 方法和标准状态码"
        },
        "分层系统": {
            "描述": "通过层级组织系统",
            "优势": "可扩展性和安全性",
            "示例": "负载均衡器、API 网关、安全层"
        },
        "按需代码": {
            "描述": "可选的代码下载",
            "优势": "可扩展性",
            "示例": "JavaScript 代码按需加载"
        }
    }

    for principle, details in principles.items():
        print(f"\n{principle}:")
        print(f"  描述: {details['描述']}")
        print(f"  优势: {details['优势']}")
        print(f"  示例: {details['示例']}")

rest_principles()

# 2. HTTP 方法和状态码
print("\nHTTP 方法和状态码:")

def http_methods_status_codes():
    """HTTP 方法和状态码"""

    methods = {
        "GET": {
            "用途": "获取资源",
            "安全": True,
            "幂等": True,
            "示例": "GET /api/users - 获取用户列表"
        },
        "POST": {
            "用途": "创建资源",
            "安全": False,
            "幂等": False,
            "示例": "POST /api/users - 创建新用户"
        },
        "PUT": {
            "用途": "更新/替换资源",
            "安全": False,
            "幂等": True,
            "示例": "PUT /api/users/123 - 更新用户123"
        },
        "PATCH": {
            "用途": "部分更新资源",
            "安全": False,
            "幂等": True,
            "示例": "PATCH /api/users/123 - 部分更新用户123"
        },
        "DELETE": {
            "用途": "删除资源",
            "安全": False,
            "幂等": True,
            "示例": "DELETE /api/users/123 - 删除用户123"
        },
        "HEAD": {
            "用途": "获取资源元数据",
            "安全": True,
            "幂等": True,
            "示例": "HEAD /api/users - 获取用户列表的元数据"
        },
        "OPTIONS": {
            "用途": "获取支持的方法",
            "安全": True,
            "幂等": True,
            "示例": "OPTIONS /api/users - 获取支持的HTTP方法"
        }
    }

    print("HTTP 方法:")
    for method, details in methods.items():
        print(f"  {method}: {details['用途']} ({'安全' if details['安全'] else '不安全'}, {'幂等' if details['幂等'] else '不幂等'})")
        print(f"    示例: {details['示例']}")

    status_codes = {
        "2xx 成功": {
            "200": "OK - 请求成功",
            "201": "Created - 资源已创建",
            "204": "No Content - 无内容"
        },
        "3xx 重定向": {
            "301": "Moved Permanently - 永久移动",
            "302": "Found - 临时移动",
            "304": "Not Modified - 未修改"
        },
        "4xx 客户端错误": {
            "400": "Bad Request - 错误请求",
            "401": "Unauthorized - 未授权",
            "403": "Forbidden - 禁止访问",
            "404": "Not Found - 未找到",
            "422": "Unprocessable Entity - 无法处理的实体"
        },
        "5xx 服务器错误": {
            "500": "Internal Server Error - 内部服务器错误",
            "502": "Bad Gateway - 错误网关",
            "503": "Service Unavailable - 服务不可用"
        }
    }

    print("\nHTTP 状态码:")
    for category, codes in status_codes.items():
        print(f"  {category}:")
        for code, description in codes.items():
            print(f"    {code}: {description}")

http_methods_status_codes()

# 3. 资源设计
print("\n资源设计:")

@dataclass
class User:
    """用户资源"""
    id: str
    username: str
    email: str
    created_at: datetime
    updated_at: datetime

@dataclass
class Post:
    """文章资源"""
    id: str
    title: str
    content: str
    author_id: str
    created_at: datetime
    updated_at: datetime

@dataclass
class Comment:
    """评论资源"""
    id: str
    content: str
    post_id: str
    author_id: str
    created_at: datetime

class MockDatabase:
    """模拟数据库"""

    def __init__(self):
        self.users = {}
        self.posts = {}
        self.comments = {}

    def create_user(self, username: str, email: str) -> User:
        """创建用户"""
        user_id = str(uuid.uuid4())
        user = User(
            id=user_id,
            username=username,
            email=email,
            created_at=datetime.now(),
            updated_at=datetime.now()
        )
        self.users[user_id] = user
        return user

    def get_user(self, user_id: str) -> Optional[User]:
        """获取用户"""
        return self.users.get(user_id)

    def get_users(self) -> List[User]:
        """获取所有用户"""
        return list(self.users.values())

    def create_post(self, title: str, content: str, author_id: str) -> Post:
        """创建文章"""
        post_id = str(uuid.uuid4())
        post = Post(
            id=post_id,
            title=title,
            content=content,
            author_id=author_id,
            created_at=datetime.now(),
            updated_at=datetime.now()
        )
        self.posts[post_id] = post
        return post

    def get_post(self, post_id: str) -> Optional[Post]:
        """获取文章"""
        return self.posts.get(post_id)

    def get_posts(self) -> List[Post]:
        """获取所有文章"""
        return list(self.posts.values())

# 4. API 响应格式
print("\nAPI 响应格式:")

def create_api_response(success: bool, data: Any = None, message: str = "",
                       errors: List[str] = None, status_code: int = 200) -> Dict:
    """创建标准 API 响应"""

    response = {
        "success": success,
        "timestamp": datetime.now().isoformat(),
        "version": "1.0"
    }

    if data is not None:
        response["data"] = data

    if message:
        response["message"] = message

    if errors:
        response["errors"] = errors

    return response

def paginate_response(items: List[Any], page: int = 1, per_page: int = 10,
                     total: int = None) -> Dict:
    """创建分页响应"""

    if total is None:
        total = len(items)

    total_pages = (total + per_page - 1) // per_page
    start_idx = (page - 1) * per_page
    end_idx = start_idx + per_page

    return {
        "items": items[start_idx:end_idx],
        "pagination": {
            "page": page,
            "per_page": per_page,
            "total": total,
            "total_pages": total_pages,
            "has_next": page < total_pages,
            "has_prev": page > 1
        }
    }

# 5. REST API 实现
print("\nREST API 实现:")

class RESTAPI:
    """REST API 实现"""

    def __init__(self):
        self.db = MockDatabase()

    def handle_request(self, method: str, path: str, data: Dict = None) -> Dict:
        """处理 API 请求"""

        # 解析路径
        path_parts = path.strip('/').split('/')
        resource = path_parts[0] if path_parts[0] else None
        resource_id = path_parts[1] if len(path_parts) > 1 else None

        # 路由到相应的处理方法
        if resource == 'users':
            return self.handle_users(method, resource_id, data)
        elif resource == 'posts':
            return self.handle_posts(method, resource_id, data)
        elif resource == 'comments':
            return self.handle_comments(method, resource_id, data)
        else:
            return create_api_response(False, message="Resource not found", status_code=404)

    def handle_users(self, method: str, user_id: str = None, data: Dict = None) -> Dict:
        """处理用户相关请求"""

        if method == 'GET':
            if user_id:
                # 获取特定用户
                user = self.db.get_user(user_id)
                if user:
                    return create_api_response(True, asdict(user))
                else:
                    return create_api_response(False, message="User not found", status_code=404)
            else:
                # 获取所有用户
                users = self.db.get_users()
                return create_api_response(True, [asdict(user) for user in users])

        elif method == 'POST' and not user_id:
            # 创建用户
            if not data or not all(key in data for key in ['username', 'email']):
                return create_api_response(False, message="Missing required fields", status_code=400)

            user = self.db.create_user(data['username'], data['email'])
            return create_api_response(True, asdict(user), message="User created", status_code=201)

        else:
            return create_api_response(False, message="Method not allowed", status_code=405)

    def handle_posts(self, method: str, post_id: str = None, data: Dict = None) -> Dict:
        """处理文章相关请求"""

        if method == 'GET':
            if post_id:
                # 获取特定文章
                post = self.db.get_post(post_id)
                if post:
                    return create_api_response(True, asdict(post))
                else:
                    return create_api_response(False, message="Post not found", status_code=404)
            else:
                # 获取所有文章
                posts = self.db.get_posts()
                return create_api_response(True, [asdict(post) for post in posts])

        elif method == 'POST' and not post_id:
            # 创建文章
            if not data or not all(key in data for key in ['title', 'content', 'author_id']):
                return create_api_response(False, message="Missing required fields", status_code=400)

            post = self.db.create_post(data['title'], data['content'], data['author_id'])
            return create_api_response(True, asdict(post), message="Post created", status_code=201)

        else:
            return create_api_response(False, message="Method not allowed", status_code=405)

    def handle_comments(self, method: str, comment_id: str = None, data: Dict = None) -> Dict:
        """处理评论相关请求"""
        # 评论处理逻辑（简化版）
        return create_api_response(True, {"message": "Comments API - Not fully implemented"})

# 6. API 测试
print("\nAPI 测试:")

def test_rest_api():
    """测试 REST API"""

    api = RESTAPI()

    # 创建用户
    print("创建用户:")
    response = api.handle_request('POST', '/users', {
        'username': 'john_doe',
        'email': 'john@example.com'
    })
    print(json.dumps(response, indent=2, ensure_ascii=False))

    # 获取用户列表
    print("\n获取用户列表:")
    response = api.handle_request('GET', '/users')
    print(json.dumps(response, indent=2, ensure_ascii=False))

    # 创建文章
    print("\n创建文章:")
    user_id = response['data'][0]['id'] if response['data'] else 'mock_user_id'
    response = api.handle_request('POST', '/posts', {
        'title': '我的第一篇文章',
        'content': '这是文章的内容',
        'author_id': user_id
    })
    print(json.dumps(response, indent=2, ensure_ascii=False))

    # 获取文章列表
    print("\n获取文章列表:")
    response = api.handle_request('GET', '/posts')
    print(json.dumps(response, indent=2, ensure_ascii=False))

test_rest_api()

# 7. API 文档
print("\nAPI 文档:")

def generate_api_documentation():
    """生成 API 文档"""

    documentation = """
# REST API 文档

## 基础信息
- **版本**: 1.0
- **基础URL**: https://api.example.com
- **认证**: Bearer Token

## 资源

### 用户 (Users)
管理用户账户

#### 获取用户列表
```
GET /users
```

#### 获取特定用户
```
GET /users/{user_id}
```

#### 创建用户
```
POST /users
Content-Type: application/json

{
  "username": "john_doe",
  "email": "john@example.com"
}
```

### 文章 (Posts)
管理文章内容

#### 获取文章列表
```
GET /posts
```

#### 获取特定文章
```
GET /posts/{post_id}
```

#### 创建文章
```
POST /posts
Content-Type: application/json

{
  "title": "文章标题",
  "content": "文章内容",
  "author_id": "用户ID"
}
```

## 响应格式
所有 API 响应都遵循以下格式：

```json
{
  "success": true,
  "timestamp": "2025-01-30T12:00:00",
  "version": "1.0",
  "data": { ... },
  "message": "操作成功"
}
```

## 错误处理
错误响应格式：

```json
{
  "success": false,
  "timestamp": "2025-01-30T12:00:00",
  "version": "1.0",
  "errors": ["错误信息"],
  "message": "操作失败"
}
```

## 状态码
- `200` - 请求成功
- `201` - 资源创建成功
- `400` - 请求参数错误
- `401` - 未授权
- `404` - 资源不存在
- `500` - 服务器内部错误
"""

    print(documentation)

generate_api_documentation()

print("REST API 设计基础示例已定义")
```

## 29.5 Web 安全基础

### 29.5.1 常见安全威胁

```python
# Web 安全基础
print("Web 安全基础:")

import hashlib
import hmac
import secrets
import bcrypt
import jwt
from datetime import datetime, timedelta
import re
from typing import Dict, List, Optional
import json

# 1. 密码安全
print("密码安全:")

class PasswordSecurity:
    """密码安全管理"""

    @staticmethod
    def hash_password(password: str) -> str:
        """使用 bcrypt 哈希密码"""
        try:
            # 生成盐并哈希
            salt = bcrypt.gensalt()
            hashed = bcrypt.hashpw(password.encode('utf-8'), salt)
            return hashed.decode('utf-8')
        except:
            # 如果 bcrypt 不可用，使用简单的哈希（仅用于演示）
            return hashlib.sha256(password.encode()).hexdigest()

    @staticmethod
    def verify_password(password: str, hashed: str) -> bool:
        """验证密码"""
        try:
            return bcrypt.checkpw(password.encode('utf-8'), hashed.encode('utf-8'))
        except:
            # 如果 bcrypt 不可用，使用简单的验证（仅用于演示）
            return hashlib.sha256(password.encode()).hexdigest() == hashed

    @staticmethod
    def generate_secure_password(length: int = 12) -> str:
        """生成安全密码"""
        chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*"
        return ''.join(secrets.choice(chars) for _ in range(length))

    @staticmethod
    def validate_password_strength(password: str) -> Dict[str, bool]:
        """验证密码强度"""
        checks = {
            'length': len(password) >= 8,
            'uppercase': bool(re.search(r'[A-Z]', password)),
            'lowercase': bool(re.search(r'[a-z]', password)),
            'digits': bool(re.search(r'\d', password)),
            'special': bool(re.search(r'[!@#$%^&*()_+\-=\[\]{};\':"\\|,.<>\/?]', password))
        }

        checks['overall'] = all(checks.values())
        return checks

# 2. JWT 令牌
print("\nJWT 令牌:")

class JWTManager:
    """JWT 令牌管理"""

    def __init__(self, secret_key: str):
        self.secret_key = secret_key

    def generate_token(self, payload: Dict, expires_in: int = 3600) -> str:
        """生成 JWT 令牌"""
        # 添加过期时间
        payload['exp'] = datetime.utcnow() + timedelta(seconds=expires_in)
        payload['iat'] = datetime.utcnow()

        # 编码头部
        header = {
            'alg': 'HS256',
            'typ': 'JWT'
        }

        # Base64 编码
        import base64

        def base64url_encode(data: bytes) -> str:
            return base64.urlsafe_b64encode(data).rstrip(b'=').decode('utf-8')

        # 创建令牌
        header_b64 = base64url_encode(json.dumps(header).encode('utf-8'))
        payload_b64 = base64url_encode(json.dumps(payload).encode('utf-8'))

        # 创建签名
        message = f"{header_b64}.{payload_b64}"
        signature = hmac.new(
            self.secret_key.encode('utf-8'),
            message.encode('utf-8'),
            hashlib.sha256
        ).digest()
        signature_b64 = base64url_encode(signature)

        return f"{header_b64}.{payload_b64}.{signature_b64}"

    def verify_token(self, token: str) -> Optional[Dict]:
        """验证 JWT 令牌"""
        try:
            import base64

            def base64url_decode(data: str) -> bytes:
                # 添加填充
                missing_padding = len(data) % 4
                if missing_padding:
                    data += '=' * (4 - missing_padding)
                return base64.urlsafe_b64decode(data)

            # 分割令牌
            parts = token.split('.')
            if len(parts) != 3:
                return None

            header_b64, payload_b64, signature_b64 = parts

            # 验证签名
            message = f"{header_b64}.{payload_b64}"
            expected_signature = hmac.new(
                self.secret_key.encode('utf-8'),
                message.encode('utf-8'),
                hashlib.sha256
            ).digest()

            actual_signature = base64url_decode(signature_b64)

            if not hmac.compare_digest(expected_signature, actual_signature):
                return None

            # 解码载荷
            payload = json.loads(base64url_decode(payload_b64).decode('utf-8'))

            # 检查过期时间
            if 'exp' in payload:
                exp_time = datetime.fromtimestamp(payload['exp'])
                if datetime.utcnow() > exp_time:
                    return None

            return payload

        except Exception:
            return None

# 3. 输入验证和清理
print("\n输入验证和清理:")

class InputValidator:
    """输入验证器"""

    @staticmethod
    def sanitize_string(input_str: str, max_length: int = 1000) -> str:
        """清理字符串输入"""
        if not input_str:
            return ""

        # 移除前后的空白字符
        cleaned = input_str.strip()

        # 限制长度
        if len(cleaned) > max_length:
            cleaned = cleaned[:max_length]

        # 移除潜在的 XSS 字符
        cleaned = re.sub(r'[<>]', '', cleaned)

        return cleaned

    @staticmethod
    def validate_email(email: str) -> bool:
        """验证邮箱格式"""
        pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
        return bool(re.match(pattern, email))

    @staticmethod
    def validate_username(username: str) -> bool:
        """验证用户名"""
        if not username or len(username) < 3 or len(username) > 20:
            return False

        # 只允许字母、数字、下划线
        pattern = r'^[a-zA-Z0-9_]+$'
        return bool(re.match(pattern, username))

    @staticmethod
    def validate_sql_injection(input_str: str) -> bool:
        """检查 SQL 注入"""
        # 简单的 SQL 注入检测
        sql_patterns = [
            r';\s*--',  # SQL 注释
            r';\s*/\*',  # SQL 注释
            r'union\s+select',  # UNION SELECT
            r'1=1',  # 经典注入
            r'or\s+1=1',  # OR 1=1
        ]

        input_lower = input_str.lower()
        for pattern in sql_patterns:
            if re.search(pattern, input_lower):
                return False
        return True

    @staticmethod
    def validate_xss(input_str: str) -> bool:
        """检查 XSS 攻击"""
        xss_patterns = [
            r'<script[^>]*>.*?</script>',  # script 标签
            r'javascript:',  # javascript 协议
            r'on\w+\s*=',  # 事件处理器
            r'<iframe[^>]*>.*?</iframe>',  # iframe 标签
        ]

        input_lower = input_str.lower()
        for pattern in xss_patterns:
            if re.search(pattern, input_lower, re.IGNORECASE | re.DOTALL):
                return False
        return True

# 4. CSRF 保护
print("\nCSRF 保护:")

class CSRFProtection:
    """CSRF 保护"""

    @staticmethod
    def generate_token() -> str:
        """生成 CSRF 令牌"""
        return secrets.token_hex(32)

    @staticmethod
    def validate_token(session_token: str, request_token: str) -> bool:
        """验证 CSRF 令牌"""
        if not session_token or not request_token:
            return False

        return hmac.compare_digest(session_token, request_token)

# 5. 速率限制
print("\n速率限制:")

class RateLimiter:
    """速率限制器"""

    def __init__(self, max_requests: int = 100, window_seconds: int = 60):
        self.max_requests = max_requests
        self.window_seconds = window_seconds
        self.requests = {}  # 存储请求记录

    def is_allowed(self, client_id: str) -> bool:
        """检查是否允许请求"""
        current_time = datetime.now()

        # 获取或创建客户端记录
        if client_id not in self.requests:
            self.requests[client_id] = []

        # 清理过期的请求
        self.requests[client_id] = [
            req_time for req_time in self.requests[client_id]
            if (current_time - req_time).seconds < self.window_seconds
        ]

        # 检查请求数量
        if len(self.requests[client_id]) >= self.max_requests:
            return False

        # 记录新请求
        self.requests[client_id].append(current_time)
        return True

# 6. HTTPS 和 SSL/TLS
print("\nHTTPS 和 SSL/TLS:")

def ssl_tls_best_practices():
    """SSL/TLS 最佳实践"""

    practices = {
        "使用强加密": {
            "描述": "使用 TLS 1.2 或更高版本",
            "原因": "防止降级攻击和弱加密"
        },
        "证书验证": {
            "描述": "始终验证服务器证书",
            "原因": "防止中间人攻击"
        },
        "HSTS 头": {
            "描述": "使用 HTTP Strict Transport Security",
            "原因": "强制使用 HTTPS"
        },
        "证书固定": {
            "描述": "固定服务器证书",
            "原因": "防止证书伪造"
        },
        "安全重定向": {
            "描述": "HTTP 重定向到 HTTPS",
            "原因": "防止协议降级"
        }
    }

    print("SSL/TLS 最佳实践:")
    for practice, details in practices.items():
        print(f"  {practice}:")
        print(f"    描述: {details['描述']}")
        print(f"    原因: {details['原因']}")

ssl_tls_best_practices()

# 7. 安全头信息
print("\n安全头信息:")

def security_headers():
    """安全头信息"""

    headers = {
        "Content-Security-Policy (CSP)": {
            "用途": "防止 XSS 攻击",
            "示例": "default-src 'self'; script-src 'self' 'unsafe-inline'"
        },
        "X-Frame-Options": {
            "用途": "防止点击劫持",
            "示例": "DENY 或 SAMEORIGIN"
        },
        "X-Content-Type-Options": {
            "用途": "防止 MIME 类型嗅探",
            "示例": "nosniff"
        },
        "Strict-Transport-Security (HSTS)": {
            "用途": "强制 HTTPS",
            "示例": "max-age=31536000; includeSubDomains"
        },
        "X-XSS-Protection": {
            "用途": "启用 XSS 过滤",
            "示例": "1; mode=block"
        },
        "Referrer-Policy": {
            "用途": "控制 Referer 头",
            "示例": "strict-origin-when-cross-origin"
        }
    }

    print("安全头信息:")
    for header, details in headers.items():
        print(f"  {header}:")
        print(f"    用途: {details['用途']}")
        print(f"    示例: {details['示例']}")

security_headers()

# 8. 安全日志记录
print("\n安全日志记录:")

class SecurityLogger:
    """安全日志记录器"""

    def __init__(self, log_file: str = 'security.log'):
        self.log_file = log_file

    def log_security_event(self, event_type: str, details: Dict, ip_address: str = None):
        """记录安全事件"""
        timestamp = datetime.now().isoformat()
        log_entry = {
            'timestamp': timestamp,
            'event_type': event_type,
            'details': details,
            'ip_address': ip_address or 'unknown'
        }

        # 写入日志文件
        try:
            with open(self.log_file, 'a', encoding='utf-8') as f:
                f.write(json.dumps(log_entry, ensure_ascii=False) + '\n')
        except Exception as e:
            print(f"日志记录失败: {e}")

    def log_failed_login(self, username: str, ip_address: str):
        """记录登录失败"""
        self.log_security_event('FAILED_LOGIN', {
            'username': username,
            'reason': 'invalid_credentials'
        }, ip_address)

    def log_suspicious_activity(self, activity: str, ip_address: str):
        """记录可疑活动"""
        self.log_security_event('SUSPICIOUS_ACTIVITY', {
            'activity': activity
        }, ip_address)

# 9. 综合安全示例
print("\n综合安全示例:")

def secure_web_application_demo():
    """安全 Web 应用演示"""

    # 初始化安全组件
    password_manager = PasswordSecurity()
    jwt_manager = JWTManager('your-secret-key-here')
    validator = InputValidator()
    csrf_protector = CSRFProtection()
    rate_limiter = RateLimiter(max_requests=10, window_seconds=60)
    security_logger = SecurityLogger()

    print("=== 密码管理演示 ===")

    # 生成安全密码
    secure_password = password_manager.generate_secure_password()
    print(f"生成的密码: {secure_password}")

    # 验证密码强度
    strength = password_manager.validate_password_strength(secure_password)
    print(f"密码强度检查: {strength}")

    # 哈希密码
    hashed = password_manager.hash_password(secure_password)
    print(f"密码哈希: {hashed[:20]}...")

    # 验证密码
    is_valid = password_manager.verify_password(secure_password, hashed)
    print(f"密码验证: {'成功' if is_valid else '失败'}")

    print("\n=== JWT 令牌演示 ===")

    # 生成令牌
    payload = {'user_id': '123', 'username': 'john_doe'}
    token = jwt_manager.generate_token(payload)
    print(f"生成的令牌: {token[:50]}...")

    # 验证令牌
    decoded = jwt_manager.verify_token(token)
    print(f"令牌验证: {'成功' if decoded else '失败'}")
    if decoded:
        print(f"解码内容: {decoded}")

    print("\n=== 输入验证演示 ===")

    # 测试输入验证
    test_inputs = [
        "<script>alert('xss')</script>",
        "normal_user@example.com",
        "user123",
        "SELECT * FROM users; --"
    ]

    for test_input in test_inputs:
        email_valid = validator.validate_email(test_input)
        xss_safe = validator.validate_xss(test_input)
        sql_safe = validator.validate_sql_injection(test_input)

        print(f"输入: {test_input}")
        print(f"  邮箱格式: {'有效' if email_valid else '无效'}")
        print(f"  XSS 安全: {'安全' if xss_safe else '不安全'}")
        print(f"  SQL 注入: {'安全' if sql_safe else '不安全'}")

    print("\n=== 速率限制演示 ===")

    # 测试速率限制
    client_id = '192.168.1.100'
    for i in range(15):
        allowed = rate_limiter.is_allowed(client_id)
        print(f"请求 {i+1}: {'允许' if allowed else '拒绝'}")

        if not allowed:
            security_logger.log_suspicious_activity('rate_limit_exceeded', client_id)
            break

    print("\n=== 安全日志演示 ===")

    # 记录安全事件
    security_logger.log_failed_login('hacker', '10.0.0.1')
    security_logger.log_suspicious_activity('multiple_failed_logins', '10.0.0.1')

    print("安全事件已记录到日志文件")

secure_web_application_demo()

print("Web 安全基础示例已定义")
```

## 总结

本章介绍了 Web 应用的基础知识：
- 掌握 HTTP 协议，包括请求方法、状态码、头信息和 Cookie
- 学会使用 Flask 框架构建 Web 应用，包括路由、模板、表单和数据库集成
- 理解 Django 框架的核心概念和 MTV 模式
- 掌握 REST API 设计原则和实现方法
- 了解 Web 安全基础知识，包括密码安全、JWT 认证、输入验证和常见威胁防护

通过本章的学习，你应该能够：
1. 使用 HTTP 协议进行客户端-服务器通信
2. 使用 Flask 构建简单的 Web 应用
3. 理解 Django 的项目结构和开发模式
4. 设计和实现 RESTful API
5. 实施基本的 Web 安全措施

这是教程的最后一章！恭喜你完成了整个 Python 3.11 教程的学习。从基础语法到高级特性，从数据处理到 Web 开发，你现在已经掌握了 Python 编程的核心技能。

接下来的步骤：
1. 实践所学知识，构建自己的项目
2. 深入学习特定领域的框架和库
3. 参与开源项目，提升编程水平
4. 关注 Python 社区的最新发展和最佳实践

祝你在 Python 编程之路上越走越远！
