引用计数：对于一块内存，多一个指针指向它，指针的引用计数就会+1。
对于指针指针赋值来说，=号右边的引用计数+1，=左边的引用计数-1.下图中sp1 -1 ，sp4+1
![](images/C++智能指针_image_1.png)


# 1. RAII
RAII 是 resource acquisition is initialization 的缩写，意为“资源获取即初始化”。它是 C++ 之父 Bjarne Stroustrup 提出的设计理念，其核心是把资源和对象的生命周期绑定，**对象创建获取资源，对象销毁释放资源**。在 RAII 的指导下，C++ 把底层的资源管理问题提升到了对象生命周期管理的更高层次。

# 2. unique_ptr

unique_ptr的原理很简单，就是一个“得不到就毁掉”的理念，直接把拷贝和赋值禁止了。

对于用不上赋值拷贝的场景的时候，我们选择unique_ptr也是一个不错的选择。

```cpp
template<class T>
	class unique_ptr
	{
	public:
		unique_ptr(T* ptr = nullptr)
			:_ptr(ptr)
		{}

		//防拷贝
		unique_ptr(unique_ptr<T>& ap) = delete;
		unique_ptr<T>& operator = (unique_ptr<T>& ap) = delete;

		~SmartPtr()
		{
			if (_ptr)delete _ptr;
		}
        //运算符重载，支持解引用和->符号
		T& operator *()
		{
			return *_ptr;
		}
		T* operator ->()
		{
			return _ptr;
		}
	private:
		T* _ptr;
	};

```

# 3. shared_ptr
我们可以对一个资源添加一个计数器，让所有管理该资源的智能共用这个计数器，倘若发生拷贝，计数器加一，倘若有析构发生， 计数器减一，当计数器等于0的时候，就把对象析构掉。
所有的智能指针共同维护着两个地址：一个时对象资源的地址，一个是引用计数的地址（不是一个shared_ptr有一个int计数，而是共同维护一个int*）

```cpp
template<class T>
	class shared_ptr
	{
	public:
		shared_ptr(T*ptr =nullptr)
			:_ptr(ptr),_pcount(new int(1))
		{}
		//拷贝构造,它们共同的计数+1
		shared_ptr(const shared_ptr<T>& sp)
			_ptr(sp._ptr),_pcount(sp._pcount)
		{
			++(*_pcount);
		}
		//拷贝赋值，右＋左-
		shared_ptr<T>& operator = (shared_ptr<T>& sp)
		{
			if (_ptr != sp._ptr) {
        		//拷贝赋值时，=左边计数-1，=右边计数+1
				if (--(*_pcount) == 0){
					delete _pcount;
					delete _ptr;
				}
				_ptr = sp._ptr;
				_pcount = sp._pcount;
				//此时++(*_pcount)和++(*sp._pcount)是等价的
				++(*_pcount);
			}
			return *this;
		}
		T& operator *()
		{
			return *_ptr;
		}

		T* operator ->()
		{
			return _ptr;
		}


		~shared_ptr()
		{   //析构时，计数-1，然后判断：如果引用计数为0才删除指向资源
			if (--(*_pcount) == 0 && _ptr) {
				delete _pcount;
				delete _ptr;
			}
		}
	private:
		T* _ptr;
		int* _pcount;
	};

```
- 拷贝构造

![在这里插入图片描述](https://img-blog.csdnimg.cn/9e243443de3b4f20a111ac27dc2183b1.png)

- 赋值拷贝

赋值拷贝需要注意两点：

1. 在被赋值之前的对象需要将自己析构，也就是放弃当前资源的管理权，然后再去被赋值，取得新的管理权。
2. 避免自己对自己赋值，按照1中的机制，如果自己对自己赋值，会造成无谓的操作，或者误析构资源。

![在这里插入图片描述](https://img-blog.csdnimg.cn/04c6c45110564c909f23f2cb17f414ab.png)
# 4. weak_ptr


如果你仔细思考 `std::shared_ptr` 就会发现依然存在着资源无法释放的问题。看下面这个例子：

```cpp
struct A;  
struct B;  
  
struct A {  
    std::shared_ptr<B> pointer;  
    ~A() {  
        std::cout << "A 被销毁" << std::endl;  
    }  
};  
struct B {  
    std::shared_ptr<A> pointer;  
    ~B() {  
        std::cout << "B 被销毁" << std::endl;  
    }  
};  
int main() {  
    auto a = std::make_shared<A>();  
    auto b = std::make_shared<B>();  
    a->pointer = b;  
    b->pointer = a;  
}
```

运行结果是 A, B 都不会被销毁，这是因为 a,b 内部的 pointer 同时又引用了 `a,b`，这使得 `a,b` 的引用计数均变为了 2，**而离开作用域时，`a,b` 智能指针被析构**，却只能造成这块区域的引用计数-1，这样就导致了 `a,b` 对象指向的内存区域引用计数不为零，而外部已经没有办法找到这块区域了，也就造成了内存泄露。
那么A节点资源什么时候析构呢, 当B->pointer析构，也就是当B节点资源析构，那么B节点资源什么时候析构呢，当A->pointer析构，也就是当A节点资源析构…此时形成了一个类似于“死锁”的情况。
如图 ：

![图 5.1](https://changkun.de/modern-cpp/assets/figures/pointers1.png)

解决这个问题的办法就是使用弱引用指针 `std::weak_ptr`，`std::weak_ptr`是一种弱引用（相比较而言 `std::shared_ptr` 就是一种强引用）。弱引用不会引起引用计数增加，当换用弱引用时候，最终的释放流程如图所示(**A计数为1，B计数为2，离开作用域时a,b销毁，由于此时A的计数为0，B计数为1，A销毁，所以A->pointer销毁，B的计数-1，变为0，B也销毁**)：
```cpp
struct B {  
    std::weak_ptr<A> pointer;  
    ~B() {  
        std::cout << "B 被销毁" << std::endl;  
    }  
};
```

![图 5.2](https://changkun.de/modern-cpp/assets/figures/pointers2.png)

在上图中，最后一步只剩下 B，而 B 并没有任何智能指针引用它，因此这块内存资源也会被释放。

`std::weak_ptr` 没有 `*` 运算符和 `->` 运算符，所以不能够对资源进行操作，它可以用于检查 `std::shared_ptr` 是否存在，其 `expired()` 方法能在资源未被释放时，会返回 `false`，否则返回 `true`；除此之外，它也可以用于获取指向原始对象的 `std::shared_ptr` 指针，其 `lock()` 方法在原始对象未被释放时，返回一个指向原始对象的 `std::shared_ptr` 指针，进而访问原始对象的资源，否则返回`nullptr`。

weak_ptr 不是一个RALL智能指针，它不参与资源的管理，他是专门用来解决引用计数的，我们可以使用一个shared_ptr 来初始化一个weak_ptr,但是weak_ptr 不增加引用计数，不参与管理，但是也像指针一样访问修改资源。

实现如下：
```cpp
	template<class T>
	class weak_ptr
	{
	public:
		weak_ptr()
			:_ptr(nullptr)
		{}
		weak_ptr(shared_ptr<T>& sp)
			:_ptr(sp.get()),_pcount(sp.use_count())
		{}
		weak_ptr(weak_ptr<T>& sp)
			:_ptr(sp._ptr), _pcount(sp._pcount)
		{}
		weak_ptr& operator = (shared_ptr<T>& sp)
		{
			_ptr = sp.get();
			*_pcount = sp.use_count();
			return *this;
		}
		weak_ptr& operator = (weak_ptr<T>& sp)
		{
			_ptr = sp._ptr;
			_pcount = sp._pcount;
			return *this;
		}
		
		bool expired() const  { return *_pcount == 0; }
		
		shared_ptr<T> lock()  {
		if (expired()) { 
    		return shared_ptr<T>(nullptr);
    	} else { 
		    return SharedPtr<T>(*this); 
		}
	private:
		T* _ptr;
		int* _pcount;

	};

```


# 5 自定义删除器

不管是我们自己实现的shared_ptr还是库中的shared_ptr,我们在析构的时候默认都是 delete _ptr,如果我们托管的类型是 new T[] ,或者 malloc出来的话，就导致类型不是匹配的，无法析构。

为此，shared_ptr提供了 定制删除器，我们可以在构造的时候作为参数传入。如果我们不传参，就默认使用delete

![[images/C++智能指针_image_2.png]]

```cpp

	template<class T>
	struct DeleteArray
	{
		void operator()(T* ptr)
		{
			delete[]ptr;
		}
	};
	
	void test_deletor()
	{
		DeleteArray<string>da; //使用仿函数定制
		std::shared_ptr<string>s2(new string[10], da);

		std::shared_ptr<string>s3((string*)malloc(sizeof(string)),
		 [](string* ptr) {free(ptr); }); //使用lamdba 定制
	}

```