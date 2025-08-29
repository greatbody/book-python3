# 第28章：数据分析基础

## 28.1 NumPy 数组操作

### 28.1.1 NumPy 基础

```python
# NumPy 数组操作
print("NumPy 数组操作:")

# 注意：需要安装 numpy: pip install numpy
try:
    import numpy as np
except ImportError:
    print("需要安装 numpy: pip install numpy")
    np = None

if np:
    # 1. 数组创建
    print("数组创建:")

    # 从列表创建数组
    list_data = [1, 2, 3, 4, 5]
    arr1 = np.array(list_data)
    print(f"从列表创建: {arr1}, 类型: {type(arr1)}, 形状: {arr1.shape}")

    # 二维数组
    matrix_data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
    arr2 = np.array(matrix_data)
    print(f"二维数组: \n{arr2}")
    print(f"形状: {arr2.shape}, 维度: {arr2.ndim}, 大小: {arr2.size}")

    # 特殊数组
    zeros_arr = np.zeros((3, 4))
    ones_arr = np.ones((2, 3))
    full_arr = np.full((2, 2), 7)
    identity_arr = np.eye(3)

    print(f"零数组: \n{zeros_arr}")
    print(f"全1数组: \n{ones_arr}")
    print(f"全7数组: \n{full_arr}")
    print(f"单位矩阵: \n{identity_arr}")

    # 范围数组
    range_arr = np.arange(0, 10, 2)
    linspace_arr = np.linspace(0, 1, 5)
    print(f"范围数组: {range_arr}")
    print(f"等间距数组: {linspace_arr}")

    # 随机数组
    random_arr = np.random.rand(3, 3)
    randint_arr = np.random.randint(1, 10, (2, 3))
    print(f"随机数组: \n{random_arr}")
    print(f"随机整数数组: \n{randint_arr}")

    # 2. 数组属性
    print("\n数组属性:")

    test_arr = np.array([[1, 2, 3], [4, 5, 6]])
    print(f"数组: \n{test_arr}")
    print(f"形状 (shape): {test_arr.shape}")
    print(f"维度 (ndim): {test_arr.ndim}")
    print(f"大小 (size): {test_arr.size}")
    print(f"数据类型 (dtype): {test_arr.dtype}")
    print(f"元素大小 (itemsize): {test_arr.itemsize} bytes")
    print(f"总大小 (nbytes): {test_arr.nbytes} bytes")

    # 3. 数组索引和切片
    print("\n数组索引和切片:")

    arr = np.arange(12).reshape(3, 4)
    print(f"原始数组: \n{arr}")

    # 基本索引
    print(f"arr[0, 0]: {arr[0, 0]}")
    print(f"arr[1, 2]: {arr[1, 2]}")

    # 行切片
    print(f"第一行: {arr[0]}")
    print(f"最后一行: {arr[-1]}")

    # 列切片
    print(f"第一列: {arr[:, 0]}")
    print(f"第二列: {arr[:, 1]}")

    # 子数组切片
    print(f"前两行: \n{arr[:2]}")
    print(f"后两列: \n{arr[:, -2:]}")
    print(f"中间子数组: \n{arr[1:3, 1:3]}")

    # 步长切片
    print(f"每隔一行: \n{arr[::2]}")
    print(f"每隔一列: \n{arr[:, ::2]}")

    # 4. 数组操作
    print("\n数组操作:")

    a = np.array([1, 2, 3])
    b = np.array([4, 5, 6])

    print(f"a: {a}")
    print(f"b: {b}")

    # 算术运算
    print(f"a + b: {a + b}")
    print(f"a - b: {a - b}")
    print(f"a * b: {a * b}")
    print(f"a / b: {a / b}")
    print(f"a ** 2: {a ** 2}")

    # 比较运算
    print(f"a > 2: {a > 2}")
    print(f"a == b: {a == b}")

    # 数学函数
    print(f"sin(a): {np.sin(a)}")
    print(f"exp(a): {np.exp(a)}")
    print(f"sqrt(a): {np.sqrt(a)}")
    print(f"log(a): {np.log(a + 1)}")  # 加1避免log(0)

    # 5. 数组形状操作
    print("\n数组形状操作:")

    arr = np.arange(12)
    print(f"一维数组: {arr}")

    # 重塑形状
    reshaped = arr.reshape(3, 4)
    print(f"重塑为 3x4: \n{reshaped}")

    # 展平
    flattened = reshaped.flatten()
    print(f"展平: {flattened}")

    # 转置
    transposed = reshaped.T
    print(f"转置: \n{transposed}")

    # 添加维度
    expanded = arr[np.newaxis, :]
    print(f"添加行维度: {expanded.shape}")

    expanded_col = arr[:, np.newaxis]
    print(f"添加列维度: {expanded_col.shape}")

    # 6. 数组拼接和分割
    print("\n数组拼接和分割:")

    a = np.array([[1, 2], [3, 4]])
    b = np.array([[5, 6], [7, 8]])

    print(f"a: \n{a}")
    print(f"b: \n{b}")

    # 垂直拼接
    vstacked = np.vstack([a, b])
    print(f"垂直拼接: \n{vstacked}")

    # 水平拼接
    hstacked = np.hstack([a, b])
    print(f"水平拼接: \n{hstacked}")

    # 深度拼接
    dstacked = np.dstack([a, b])
    print(f"深度拼接: \n{dstacked}")

    # 分割
    large_arr = np.arange(16).reshape(4, 4)
    print(f"大数组: \n{large_arr}")

    # 垂直分割
    v_split = np.vsplit(large_arr, 2)
    print(f"垂直分割为2部分:")
    for i, part in enumerate(v_split):
        print(f"  部分{i}: \n{part}")

    # 水平分割
    h_split = np.hsplit(large_arr, 2)
    print(f"水平分割为2部分:")
    for i, part in enumerate(h_split):
        print(f"  部分{i}: \n{part}")

    # 7. 数组统计
    print("\n数组统计:")

    data = np.random.randn(1000)
    print(f"数据样本: {data[:10]}")

    print(f"均值: {np.mean(data):.4f}")
    print(f"中位数: {np.median(data):.4f}")
    print(f"标准差: {np.std(data):.4f}")
    print(f"方差: {np.var(data):.4f}")
    print(f"最小值: {np.min(data):.4f}")
    print(f"最大值: {np.max(data):.4f}")
    print(f"25% 分位数: {np.percentile(data, 25):.4f}")
    print(f"75% 分位数: {np.percentile(data, 75):.4f}")

    # 8. 数组排序和搜索
    print("\n数组排序和搜索:")

    unsorted = np.array([3, 1, 4, 1, 5, 9, 2, 6])
    print(f"未排序数组: {unsorted}")

    sorted_arr = np.sort(unsorted)
    print(f"排序后: {sorted_arr}")

    # 排序索引
    indices = np.argsort(unsorted)
    print(f"排序索引: {indices}")

    # 搜索
    search_val = 4
    found_indices = np.where(unsorted == search_val)
    print(f"值 {search_val} 的索引: {found_indices[0]}")

    # 查找最大/最小值索引
    max_idx = np.argmax(unsorted)
    min_idx = np.argmin(unsorted)
    print(f"最大值索引: {max_idx}, 值: {unsorted[max_idx]}")
    print(f"最小值索引: {min_idx}, 值: {unsorted[min_idx]}")

    print("NumPy 基础示例已定义")
else:
    print("NumPy 未安装，跳过相关示例")
```

### 28.1.2 NumPy 高级用法

```python
# NumPy 高级用法
print("NumPy 高级用法:")

try:
    import numpy as np
except ImportError:
    print("需要安装 numpy")
    np = None

if np:
    # 1. 广播机制
    print("广播机制:")

    # 标量与数组
    arr = np.array([1, 2, 3])
    scalar = 2
    print(f"数组: {arr}")
    print(f"标量: {scalar}")
    print(f"数组 + 标量: {arr + scalar}")
    print(f"数组 * 标量: {arr * scalar}")

    # 不同形状数组的广播
    a = np.array([[1, 2, 3]])
    b = np.array([[4], [5]])
    print(f"\na 形状: {a.shape}")
    print(f"a: \n{a}")
    print(f"b 形状: {b.shape}")
    print(f"b: \n{b}")
    print(f"a + b: \n{a + b}")

    # 2. 高级索引
    print("\n高级索引:")

    arr = np.arange(12).reshape(3, 4)
    print(f"原始数组: \n{arr}")

    # 整数数组索引
    rows = np.array([0, 2])
    cols = np.array([1, 3])
    print(f"行索引: {rows}")
    print(f"列索引: {cols}")
    print(f"高级索引结果: {arr[rows, cols]}")

    # 布尔索引
    bool_mask = arr > 5
    print(f"布尔掩码: \n{bool_mask}")
    print(f"布尔索引结果: {arr[bool_mask]}")

    # 条件赋值
    arr_copy = arr.copy()
    arr_copy[arr_copy > 5] = -1
    print(f"条件赋值后: \n{arr_copy}")

    # 3. 数组函数
    print("\n数组函数:")

    # 通用函数 (ufunc)
    arr = np.array([0, 1, 2, 3, 4])
    print(f"原始数组: {arr}")

    print(f"sin: {np.sin(arr)}")
    print(f"cos: {np.cos(arr)}")
    print(f"exp: {np.exp(arr)}")
    print(f"sqrt: {np.sqrt(arr + 1)}")  # 加1避免sqrt(0)

    # 聚合函数
    data = np.random.randn(10)
    print(f"\n随机数据: {data}")

    print(f"求和: {np.sum(data):.4f}")
    print(f"累积和: {np.cumsum(data)}")
    print(f"累积积: {np.cumprod(data)}")

    # 4. 线性代数
    print("\n线性代数:")

    # 矩阵乘法
    A = np.array([[1, 2], [3, 4]])
    B = np.array([[5, 6], [7, 8]])

    print(f"A: \n{A}")
    print(f"B: \n{B}")

    print(f"A @ B (矩阵乘法): \n{A @ B}")
    print(f"np.dot(A, B): \n{np.dot(A, B)}")

    # 矩阵运算
    print(f"A 的转置: \n{A.T}")
    print(f"A 的逆矩阵: \n{np.linalg.inv(A)}")
    print(f"A 的行列式: {np.linalg.det(A)}")

    # 特征值和特征向量
    eigenvalues, eigenvectors = np.linalg.eig(A)
    print(f"特征值: {eigenvalues}")
    print(f"特征向量: \n{eigenvectors}")

    # 5. 随机数生成
    print("\n随机数生成:")

    # 设置随机种子
    np.random.seed(42)

    # 均匀分布
    uniform = np.random.rand(5)
    print(f"均匀分布 [0,1): {uniform}")

    # 正态分布
    normal = np.random.randn(5)
    print(f"标准正态分布: {normal}")

    # 整数随机
    integers = np.random.randint(1, 10, 5)
    print(f"1-10 之间的整数: {integers}")

    # 选择
    choices = np.random.choice(['A', 'B', 'C'], 5)
    print(f"随机选择: {choices}")

    # 排列和打乱
    arr = np.arange(10)
    print(f"原始数组: {arr}")

    shuffled = arr.copy()
    np.random.shuffle(shuffled)
    print(f"打乱后: {shuffled}")

    permutation = np.random.permutation(10)
    print(f"随机排列: {permutation}")

    # 6. 文件输入输出
    print("\n文件输入输出:")

    # 创建测试数据
    data = np.random.randn(5, 3)
    print(f"测试数据: \n{data}")

    # 保存为文本文件
    np.savetxt('test_data.txt', data, delimiter=',', header='col1,col2,col3')
    print("数据已保存到 test_data.txt")

    # 从文本文件加载
    loaded_data = np.loadtxt('test_data.txt', delimiter=',', skiprows=1)
    print(f"从文件加载的数据: \n{loaded_data}")

    # 二进制格式
    np.save('test_data.npy', data)
    loaded_binary = np.load('test_data.npy')
    print(f"二进制加载的数据: \n{loaded_binary}")

    # 清理文件
    import os
    if os.path.exists('test_data.txt'):
        os.remove('test_data.txt')
    if os.path.exists('test_data.npy'):
        os.remove('test_data.npy')

    # 7. 性能优化
    print("\n性能优化:")

    # 向量化 vs 循环
    import time

    large_array = np.random.randn(1000000)

    # 向量化操作
    start_time = time.time()
    result_vectorized = np.sin(large_array) + np.cos(large_array)
    vectorized_time = time.time() - start_time

    # 循环操作
    start_time = time.time()
    result_loop = np.zeros_like(large_array)
    for i in range(len(large_array)):
        result_loop[i] = np.sin(large_array[i]) + np.cos(large_array[i])
    loop_time = time.time() - start_time

    print(f"向量化时间: {vectorized_time:.4f} 秒")
    print(f"循环时间: {loop_time:.4f} 秒")
    print(f"性能提升: {loop_time / vectorized_time:.1f} 倍")

    # 内存布局
    arr_c = np.random.randn(1000, 1000)
    arr_f = np.asfortranarray(arr_c.copy())

    # C 风格访问
    start_time = time.time()
    sum_c = np.sum(arr_c)
    c_time = time.time() - start_time

    # Fortran 风格访问
    start_time = time.time()
    sum_f = np.sum(arr_f)
    f_time = time.time() - start_time

    print(f"C 风格求和时间: {c_time:.4f} 秒")
    print(f"Fortran 风格求和时间: {f_time:.4f} 秒")

    # 8. 高级数组操作
    print("\n高级数组操作:")

    # 数组视图 vs 复制
    original = np.arange(12).reshape(3, 4)
    print(f"原始数组: \n{original}")

    # 视图（共享内存）
    view = original[:2, :2]
    view[0, 0] = 999
    print(f"修改视图后原始数组: \n{original}")

    # 复制（独立内存）
    original = np.arange(12).reshape(3, 4)  # 重新创建
    copy_arr = original[:2, :2].copy()
    copy_arr[0, 0] = 888
    print(f"修改复制后原始数组: \n{original}")

    # 数组堆叠
    a = np.array([1, 2, 3])
    b = np.array([4, 5, 6])

    print(f"数组 a: {a}")
    print(f"数组 b: {b}")

    print(f"水平堆叠: {np.hstack([a, b])}")
    print(f"垂直堆叠: {np.vstack([a, b])}")
    print(f"深度堆叠: {np.dstack([a, b])}")

    print("NumPy 高级用法示例已定义")
else:
    print("NumPy 未安装，跳过高级用法示例")
```

## 28.2 Pandas 数据处理

### 28.2.1 Pandas 基础

```python
# Pandas 数据处理
print("Pandas 数据处理:")

# 注意：需要安装 pandas: pip install pandas
try:
    import pandas as pd
except ImportError:
    print("需要安装 pandas: pip install pandas")
    pd = None

if pd:
    # 1. Series 创建和操作
    print("Series 创建和操作:")

    # 从列表创建 Series
    data = [1, 3, 5, 6, 2, 4]
    series = pd.Series(data)
    print(f"Series: \n{series}")
    print(f"索引: {series.index.tolist()}")
    print(f"值: {series.values}")

    # 自定义索引
    custom_series = pd.Series(data, index=['a', 'b', 'c', 'd', 'e', 'f'])
    print(f"\n自定义索引 Series: \n{custom_series}")

    # 从字典创建
    dict_data = {'Alice': 25, 'Bob': 30, 'Charlie': 35}
    dict_series = pd.Series(dict_data)
    print(f"\n从字典创建: \n{dict_series}")

    # Series 访问
    print(f"series[0]: {series[0]}")
    print(f"custom_series['a']: {custom_series['a']}")
    print(f"series[1:4]: \n{series[1:4]}")

    # 2. DataFrame 创建
    print("\nDataFrame 创建:")

    # 从字典创建
    data = {
        'Name': ['Alice', 'Bob', 'Charlie', 'Diana'],
        'Age': [25, 30, 35, 28],
        'City': ['Beijing', 'Shanghai', 'Shenzhen', 'Guangzhou'],
        'Salary': [50000, 60000, 70000, 55000]
    }

    df = pd.DataFrame(data)
    print(f"DataFrame: \n{df}")

    # 自定义索引
    df_custom = pd.DataFrame(data, index=['emp1', 'emp2', 'emp3', 'emp4'])
    print(f"\n自定义索引 DataFrame: \n{df_custom}")

    # 从列表创建
    list_data = [
        ['Alice', 25, 'Beijing', 50000],
        ['Bob', 30, 'Shanghai', 60000],
        ['Charlie', 35, 'Shenzhen', 70000],
        ['Diana', 28, 'Guangzhou', 55000]
    ]

    df_from_list = pd.DataFrame(list_data, columns=['Name', 'Age', 'City', 'Salary'])
    print(f"\n从列表创建: \n{df_from_list}")

    # 3. DataFrame 操作
    print("\nDataFrame 操作:")

    print(f"DataFrame 形状: {df.shape}")
    print(f"DataFrame 列: {df.columns.tolist()}")
    print(f"DataFrame 索引: {df.index.tolist()}")
    print(f"DataFrame 数据类型: \n{df.dtypes}")

    # 查看前几行
    print(f"\n前3行: \n{df.head(3)}")

    # 查看后几行
    print(f"\n后2行: \n{df.tail(2)}")

    # 基本统计信息
    print(f"\n基本统计: \n{df.describe()}")

    # 转置
    print(f"\n转置: \n{df.T}")

    # 4. 数据选择和过滤
    print("\n数据选择和过滤:")

    # 列选择
    print(f"Name 列: \n{df['Name']}")
    print(f"多个列: \n{df[['Name', 'Age']]}")

    # 行选择
    print(f"第一行: \n{df.iloc[0]}")
    print(f"前两行: \n{df.iloc[:2]}")
    print(f"按标签选择: \n{df.loc[0] if 0 in df.index else '索引0不存在'}")

    # 条件过滤
    print(f"年龄大于28: \n{df[df['Age'] > 28]}")
    print(f"城市是北京: \n{df[df['City'] == 'Beijing']}")
    print(f"薪资在50000-60000之间: \n{df[(df['Salary'] >= 50000) & (df['Salary'] <= 60000)]}")

    # 5. 数据修改
    print("\n数据修改:")

    df_copy = df.copy()

    # 修改单个值
    df_copy.loc[0, 'Age'] = 26
    print(f"修改年龄后: \n{df_copy}")

    # 添加新列
    df_copy['Department'] = ['Engineering', 'Sales', 'Marketing', 'HR']
    print(f"\n添加部门列: \n{df_copy}")

    # 修改现有列
    df_copy['Salary'] = df_copy['Salary'] * 1.1  # 10% 涨薪
    print(f"\n涨薪10%后: \n{df_copy}")

    # 删除列
    df_copy = df_copy.drop('Department', axis=1)
    print(f"\n删除部门列: \n{df_copy}")

    # 6. 数据排序
    print("\n数据排序:")

    # 按年龄排序
    sorted_by_age = df.sort_values('Age')
    print(f"按年龄升序: \n{sorted_by_age}")

    # 按薪资降序
    sorted_by_salary = df.sort_values('Salary', ascending=False)
    print(f"按薪资降序: \n{sorted_by_salary}")

    # 多列排序
    sorted_multi = df.sort_values(['City', 'Age'])
    print(f"按城市和年龄排序: \n{sorted_multi}")

    # 7. 分组和聚合
    print("\n分组和聚合:")

    # 按城市分组
    grouped = df.groupby('City')
    print(f"按城市分组的均值: \n{grouped.mean(numeric_only=True)}")

    # 多个聚合函数
    agg_result = df.groupby('City').agg({
        'Age': ['mean', 'min', 'max'],
        'Salary': ['sum', 'mean']
    })
    print(f"\n多个聚合函数: \n{agg_result}")

    # 8. 数据合并
    print("\n数据合并:")

    # 创建另一个 DataFrame
    additional_data = {
        'Name': ['Alice', 'Bob', 'Charlie', 'Diana'],
        'Department': ['Engineering', 'Sales', 'Marketing', 'HR'],
        'Experience': [3, 5, 7, 4]
    }

    df_additional = pd.DataFrame(additional_data)

    print(f"原始数据: \n{df}")
    print(f"\n附加数据: \n{df_additional}")

    # 合并
    merged = pd.merge(df, df_additional, on='Name')
    print(f"\n合并结果: \n{merged}")

    print("Pandas 基础示例已定义")
else:
    print("Pandas 未安装，跳过相关示例")
```

### 28.2.2 Pandas 高级用法

```python
# Pandas 高级用法
print("Pandas 高级用法:")

try:
    import pandas as pd
    import numpy as np
except ImportError:
    print("需要安装相关包")
    pd = None

if pd:
    # 1. 时间序列数据
    print("时间序列数据:")

    # 创建时间序列
    dates = pd.date_range('2023-01-01', periods=10, freq='D')
    ts_data = pd.Series(np.random.randn(10), index=dates)
    print(f"时间序列: \n{ts_data}")

    # 时间序列操作
    print(f"2023年1月的数据: \n{ts_data['2023-01']}")
    print(f"重采样为周: \n{ts_data.resample('W').mean()}")

    # 移动窗口
    print(f"7日移动平均: \n{ts_data.rolling(window=3).mean()}")

    # 2. 数据透视表
    print("\n数据透视表:")

    # 创建销售数据
    sales_data = {
        'Date': pd.date_range('2023-01-01', periods=20),
        'Product': np.random.choice(['A', 'B', 'C'], 20),
        'Region': np.random.choice(['North', 'South', 'East', 'West'], 20),
        'Sales': np.random.randint(100, 1000, 20)
    }

    df_sales = pd.DataFrame(sales_data)
    print(f"销售数据: \n{df_sales.head()}")

    # 基本透视表
    pivot_basic = df_sales.pivot_table(values='Sales', index='Product', columns='Region', aggfunc='sum')
    print(f"\n基本透视表: \n{pivot_basic}")

    # 多重聚合
    pivot_multi = df_sales.pivot_table(values='Sales', index='Product', columns='Region',
                                      aggfunc=['sum', 'mean', 'count'])
    print(f"\n多重聚合透视表: \n{pivot_multi}")

    # 3. 数据清理
    print("\n数据清理:")

    # 创建包含缺失值的数据
    messy_data = {
        'A': [1, 2, np.nan, 4, 5],
        'B': [np.nan, 2, 3, 4, np.nan],
        'C': ['a', 'b', 'c', 'd', 'e'],
        'D': [1, 2, 3, 4, 5]
    }

    df_messy = pd.DataFrame(messy_data)
    print(f"原始数据: \n{df_messy}")

    # 检查缺失值
    print(f"\n缺失值统计: \n{df_messy.isnull().sum()}")

    # 删除包含缺失值的行
    df_dropped = df_messy.dropna()
    print(f"\n删除缺失值后: \n{df_dropped}")

    # 填充缺失值
    df_filled = df_messy.fillna(df_messy.mean(numeric_only=True))
    print(f"\n填充缺失值后: \n{df_filled}")

    # 4. 数据转换
    print("\n数据转换:")

    # 创建测试数据
    transform_data = {
        'Name': ['alice', 'BOB', 'Charlie', 'diana'],
        'Age': [25, 30, 35, 28],
        'Score': [85.5, 92.3, 78.8, 88.9]
    }

    df_transform = pd.DataFrame(transform_data)
    print(f"原始数据: \n{df_transform}")

    # 字符串转换
    df_transform['Name'] = df_transform['Name'].str.title()
    print(f"\n姓名格式化后: \n{df_transform}")

    # 数值转换
    df_transform['Score_Int'] = df_transform['Score'].astype(int)
    print(f"\n添加整数分数列: \n{df_transform}")

    # 分类数据
    df_transform['Grade'] = pd.cut(df_transform['Score'],
                                  bins=[0, 80, 90, 100],
                                  labels=['C', 'B', 'A'])
    print(f"\n添加等级列: \n{df_transform}")

    # 5. 多表连接
    print("\n多表连接:")

    # 员工表
    employees = pd.DataFrame({
        'emp_id': [1, 2, 3, 4],
        'name': ['Alice', 'Bob', 'Charlie', 'Diana'],
        'dept_id': [10, 20, 10, 30]
    })

    # 部门表
    departments = pd.DataFrame({
        'dept_id': [10, 20, 30],
        'dept_name': ['Engineering', 'Sales', 'HR']
    })

    # 薪资表
    salaries = pd.DataFrame({
        'emp_id': [1, 2, 3, 4],
        'salary': [50000, 60000, 70000, 55000]
    })

    print("员工表:")
    print(employees)
    print("\n部门表:")
    print(departments)
    print("\n薪资表:")
    print(salaries)

    # 内连接
    emp_dept = pd.merge(employees, departments, on='dept_id')
    print(f"\n员工部门连接: \n{emp_dept}")

    # 多表连接
    full_info = pd.merge(emp_dept, salaries, on='emp_id')
    print(f"\n完整信息: \n{full_info}")

    # 左连接
    left_join = pd.merge(employees, departments, on='dept_id', how='left')
    print(f"\n左连接: \n{left_join}")

    # 6. 数据重塑
    print("\n数据重塑:")

    # 创建宽格式数据
    wide_data = pd.DataFrame({
        'student': ['Alice', 'Bob', 'Charlie'],
        'math': [90, 85, 88],
        'english': [85, 90, 87],
        'science': [88, 87, 92]
    })

    print(f"宽格式数据: \n{wide_data}")

    # 转换为长格式
    long_data = pd.melt(wide_data, id_vars=['student'], var_name='subject', value_name='score')
    print(f"\n长格式数据: \n{long_data}")

    # 转换回宽格式
    wide_again = long_data.pivot(index='student', columns='subject', values='score')
    print(f"\n转换回宽格式: \n{wide_again}")

    # 7. 性能优化
    print("\n性能优化:")

    # 创建大数据集
    large_df = pd.DataFrame({
        'A': np.random.randn(100000),
        'B': np.random.randn(100000),
        'C': np.random.choice(['X', 'Y', 'Z'], 100000)
    })

    print(f"大数据集形状: {large_df.shape}")
    print(f"内存使用: {large_df.memory_usage(deep=True).sum() / 1024 / 1024:.2f} MB")

    # 数据类型优化
    optimized_df = large_df.copy()
    optimized_df['A'] = optimized_df['A'].astype(np.float32)  # 降低精度
    optimized_df['C'] = optimized_df['C'].astype('category')  # 分类数据

    print(f"优化后内存使用: {optimized_df.memory_usage(deep=True).sum() / 1024 / 1024:.2f} MB")

    # 向量化操作 vs 循环
    import time

    # 向量化操作
    start_time = time.time()
    result_vectorized = large_df['A'] + large_df['B']
    vectorized_time = time.time() - start_time

    # 循环操作
    start_time = time.time()
    result_loop = []
    for i in range(len(large_df)):
        result_loop.append(large_df.iloc[i]['A'] + large_df.iloc[i]['B'])
    loop_time = time.time() - start_time

    print(f"向量化时间: {vectorized_time:.4f} 秒")
    print(f"循环时间: {loop_time:.4f} 秒")
    print(f"性能提升: {loop_time / vectorized_time:.1f} 倍")

    # 8. 数据验证和清理
    print("\n数据验证和清理:")

    # 创建测试数据
    validation_data = {
        'id': [1, 2, 3, 4, 5],
        'email': ['alice@example.com', 'bob@', 'charlie@example.com', '', 'diana@example.com'],
        'age': [25, 30, -5, 35, 28],
        'score': [85, 92, 78, 88, 150]  # 150超出范围
    }

    df_validation = pd.DataFrame(validation_data)
    print(f"原始数据: \n{df_validation}")

    # 定义验证规则
    def validate_email(email):
        if not email or '@' not in email:
            return False
        return True

    def validate_age(age):
        return 0 <= age <= 120

    def validate_score(score):
        return 0 <= score <= 100

    # 应用验证
    df_validation['email_valid'] = df_validation['email'].apply(validate_email)
    df_validation['age_valid'] = df_validation['age'].apply(validate_age)
    df_validation['score_valid'] = df_validation['score'].apply(validate_score)

    print(f"\n验证结果: \n{df_validation}")

    # 清理无效数据
    df_clean = df_validation[
        df_validation['email_valid'] &
        df_validation['age_valid'] &
        df_validation['score_valid']
    ].drop(['email_valid', 'age_valid', 'score_valid'], axis=1)

    print(f"\n清理后数据: \n{df_clean}")

    print("Pandas 高级用法示例已定义")
else:
    print("相关包未安装，跳过高级用法示例")
```

## 28.3 Matplotlib 数据可视化

### 28.3.1 Matplotlib 基础

```python
# Matplotlib 数据可视化
print("Matplotlib 数据可视化:")

# 注意：需要安装 matplotlib: pip install matplotlib
try:
    import matplotlib.pyplot as plt
    import numpy as np
except ImportError:
    print("需要安装 matplotlib 和 numpy")
    plt = None

if plt:
    # 设置中文字体支持
    plt.rcParams['font.sans-serif'] = ['SimHei', 'Arial Unicode MS', 'DejaVu Sans']
    plt.rcParams['axes.unicode_minus'] = False

    # 1. 基本绘图
    print("基本绘图:")

    # 创建数据
    x = np.linspace(0, 10, 100)
    y = np.sin(x)

    # 创建图形
    plt.figure(figsize=(10, 6))

    # 绘制线条
    plt.plot(x, y, label='sin(x)', color='blue', linewidth=2)

    # 添加标题和标签
    plt.title('正弦函数')
    plt.xlabel('x 值')
    plt.ylabel('sin(x)')

    # 添加网格和图例
    plt.grid(True, alpha=0.3)
    plt.legend()

    # 显示图形
    plt.show()

    # 2. 多条线条
    print("\n多条线条:")

    x = np.linspace(0, 10, 100)
    y1 = np.sin(x)
    y2 = np.cos(x)
    y3 = np.sin(x + np.pi/4)

    plt.figure(figsize=(10, 6))

    plt.plot(x, y1, label='sin(x)', color='red', linestyle='-', marker='o', markersize=3)
    plt.plot(x, y2, label='cos(x)', color='green', linestyle='--', marker='s', markersize=3)
    plt.plot(x, y3, label='sin(x+π/4)', color='blue', linestyle='-.', marker='^', markersize=3)

    plt.title('三角函数对比')
    plt.xlabel('x')
    plt.ylabel('y')
    plt.legend()
    plt.grid(True, alpha=0.3)

    plt.show()

    # 3. 子图
    print("\n子图:")

    fig, axes = plt.subplots(2, 2, figsize=(12, 8))

    x = np.linspace(0, 10, 100)

    # 子图1: 线性函数
    axes[0, 0].plot(x, x, 'r-')
    axes[0, 0].set_title('线性函数 y=x')
    axes[0, 0].grid(True)

    # 子图2: 二次函数
    axes[0, 1].plot(x, x**2, 'g-')
    axes[0, 1].set_title('二次函数 y=x²')
    axes[0, 1].grid(True)

    # 子图3: 三角函数
    axes[1, 0].plot(x, np.sin(x), 'b-')
    axes[1, 0].set_title('正弦函数')
    axes[1, 0].grid(True)

    # 子图4: 指数函数
    axes[1, 1].plot(x, np.exp(x/5), 'm-')
    axes[1, 1].set_title('指数函数')
    axes[1, 1].grid(True)

    plt.tight_layout()
    plt.show()

    # 4. 散点图
    print("\n散点图:")

    # 生成随机数据
    np.random.seed(42)
    x = np.random.randn(100)
    y = 2 * x + 1 + np.random.randn(100) * 0.5

    plt.figure(figsize=(8, 6))
    plt.scatter(x, y, alpha=0.6, c=y, cmap='viridis', s=50)

    plt.title('散点图示例')
    plt.xlabel('X 值')
    plt.ylabel('Y 值')
    plt.colorbar(label='Y 值')
    plt.grid(True, alpha=0.3)

    plt.show()

    # 5. 柱状图
    print("\n柱状图:")

    categories = ['A', 'B', 'C', 'D', 'E']
    values = [23, 45, 56, 78, 32]

    plt.figure(figsize=(8, 6))
    bars = plt.bar(categories, values, color=['red', 'green', 'blue', 'orange', 'purple'])

    # 添加数值标签
    for bar, value in zip(bars, values):
        plt.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 1,
                str(value), ha='center', va='bottom')

    plt.title('柱状图示例')
    plt.xlabel('类别')
    plt.ylabel('数值')
    plt.grid(True, alpha=0.3, axis='y')

    plt.show()

    # 6. 饼图
    print("\n饼图:")

    sizes = [30, 25, 20, 15, 10]
    labels = ['类别A', '类别B', '类别C', '类别D', '类别E']
    colors = ['red', 'green', 'blue', 'orange', 'purple']
    explode = (0.1, 0, 0, 0, 0)  # 突出第一个扇形

    plt.figure(figsize=(8, 6))
    plt.pie(sizes, explode=explode, labels=labels, colors=colors,
            autopct='%1.1f%%', shadow=True, startangle=90)

    plt.title('饼图示例')
    plt.axis('equal')  # 确保饼图是圆的

    plt.show()

    # 7. 直方图
    print("\n直方图:")

    # 生成正态分布数据
    data = np.random.normal(0, 1, 1000)

    plt.figure(figsize=(8, 6))
    n, bins, patches = plt.hist(data, bins=30, alpha=0.7, color='skyblue', edgecolor='black')

    plt.title('直方图示例 - 正态分布')
    plt.xlabel('值')
    plt.ylabel('频次')
    plt.grid(True, alpha=0.3)

    # 添加统计信息
    plt.axvline(np.mean(data), color='red', linestyle='--', label=f'均值: {np.mean(data):.2f}')
    plt.axvline(np.median(data), color='green', linestyle='--', label=f'中位数: {np.median(data):.2f}')
    plt.legend()

    plt.show()

    # 8. 箱线图
    print("\n箱线图:")

    # 生成多组数据
    data1 = np.random.normal(0, 1, 100)
    data2 = np.random.normal(1, 1.5, 100)
    data3 = np.random.normal(-1, 2, 100)

    data = [data1, data2, data3]
    labels = ['组1', '组2', '组3']

    plt.figure(figsize=(8, 6))
    box = plt.boxplot(data, labels=labels, patch_artist=True)

    # 设置箱体的颜色
    colors = ['lightblue', 'lightgreen', 'lightcoral']
    for patch, color in zip(box['boxes'], colors):
        patch.set_facecolor(color)

    plt.title('箱线图示例')
    plt.ylabel('值')
    plt.grid(True, alpha=0.3, axis='y')

    plt.show()

    print("Matplotlib 基础示例已定义")
else:
    print("Matplotlib 未安装，跳过相关示例")
```

### 28.3.2 Matplotlib 高级用法

```python
# Matplotlib 高级用法
print("Matplotlib 高级用法:")

try:
    import matplotlib.pyplot as plt
    import numpy as np
    from mpl_toolkits.mplot3d import Axes3D
except ImportError:
    print("需要安装相关包")
    plt = None

if plt:
    # 设置中文字体支持
    plt.rcParams['font.sans-serif'] = ['SimHei', 'Arial Unicode MS', 'DejaVu Sans']
    plt.rcParams['axes.unicode_minus'] = False

    # 1. 3D 绘图
    print("3D 绘图:")

    fig = plt.figure(figsize=(10, 8))
    ax = fig.add_subplot(111, projection='3d')

    # 生成数据
    x = np.linspace(-5, 5, 100)
    y = np.linspace(-5, 5, 100)
    X, Y = np.meshgrid(x, y)
    Z = np.sin(np.sqrt(X**2 + Y**2))

    # 3D 曲面图
    surf = ax.plot_surface(X, Y, Z, cmap='viridis', alpha=0.8)

    # 添加颜色条
    fig.colorbar(surf, ax=ax, shrink=0.5, aspect=5)

    ax.set_title('3D 曲面图')
    ax.set_xlabel('X 轴')
    ax.set_ylabel('Y 轴')
    ax.set_zlabel('Z 轴')

    plt.show()

    # 2. 动画
    print("\n动画:")

    from matplotlib.animation import FuncAnimation

    fig, ax = plt.subplots(figsize=(8, 6))
    x = np.linspace(0, 2*np.pi, 100)
    line, = ax.plot(x, np.sin(x))

    def animate(frame):
        y = np.sin(x + frame * 0.1)
        line.set_ydata(y)
        return line,

    ani = FuncAnimation(fig, animate, frames=100, interval=50, blit=True)
    plt.title('正弦波动画')
    plt.xlabel('x')
    plt.ylabel('sin(x)')
    plt.grid(True)

    # 保存动画（需要安装 ffmpeg）
    # ani.save('sine_wave.gif', writer='pillow', fps=20)

    plt.show()

    # 3. 自定义样式
    print("\n自定义样式:")

    # 设置样式
    plt.style.use('seaborn-v0_8-darkgrid')

    x = np.linspace(0, 10, 100)
    y1 = np.sin(x)
    y2 = np.cos(x)

    fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 5))

    # 左侧子图
    ax1.plot(x, y1, 'r-', linewidth=2, label='sin(x)')
    ax1.plot(x, y2, 'b--', linewidth=2, label='cos(x)')
    ax1.set_title('三角函数', fontsize=14, fontweight='bold')
    ax1.set_xlabel('x', fontsize=12)
    ax1.set_ylabel('y', fontsize=12)
    ax1.legend(fontsize=10)
    ax1.grid(True, alpha=0.3)

    # 右侧子图 - 散点图
    np.random.seed(42)
    scatter_x = np.random.randn(50)
    scatter_y = np.random.randn(50)
    colors = np.random.rand(50)
    sizes = np.random.rand(50) * 100

    scatter = ax2.scatter(scatter_x, scatter_y, c=colors, s=sizes, alpha=0.6, cmap='plasma')
    ax2.set_title('彩色散点图', fontsize=14, fontweight='bold')
    ax2.set_xlabel('X 值', fontsize=12)
    ax2.set_ylabel('Y 值', fontsize=12)

    # 添加颜色条
    plt.colorbar(scatter, ax=ax2, label='颜色值')

    plt.tight_layout()
    plt.show()

    # 4. 多轴图
    print("\n多轴图:")

    fig, ax1 = plt.subplots(figsize=(10, 6))

    x = np.linspace(0, 10, 100)
    y1 = np.sin(x)
    y2 = np.exp(x / 10)

    # 左侧 Y 轴
    color = 'tab:red'
    ax1.set_xlabel('x')
    ax1.set_ylabel('sin(x)', color=color)
    ax1.plot(x, y1, color=color, linewidth=2)
    ax1.tick_params(axis='y', labelcolor=color)

    # 右侧 Y 轴
    ax2 = ax1.twinx()
    color = 'tab:blue'
    ax2.set_ylabel('exp(x/10)', color=color)
    ax2.plot(x, y2, color=color, linewidth=2, linestyle='--')
    ax2.tick_params(axis='y', labelcolor=color)

    plt.title('双轴图示例')
    plt.grid(True, alpha=0.3)
    plt.show()

    # 5. 统计图表
    print("\n统计图表:")

    # 生成数据
    np.random.seed(42)
    data1 = np.random.normal(0, 1, 1000)
    data2 = np.random.normal(2, 1.5, 1000)

    fig, axes = plt.subplots(2, 2, figsize=(12, 10))

    # 直方图
    axes[0, 0].hist(data1, bins=30, alpha=0.7, color='skyblue', edgecolor='black')
    axes[0, 0].set_title('数据集1直方图')
    axes[0, 0].set_xlabel('值')
    axes[0, 0].set_ylabel('频次')
    axes[0, 0].grid(True, alpha=0.3)

    # 箱线图
    axes[0, 1].boxplot([data1, data2], labels=['数据集1', '数据集2'])
    axes[0, 1].set_title('箱线图对比')
    axes[0, 1].set_ylabel('值')
    axes[0, 1].grid(True, alpha=0.3)

    # Q-Q图
    from scipy import stats
    z_scores1 = stats.zscore(data1)
    z_scores2 = stats.zscore(data2)

    axes[1, 0].scatter(np.sort(z_scores1), np.sort(z_scores2), alpha=0.6)
    axes[1, 0].plot([-3, 3], [-3, 3], 'r--', alpha=0.7)
    axes[1, 0].set_title('Q-Q图')
    axes[1, 0].set_xlabel('数据集1标准分')
    axes[1, 0].set_ylabel('数据集2标准分')
    axes[1, 0].grid(True, alpha=0.3)

    # 小提琴图
    axes[1, 1].violinplot([data1, data2], showmeans=True, showmedians=True)
    axes[1, 1].set_title('小提琴图')
    axes[1, 1].set_xticks([1, 2])
    axes[1, 1].set_xticklabels(['数据集1', '数据集2'])
    axes[1, 1].set_ylabel('值')
    axes[1, 1].grid(True, alpha=0.3)

    plt.tight_layout()
    plt.show()

    # 6. 地图可视化
    print("\n地图可视化:")

    # 注意：需要安装 basemap 或 cartopy
    # 这里使用简单的经纬度散点图作为示例

    # 生成模拟地理数据
    np.random.seed(42)
    lats = np.random.uniform(-90, 90, 100)
    lons = np.random.uniform(-180, 180, 100)
    values = np.random.rand(100)

    fig, ax = plt.subplots(figsize=(12, 6), subplot_kw={'projection': 'mollweide'})

    # 转换为弧度
    lons_rad = np.radians(lons)
    lats_rad = np.radians(lats)

    # 绘制散点
    scatter = ax.scatter(lons_rad, lats_rad, c=values, s=20, alpha=0.7, cmap='viridis')

    ax.set_title('全球数据分布', pad=20)
    ax.grid(True, alpha=0.3)

    # 添加颜色条
    plt.colorbar(scatter, ax=ax, label='数值', shrink=0.6)

    plt.show()

    # 7. 交互式图表
    print("\n交互式图表:")

    # 注意：交互式功能在 Jupyter notebook 或 IPython 中效果更好

    fig, ax = plt.subplots(figsize=(10, 6))

    x = np.linspace(0, 10, 100)
    y = np.sin(x)

    line, = ax.plot(x, y, 'b-', linewidth=2, label='sin(x)')

    # 添加交互元素
    ax.axhline(y=0, color='k', linestyle='--', alpha=0.5)
    ax.axvline(x=0, color='k', linestyle='--', alpha=0.5)

    # 设置网格和标签
    ax.grid(True, alpha=0.3)
    ax.set_title('交互式图表示例')
    ax.set_xlabel('x')
    ax.set_ylabel('y')
    ax.legend()

    # 添加注释
    ax.annotate('最大值', xy=(np.pi/2, 1), xytext=(np.pi/2 + 1, 0.8),
                arrowprops=dict(facecolor='black', shrink=0.05))

    plt.show()

    # 8. 导出和保存
    print("\n导出和保存:")

    # 创建示例图表
    fig, axes = plt.subplots(2, 2, figsize=(12, 8))

    x = np.linspace(0, 10, 100)

    # 各种图表类型
    axes[0, 0].plot(x, np.sin(x), 'r-')
    axes[0, 0].set_title('线条图')

    axes[0, 1].bar(['A', 'B', 'C'], [1, 3, 2], color='skyblue')
    axes[0, 1].set_title('柱状图')

    axes[1, 0].scatter(np.random.randn(50), np.random.randn(50), alpha=0.6)
    axes[1, 0].set_title('散点图')

    axes[1, 1].hist(np.random.randn(1000), bins=30, alpha=0.7)
    axes[1, 1].set_title('直方图')

    plt.tight_layout()

    # 保存为不同格式
    plt.savefig('chart_examples.png', dpi=300, bbox_inches='tight')
    plt.savefig('chart_examples.pdf', bbox_inches='tight')
    plt.savefig('chart_examples.svg', bbox_inches='tight')

    print("图表已保存为 PNG、PDF 和 SVG 格式")

    plt.show()

    print("Matplotlib 高级用法示例已定义")
else:
    print("相关包未安装，跳过高级用法示例")
```

## 28.4 数据分析工作流

### 28.4.1 数据分析流程

```python
# 数据分析工作流
print("数据分析工作流:")

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from pathlib import Path
import warnings
warnings.filterwarnings('ignore')

# 1. 数据获取和加载
print("数据获取和加载:")

def load_data(file_path):
    """加载数据文件"""
    if not Path(file_path).exists():
        print(f"文件不存在: {file_path}")
        return None

    try:
        if file_path.endswith('.csv'):
            df = pd.read_csv(file_path)
        elif file_path.endswith('.xlsx'):
            df = pd.read_excel(file_path)
        elif file_path.endswith('.json'):
            df = pd.read_json(file_path)
        else:
            print(f"不支持的文件格式: {file_path}")
            return None

        print(f"成功加载数据: {df.shape[0]} 行, {df.shape[1]} 列")
        return df

    except Exception as e:
        print(f"加载数据失败: {e}")
        return None

# 2. 数据探索
print("\n数据探索:")

def explore_data(df):
    """探索性数据分析"""
    print("=== 数据概览 ===")
    print(f"数据形状: {df.shape}")
    print(f"数据类型:\n{df.dtypes}")
    print(f"\n前5行数据:\n{df.head()}")

    print("\n=== 基本统计 ===")
    print(df.describe())

    print("\n=== 缺失值统计 ===")
    missing = df.isnull().sum()
    print(missing[missing > 0])

    print("\n=== 重复值检查 ===")
    duplicates = df.duplicated().sum()
    print(f"重复行数: {duplicates}")

    # 数值列的相关性
    numeric_cols = df.select_dtypes(include=[np.number]).columns
    if len(numeric_cols) > 1:
        print("\n=== 数值列相关性 ===")
        print(df[numeric_cols].corr())

# 3. 数据清理
print("\n数据清理:")

def clean_data(df):
    """数据清理"""
    df_clean = df.copy()

    print("=== 数据清理过程 ===")

    # 处理缺失值
    print(f"清理前缺失值: {df_clean.isnull().sum().sum()}")

    # 数值列用中位数填充
    numeric_cols = df_clean.select_dtypes(include=[np.number]).columns
    for col in numeric_cols:
        if df_clean[col].isnull().sum() > 0:
            median_val = df_clean[col].median()
            df_clean[col].fillna(median_val, inplace=True)
            print(f"列 {col} 缺失值用中位数 {median_val} 填充")

    # 分类列用众数填充
    categorical_cols = df_clean.select_dtypes(include=['object']).columns
    for col in categorical_cols:
        if df_clean[col].isnull().sum() > 0:
            mode_val = df_clean[col].mode().iloc[0] if not df_clean[col].mode().empty else 'Unknown'
            df_clean[col].fillna(mode_val, inplace=True)
            print(f"列 {col} 缺失值用众数 '{mode_val}' 填充")

    # 删除重复行
    initial_rows = len(df_clean)
    df_clean.drop_duplicates(inplace=True)
    removed_rows = initial_rows - len(df_clean)
    print(f"删除了 {removed_rows} 行重复数据")

    print(f"清理后数据形状: {df_clean.shape}")
    print(f"清理后缺失值: {df_clean.isnull().sum().sum()}")

    return df_clean

# 4. 数据转换
print("\n数据转换:")

def transform_data(df):
    """数据转换"""
    df_transformed = df.copy()

    print("=== 数据转换 ===")

    # 数值列标准化
    numeric_cols = df_transformed.select_dtypes(include=[np.number]).columns
    for col in numeric_cols:
        if df_transformed[col].std() > 0:  # 避免除零错误
            mean_val = df_transformed[col].mean()
            std_val = df_transformed[col].std()
            df_transformed[f'{col}_normalized'] = (df_transformed[col] - mean_val) / std_val
            print(f"列 {col} 已标准化")

    # 分类变量编码
    categorical_cols = df_transformed.select_dtypes(include=['object']).columns
    for col in categorical_cols:
        if df_transformed[col].nunique() <= 10:  # 类别数不太多时进行独热编码
            dummies = pd.get_dummies(df_transformed[col], prefix=col)
            df_transformed = pd.concat([df_transformed, dummies], axis=1)
            df_transformed.drop(col, axis=1, inplace=True)
            print(f"列 {col} 已进行独热编码")

    print(f"转换后数据形状: {df_transformed.shape}")
    return df_transformed

# 5. 数据可视化
print("\n数据可视化:")

def visualize_data(df):
    """数据可视化"""
    print("=== 数据可视化 ===")

    # 设置中文字体
    plt.rcParams['font.sans-serif'] = ['SimHei', 'Arial Unicode MS', 'DejaVu Sans']
    plt.rcParams['axes.unicode_minus'] = False

    numeric_cols = df.select_dtypes(include=[np.number]).columns

    if len(numeric_cols) == 0:
        print("没有数值列可以可视化")
        return

    # 创建子图
    n_cols = min(3, len(numeric_cols))
    n_rows = (len(numeric_cols) + n_cols - 1) // n_cols

    fig, axes = plt.subplots(n_rows, n_cols, figsize=(5*n_cols, 4*n_rows))
    if n_rows == 1 and n_cols == 1:
        axes = [axes]
    elif n_rows == 1:
        axes = axes.flatten()
    else:
        axes = axes.flatten()

    for i, col in enumerate(numeric_cols[:len(axes)]):
        ax = axes[i]

        # 绘制直方图
        ax.hist(df[col].dropna(), bins=30, alpha=0.7, color='skyblue', edgecolor='black')
        ax.set_title(f'{col} 分布')
        ax.set_xlabel(col)
        ax.set_ylabel('频次')
        ax.grid(True, alpha=0.3)

    plt.tight_layout()
    plt.savefig('data_visualization.png', dpi=150, bbox_inches='tight')
    plt.show()

    # 相关性热力图
    if len(numeric_cols) > 1:
        plt.figure(figsize=(10, 8))
        corr_matrix = df[numeric_cols].corr()

        plt.imshow(corr_matrix, cmap='coolwarm', aspect='auto')
        plt.colorbar()

        plt.xticks(range(len(numeric_cols)), numeric_cols, rotation=45)
        plt.yticks(range(len(numeric_cols)), numeric_cols)

        # 添加相关系数文本
        for i in range(len(numeric_cols)):
            for j in range(len(numeric_cols)):
                plt.text(j, i, '.2f',
                        ha='center', va='center',
                        color='white' if abs(corr_matrix.iloc[i, j]) > 0.5 else 'black')

        plt.title('相关性热力图')
        plt.tight_layout()
        plt.savefig('correlation_heatmap.png', dpi=150, bbox_inches='tight')
        plt.show()

# 6. 完整的数据分析流程
print("\n完整的数据分析流程:")

def run_data_analysis(file_path):
    """运行完整的数据分析流程"""

    print(f"开始分析文件: {file_path}")

    # 1. 加载数据
    df = load_data(file_path)
    if df is None:
        return None

    # 2. 数据探索
    explore_data(df)

    # 3. 数据清理
    df_clean = clean_data(df)

    # 4. 数据转换
    df_transformed = transform_data(df_clean)

    # 5. 数据可视化
    visualize_data(df_transformed)

    # 6. 保存处理后的数据
    output_file = Path(file_path).stem + '_processed.csv'
    df_transformed.to_csv(output_file, index=False)
    print(f"\n处理后的数据已保存到: {output_file}")

    return df_transformed

# 7. 批量数据处理
print("\n批量数据处理:")

def batch_process_data(directory_path):
    """批量处理目录中的所有数据文件"""

    directory = Path(directory_path)
    if not directory.exists():
        print(f"目录不存在: {directory_path}")
        return

    supported_extensions = ['.csv', '.xlsx', '.json']

    processed_files = []
    for file_path in directory.iterdir():
        if file_path.suffix.lower() in supported_extensions:
            print(f"\n{'='*50}")
            print(f"处理文件: {file_path.name}")
            print('='*50)

            try:
                result = run_data_analysis(str(file_path))
                if result is not None:
                    processed_files.append(file_path.name)
            except Exception as e:
                print(f"处理文件 {file_path.name} 时出错: {e}")

    print(f"\n{'='*50}")
    print(f"批量处理完成，共处理了 {len(processed_files)} 个文件:")
    for file in processed_files:
        print(f"  - {file}")

# 8. 报告生成
print("\n报告生成:")

def generate_report(df, output_file='analysis_report.txt'):
    """生成分析报告"""

    with open(output_file, 'w', encoding='utf-8') as f:
        f.write("数据分析报告\n")
        f.write("="*50 + "\n\n")

        # 基本信息
        f.write("1. 基本信息\n")
        f.write("-" * 20 + "\n")
        f.write(f"数据形状: {df.shape[0]} 行, {df.shape[1]} 列\n")
        f.write(f"数据大小: {df.memory_usage(deep=True).sum() / 1024:.1f} KB\n\n")

        # 数据类型
        f.write("2. 数据类型\n")
        f.write("-" * 20 + "\n")
        for col, dtype in df.dtypes.items():
            f.write(f"{col}: {dtype}\n")
        f.write("\n")

        # 数值列统计
        numeric_cols = df.select_dtypes(include=[np.number]).columns
        if len(numeric_cols) > 0:
            f.write("3. 数值列统计\n")
            f.write("-" * 20 + "\n")
            stats = df[numeric_cols].describe()
            f.write(str(stats))
            f.write("\n\n")

        # 分类列统计
        categorical_cols = df.select_dtypes(include=['object']).columns
        if len(categorical_cols) > 0:
            f.write("4. 分类列统计\n")
            f.write("-" * 20 + "\n")
            for col in categorical_cols:
                f.write(f"{col}:\n")
                value_counts = df[col].value_counts()
                for value, count in value_counts.items():
                    f.write(f"  {value}: {count}\n")
                f.write("\n")

        # 缺失值信息
        f.write("5. 缺失值信息\n")
        f.write("-" * 20 + "\n")
        missing = df.isnull().sum()
        if missing.sum() > 0:
            for col, count in missing[missing > 0].items():
                f.write(f"{col}: {count} 个缺失值\n")
        else:
            f.write("无缺失值\n")
        f.write("\n")

        # 重复值信息
        f.write("6. 重复值信息\n")
        f.write("-" * 20 + "\n")
        duplicates = df.duplicated().sum()
        f.write(f"重复行数: {duplicates}\n\n")

        f.write("报告生成完成\n")

    print(f"分析报告已保存到: {output_file}")

print("数据分析工作流示例已定义")
print("这些函数提供了完整的数据分析流程:")
print("1. 数据加载和探索")
print("2. 数据清理和转换")
print("3. 数据可视化")
print("4. 批量处理和报告生成")
```

### 28.4.2 数据分析最佳实践

```python
# 数据分析最佳实践
print("数据分析最佳实践:")

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from pathlib import Path
import time
import logging
from typing import Dict, List, Optional, Union
import warnings

# 1. 项目结构和组织
print("项目结构和组织:")

def create_project_structure(project_name: str):
    """创建标准的数据分析项目结构"""

    base_path = Path(project_name)
    directories = [
        'data/raw',           # 原始数据
        'data/processed',     # 处理后的数据
        'data/external',      # 外部数据
        'notebooks',          # Jupyter notebooks
        'src',               # 源代码
        'src/data',          # 数据处理代码
        'src/features',      # 特征工程代码
        'src/models',        # 模型代码
        'src/visualization', # 可视化代码
        'reports',           # 报告
        'reports/figures',   # 图表
        'models',            # 训练好的模型
        'config',            # 配置文件
        'logs'               # 日志文件
    ]

    # 创建目录
    for dir_path in directories:
        (base_path / dir_path).mkdir(parents=True, exist_ok=True)

    # 创建标准文件
    files_to_create = {
        'README.md': f'# {project_name}\n\n数据分析项目描述',
        'requirements.txt': 'pandas\nnumpy\nmatplotlib\nseaborn\nscikit-learn\njupyter',
        '.gitignore': '__pycache__/\n*.pyc\n.DS_Store\n*.log\nmodels/*.pkl',
        'config/config.yaml': 'project:\n  name: {project_name}\n  version: "1.0.0"',
        'src/__init__.py': '',
        'src/data/__init__.py': '',
        'src/features/__init__.py': '',
        'src/models/__init__.py': '',
        'src/visualization/__init__.py': ''
    }

    for file_path, content in files_to_create.items():
        full_path = base_path / file_path
        full_path.parent.mkdir(parents=True, exist_ok=True)
        full_path.write_text(content)

    print(f"项目结构已创建: {project_name}")
    return base_path

# 2. 日志和错误处理
print("\n日志和错误处理:")

def setup_logging(log_file: str = 'analysis.log', level: int = logging.INFO):
    """设置日志系统"""

    # 创建日志格式
    formatter = logging.Formatter(
        '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
    )

    # 文件处理器
    file_handler = logging.FileHandler(log_file)
    file_handler.setFormatter(formatter)
    file_handler.setLevel(level)

    # 控制台处理器
    console_handler = logging.StreamHandler()
    console_handler.setFormatter(formatter)
    console_handler.setLevel(level)

    # 配置根日志器
    logging.getLogger().setLevel(level)
    logging.getLogger().addHandler(file_handler)
    logging.getLogger().addHandler(console_handler)

    return logging.getLogger(__name__)

class DataAnalysisError(Exception):
    """数据分析错误基类"""
    pass

class DataLoadError(DataAnalysisError):
    """数据加载错误"""
    pass

class DataProcessingError(DataAnalysisError):
    """数据处理错误"""
    pass

def safe_operation(operation_func):
    """安全操作装饰器"""
    def wrapper(*args, **kwargs):
        logger = logging.getLogger(__name__)
        try:
            start_time = time.time()
            result = operation_func(*args, **kwargs)
            end_time = time.time()
            logger.info(".2f")
            return result
        except Exception as e:
            logger.error(f"操作失败: {e}")
            raise
    return wrapper

# 3. 配置管理
print("\n配置管理:")

import yaml
from dataclasses import dataclass, asdict

@dataclass
class DataConfig:
    """数据配置"""
    input_path: str
    output_path: str
    file_format: str = 'csv'
    encoding: str = 'utf-8'
    chunk_size: Optional[int] = None

@dataclass
class ModelConfig:
    """模型配置"""
    model_type: str = 'linear'
    test_size: float = 0.2
    random_state: int = 42

@dataclass
class AnalysisConfig:
    """分析配置"""
    data: DataConfig
    model: ModelConfig
    enable_logging: bool = True
    log_level: str = 'INFO'

class ConfigManager:
    """配置管理器"""

    def __init__(self, config_file: str):
        self.config_file = Path(config_file)
        self.config = None

    def load_config(self) -> AnalysisConfig:
        """加载配置"""
        if not self.config_file.exists():
            # 创建默认配置
            self.config = AnalysisConfig(
                data=DataConfig(
                    input_path='data/raw/input.csv',
                    output_path='data/processed/output.csv'
                ),
                model=ModelConfig()
            )
            self.save_config()
        else:
            with open(self.config_file, 'r', encoding='utf-8') as f:
                config_dict = yaml.safe_load(f)

            # 转换为数据类
            data_config = DataConfig(**config_dict['data'])
            model_config = ModelConfig(**config_dict['model'])

            self.config = AnalysisConfig(
                data=data_config,
                model=model_config,
                **{k: v for k, v in config_dict.items()
                   if k not in ['data', 'model']}
            )

        return self.config

    def save_config(self):
        """保存配置"""
        if self.config:
            config_dict = asdict(self.config)
            with open(self.config_file, 'w', encoding='utf-8') as f:
                yaml.dump(config_dict, f, default_flow_style=False)

# 4. 数据验证
print("\n数据验证:")

from typing import Callable, Any

class DataValidator:
    """数据验证器"""

    def __init__(self):
        self.rules = []

    def add_rule(self, column: str, rule_func: Callable, error_message: str):
        """添加验证规则"""
        self.rules.append({
            'column': column,
            'rule': rule_func,
            'message': error_message
        })

    def validate(self, df: pd.DataFrame) -> List[str]:
        """验证数据"""
        errors = []

        for rule in self.rules:
            column = rule['column']
            rule_func = rule['rule']
            message = rule['message']

            if column not in df.columns:
                errors.append(f"列不存在: {column}")
                continue

            # 应用规则
            mask = df[column].apply(rule_func)
            invalid_count = (~mask).sum()

            if invalid_count > 0:
                errors.append(f"{message}: {invalid_count} 行不符合要求")

        return errors

def create_common_validator() -> DataValidator:
    """创建常用验证器"""

    validator = DataValidator()

    # 年龄验证（0-120岁）
    validator.add_rule(
        'age',
        lambda x: pd.isna(x) or (isinstance(x, (int, float)) and 0 <= x <= 120),
        "年龄必须在0-120岁之间"
    )

    # 邮箱验证
    import re
    email_pattern = re.compile(r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$')
    validator.add_rule(
        'email',
        lambda x: pd.isna(x) or email_pattern.match(str(x)),
        "邮箱格式不正确"
    )

    # 分数验证（0-100）
    validator.add_rule(
        'score',
        lambda x: pd.isna(x) or (isinstance(x, (int, float)) and 0 <= x <= 100),
        "分数必须在0-100之间"
    )

    return validator

# 5. 性能优化
print("\n性能优化:")

def optimize_dataframe(df: pd.DataFrame) -> pd.DataFrame:
    """优化DataFrame性能"""

    df_optimized = df.copy()

    # 优化数据类型
    for col in df_optimized.select_dtypes(include=['float64']).columns:
        df_optimized[col] = df_optimized[col].astype('float32')

    for col in df_optimized.select_dtypes(include=['int64']).columns:
        if df_optimized[col].min() >= 0:
            df_optimized[col] = df_optimized[col].astype('uint32')
        else:
            df_optimized[col] = df_optimized[col].astype('int32')

    # 分类数据优化
    for col in df_optimized.select_dtypes(include=['object']).columns:
        if df_optimized[col].nunique() / len(df_optimized) < 0.5:  # 唯一值比例小于50%
            df_optimized[col] = df_optimized[col].astype('category')

    # 计算内存使用
    original_memory = df.memory_usage(deep=True).sum() / 1024 / 1024
    optimized_memory = df_optimized.memory_usage(deep=True).sum() / 1024 / 1024

    print(".1f")

    return df_optimized

def chunked_processing(file_path: str, chunk_size: int = 10000,
                      processor_func: Callable = None) -> pd.DataFrame:
    """分块处理大文件"""

    if processor_func is None:
        processor_func = lambda x: x

    chunks = []
    for chunk in pd.read_csv(file_path, chunksize=chunk_size):
        processed_chunk = processor_func(chunk)
        chunks.append(processed_chunk)

    return pd.concat(chunks, ignore_index=True)

# 6. 代码重用和模块化
print("\n代码重用和模块化:")

class DataPipeline:
    """数据处理管道"""

    def __init__(self):
        self.steps = []

    def add_step(self, step_func: Callable, name: str = None):
        """添加处理步骤"""
        self.steps.append({
            'func': step_func,
            'name': name or step_func.__name__
        })

    def execute(self, data: Any) -> Any:
        """执行管道"""
        logger = logging.getLogger(__name__)
        result = data

        for step in self.steps:
            logger.info(f"执行步骤: {step['name']}")
            start_time = time.time()

            result = step['func'](result)

            end_time = time.time()
            logger.info(".2f")

        return result

# 7. 测试驱动开发
print("\n测试驱动开发:")

def create_test_data(n_rows: int = 100) -> pd.DataFrame:
    """创建测试数据"""

    np.random.seed(42)

    data = {
        'id': range(1, n_rows + 1),
        'name': [f'User_{i}' for i in range(1, n_rows + 1)],
        'age': np.random.randint(18, 80, n_rows),
        'score': np.random.uniform(0, 100, n_rows),
        'category': np.random.choice(['A', 'B', 'C'], n_rows),
        'date': pd.date_range('2023-01-01', periods=n_rows, freq='D')
    }

    return pd.DataFrame(data)

def run_data_quality_tests(df: pd.DataFrame) -> Dict[str, bool]:
    """运行数据质量测试"""

    tests = {}

    # 测试1: 非空检查
    tests['no_null_values'] = not df.isnull().any().any()

    # 测试2: 数据类型检查
    expected_types = {
        'id': 'int64',
        'name': 'object',
        'age': 'int64',
        'score': 'float64',
        'category': 'object'
    }

    type_checks = []
    for col, expected_type in expected_types.items():
        if col in df.columns:
            type_checks.append(str(df[col].dtype) == expected_type)

    tests['correct_data_types'] = all(type_checks)

    # 测试3: 数值范围检查
    if 'age' in df.columns:
        tests['age_range'] = (df['age'] >= 0).all() and (df['age'] <= 120).all()

    if 'score' in df.columns:
        tests['score_range'] = (df['score'] >= 0).all() and (df['score'] <= 100).all()

    # 测试4: 唯一性检查
    if 'id' in df.columns:
        tests['unique_ids'] = df['id'].is_unique

    return tests

# 8. 文档和报告
print("\n文档和报告:")

def generate_analysis_summary(df: pd.DataFrame, output_file: str = 'analysis_summary.md'):
    """生成分析摘要"""

    summary = f"""# 数据分析摘要

## 数据概览
- 数据行数: {df.shape[0]:,}
- 数据列数: {df.shape[1]}
- 内存使用: {df.memory_usage(deep=True).sum() / 1024 / 1024:.1f} MB

## 数据质量
"""

    # 缺失值统计
    missing = df.isnull().sum()
    if missing.sum() > 0:
        summary += "\n### 缺失值统计\n"
        for col, count in missing[missing > 0].items():
            summary += f"- {col}: {count} ({count/len(df)*100:.1f}%)\n"

    # 数值列统计
    numeric_cols = df.select_dtypes(include=[np.number]).columns
    if len(numeric_cols) > 0:
        summary += "\n### 数值列统计\n"
        stats = df[numeric_cols].describe()
        summary += f"```\n{stats.to_string()}\n```"

    # 分类列统计
    categorical_cols = df.select_dtypes(include=['object', 'category']).columns
    if len(categorical_cols) > 0:
        summary += "\n### 分类列统计\n"
        for col in categorical_cols:
            summary += f"\n#### {col}\n"
            value_counts = df[col].value_counts()
            for value, count in value_counts.head().items():
                summary += f"- {value}: {count} ({count/len(df)*100:.1f}%)\n"

    # 保存报告
    with open(output_file, 'w', encoding='utf-8') as f:
        f.write(summary)

    print(f"分析摘要已保存到: {output_file}")

# 9. 完整的最佳实践示例
print("\n完整的最佳实践示例:")

def run_best_practice_analysis():
    """运行最佳实践数据分析"""

    # 设置日志
    logger = setup_logging()
    logger.info("开始数据分析")

    try:
        # 创建项目结构
        project_path = create_project_structure('sample_analysis')
        logger.info(f"项目结构创建完成: {project_path}")

        # 创建测试数据
        test_data = create_test_data(1000)
        test_file = project_path / 'data' / 'raw' / 'sample_data.csv'
        test_data.to_csv(test_file, index=False)
        logger.info(f"测试数据创建完成: {test_file}")

        # 运行数据质量测试
        quality_tests = run_data_quality_tests(test_data)
        logger.info(f"数据质量测试结果: {quality_tests}")

        # 优化数据
        optimized_data = optimize_dataframe(test_data)
        logger.info("数据优化完成")

        # 生成分析摘要
        summary_file = project_path / 'reports' / 'analysis_summary.md'
        generate_analysis_summary(optimized_data, str(summary_file))
        logger.info(f"分析摘要生成完成: {summary_file}")

        # 保存处理后的数据
        processed_file = project_path / 'data' / 'processed' / 'optimized_data.csv'
        optimized_data.to_csv(processed_file, index=False)
        logger.info(f"处理后数据保存完成: {processed_file}")

        logger.info("数据分析完成")

        return {
            'project_path': project_path,
            'quality_tests': quality_tests,
            'original_shape': test_data.shape,
            'optimized_shape': optimized_data.shape
        }

    except Exception as e:
        logger.error(f"数据分析失败: {e}")
        raise

print("数据分析最佳实践示例已定义")
print("这些最佳实践包括:")
print("1. 标准项目结构")
print("2. 日志和错误处理")
print("3. 配置管理")
print("4. 数据验证")
print("5. 性能优化")
print("6. 代码重用和模块化")
print("7. 测试驱动开发")
print("8. 文档和报告生成")
```

## 总结

本章介绍了 Python 数据分析的基础知识：
- 掌握 NumPy 数组操作，包括创建、索引、切片和数学运算
- 学会 Pandas 数据处理，包括 Series 和 DataFrame 的操作
- 理解 Matplotlib 数据可视化，包括各种图表类型和自定义选项
- 掌握完整的数据分析工作流，从数据加载到报告生成
- 遵循数据分析的最佳实践，包括项目结构、日志管理和性能优化

通过本章的学习，你应该能够：
1. 使用 NumPy 进行高效的数值计算和数组操作
2. 使用 Pandas 进行复杂的数据处理和分析任务
3. 创建各种类型的数据可视化图表
4. 实现完整的数据分析流程
5. 遵循数据分析的最佳实践和标准

下一章将介绍 Web 应用基础。
