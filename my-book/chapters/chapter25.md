# 第25章：并发编程

## 25.1 多线程编程基础

### 25.1.1 线程基础概念

```python
# 线程基础概念
print("线程基础概念:")

import threading
import time

# 1. 创建和启动线程
def worker(name, delay):
    """工作线程函数"""
    print(f"线程 {name} 开始执行")
    time.sleep(delay)
    print(f"线程 {name} 执行完成")

print("创建和启动线程:")

# 创建线程
thread1 = threading.Thread(target=worker, args=("Thread-1", 2))
thread2 = threading.Thread(target=worker, args=("Thread-2", 1))

# 启动线程
print("启动线程...")
thread1.start()
thread2.start()

# 等待线程完成
thread1.join()
thread2.join()

print("所有线程执行完成")

# 2. 线程的状态
def thread_states():
    """演示线程状态"""
    def long_running_task():
        print("任务开始")
        time.sleep(3)
        print("任务完成")

    thread = threading.Thread(target=long_running_task)

    print(f"创建后状态: {thread.is_alive()}")
    thread.start()
    print(f"启动后状态: {thread.is_alive()}")

    thread.join()
    print(f"完成后状态: {thread.is_alive()}")

print("\n线程状态:")
thread_states()

# 3. 线程的基本属性
def thread_properties():
    """线程属性演示"""
    current_thread = threading.current_thread()
    print(f"当前线程: {current_thread.name}")
    print(f"线程ID: {current_thread.ident}")
    print(f"是否守护线程: {current_thread.daemon}")
    print(f"线程存活: {current_thread.is_alive()}")

print("\n线程属性:")
thread_properties()

# 4. 守护线程
def daemon_threads():
    """守护线程演示"""
    def daemon_worker():
        while True:
            print("守护线程运行中...")
            time.sleep(1)

    def normal_worker():
        for i in range(3):
            print(f"普通线程: {i}")
            time.sleep(1)

    # 创建守护线程
    daemon_thread = threading.Thread(target=daemon_worker)
    daemon_thread.daemon = True

    # 创建普通线程
    normal_thread = threading.Thread(target=normal_worker)

    daemon_thread.start()
    normal_thread.start()

    # 等待普通线程完成
    normal_thread.join()

    print("普通线程完成，程序退出（守护线程也会被终止）")

print("\n守护线程:")
# 注意：守护线程示例在实际运行时会持续运行，这里只是演示代码结构
# daemon_threads()

# 5. 线程的命名
def named_threads():
    """命名线程"""
    def named_worker():
        current = threading.current_thread()
        print(f"线程名: {current.name}, ID: {current.ident}")

    # 创建带名称的线程
    threads = []
    for i in range(3):
        thread = threading.Thread(target=named_worker, name=f"Worker-{i+1}")
        threads.append(thread)
        thread.start()

    # 等待所有线程
    for thread in threads:
        thread.join()

print("\n命名线程:")
named_threads()
```

### 25.1.2 线程同步

```python
# 线程同步
print("线程同步:")

import threading
import time

# 1. Lock 互斥锁
class Counter:
    """线程安全的计数器"""

    def __init__(self):
        self.value = 0
        self.lock = threading.Lock()

    def increment(self):
        with self.lock:
            current = self.value
            time.sleep(0.01)  # 模拟处理时间
            self.value = current + 1

    def get_value(self):
        with self.lock:
            return self.value

def test_lock():
    """测试Lock"""
    counter = Counter()
    threads = []

    def worker():
        for _ in range(100):
            counter.increment()

    # 创建10个线程
    for i in range(10):
        thread = threading.Thread(target=worker)
        threads.append(thread)
        thread.start()

    # 等待所有线程
    for thread in threads:
        thread.join()

    print(f"最终计数: {counter.get_value()} (期望: 1000)")

print("Lock 互斥锁:")
test_lock()

# 2. RLock 可重入锁
class RecursiveCounter:
    """使用RLock的递归计数器"""

    def __init__(self):
        self.value = 0
        self.rlock = threading.RLock()

    def recursive_increment(self, n):
        with self.rlock:
            if n > 0:
                self.value += 1
                self.recursive_increment(n - 1)

    def get_value(self):
        with self.rlock:
            return self.value

def test_rlock():
    """测试RLock"""
    counter = RecursiveCounter()

    def worker():
        counter.recursive_increment(50)

    threads = []
    for i in range(5):
        thread = threading.Thread(target=worker)
        threads.append(thread)
        thread.start()

    for thread in threads:
        thread.join()

    print(f"递归计数结果: {counter.get_value()} (期望: 250)")

print("\nRLock 可重入锁:")
test_rlock()

# 3. Condition 条件变量
class BoundedBuffer:
    """有界缓冲区"""

    def __init__(self, size):
        self.size = size
        self.buffer = []
        self.condition = threading.Condition()

    def put(self, item):
        with self.condition:
            while len(self.buffer) >= self.size:
                print("缓冲区满，等待消费...")
                self.condition.wait()

            self.buffer.append(item)
            print(f"生产: {item}, 缓冲区大小: {len(self.buffer)}")
            self.condition.notify()

    def get(self):
        with self.condition:
            while not self.buffer:
                print("缓冲区空，等待生产...")
                self.condition.wait()

            item = self.buffer.pop(0)
            print(f"消费: {item}, 缓冲区大小: {len(self.buffer)}")
            self.condition.notify()
            return item

def test_condition():
    """测试Condition"""
    buffer = BoundedBuffer(3)

    def producer():
        for i in range(5):
            buffer.put(f"Item-{i}")
            time.sleep(0.1)

    def consumer():
        for i in range(5):
            item = buffer.get()
            time.sleep(0.2)

    producer_thread = threading.Thread(target=producer)
    consumer_thread = threading.Thread(target=consumer)

    producer_thread.start()
    consumer_thread.start()

    producer_thread.join()
    consumer_thread.join()

print("\nCondition 条件变量:")
test_condition()

# 4. Semaphore 信号量
class ResourcePool:
    """资源池"""

    def __init__(self, size):
        self.size = size
        self.semaphore = threading.Semaphore(size)
        self.resources = [f"Resource-{i}" for i in range(size)]

    def acquire_resource(self):
        self.semaphore.acquire()
        with threading.Lock():
            if self.resources:
                return self.resources.pop()
            return None

    def release_resource(self, resource):
        with threading.Lock():
            self.resources.append(resource)
        self.semaphore.release()

def test_semaphore():
    """测试Semaphore"""
    pool = ResourcePool(3)

    def worker(worker_id):
        resource = pool.acquire_resource()
        print(f"Worker {worker_id} 获取到: {resource}")
        time.sleep(1)
        pool.release_resource(resource)
        print(f"Worker {worker_id} 释放: {resource}")

    threads = []
    for i in range(6):
        thread = threading.Thread(target=worker, args=(i,))
        threads.append(thread)
        thread.start()

    for thread in threads:
        thread.join()

print("\nSemaphore 信号量:")
test_semaphore()

# 5. Event 事件
def test_event():
    """测试Event"""
    event = threading.Event()

    def waiter():
        print("等待事件...")
        event.wait()
        print("事件被触发！")

    def trigger():
        time.sleep(2)
        print("触发事件")
        event.set()

    waiter_thread = threading.Thread(target=waiter)
    trigger_thread = threading.Thread(target=trigger)

    waiter_thread.start()
    trigger_thread.start()

    waiter_thread.join()
    trigger_thread.join()

print("\nEvent 事件:")
test_event()
```

### 25.1.3 线程池

```python
# 线程池
print("线程池:")

from concurrent.futures import ThreadPoolExecutor, as_completed
import time

# 1. 基本线程池使用
def task(n):
    """模拟任务"""
    time.sleep(1)
    return n * n

def basic_thread_pool():
    """基本线程池使用"""
    print("基本线程池使用:")

    with ThreadPoolExecutor(max_workers=3) as executor:
        # 提交任务
        futures = [executor.submit(task, i) for i in range(5)]

        # 获取结果
        for future in as_completed(futures):
            result = future.result()
            print(f"任务结果: {result}")

print("基本线程池:")
start_time = time.time()
basic_thread_pool()
print(".2f")

# 2. 线程池映射
def task_with_exception(x):
    """可能抛出异常的任务"""
    if x == 3:
        raise ValueError(f"错误值: {x}")
    time.sleep(0.5)
    return x * 2

def thread_pool_map():
    """线程池映射"""
    print("\n线程池映射:")

    with ThreadPoolExecutor(max_workers=3) as executor:
        numbers = [1, 2, 3, 4, 5]

        try:
            results = list(executor.map(task_with_exception, numbers))
            print(f"所有结果: {results}")
        except Exception as e:
            print(f"捕获异常: {e}")

thread_pool_map()

# 3. 线程池回调
def callback_example():
    """回调示例"""
    def task_with_callback(n):
        time.sleep(0.5)
        return n ** 2

    def on_complete(future):
        result = future.result()
        print(f"任务完成，结果: {result}")

    print("\n线程池回调:")

    with ThreadPoolExecutor(max_workers=2) as executor:
        futures = [executor.submit(task_with_callback, i) for i in range(4)]

        for future in futures:
            future.add_done_callback(on_complete)

        # 等待所有任务完成
        for future in futures:
            future.result()

callback_example()

# 4. 超时处理
def timeout_example():
    """超时处理"""
    def slow_task(delay):
        time.sleep(delay)
        return f"完成延迟 {delay} 秒"

    print("\n超时处理:")

    with ThreadPoolExecutor(max_workers=2) as executor:
        # 提交任务
        future1 = executor.submit(slow_task, 1)
        future2 = executor.submit(slow_task, 3)

        try:
            result1 = future1.result(timeout=2)
            print(f"任务1结果: {result1}")
        except TimeoutError:
            print("任务1超时")

        try:
            result2 = future2.result(timeout=2)
            print(f"任务2结果: {result2}")
        except TimeoutError:
            print("任务2超时")

timeout_example()

# 5. 取消任务
def cancellation_example():
    """任务取消"""
    def cancellable_task(name):
        for i in range(10):
            if threading.current_thread().stopped():
                print(f"任务 {name} 被取消")
                return
            print(f"任务 {name}: {i}")
            time.sleep(0.5)
        return f"任务 {name} 完成"

    print("\n任务取消:")

    with ThreadPoolExecutor(max_workers=2) as executor:
        future = executor.submit(cancellable_task, "Test")

        time.sleep(2)

        # 取消任务
        cancelled = future.cancel()
        print(f"任务取消结果: {cancelled}")

        if not cancelled:
            try:
                result = future.result()
                print(f"任务结果: {result}")
            except Exception as e:
                print(f"任务异常: {e}")

cancellation_example()
```

## 25.2 多进程编程

### 25.2.1 进程基础

```python
# 进程基础
print("进程基础:")

import multiprocessing
import os
import time

# 1. 创建进程
def process_worker(name):
    """进程工作函数"""
    print(f"进程 {name} 开始执行，PID: {os.getpid()}")
    time.sleep(2)
    print(f"进程 {name} 执行完成")

def basic_processes():
    """基本进程创建"""
    print("基本进程创建:")

    # 创建进程
    process1 = multiprocessing.Process(target=process_worker, args=("Process-1",))
    process2 = multiprocessing.Process(target=process_worker, args=("Process-2",))

    print(f"主进程PID: {os.getpid()}")

    # 启动进程
    process1.start()
    process2.start()

    # 等待进程完成
    process1.join()
    process2.join()

    print("所有进程执行完成")

print("创建进程:")
basic_processes()

# 2. 进程池
def pool_worker(n):
    """进程池工作函数"""
    return n * n

def process_pool():
    """进程池使用"""
    print("\n进程池:")

    with multiprocessing.Pool(processes=4) as pool:
        numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

        # 映射任务
        results = pool.map(pool_worker, numbers)
        print(f"映射结果: {results}")

        # 应用单个任务
        result = pool.apply(pool_worker, (10,))
        print(f"单个任务结果: {result}")

        # 异步应用
        async_result = pool.apply_async(pool_worker, (15,))
        print(f"异步结果: {async_result.get()}")

process_pool()

# 3. 进程间通信 - Queue
def queue_communication():
    """进程间队列通信"""
    def producer(queue):
        for i in range(5):
            item = f"Item-{i}"
            queue.put(item)
            print(f"生产: {item}")
            time.sleep(0.5)

    def consumer(queue):
        while True:
            try:
                item = queue.get(timeout=2)
                print(f"消费: {item}")
            except:
                print("队列为空，消费者退出")
                break

    print("\n进程间通信 - Queue:")

    queue = multiprocessing.Queue()

    producer_process = multiprocessing.Process(target=producer, args=(queue,))
    consumer_process = multiprocessing.Process(target=consumer, args=(queue,))

    producer_process.start()
    consumer_process.start()

    producer_process.join()
    consumer_process.join()

queue_communication()

# 4. 进程间通信 - Pipe
def pipe_communication():
    """进程间管道通信"""
    def sender(pipe):
        messages = ["Hello", "World", "Python", "Multiprocessing"]
        for msg in messages:
            pipe.send(msg)
            print(f"发送: {msg}")
            time.sleep(0.5)
        pipe.send(None)  # 发送结束信号

    def receiver(pipe):
        while True:
            msg = pipe.recv()
            if msg is None:
                print("接收完成")
                break
            print(f"接收: {msg}")

    print("\n进程间通信 - Pipe:")

    parent_conn, child_conn = multiprocessing.Pipe()

    sender_process = multiprocessing.Process(target=sender, args=(child_conn,))
    receiver_process = multiprocessing.Process(target=receiver, args=(parent_conn,))

    sender_process.start()
    receiver_process.start()

    sender_process.join()
    receiver_process.join()

pipe_communication()

# 5. 共享内存
def shared_memory():
    """共享内存"""
    def worker(shared_value, lock):
        with lock:
            shared_value.value += 1
            print(f"进程 {os.getpid()} 增加值: {shared_value.value}")

    print("\n共享内存:")

    # 创建共享值
    shared_value = multiprocessing.Value('i', 0)
    lock = multiprocessing.Lock()

    processes = []
    for i in range(5):
        p = multiprocessing.Process(target=worker, args=(shared_value, lock))
        processes.append(p)
        p.start()

    for p in processes:
        p.join()

    print(f"最终共享值: {shared_value.value}")

shared_memory()

# 6. 进程同步
def process_synchronization():
    """进程同步"""
    def synchronized_worker(semaphore, worker_id):
        print(f"进程 {worker_id} 等待信号量...")
        semaphore.acquire()
        print(f"进程 {worker_id} 获取信号量，开始工作")
        time.sleep(1)
        print(f"进程 {worker_id} 工作完成，释放信号量")
        semaphore.release()

    print("\n进程同步:")

    semaphore = multiprocessing.Semaphore(2)  # 允许2个进程同时访问

    processes = []
    for i in range(5):
        p = multiprocessing.Process(target=synchronized_worker, args=(semaphore, i))
        processes.append(p)
        p.start()

    for p in processes:
        p.join()

process_synchronization()
```

### 25.2.2 进程池高级用法

```python
# 进程池高级用法
print("进程池高级用法:")

import multiprocessing
from concurrent.futures import ProcessPoolExecutor
import time

# 1. ProcessPoolExecutor
def process_pool_executor():
    """ProcessPoolExecutor 使用"""
    def cpu_intensive_task(n):
        """CPU密集型任务"""
        result = 0
        for i in range(n * 100000):
            result += i ** 2
        return result

    print("ProcessPoolExecutor:")

    with ProcessPoolExecutor(max_workers=4) as executor:
        numbers = [1, 2, 3, 4, 5]

        # 提交任务
        futures = [executor.submit(cpu_intensive_task, n) for n in numbers]

        # 获取结果
        for future in concurrent.futures.as_completed(futures):
            result = future.result()
            print(f"任务完成，结果长度: {len(str(result))}")

process_pool_executor()

# 2. 进程池错误处理
def error_handling():
    """错误处理"""
    def task_that_may_fail(n):
        if n == 3:
            raise ValueError(f"任务 {n} 失败")
        return n * 2

    print("\n进程池错误处理:")

    with multiprocessing.Pool(processes=3) as pool:
        numbers = [1, 2, 3, 4, 5]

        try:
            results = pool.map(task_that_may_fail, numbers)
            print(f"结果: {results}")
        except Exception as e:
            print(f"捕获异常: {e}")

error_handling()

# 3. 进程池回调
def pool_callbacks():
    """进程池回调"""
    def task_with_callback(n):
        time.sleep(0.5)
        return n ** 2

    def success_callback(result):
        print(f"成功: {result}")

    def error_callback(error):
        print(f"错误: {error}")

    print("\n进程池回调:")

    with multiprocessing.Pool(processes=2) as pool:
        numbers = [1, 2, 3, 4]

        results = []
        for n in numbers:
            result = pool.apply_async(
                task_with_callback,
                (n,),
                callback=success_callback,
                error_callback=error_callback
            )
            results.append(result)

        # 等待所有任务完成
        for result in results:
            result.get()

pool_callbacks()

# 4. 进程池分块处理
def chunk_processing():
    """分块处理大数据"""
    def process_chunk(chunk):
        """处理数据块"""
        return [x * 2 for x in chunk]

    print("\n进程池分块处理:")

    # 大数据集
    data = list(range(1000))

    # 分块大小
    chunk_size = 100
    chunks = [data[i:i + chunk_size] for i in range(0, len(data), chunk_size)]

    with multiprocessing.Pool(processes=4) as pool:
        # 并行处理每个块
        results = pool.map(process_chunk, chunks)

        # 合并结果
        final_result = []
        for chunk_result in results:
            final_result.extend(chunk_result)

        print(f"处理了 {len(final_result)} 个数据项")
        print(f"前10个结果: {final_result[:10]}")

chunk_processing()

# 5. 进程池资源管理
def resource_management():
    """资源管理"""
    def resource_intensive_task(task_id, shared_resource):
        """资源密集型任务"""
        print(f"任务 {task_id} 开始，PID: {os.getpid()}")

        # 模拟资源使用
        time.sleep(1)

        with shared_resource:
            print(f"任务 {task_id} 使用共享资源")

        print(f"任务 {task_id} 完成")
        return f"任务 {task_id} 结果"

    print("\n进程池资源管理:")

    # 共享资源
    resource_lock = multiprocessing.Lock()

    with multiprocessing.Pool(processes=3) as pool:
        tasks = [(i, resource_lock) for i in range(6)]

        # 使用starmap并行执行
        results = pool.starmap(resource_intensive_task, tasks)

        print(f"所有任务结果: {results}")

resource_management()
```

## 25.3 异步编程

### 25.3.1 asyncio 基础

```python
# asyncio 基础
print("asyncio 基础:")

import asyncio
import time

# 1. 基本协程
async def basic_coroutine():
    """基本协程"""
    print("协程开始")
    await asyncio.sleep(1)
    print("协程结束")
    return "完成"

async def run_basic_coroutine():
    """运行基本协程"""
    print("运行基本协程:")

    result = await basic_coroutine()
    print(f"结果: {result}")

# asyncio.run(run_basic_coroutine())

# 2. 并发执行协程
async def concurrent_coroutines():
    """并发执行协程"""
    async def task(name, delay):
        print(f"任务 {name} 开始")
        await asyncio.sleep(delay)
        print(f"任务 {name} 完成")
        return f"任务 {name} 结果"

    print("\n并发执行协程:")

    # 创建任务
    task1 = asyncio.create_task(task("A", 2))
    task2 = asyncio.create_task(task("B", 1))
    task3 = asyncio.create_task(task("C", 3))

    # 等待所有任务完成
    results = await asyncio.gather(task1, task2, task3)
    print(f"所有结果: {results}")

# asyncio.run(concurrent_coroutines())

# 3. 异步上下文管理器
class AsyncContextManager:
    """异步上下文管理器"""

    async def __aenter__(self):
        print("进入异步上下文")
        await asyncio.sleep(0.5)
        return self

    async def __aexit__(self, exc_type, exc_val, exc_tb):
        print("退出异步上下文")
        await asyncio.sleep(0.5)

async def async_context_example():
    """异步上下文管理器示例"""
    print("\n异步上下文管理器:")

    async with AsyncContextManager() as manager:
        print("在异步上下文中")
        await asyncio.sleep(1)

# asyncio.run(async_context_example())

# 4. 异步迭代器
class AsyncIterator:
    """异步迭代器"""

    def __init__(self, data):
        self.data = data
        self.index = 0

    def __aiter__(self):
        return self

    async def __anext__(self):
        if self.index >= len(self.data):
            raise StopAsyncIteration

        await asyncio.sleep(0.1)  # 模拟异步操作
        value = self.data[self.index]
        self.index += 1
        return value

async def async_iterator_example():
    """异步迭代器示例"""
    print("\n异步迭代器:")

    async_iter = AsyncIterator([1, 2, 3, 4, 5])

    async for value in async_iter:
        print(f"异步迭代值: {value}")

# asyncio.run(async_iterator_example())

# 5. 异步生成器
async def async_generator():
    """异步生成器"""
    for i in range(5):
        await asyncio.sleep(0.5)
        yield i * 2

async def async_generator_example():
    """异步生成器示例"""
    print("\n异步生成器:")

    async for value in async_generator():
        print(f"异步生成值: {value}")

# asyncio.run(async_generator_example())

# 6. 任务组
async def task_group_example():
    """任务组示例"""
    async def worker(name):
        print(f"Worker {name} 开始")
        await asyncio.sleep(1)
        print(f"Worker {name} 完成")
        return f"Worker {name} 结果"

    print("\n任务组:")

    async with asyncio.TaskGroup() as tg:
        task1 = tg.create_task(worker("A"))
        task2 = tg.create_task(worker("B"))
        task3 = tg.create_task(worker("C"))

    # 任务已完成
    print(f"任务1结果: {task1.result()}")
    print(f"任务2结果: {task2.result()}")
    print(f"任务3结果: {task3.result()}")

# asyncio.run(task_group_example())
```

### 25.3.2 异步IO操作

```python
# 异步IO操作
print("异步IO操作:")

import asyncio
import aiofiles
import aiohttp
import json

# 1. 异步文件操作
async def async_file_operations():
    """异步文件操作"""
    print("异步文件操作:")

    # 写入文件
    async with aiofiles.open('async_test.txt', 'w') as f:
        await f.write("Hello Async World\n")
        await f.write("This is async file IO\n")

    # 读取文件
    async with aiofiles.open('async_test.txt', 'r') as f:
        content = await f.read()
        print(f"文件内容:\n{content}")

    # 逐行读取
    async with aiofiles.open('async_test.txt', 'r') as f:
        async for line in f:
            print(f"行: {line.strip()}")

# asyncio.run(async_file_operations())

# 2. 异步HTTP请求
async def async_http_requests():
    """异步HTTP请求"""
    print("\n异步HTTP请求:")

    async with aiohttp.ClientSession() as session:
        # 并发请求多个URL
        urls = [
            'https://httpbin.org/get',
            'https://httpbin.org/uuid',
            'https://httpbin.org/json'
        ]

        async def fetch_url(url):
            async with session.get(url) as response:
                data = await response.json()
                return f"URL: {url}, Status: {response.status}"

        # 并发执行
        tasks = [fetch_url(url) for url in urls]
        results = await asyncio.gather(*tasks)

        for result in results:
            print(result)

# asyncio.run(async_http_requests())

# 3. 异步数据库操作模拟
class AsyncDatabase:
    """异步数据库模拟"""

    def __init__(self):
        self.data = {}

    async def connect(self):
        """模拟连接数据库"""
        await asyncio.sleep(0.5)
        print("数据库连接成功")

    async def insert(self, key, value):
        """插入数据"""
        await asyncio.sleep(0.1)
        self.data[key] = value
        print(f"插入: {key} = {value}")

    async def query(self, key):
        """查询数据"""
        await asyncio.sleep(0.1)
        return self.data.get(key)

    async def close(self):
        """关闭连接"""
        await asyncio.sleep(0.2)
        print("数据库连接关闭")

async def async_database_example():
    """异步数据库示例"""
    print("\n异步数据库操作:")

    db = AsyncDatabase()

    try:
        await db.connect()

        # 并发插入数据
        insert_tasks = [
            db.insert(f"key{i}", f"value{i}")
            for i in range(5)
        ]
        await asyncio.gather(*insert_tasks)

        # 并发查询数据
        query_tasks = [
            db.query(f"key{i}")
            for i in range(5)
        ]
        results = await asyncio.gather(*query_tasks)

        print(f"查询结果: {results}")

    finally:
        await db.close()

# asyncio.run(async_database_example())

# 4. 异步队列
async def async_queue_example():
    """异步队列示例"""
    print("\n异步队列:")

    queue = asyncio.Queue()

    async def producer():
        """生产者"""
        for i in range(5):
            item = f"Item-{i}"
            await queue.put(item)
            print(f"生产: {item}")
            await asyncio.sleep(0.5)

    async def consumer():
        """消费者"""
        while True:
            item = await queue.get()
            print(f"消费: {item}")
            queue.task_done()

            if item == "Item-4":  # 最后一个项目
                break

    # 启动生产者和消费者
    await asyncio.gather(producer(), consumer())

    # 等待队列处理完成
    await queue.join()
    print("队列处理完成")

# asyncio.run(async_queue_example())

# 5. 异步事件
async def async_event_example():
    """异步事件示例"""
    print("\n异步事件:")

    event = asyncio.Event()

    async def waiter():
        """等待者"""
        print("等待事件...")
        await event.wait()
        print("事件被触发！")

    async def trigger():
        """触发者"""
        await asyncio.sleep(2)
        print("触发事件")
        event.set()

    await asyncio.gather(waiter(), trigger())

# asyncio.run(async_event_example())

# 6. 异步锁
async def async_lock_example():
    """异步锁示例"""
    print("\n异步锁:")

    lock = asyncio.Lock()
    counter = 0

    async def increment_counter(name):
        nonlocal counter
        async with lock:
            print(f"{name} 获取锁")
            current = counter
            await asyncio.sleep(0.1)  # 模拟处理
            counter = current + 1
            print(f"{name} 释放锁，计数器: {counter}")

    # 并发执行
    tasks = [
        increment_counter(f"Task-{i}")
        for i in range(5)
    ]

    await asyncio.gather(*tasks)
    print(f"最终计数器值: {counter}")

# asyncio.run(async_lock_example())
```

### 25.3.3 异步编程模式

```python
# 异步编程模式
print("异步编程模式:")

import asyncio
from typing import List, Dict, Any

# 1. 生产者-消费者模式
class AsyncProducerConsumer:
    """异步生产者-消费者"""

    def __init__(self, queue_size=10):
        self.queue = asyncio.Queue(maxsize=queue_size)
        self.producers = []
        self.consumers = []

    async def producer(self, producer_id: int, items: List[Any]):
        """生产者"""
        for item in items:
            await self.queue.put(item)
            print(f"生产者 {producer_id} 生产: {item}")
            await asyncio.sleep(0.1)

        # 发送结束信号
        await self.queue.put(None)

    async def consumer(self, consumer_id: int):
        """消费者"""
        while True:
            item = await self.queue.get()
            if item is None:
                print(f"消费者 {consumer_id} 收到结束信号")
                break

            print(f"消费者 {consumer_id} 消费: {item}")
            await asyncio.sleep(0.2)

    async def run(self, producer_data: Dict[int, List[Any]]):
        """运行生产者-消费者系统"""
        # 启动消费者
        consumer_tasks = [
            asyncio.create_task(self.consumer(i))
            for i in range(2)
        ]

        # 启动生产者
        producer_tasks = [
            asyncio.create_task(self.producer(pid, data))
            for pid, data in producer_data.items()
        ]

        # 等待所有生产者完成
        await asyncio.gather(*producer_tasks)

        # 等待所有消费者完成
        await asyncio.gather(*consumer_tasks)

async def producer_consumer_example():
    """生产者-消费者示例"""
    print("生产者-消费者模式:")

    pc = AsyncProducerConsumer()

    producer_data = {
        1: ['A1', 'A2', 'A3'],
        2: ['B1', 'B2', 'B3', 'B4'],
        3: ['C1', 'C2']
    }

    await pc.run(producer_data)

# asyncio.run(producer_consumer_example())

# 2. 异步管道模式
class AsyncPipeline:
    """异步管道"""

    def __init__(self):
        self.stages = []

    def add_stage(self, stage_func):
        """添加处理阶段"""
        self.stages.append(stage_func)

    async def process(self, data):
        """处理数据"""
        result = data
        for stage in self.stages:
            result = await stage(result)
        return result

async def pipeline_example():
    """管道示例"""
    print("\n异步管道模式:")

    async def stage1(data):
        """阶段1: 数据清理"""
        await asyncio.sleep(0.1)
        return [x.strip().upper() for x in data if x.strip()]

    async def stage2(data):
        """阶段2: 数据转换"""
        await asyncio.sleep(0.1)
        return [f"[{x}]" for x in data]

    async def stage3(data):
        """阶段3: 数据过滤"""
        await asyncio.sleep(0.1)
        return [x for x in data if len(x) > 4]

    pipeline = AsyncPipeline()
    pipeline.add_stage(stage1)
    pipeline.add_stage(stage2)
    pipeline.add_stage(stage3)

    input_data = ["  hello  ", "", "world", "  ", "python  "]
    result = await pipeline.process(input_data)

    print(f"输入: {input_data}")
    print(f"输出: {result}")

# asyncio.run(pipeline_example())

# 3. 异步状态机
class AsyncStateMachine:
    """异步状态机"""

    def __init__(self):
        self.state = "idle"
        self.data = None

    async def transition(self, event):
        """状态转换"""
        if self.state == "idle" and event == "start":
            self.state = "running"
            print("状态: idle -> running")
            await self.process_data()

        elif self.state == "running" and event == "pause":
            self.state = "paused"
            print("状态: running -> paused")

        elif self.state == "paused" and event == "resume":
            self.state = "running"
            print("状态: paused -> running")
            await self.process_data()

        elif event == "stop":
            self.state = "stopped"
            print(f"状态: {self.state} -> stopped")

    async def process_data(self):
        """处理数据"""
        while self.state == "running":
            print(f"处理数据中... 状态: {self.state}")
            await asyncio.sleep(1)

async def state_machine_example():
    """状态机示例"""
    print("\n异步状态机:")

    sm = AsyncStateMachine()

    # 状态转换序列
    events = ["start", "pause", "resume", "stop"]

    for event in events:
        await sm.transition(event)
        await asyncio.sleep(0.5)

# asyncio.run(state_machine_example())

# 4. 异步观察者模式
class AsyncSubject:
    """异步主题"""

    def __init__(self):
        self.observers = []

    def attach(self, observer):
        """添加观察者"""
        self.observers.append(observer)

    def detach(self, observer):
        """移除观察者"""
        self.observers.remove(observer)

    async def notify(self, data):
        """通知所有观察者"""
        tasks = [
            observer.update(data)
            for observer in self.observers
        ]
        await asyncio.gather(*tasks)

class AsyncObserver:
    """异步观察者"""

    def __init__(self, name):
        self.name = name

    async def update(self, data):
        """更新方法"""
        await asyncio.sleep(0.1)
        print(f"观察者 {self.name} 收到数据: {data}")

async def observer_example():
    """观察者模式示例"""
    print("\n异步观察者模式:")

    subject = AsyncSubject()

    # 添加观察者
    observer1 = AsyncObserver("Observer1")
    observer2 = AsyncObserver("Observer2")
    observer3 = AsyncObserver("Observer3")

    subject.attach(observer1)
    subject.attach(observer2)
    subject.attach(observer3)

    # 发送通知
    await subject.notify("事件1")
    await subject.notify("事件2")

    # 移除一个观察者
    subject.detach(observer2)
    await subject.notify("事件3")

# asyncio.run(observer_example())

# 5. 异步策略模式
class AsyncStrategy:
    """异步策略基类"""

    async def execute(self, data):
        """执行策略"""
        raise NotImplementedError

class AsyncStrategyA(AsyncStrategy):
    """策略A"""

    async def execute(self, data):
        await asyncio.sleep(0.5)
        return f"策略A处理: {data.upper()}"

class AsyncStrategyB(AsyncStrategy):
    """策略B"""

    async def execute(self, data):
        await asyncio.sleep(0.3)
        return f"策略B处理: {data.lower()}"

class AsyncContext:
    """异步上下文"""

    def __init__(self, strategy: AsyncStrategy):
        self.strategy = strategy

    async def execute_strategy(self, data):
        """执行策略"""
        return await self.strategy.execute(data)

async def strategy_example():
    """策略模式示例"""
    print("\n异步策略模式:")

    data = "Hello World"

    # 使用策略A
    context = AsyncContext(AsyncStrategyA())
    result_a = await context.execute_strategy(data)
    print(result_a)

    # 使用策略B
    context = AsyncContext(AsyncStrategyB())
    result_b = await context.execute_strategy(data)
    print(result_b)

# asyncio.run(strategy_example())
```

## 25.4 并发设计模式

### 25.4.1 常见并发模式

```python
# 常见并发模式
print("常见并发模式:")

import threading
import multiprocessing
import asyncio
import time
import queue

# 1. 单例模式（线程安全）
class ThreadSafeSingleton:
    """线程安全的单例模式"""

    _instance = None
    _lock = threading.Lock()

    def __new__(cls):
        if cls._instance is None:
            with cls._lock:
                if cls._instance is None:
                    cls._instance = super().__new__(cls)
        return cls._instance

    def __init__(self):
        if not hasattr(self, 'initialized'):
            self.initialized = True
            self.data = []

    def add_data(self, item):
        with self._lock:
            self.data.append(item)

    def get_data(self):
        with self._lock:
            return self.data.copy()

def singleton_example():
    """单例模式示例"""
    print("线程安全的单例模式:")

    def worker(singleton, worker_id):
        for i in range(5):
            singleton.add_data(f"Worker-{worker_id}-Item-{i}")
            time.sleep(0.01)

    singleton = ThreadSafeSingleton()

    threads = []
    for i in range(3):
        thread = threading.Thread(target=worker, args=(singleton, i))
        threads.append(thread)
        thread.start()

    for thread in threads:
        thread.join()

    print(f"单例数据长度: {len(singleton.get_data())}")

singleton_example()

# 2. 对象池模式
class ObjectPool:
    """对象池模式"""

    def __init__(self, object_factory, pool_size=5):
        self.object_factory = object_factory
        self.pool_size = pool_size
        self.pool = queue.Queue(maxsize=pool_size)
        self.lock = threading.Lock()

        # 初始化对象池
        for _ in range(pool_size):
            self.pool.put(object_factory())

    def acquire(self):
        """获取对象"""
        return self.pool.get()

    def release(self, obj):
        """释放对象"""
        self.pool.put(obj)

def pool_example():
    """对象池示例"""
    print("\n对象池模式:")

    class ExpensiveObject:
        def __init__(self):
            self.id = id(self)
            time.sleep(0.1)  # 模拟创建耗时

        def process(self, data):
            return f"Object-{self.id}: {data}"

    pool = ObjectPool(ExpensiveObject, pool_size=3)

    def worker(worker_id):
        # 获取对象
        obj = pool.acquire()
        result = obj.process(f"Worker-{worker_id}")
        print(result)
        time.sleep(0.1)
        # 释放对象
        pool.release(obj)

    threads = []
    for i in range(6):
        thread = threading.Thread(target=worker, args=(i,))
        threads.append(thread)
        thread.start()

    for thread in threads:
        thread.join()

pool_example()

# 3. 生产者-消费者模式（多线程）
class ProducerConsumer:
    """生产者-消费者模式"""

    def __init__(self, queue_size=10):
        self.queue = queue.Queue(maxsize=queue_size)
        self.producer_condition = threading.Condition()
        self.consumer_condition = threading.Condition()

    def produce(self, item):
        """生产项目"""
        with self.producer_condition:
            while self.queue.full():
                self.producer_condition.wait()

            self.queue.put(item)
            print(f"生产: {item}")

            with self.consumer_condition:
                self.consumer_condition.notify()

    def consume(self):
        """消费项目"""
        with self.consumer_condition:
            while self.queue.empty():
                self.consumer_condition.wait()

            item = self.queue.get()
            print(f"消费: {item}")

            with self.producer_condition:
                self.producer_condition.notify()

            return item

def producer_consumer_threading():
    """多线程生产者-消费者"""
    print("\n多线程生产者-消费者模式:")

    pc = ProducerConsumer(queue_size=3)

    def producer():
        for i in range(5):
            pc.produce(f"Item-{i}")
            time.sleep(0.2)

    def consumer():
        for i in range(5):
            item = pc.consume()
            time.sleep(0.3)

    producer_thread = threading.Thread(target=producer)
    consumer_thread = threading.Thread(target=consumer)

    producer_thread.start()
    consumer_thread.start()

    producer_thread.join()
    consumer_thread.join()

producer_consumer_threading()

# 4. 工作线程池模式
class WorkerPool:
    """工作线程池"""

    def __init__(self, num_workers=4):
        self.num_workers = num_workers
        self.task_queue = queue.Queue()
        self.result_queue = queue.Queue()
        self.workers = []
        self.shutdown_event = threading.Event()

    def start(self):
        """启动工作线程"""
        for i in range(self.num_workers):
            worker = threading.Thread(target=self._worker_loop, args=(i,))
            worker.daemon = True
            worker.start()
            self.workers.append(worker)

    def submit(self, task_func, *args, **kwargs):
        """提交任务"""
        future = Future()
        self.task_queue.put((task_func, args, kwargs, future))
        return future

    def shutdown(self):
        """关闭线程池"""
        self.shutdown_event.set()
        for worker in self.workers:
            worker.join()

    def _worker_loop(self, worker_id):
        """工作线程循环"""
        while not self.shutdown_event.is_set():
            try:
                task_func, args, kwargs, future = self.task_queue.get(timeout=1)
                try:
                    result = task_func(*args, **kwargs)
                    future.set_result(result)
                except Exception as e:
                    future.set_exception(e)
                finally:
                    self.task_queue.task_done()
            except queue.Empty:
                continue

class Future:
    """简化的Future实现"""

    def __init__(self):
        self.result_value = None
        self.exception = None
        self.done = False
        self.condition = threading.Condition()

    def set_result(self, result):
        with self.condition:
            self.result_value = result
            self.done = True
            self.condition.notify_all()

    def set_exception(self, exception):
        with self.condition:
            self.exception = exception
            self.done = True
            self.condition.notify_all()

    def get(self, timeout=None):
        with self.condition:
            while not self.done:
                self.condition.wait(timeout)
                if not self.done:
                    raise TimeoutError("Future timeout")

            if self.exception:
                raise self.exception
            return self.result_value

def worker_pool_example():
    """工作线程池示例"""
    print("\n工作线程池模式:")

    def task(n):
        time.sleep(0.5)
        return n * n

    pool = WorkerPool(num_workers=3)
    pool.start()

    # 提交任务
    futures = [pool.submit(task, i) for i in range(5)]

    # 获取结果
    for i, future in enumerate(futures):
        result = future.get()
        print(f"任务 {i} 结果: {result}")

    pool.shutdown()

worker_pool_example()

# 5. 读写锁模式
class ReadWriteLock:
    """读写锁"""

    def __init__(self):
        self.readers = 0
        self.writers = 0
        self.condition = threading.Condition()
        self.write_condition = threading.Condition()

    def acquire_read(self):
        """获取读锁"""
        with self.condition:
            while self.writers > 0:
                self.condition.wait()
            self.readers += 1

    def release_read(self):
        """释放读锁"""
        with self.condition:
            self.readers -= 1
            if self.readers == 0:
                self.write_condition.notify()

    def acquire_write(self):
        """获取写锁"""
        with self.condition:
            while self.readers > 0 or self.writers > 0:
                if self.readers > 0:
                    self.condition.wait()
                else:
                    self.write_condition.wait()
            self.writers += 1

    def release_write(self):
        """释放写锁"""
        with self.condition:
            self.writers -= 1
            self.condition.notify_all()
            self.write_condition.notify()

class ReadWriteResource:
    """读写资源"""

    def __init__(self):
        self.data = {}
        self.lock = ReadWriteLock()

    def read(self, key):
        """读取数据"""
        self.lock.acquire_read()
        try:
            return self.data.get(key)
        finally:
            self.lock.release_read()

    def write(self, key, value):
        """写入数据"""
        self.lock.acquire_write()
        try:
            self.data[key] = value
        finally:
            self.lock.release_write()

def read_write_lock_example():
    """读写锁示例"""
    print("\n读写锁模式:")

    resource = ReadWriteResource()

    def reader(reader_id):
        for i in range(3):
            value = resource.read("key")
            print(f"读者 {reader_id} 读取: {value}")
            time.sleep(0.1)

    def writer(writer_id):
        for i in range(2):
            resource.write("key", f"Value-{writer_id}-{i}")
            print(f"写者 {writer_id} 写入完成")
            time.sleep(0.2)

    threads = []

    # 启动读者
    for i in range(3):
        thread = threading.Thread(target=reader, args=(i,))
        threads.append(thread)

    # 启动写者
    for i in range(2):
        thread = threading.Thread(target=writer, args=(i,))
        threads.append(thread)

    for thread in threads:
        thread.start()

    for thread in threads:
        thread.join()

read_write_lock_example()
```

### 25.4.2 并发最佳实践

```python
# 并发最佳实践
print("并发最佳实践:")

import threading
import multiprocessing
import asyncio
import time
from contextlib import contextmanager

# 1. 资源管理最佳实践
@contextmanager
def managed_resource(resource_type):
    """资源管理器"""
    resource = acquire_resource(resource_type)
    try:
        yield resource
    finally:
        release_resource(resource)

def acquire_resource(resource_type):
    """获取资源"""
    print(f"获取 {resource_type} 资源")
    time.sleep(0.1)
    return f"{resource_type}-instance"

def release_resource(resource):
    """释放资源"""
    print(f"释放 {resource} 资源")
    time.sleep(0.1)

def resource_management_best_practice():
    """资源管理最佳实践"""
    print("资源管理最佳实践:")

    def worker():
        with managed_resource("database") as db:
            print(f"使用资源: {db}")
            time.sleep(0.2)

    threads = []
    for i in range(3):
        thread = threading.Thread(target=worker)
        threads.append(thread)
        thread.start()

    for thread in threads:
        thread.join()

resource_management_best_practice()

# 2. 错误处理和恢复
class ResilientWorker:
    """有弹性的工作线程"""

    def __init__(self, max_retries=3):
        self.max_retries = max_retries
        self.failures = 0

    def do_work(self):
        """执行工作（可能失败）"""
        if self.failures < 2:  # 前两次故意失败
            self.failures += 1
            raise Exception(f"工作失败 #{self.failures}")

        return "工作成功完成"

    def execute_with_retry(self):
        """带重试的工作执行"""
        for attempt in range(self.max_retries):
            try:
                result = self.do_work()
                print(f"成功: {result}")
                return result
            except Exception as e:
                print(f"尝试 {attempt + 1} 失败: {e}")
                if attempt < self.max_retries - 1:
                    time.sleep(0.5 * (2 ** attempt))  # 指数退避
                else:
                    print("达到最大重试次数")

        return None

def error_handling_best_practice():
    """错误处理最佳实践"""
    print("\n错误处理和恢复:")

    worker = ResilientWorker()

    threads = []
    for i in range(3):
        thread = threading.Thread(target=lambda: worker.execute_with_retry())
        threads.append(thread)
        thread.start()

    for thread in threads:
        thread.join()

error_handling_best_practice()

# 3. 性能监控
class PerformanceMonitor:
    """性能监控器"""

    def __init__(self):
        self.metrics = {}
        self.lock = threading.Lock()

    def record_metric(self, name, value):
        """记录指标"""
        with self.lock:
            if name not in self.metrics:
                self.metrics[name] = []
            self.metrics[name].append((time.time(), value))

    def get_average(self, name):
        """获取平均值"""
        with self.lock:
            if name not in self.metrics:
                return 0
            values = [v for _, v in self.metrics[name]]
            return sum(values) / len(values) if values else 0

    def report(self):
        """生成报告"""
        with self.lock:
            print("性能报告:")
            for name, data in self.metrics.items():
                avg = self.get_average(name)
                count = len(data)
                print(f"  {name}: 平均={avg:.2f}, 样本数={count}")

def performance_monitoring():
    """性能监控示例"""
    print("\n性能监控:")

    monitor = PerformanceMonitor()

    def monitored_task(task_id):
        start_time = time.time()

        # 模拟工作
        time.sleep(0.1 * (task_id + 1))

        duration = time.time() - start_time
        monitor.record_metric("task_duration", duration)
        monitor.record_metric("task_count", 1)

        print(f"任务 {task_id} 完成，耗时: {duration:.2f}秒")

    threads = []
    for i in range(5):
        thread = threading.Thread(target=monitored_task, args=(i,))
        threads.append(thread)
        thread.start()

    for thread in threads:
        thread.join()

    monitor.report()

performance_monitoring()

# 4. 优雅关闭
class GracefulShutdown:
    """优雅关闭管理器"""

    def __init__(self):
        self.shutdown_event = threading.Event()
        self.workers = []
        self.lock = threading.Lock()

    def add_worker(self, worker_func, *args, **kwargs):
        """添加工作线程"""
        def wrapped_worker():
            try:
                worker_func(*args, **kwargs)
            except Exception as e:
                print(f"工作线程异常: {e}")
            finally:
                with self.lock:
                    if worker_func in self.workers:
                        self.workers.remove(worker_func)

        with self.lock:
            self.workers.append(worker_func)

        thread = threading.Thread(target=wrapped_worker)
        thread.daemon = True
        thread.start()
        return thread

    def shutdown(self, timeout=5):
        """优雅关闭"""
        print("开始优雅关闭...")

        # 设置关闭事件
        self.shutdown_event.set()

        # 等待工作线程完成
        start_time = time.time()
        while self.workers and (time.time() - start_time) < timeout:
            time.sleep(0.1)

        if self.workers:
            print(f"还有 {len(self.workers)} 个工作线程未完成")
        else:
            print("所有工作线程已完成")

def graceful_shutdown_example():
    """优雅关闭示例"""
    print("\n优雅关闭:")

    shutdown_manager = GracefulShutdown()

    def worker(name):
        for i in range(10):
            if shutdown_manager.shutdown_event.is_set():
                print(f"工作线程 {name} 收到关闭信号")
                break
            print(f"工作线程 {name}: {i}")
            time.sleep(0.5)
        print(f"工作线程 {name} 退出")

    # 启动工作线程
    for i in range(3):
        shutdown_manager.add_worker(worker, f"Worker-{i}")

    # 运行一段时间后关闭
    time.sleep(2)
    shutdown_manager.shutdown(timeout=3)

graceful_shutdown_example()

# 5. 并发安全的数据结构
class ConcurrentList:
    """并发安全的列表"""

    def __init__(self):
        self.items = []
        self.lock = threading.RLock()

    def append(self, item):
        """添加元素"""
        with self.lock:
            self.items.append(item)

    def remove(self, item):
        """移除元素"""
        with self.lock:
            if item in self.items:
                self.items.remove(item)

    def get_all(self):
        """获取所有元素"""
        with self.lock:
            return self.items.copy()

    def __len__(self):
        """获取长度"""
        with self.lock:
            return len(self.items)

def concurrent_data_structure():
    """并发安全数据结构示例"""
    print("\n并发安全的数据结构:")

    concurrent_list = ConcurrentList()

    def list_worker(worker_id):
        # 添加元素
        for i in range(5):
            concurrent_list.append(f"Worker-{worker_id}-Item-{i}")
            time.sleep(0.01)

        # 移除一些元素
        for i in range(2):
            concurrent_list.remove(f"Worker-{worker_id}-Item-{i}")

    threads = []
    for i in range(3):
        thread = threading.Thread(target=list_worker, args=(i,))
        threads.append(thread)
        thread.start()

    for thread in threads:
        thread.join()

    print(f"最终列表长度: {len(concurrent_list)}")
    all_items = concurrent_list.get_all()
    print(f"前10个元素: {all_items[:10]}")

concurrent_data_structure()

# 6. 避免死锁的策略
class DeadlockAvoidance:
    """死锁避免"""

    def __init__(self):
        self.lock1 = threading.Lock()
        self.lock2 = threading.Lock()

    def method_a(self):
        """方法A - 总是按相同顺序获取锁"""
        print("方法A: 获取锁1")
        with self.lock1:
            time.sleep(0.1)
            print("方法A: 获取锁2")
            with self.lock2:
                print("方法A: 执行临界区")
                time.sleep(0.1)

    def method_b(self):
        """方法B - 总是按相同顺序获取锁"""
        print("方法B: 获取锁1")
        with self.lock1:
            time.sleep(0.1)
            print("方法B: 获取锁2")
            with self.lock2:
                print("方法B: 执行临界区")
                time.sleep(0.1)

def deadlock_avoidance():
    """死锁避免示例"""
    print("\n死锁避免策略:")

    da = DeadlockAvoidance()

    thread1 = threading.Thread(target=da.method_a)
    thread2 = threading.Thread(target=da.method_b)

    thread1.start()
    thread2.start()

    thread1.join()
    thread2.join()

    print("死锁避免成功")

deadlock_avoidance()
```

## 总结

本章介绍了 Python 的并发编程：
- 掌握多线程编程的基础概念、线程同步机制和线程池使用
- 学会多进程编程、进程间通信和进程池的高级用法
- 深入理解异步编程和 asyncio 的核心概念和实际应用
- 掌握常见的并发设计模式和最佳实践

通过本章的学习，你应该能够：
1. 编写线程安全的代码，使用适当的同步机制
2. 根据应用场景选择合适的并发模型（多线程、多进程或异步）
3. 实现生产者-消费者、对象池等常见并发模式
4. 处理并发编程中的常见问题，如死锁、竞态条件等
5. 编写高性能、可维护的并发代码

下一章将介绍类型提示的基础知识。
