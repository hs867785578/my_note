
## 1.迭代器的几种方式

### 最简单的方法：类中实现__iter__和__next__方法，其中__iter__返回self

```python
class MyIterator:
    def __init__(self, start, end):
        self.current = start
        self.end = end

    def __iter__(self):
        return self

    def __next__(self):
        if self.current < self.end:
            self.current += 1
            return self.current - 1
        else:
            raise StopIteration

# 使用
my_iter = MyIterator(1, 5)
for num in my_iter:
    print(num)
```
### 返回其他迭代器
```python
# 定义一个生成器类
class MyGenerator:
    def __init__(self, start, end):
        self.current = start
        self.end = end

    def __iter__(self):
        return self

    def __next__(self):
        if self.current < self.end:
            self.current += 1
            return self.current - 1
        else:
            raise StopIteration

# 定义一个类，其 __iter__ 方法返回 MyGenerator 的实例
class MyIterable:
    def __init__(self, start, end):
        self.start = start
        self.end = end

    def __iter__(self):
        # 返回 MyGenerator 的实例
        return MyGenerator(self.start, self.end)

# 使用
obj = MyIterable(1, 5)
for num in obj:
    print(num)
```


### 返回生成器

```python
# 定义两个生成器类
class EvenGenerator:
    def __init__(self, limit):
        self.current = 0
        self.limit = limit

    def __iter__(self):
        return self

    def __next__(self):
        if self.current < self.limit:
            value = self.current
            self.current += 2
            return value
        else:
            raise StopIteration

class OddGenerator:
    def __init__(self, limit):
        self.current = 1
        self.limit = limit

    def __iter__(self):
        return self

    def __next__(self):
        if self.current < self.limit:
            value = self.current
            self.current += 2
            return value
        else:
            raise StopIteration

# 定义一个类，其 __iter__ 方法动态返回生成器类实例
class DynamicIterable:
    def __init__(self, mode, limit):
        self.mode = mode
        self.limit = limit

    def __iter__(self):
        if self.mode == "even":
            return EvenGenerator(self.limit)
        elif self.mode == "odd":
            return OddGenerator(self.limit)
        else:
            raise ValueError("Invalid mode")

# 使用
even_iter = DynamicIterable("even", 10)
print("Even numbers:")
for num in even_iter:
    print(num)

odd_iter = DynamicIterable("odd", 10)
print("Odd numbers:")
for num in odd_iter:
    print(num)
```

### 返回生成器

```python
class DynamicIterable:
    def __init__(self, mode):
        self.mode = mode

    def __iter__(self):
        if self.mode == "even":
            return self.even_generator()
        elif self.mode == "odd":
            return self.odd_generator()
        else:
            return self.default_generator()

    def even_generator(self):
        for i in range(0, 10, 2):
            yield i

    def odd_generator(self):
        for i in range(1, 10, 2):
            yield i

    def default_generator(self):
        for i in range(10):
            yield i

# 使用
even_iter = DynamicIterable("even")
print("Even numbers:")
for num in even_iter:
    print(num)

odd_iter = DynamicIterable("odd")
print("Odd numbers:")
for num in odd_iter:
    print(num)
```