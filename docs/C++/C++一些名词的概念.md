
value category(值类别)，type(类型)，expression(表达式)，object(对象)，reference(引用)这几个名词之间到底是什么关系。

**表达式，对象，引用都有类型**。引用不是对象。对象不是表达式。表达式可以创建对象。引用绑定一个对象。只有表达式有值类别。引用和对象都是entity。

**prvalue和xvalue都是值类别**，只能用来形容表达式。右值引用这个中文表达有歧义，可以表示rvalue reference type，也可以表示一个reference的类型是rvalue reference type。

**“右值引用不是右值”，指的是一个reference作为表达式时不是右值**，但如果是一个返回的类型是rvalue reference type的表达式（比如std::move)，那它是右值中的xvalue。


创建这个临时对象的表达式的值类别肯定是prvalue，但如果该表达式是其他表达式的子表达式，那其他表达式完全可以将其包裹后让值类别变成别的（加个static_cast完事）。std::move(A{})的问题在于，这个表达式是一个函数调用，A{}创建的对象会被绑定到std::move的函数参数上去（参数的类型是reference type）。然后我们又有“the lifetime of the temporary object is extended to match the lifetime of the reference”，所以这个临时对象的lifetime等于std::move的参数的lifetime，在函数调用结束后自然就被销毁了。


![](images/C++一些名词的概念_image_1.png)
  
作者：ZhiHuReader  
链接：https://www.zhihu.com/question/604327959/answer/3140142366  
来源：知乎  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
  
作者：ZhiHuReader  
链接：https://www.zhihu.com/question/604327959/answer/3140142366  
来源：知乎  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
