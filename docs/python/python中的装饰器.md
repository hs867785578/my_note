
## 实现原理


### 简单装饰器

```python
def my_decorator(func):
    def wrapper():
        print("在函数执行前做一些事情")
        func()  # 调用被装饰的函数
        print("在函数执行后做一些事情")
    return wrapper

@my_decorator  #等价于 say_hello = my_decorator(say_hello)
def say_hello():
    print("Hello!")



say_hello()

## 输出
在函数执行前做一些事情
Hello!
在函数执行后做一些事情

```
### 带参数的装饰器

```python
def repeat(num_times):  # 外层函数，接受装饰器的参数
    def decorator(func):  # 装饰器函数，接受被装饰的函数
        def wrapper(*args, **kwargs):  # 内部函数，调用被装饰的函数
            for _ in range(num_times):
                result = func(*args, **kwargs)  # 调用被装饰的函数
            return result
        return wrapper  # 返回内部函数
    return decorator  # 返回装饰器函数

@repeat(num_times=3)  #  等价于say_hello = repeat(num_times=3)(say_hello)
def say_hello(name):
    print(f"Hello, {name}!")

say_hello("Alice")

### 输出
Hello, Alice!
Hello, Alice!
Hello, Alice!
```

### 类装饰器

```python
class MyDecorator:
    def __init__(self, func):  # 接受被装饰的函数
        self.func = func

    def __call__(self, *args, **kwargs):  # 调用被装饰的函数时执行
        print("在函数执行前做一些事情")
        result = self.func(*args, **kwargs)  # 调用被装饰的函数
        print("在函数执行后做一些事情")
        return result

@MyDecorator  # say_hello = MyDecorator(say_hello)
def say_hello(name):
    print(f"Hello, {name}!")

say_hello("Bob")
```