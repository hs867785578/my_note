
## 1 或者使用\\ 或者使用()
pep8规定一行最多不超过79个字符，所以当一行代码过长时，需要进行换行，可以通过\\来显示的表达续行，也可以用()隐式的表达续行。python会认为()之中的所有字符在一行

```python
if (a == True and
    b == False):
```

或有明确的换行。

```python
if a == True and \
   b == False:
```

```python
a = ('1' + '2' + '3' +
    '4' + '5')
```

使用显式换行可以获得同样的效果。

```python
a = '1' + '2' + '3' + \
    '4' + '5'
```
同样在函数的参数也是如此
```python
class Rectangle(Blob):

  def __init__(self, width, height,
                color='black', emphasis=None, highlight=0):
```

**使用()时又分为隐式续行和悬挂续行：**
google推荐隐式续航
隐式续行：垂直对齐于圆括号、方括号和花括号。
```python
foo = long_function_name(var_one, var_two, 
						 var_three, var_four) # 和左侧的圆括号对齐
```
foo = long_function_name(var_one, var_two, var_three, var_four) # 和左侧的圆括号对齐

悬挂续行：续行多缩进一级以同其他代码区别 悬挂续行多缩进了一级，同时第一行没有参数

```python
def long_function_name(
		var_one, var_two, var_three,
		var_four):
	 print(var_one)
# 悬挂缩进需要多缩进一级 # 区别于下面的
foo = long_function_name(
	var_one, var_two, var_three, var_four)
```
