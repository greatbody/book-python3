# 第9章：集合（Set）

## 9.1 集合的创建和特性

集合是 Python 中存储唯一元素的无序数据结构，类似于数学中的集合概念。

### 9.1.1 集合的创建

```python
# 空集合
empty_set = set()
# 注意：{} 创建的是空字典，不是空集合

# 有元素的集合
fruits = {"apple", "banana", "orange"}
numbers = {1, 2, 3, 4, 5}

# 从列表创建集合（自动去重）
list_with_duplicates = [1, 2, 2, 3, 3, 3, 4]
unique_numbers = set(list_with_duplicates)
print(unique_numbers)  # {1, 2, 3, 4}

# 从字符串创建集合
text = "hello world"
char_set = set(text)
print(char_set)  # {'h', 'e', 'l', 'o', ' ', 'w', 'r', 'd'}

# 从元组创建集合
tuple_data = (1, 2, 3, 2, 1)
tuple_set = set(tuple_data)
print(tuple_set)  # {1, 2, 3}

# 使用 set() 函数
range_set = set(range(5))
print(range_set)  # {0, 1, 2, 3, 4}
```

### 9.1.2 集合的特性

```python
# 无序性
numbers = {3, 1, 4, 1, 5, 9}
print(numbers)  # {1, 3, 4, 5, 9} (顺序可能不同)

# 唯一性（自动去重）
duplicates = {1, 2, 2, 3, 3, 3}
print(duplicates)  # {1, 2, 3}

# 可变性
mutable_set = {1, 2, 3}
mutable_set.add(4)
print(mutable_set)  # {1, 2, 3, 4}

# 元素类型可以混合
mixed_set = {1, "hello", (1, 2), 3.14}
print(mixed_set)  # {1, 3.14, (1, 2), 'hello'}

# 但是元素必须是可哈希的（不能使用列表作为元素）
# invalid_set = {1, [2, 3]}  # TypeError: unhashable type: 'list'
```

### 9.1.3 集合的基本操作

```python
fruits = {"apple", "banana", "orange", "grape"}

# 长度
print(len(fruits))  # 4

# 成员测试
print("apple" in fruits)      # True
print("pineapple" in fruits)  # False

# 遍历集合
for fruit in fruits:
    print(fruit)

# 集合不能通过索引访问
# print(fruits[0])  # TypeError: 'set' object is not subscriptable

# 但是可以转换为列表后访问
fruits_list = list(fruits)
print(fruits_list[0])  # 可以访问（但顺序不确定）
```

## 9.2 集合操作

### 9.2.1 集合代数运算

```python
set_a = {1, 2, 3, 4, 5}
set_b = {4, 5, 6, 7, 8}

# 并集（union）
union_result = set_a | set_b
print(union_result)  # {1, 2, 3, 4, 5, 6, 7, 8}

# 交集（intersection）
intersection_result = set_a & set_b
print(intersection_result)  # {4, 5}

# 差集（difference）
diff_a_b = set_a - set_b
print(diff_a_b)  # {1, 2, 3}

diff_b_a = set_b - set_a
print(diff_b_a)  # {6, 7, 8}

# 对称差集（symmetric difference）
symmetric_diff = set_a ^ set_b
print(symmetric_diff)  # {1, 2, 3, 6, 7, 8}
```

### 9.2.2 集合比较运算

```python
set_a = {1, 2, 3}
set_b = {1, 2, 3, 4}
set_c = {2, 3, 4}

# 子集判断
print(set_a <= set_b)  # True (set_a 是 set_b 的子集)
print(set_a < set_b)   # True (真子集)
print(set_a <= set_a)  # True (自身是自身的子集)
print(set_a < set_a)   # False (不是真子集)

# 超集判断
print(set_b >= set_a)  # True (set_b 是 set_a 的超集)
print(set_b > set_a)   # True (真超集)

# 相等判断
set_d = {3, 1, 2}
print(set_a == set_d)  # True (元素相同，顺序无关)
print(set_a == set_b)  # False

# 不相等判断
print(set_a != set_b)  # True
```

### 9.2.3 集合的更新操作

```python
set_a = {1, 2, 3}
set_b = {3, 4, 5}

# 原地并集更新
set_a_copy = set_a.copy()
set_a_copy |= set_b
print(set_a_copy)  # {1, 2, 3, 4, 5}

# 原地交集更新
set_a_copy = set_a.copy()
set_a_copy &= set_b
print(set_a_copy)  # {3}

# 原地差集更新
set_a_copy = set_a.copy()
set_a_copy -= set_b
print(set_a_copy)  # {1, 2}

# 原地对称差集更新
set_a_copy = set_a.copy()
set_a_copy ^= set_b
print(set_a_copy)  # {1, 2, 4, 5}
```

## 9.3 集合方法

### 9.3.1 添加和删除元素

```python
fruits = {"apple", "banana"}

# add() - 添加单个元素
fruits.add("orange")
print(fruits)  # {'banana', 'orange', 'apple'}

# 添加已存在的元素（无效果）
fruits.add("apple")
print(fruits)  # {'banana', 'orange', 'apple'} (元素不变)

# update() - 添加多个元素
fruits.update(["grape", "kiwi"])
print(fruits)  # {'banana', 'orange', 'apple', 'grape', 'kiwi'}

# remove() - 删除指定元素（元素不存在时抛出 KeyError）
fruits.remove("banana")
print(fruits)  # {'orange', 'apple', 'grape', 'kiwi'}

# discard() - 删除指定元素（元素不存在时无操作）
fruits.discard("pineapple")  # 不抛出错误
print(fruits)

# pop() - 随机删除并返回一个元素
removed_fruit = fruits.pop()
print(f"删除了: {removed_fruit}")
print(fruits)

# clear() - 清空集合
fruits.clear()
print(fruits)  # set()
```

### 9.3.2 集合运算方法

```python
set_a = {1, 2, 3, 4}
set_b = {3, 4, 5, 6}

# union() - 并集
union_result = set_a.union(set_b)
print(union_result)  # {1, 2, 3, 4, 5, 6}

# intersection() - 交集
intersection_result = set_a.intersection(set_b)
print(intersection_result)  # {3, 4}

# difference() - 差集
diff_result = set_a.difference(set_b)
print(diff_result)  # {1, 2}

# symmetric_difference() - 对称差集
sym_diff_result = set_a.symmetric_difference(set_b)
print(sym_diff_result)  # {1, 2, 5, 6}

# isdisjoint() - 检查是否不相交
disjoint_sets = {1, 2}
other_sets = {3, 4}
print(disjoint_sets.isdisjoint(other_sets))  # True
print(disjoint_sets.isdisjoint(set_a))       # False

# issubset() - 检查是否是子集
subset = {1, 2}
print(subset.issubset(set_a))  # True

# issuperset() - 检查是否是超集
superset = {1, 2, 3, 4, 5}
print(set_a.issuperset(subset))  # True
```

### 9.3.3 复制和转换

```python
original = {1, 2, 3, 4, 5}

# 复制集合
copied = original.copy()
copied2 = set(original)

# 转换为其他类型
list_version = list(original)
tuple_version = tuple(original)
print(list_version)  # [1, 2, 3, 4, 5] (顺序可能不同)
print(tuple_version) # (1, 2, 3, 4, 5)

# 集合推导式
squared = {x**2 for x in original}
print(squared)  # {1, 4, 9, 16, 25}
```

## 9.4 frozenset

frozenset 是不可变的集合类型，一旦创建就不能修改。

### 9.4.1 创建 frozenset

```python
# 空 frozenset
empty_frozen = frozenset()

# 从可迭代对象创建
numbers = frozenset([1, 2, 3, 4, 5])
fruits = frozenset(["apple", "banana", "orange"])

# 从集合创建
regular_set = {1, 2, 3}
frozen_copy = frozenset(regular_set)

print(numbers)     # frozenset({1, 2, 3, 4, 5})
print(type(numbers))  # <class 'frozenset'>
```

### 9.4.2 frozenset 的特性

```python
frozen_set = frozenset([1, 2, 3, 4, 5])

# 支持所有只读操作
print(len(frozen_set))      # 5
print(1 in frozen_set)      # True
print(frozen_set & {3, 4, 6})  # frozenset({3, 4})

# 不支持修改操作
# frozen_set.add(6)        # AttributeError
# frozen_set.remove(1)     # AttributeError
# frozen_set.clear()       # AttributeError

# 但是支持集合运算
set_a = frozenset([1, 2, 3])
set_b = frozenset([3, 4, 5])

print(set_a | set_b)  # frozenset({1, 2, 3, 4, 5})
print(set_a & set_b)  # frozenset({3})
print(set_a - set_b)  # frozenset({1, 2})
```

### 9.4.3 frozenset 的应用场景

```python
# 作为字典的键
regular_set = {1, 2, 3}
frozen_set = frozenset([1, 2, 3])

# 集合不能作为字典键
# dict_with_set_key = {regular_set: "value"}  # TypeError

# frozenset 可以作为字典键
dict_with_frozen_key = {frozen_set: "value"}
print(dict_with_frozen_key[frozenset([1, 2, 3])])  # value

# 作为集合的元素
set_of_sets = {frozenset([1, 2]), frozenset([3, 4])}
print(set_of_sets)  # {frozenset({1, 2}), frozenset({3, 4})}

# 缓存键
def get_cache_key(*args):
    return frozenset(args)

cache = {}
key = get_cache_key("param1", "param2", 42)
cache[key] = "cached_result"
print(cache[key])  # cached_result
```

### 9.4.4 set vs frozenset

```python
# 可变集合
mutable_set = {1, 2, 3}
mutable_set.add(4)
print(mutable_set)  # {1, 2, 3, 4}

# 不可变集合
immutable_set = frozenset([1, 2, 3])
# immutable_set.add(4)  # AttributeError

# 内存使用比较
import sys
mutable = {1, 2, 3, 4, 5}
immutable = frozenset([1, 2, 3, 4, 5])

print(sys.getsizeof(mutable))   # 通常更大
print(sys.getsizeof(immutable)) # 通常更小

# 哈希值（frozenset 可以作为字典键）
print(hash(immutable))  # 有哈希值
# print(hash(mutable))  # TypeError: unhashable type
```

## 总结

本章介绍了 Python 的集合数据结构：
- 集合的创建和特性（无序、唯一、可变）
- 集合的代数运算（并集、交集、差集、对称差集）
- 集合的比较运算（子集、超集判断）
- 集合的各种方法（添加、删除、运算）
- frozenset 不可变集合的使用

通过本章的学习，你应该能够：
1. 熟练创建和操作集合
2. 使用集合进行数学集合运算
3. 理解集合的唯一性和无序性
4. 选择 set 或 frozenset 处理不同场景
5. 使用集合解决去重和成员测试问题

下一章将介绍函数，这是代码组织和重用的基础。
