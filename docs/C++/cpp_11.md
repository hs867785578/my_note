# C++11 New Features and Libraries

## New Features

### Move Segmentics
Moving an object means to transfer ownership of some resource it manage to another object.

Benefits:

- Performance optimization: while copying a temporary object(for example, a `vector`) to a list, without `std::move` you have to copy every element in vector and store them in new place. `Moving` a vector is just copying some pointer and internal state over to the new one.
- Make it possible to have only one instance at a time: for example, `std::unique_ptr` is non-copyable, but you can use `move` to transfer an instance between scopes at the language level.

### Rvalue References
`Rvalue reference` is a new reference. It's a non-template type(such as `int`) and can be created with the syntax `T &&`. It can be binded only to rvalues.

### Forwarding References
`Forwarding reference` is also known as `universal references`.

- It can be created by `T&&` when `T` is a template type or `auto&&`.
- This enables `perfect forwarding`: the ability to pass arguments while maintaining their value category(e.g. lvalue stay as lvalue, temporaries are forwarded as rvalue).

### Variadic Templates
The `...` syntax creates a `parameter pack` or expands one. A template `parameter pack` is a template parameter that accepts zero or more template arguments. A template with at least one parameter pack is called a `variadic template`.
An exmple is as following:
```C++
template <typename... T>
struct arity {
  constexpr static int value = sizeof...(T);
};
static_assert(arity<>::value == 0);
static_assert(arity<char, short, int>::value == 3);
```

### Initializer Lists
A lightweight array-like container of elements created using a "braced list" syntax. For example, `1, 2, 3` creates a sequences of integars, that has type `std::initializer_list<int>`. Useful as a replacement to passing a vector of objects to a function.
```C++
int sum(const std::initializer_list<int> &list) {
  int sum = 0;
  for (auto &e : list) {
    sum += e;
  }
  return sum;
}

auto list = {1, 2, 3};
sum(list);
sum({1, 2, 3});
sum({});
```
It can also be used to initialize a vector, for example:
```C++
const std::vector<int> test({1, 2, 3, 4});
```

### Static Assertions
Assertions that are evaluated at compile-time.
Usage: `static_assert(condition, string)`. For example:
```C++
constexpr int x = 0;
constexpr int y = 1;
static_assert(x == y, "x != y");
```
If `x == y` is true, compiler will do nothing, else the compiler will report an error, the error message is `x != y`.

### `auto`
`auto` typed variables are deduced by the compiler according to the type of their initializer. However, if the compiler cannot determine the type of variable, it will report an error.
```C++
auto pi = 3.14;  // double
auto b = 1;  // int
auto l = 1, m = true, n = 1.6;  // error, 1 is int, m is bool
auto o;  // error
```

Functions can also deduce the return type using `auto`.
```C++
template <typename X, typename Y>
auto add(X x, Y y) -> decltype(x + y) {
  return x + y;
}
add(1, 2);  // int
add(1.0, 2);  // double
add(1.0, 2.0);  // double
```

### Lambda Expressions
A `lambda` is an unnamed function object capable of capturing variable in scope. It features:

- a *capture list*
- an optional set of parameters with an optional trailing return type
- a body

An example of `capture list`:

- `[]`: capture nothing
- `[=]`: capture local objects(local variables, parameters) in scope by value
- `[&]`: cpature local objects(local variables, parameters) in scope by reference
- `[this]`: capture `this` pointer by value
- `[a, &b]`: capture object `a` by value and `b` by reference

```C++
int x = 1;

auto getX = [=] { return x; };
getX(); // == 1

auto addX = [=](int y) { return x + y; };
addX(1); // == 2

auto getXRef = [&]() -> int& { return x; };
getXRef(); // int& to `x`
```

By default, value-captures cannot be modified inside the lambda because the complier-generated method is marked as `const`. The `mutable` keyword allows modifying captured variables. The keyword is placed after the parameter-list(which must be present even if it is empty).
```C++
int x = 1;

auto f1 = [&x] { x = 2; }; // OK: x is a reference and modifies the original

auto f2 = [x] { x = 2; }; // ERROR: the lambda can only perform const-operations on the captured value
// vs.
auto f3 = [x]() mutable { x = 2; }; // OK: the lambda can perform any operations on the captured value
```

### `decltype`
`decltype` is an operator which returns the *declared type* of an expression passed to it.
```C++
int a = 1; // `a` is declared as type `int`
decltype(a) b = a; // `decltype(a)` is `int`
const int& c = a; // `c` is declared as type `const int&`
decltype(c) d = a; // `decltype(c)` is `const int&`
decltype(123) e = 123; // `decltype(123)` is `int`
int&& f = 1; // `f` is declared as type `int&&`
decltype(f) g = 1; // `decltype(f) is `int&&`
decltype((a)) h = g; // `decltype((a))` is int&

template <typename X, typename Y>
auto add(X x, Y y) -> decltype(x + y) {
  return x + y;
}
add(1, 2.0); // `decltype(x + y)` => `decltype(3.0)` => `double`
```

### Type Aliases
Semantically similar to using a `typedef` but `using` of *type aliases* is easier to read and are compatible with templates.
```C++
int x = 1;

auto getX = [=] { return x; };
getX(); // == 1

auto addX = [=](int y) { return x + y; };
addX(1); // == 2

auto getXRef = [&]() -> int& { return x; };
getXRef(); // int& to `x`
```

### `nullptr`
C++11 introduces a new null pointer to replace C's `NULL` macro. `nullptr` is of type `std::nullptr_t` and can be implicitly converted into pointer types, and unlike `NULL`, not convertible to integral types except `bool`.
```C++
void foo(int);
void foo(char*);
foo(NULL); // error -- ambiguous
foo(nullptr); // calls foo(char*)
```

### Strongly-typed Enums
Type-safe enums that solve a variable of problems with C-style enums:

- implicit conversions
- inability to specify the underlying type
- scope pollutions

```C++
// Specifying underlying type as `unsigned int`
enum class Color : unsigned int { Red = 0xff0000, Green = 0xff00, Blue = 0xff };
// `Red`/`Green` in `Alert` don't conflict with `Color`
enum class Alert : bool { Red, Green };
Color c = Color::Red;
```

### Attributes
Attributes provide a universal syntax over `__attribute__(...)`, `__declspec`, etc
```C++
// `noreturn` attribute indicates `f` doesn't return.
[[ noreturn ]] void f() {
  throw "error";
}
```

### `constexpr`
`constexp` is the expression that evaluated by the compiler at compile-time. It must be a const expression that compiler can evaluate at compile-time and can be used to indicate the variables, functions, etc.
```C++
const int x = 123;
constexpr const int& y = x; // error -- constexpr variable `y` must be initialized by a constant expression
```

The difference to `const`:

- `const` variables can be initialized at run-time, but `constexp` variables must be initialized at compile-time

### Delegating Constructors
Constructors can now call other constructors in the same class using an initializer list.
```C++
struct Foo {
  int foo;
  Foo(int foo) : foo{foo} {}
  Foo() : Foo(0) {}
};

Foo foo;
foo.foo; // == 0
```

### User-defined Literals
User-defined literals allow you to extend the language and add your own syntax. To create a literal, define:
```C++
T operator ""X(...) {}
```
which returns a type `T` with a name `X`.

Note that the name of this function defines the name of the literal. **Any literal names not starting with an underscore are reserved an won't be invoked**.

String to integar conversion:
```C++
// `const char*` and `std::size_t` required as parameters.
int operator "" _int(const char* str, std::size_t) {
  return std::stoi(str);
}

"123"_int; // == 123, with type `int`
```

### Explicit Virtual Overrides
Specifies that a virtual function overrides another virtual function. If the virtual function does not override a parent's virtual function, throw a compiler error.
```C++
struct A {
  virtual void foo();
  void bar();
};

struct B : A {
  void foo() override; // correct -- B::foo overrides A::foo
  void bar() override; // error -- A::bar is not virtual
  void baz() override; // error -- B::baz does not override A::baz
};
```

### Final Specifier
Specifies that a virtual function cannot be overridden in a derived class or that a class cannot be inherited from.
```C++
struct A {
  virtual void foo();
};

struct B : A {
  virtual void foo() final;
};

struct C : B {
  virtual void foo(); // error -- declaration of 'foo' overrides a 'final' function
};

struct A final {};
struct B : A {}; // error -- base 'A' is marked 'final'
```

### Default Functions
A more elegant, efficient way to provide a default implementation of a funtion, such as a constructor.
```C++
struct A {
  A() = default;
  A(int x) : x{x} {}
  int x {1};
};
A a; // a.x == 1
A a2 {123}; // a.x == 123

struct B {
  B() : x{1} {}
  int x;
};

struct C : B {
  // Calls B::B
  C() = default;
};

C c; // c.x == 1
```

### Deleted Functions
A more elegant, efficient way to provide a deleted implementation of a function. Useful for preventing copied on objects.
```C++
class A {
  int x;

public:
  A(int x) : x{x} {};
  A(const A&) = delete;
  A& operator=(const A&) = delete;
};

A x {123};
A y = x; // error -- call to deleted copy constructor
y = x; // error -- operator= deleted
```

### Range-based For Loops
Syntactic sugar for iterating over a container's elements.
```C++
std::array<int, 5> a {1, 2, 3, 4, 5};
for (int& x : a) x *= 2;
// a == { 2, 4, 6, 8, 10 }
for (int x : a) x *= 2;
// a == { 1, 2, 3, 4, 5 }
```
If you want to change element's value, don't forget to use reference.

### Special Member Function for Move Sementics
The copy constructor and copy assignment operator are called when copies are made, and with C++11's introduction of move semantics, there is now a move constructor and move assignment operator for moves.
```C++
struct A {
  std::string s;
  A() : s{"test"} {}
  A(const A& o) : s{o.s} {}
  A(A&& o) : s{std::move(o.s)} {}
  A& operator=(A&& o) {
   s = std::move(o.s);
   return *this;
  }
};

A f(A a) {
  return a;
}

A a1 = f(A{}); // move-constructed from rvalue temporary
A a2 = std::move(a1); // move-constructed using std::move
A a3 = A{};
a2 = std::move(a3); // move-assignment using std::move
a1 = f(A{}); // move-assignment from rvalue temporary
```

### Converting Constructors
Converting constructors will convert values of braced list syntax into constructor arguments.
```C++
struct A {
  A(int) {}
  A(int, int) {}
  A(int, int, int) {}
};

A a {0, 0}; // calls A::A(int, int)
A b(0, 0); // calls A::A(int, int)
A c = {0, 0}; // calls A::A(int, int)
A d {0, 0, 0}; // calls A::A(int, int, int)
```

Note that the braced list syntax does not allow narrowing:
```C++
struct A {
  A(int) {}
};

A a(1.1); // OK
A b {1.1}; // Error narrowing conversion from double to int
```

Note that if a constructor accepts a `std::initializer_list`, it will be called instead:
```C++
struct A {
  A(int) {}
  A(int, int) {}
  A(int, int, int) {}
  A(std::initializer_list<int>) {}
};

A a {0, 0}; // calls A::A(std::initializer_list<int>)
A b(0, 0); // calls A::A(int, int)
A c = {0, 0}; // calls A::A(std::initializer_list<int>)
A d {0, 0, 0}; // calls A::A(std::initializer_list<int>)
```

### Explicit Convertion Functions
Conversion functions can now be made explicit using the `explicit` specifier.
```C++
struct A {
  operator bool() const { return true; }
};

struct B {
  explicit operator bool() const { return true; }
};

A a;
if (a); // OK calls A::operator bool()
bool ba = a; // OK copy-initialization selects A::operator bool()

B b;
if (b); // OK calls B::operator bool()
bool bb = b; // error copy-initialization does not consider B::operator bool()
```

### Inline Namespaces
All members of an inline namespace are treated as if they were part of its parent namespace, allowing specialization of functions and easing the process of versioning. This is a transitive property, if A contains B, which in turn contains C and both B and C are inline namespace, C's member can be used as if they were on A.
```C++
namespace Program {
  namespace Version1 {
    int getVersion() { return 1; }
    bool isFirstVersion() { return true; }
  }
  inline namespace Version2 {
    int getVersion() { return 2; }
  }
}

int version {Program::getVersion()};              // Uses getVersion() from Version2
int oldVersion {Program::Version1::getVersion()}; // Uses getVersion() from Version1
bool firstVersion {Program::isFirstVersion()};    // Does not compile when Version2 is added
```

### Non-static Data Member Initializers
Allows non-static members to be initialized where they are declared, potentially cleaning up constructors of default initializations.
```C++
// Default initialization prior to C++11
class Human {
    Human() : age{0} {}
  private:
    unsigned age;
};
// Default initialization on C++11
class Human {
  private:
    unsigned age {0};
};
```

### Right Angle Brackets
C++11 is now able to infer when a series of right angle brackets is used as an operator or as a closing statement of typedef, without having to add whitespaces.
```C++
typedef std::map<int, std::map <int, std::map <int, int> > > cpp98LongTypedef;
typedef std::map<int, std::map <int, std::map <int, int>>>   cpp11LongTypedef;
```

### Ref-qualified Member Functions
Member functions can now be qualified depending on whether `*this` is an lvalue or rvalue reference.
```C++
struct Bar {
  // ...
};

struct Foo {
  Bar getBar() & { return bar; }
  Bar getBar() const& { return bar; }
  Bar getBar() && { return std::move(bar); }
  Bar getBar() const&& { return std::move(bar); }
private:
  Bar bar;
};

Foo foo{};
Bar bar = foo.getBar(); // calls `Bar getBar() &`

const Foo foo2{};
Bar bar2 = foo2.getBar(); // calls `Bar Foo::getBar() const&`

Foo{}.getBar(); // calls `Bar Foo::getBar() &&`
std::move(foo).getBar(); // calls `Bar Foo::getBar() &&`

std::move(foo2).getBar(); // calls `Bar Foo::getBar() const&&`
```

### Trailing Return Types
C++11 allows functions and lambdas an alternative syntax for specifying their return types.
```C++
int f() {
  return 123;
}
// vs.
auto f() -> int {
  return 123;
}

auto g = []() -> int {
  return 123;
};
```

This feature is especially useful when certain return types cannot be resolved:
```C++
// NOTE: This does not compile!
template <typename T, typename U>
decltype(a + b) add(T a, U b) {
    return a + b;
}

// Trailing return types allows this:
template <typename T, typename U>
auto add(T a, U b) -> decltype(a + b) {
    return a + b;
}
```

### Noexcept Specifier
The `noexcept` specifier specifies whether a function could throw exceptions. It is an improved version of `throw()`.
```C++
void func1() noexcept;        // does not throw
void func2() noexcept(true);  // does not throw
void func3() throw();         // does not throw

void func4() noexcept(false); // may throw
```
Non-throwing functions are permitted to call potentially-throwing functions. Whenever an exception is thrown and the search for a handler encounters the outermost block of a non-throwing function, the function std::terminate is called.
```C++
extern void f();  // potentially-throwing
void g() noexcept {
    f();          // valid, even if f throws
    throw 42;     // valid, effectively a call to std::terminate
}
```

## New Libraries

### `std::move`
`std::move` indicates that the object passed to it may have its resources transferred. Using objects that have been moved from should be used with care, as they can be left in an unspecified state.
```C++
std::unique_ptr<int> p1 {new int{0}}; // in practice, use std::make_unique
std::unique_ptr<int> p2 = p1; // error -- cannot copy unique pointers
std::unique_ptr<int> p3 = std::move(p1); // move `p1` into `p3`
                                         // now unsafe to dereference object held by `p1`
```

### `std::forward`
Return the arguments passed to it while maintaining their value category and cv-qualifiers. Useful for generic code and factories. Used in conjunction with `forward reference`
```C++
struct A {
  A() = default;
  A(const A& o) { std::cout << "copied" << std::endl; }
  A(A&& o) { std::cout << "moved" << std::endl; }
};

template <typename T>
A wrapper(T&& arg) {
  return A{std::forward<T>(arg)};
}

wrapper(A{}); // moved
A a;
wrapper(a); // copied
wrapper(std::move(a)); // moved
```

### `std::thread`
The `std::thread` library provides a standard way to control threads, such as spawning and killing them. In the example below, multiple threads are spawned to do different calculations and then the program waits for all of them to finish.
```C++
void foo(bool clause) { /* do something... */ }

std::vector<std::thread> threadsVector;
threadsVector.emplace_back([]() {
  // Lambda function that will be invoked
});
threadsVector.emplace_back(foo, true);  // thread will run foo(true)
for (auto& thread : threadsVector) {
  thread.join(); // Wait for threads to finish
}
```

### std::to_string
Converts a numeric argument to a `std::string`
```C++
std::to_string(1.2);  // == "1.2"
std::to_string(123);  // == "123"
```

### Type Traits

Type trait defines a compile-time template-based interfaces to query or modify the properties of types.
```C++
static_assert(std::is_integral<int>::value);
static_assert(std::is_same<int, int>::value);
static_assert(std::is_same<std::conditional<true, int, double>::type, int>::value);
```

### Smart Pointers

C++11 introduces new smart pointers: 

- `std::unique_ptr`
- `std::shared_ptr`
- `std::weak_ptr`

`std::auto_ptr` now becomes deprecated and then eventually removed in C++17

`std::unique_ptr` is a non-copyable, movable pointer that manages its own heap-allocated memory. 
Note: **Prefer using the `std::make_X` helper functions as opposed to using constructors.**
```C++
std::unique_ptr<Foo> p1 {new Foo{}};  // `p1` owns `Foo`
if (p1) {
  p1->bar();
}

{
  std::unique_ptr<Foo> p2 {std::move(p1)};  // Now `p2` owns `Foo`
  f(*p2);

  p1 = std::move(p2);  // Ownership returns to `p1` -- `p2` gets destroyed
}

if (p1) {
  p1->bar();
}
// `Foo` instance is destroyed when `p1` goes out of scope
```
A `std::shared_ptr` is a smart pointer that manages a resource that is shared across multiple owners. A shared pointer holds a *control block* which has a few components such as the managed object and a reference counter. All *control block* is thread-safe, however, manipulating the managed object itself is not thread-safe.
```C++
void foo(std::shared_ptr<T> t) {
  // Do something with `t`...
}

void bar(std::shared_ptr<T> t) {
  // Do something with `t`...
}

void baz(std::shared_ptr<T> t) {
  // Do something with `t`...
}

std::shared_ptr<T> p1 {new T{}};
// Perhaps these take place in another threads?
foo(p1);
bar(p1);
baz(p1);
```

### `std::chrono`
The chrono library contains a set of utility functions and types that deal with durations, clocks, and time points. One use case of this library is benchmarking code:
```C++
std::chrono::time_point<std::chrono::steady_clock> start, end;
start = std::chrono::steady_clock::now();
// Some computations...
end = std::chrono::steady_clock::now();

std::chrono::duration<double> elapsed_seconds = end - start;
double t = elapsed_seconds.count(); // t number of seconds, represented as a `double`
```

### Tuples
Tuples are a fixed-size collection of heterogeneous values. Access the elements of a `std::tuple` by unpacking using `std::tie` or `std::get`.
```C++
// `playerProfile` has type `std::tuple<int, const char*, const char*>`.
auto playerProfile = std::make_tuple(51, "Frans Nielsen", "NYI");
std::get<0>(playerProfile); // 51
std::get<1>(playerProfile); // "Frans Nielsen"
std::get<2>(playerProfile); // "NYI"
```

### `std::tie`
Creates a tuple of lvalue references. Useful for unpacking `std::pair` and `std::tuple` objects. Use `std::ignore` as a placeholder for ignored values. In C++17, structured bindings shouhld be used instead.
```C++
// With tuples...
std::string playerName;
std::tie(std::ignore, playerName, std::ignore) = std::make_tuple(91, "John Tavares", "NYI");

// With pairs...
std::string yes, no;
std::tie(yes, no) = std::make_pair("yes", "no");
```

### `std::array`
`std::array` is a container built on top of a C-style array. Supports common container   operations such as sorting.
```C++
std::array<int, 3> a = {2, 1, 3};
std::sort(a.begin(), a.end()); // a == { 1, 2, 3 }
for (int& x : a) x *= 2; // a == { 2, 4, 6 }
```

### Unordered Container
These containers maintain average constant-time complexity for search, insert, and remove operations. In order to achieve constant-time complexity, sacrifices order for speed by hashing elements into buckets. There are four unordered containers:

- `unordered_set`
- `unordered_multiset`
- `unordered_map`
- `unordered_multimap`

### `std::make_shared`
`std::make_shared` is the recommanded way to create instance of `std::shared_ptr` due to the following reasons:

- Avoid having to use the new operator.
- Prevents code repetition when specifying the underlying type the pointer shall hold.
- It provides exception-safety. Suppose we were calling a function `foo` like:

```C++
foo(std::shared_ptr<T>{new T{}}, function_that_throws(), std::shared_ptr<T>{new T{}});
```
The compiler is free to call `new T{}`, then `function_that_throw()`, and so on. Since we have allocated data on the heap in the first construction of a `T` we have introduced a leak here. With `std::make_shared`, we are given exception-safety:
```C++
foo(std::make_shared<T>(), function_that_throws(), std::make_shared<T>());
```

- Prevents having to do two allocations. When calling `std::shared_ptr{new T{}}`, we have to allocate memory for `T`, then in the shared pointer we have to allocate memory for the control block within the pointer.

### `std::ref`
`std::ref(val)` is used to create object of type `std::reference_wrapper` that holds reference of val. Used in cases when usual reference passing using `&` does not compile or `&` is dropped due to type deduction. `std::cref` is similar but created reference wrapper holds a const reference to val.
```C++
// create a container to store reference of objects.
auto val = 99;
auto _ref = std::ref(val);
_ref++;
auto _cref = std::cref(val);
//_cref++; does not compile
std::vector<std::reference_wrapper<int>>vec; // vector<int&>vec does not compile
vec.push_back(_ref); // vec.push_back(&i) does not compile
cout << val << endl; // prints 100
cout << vec[0] << endl; // prints 100
cout << _cref; // prints 100
```

### Memory Model
C++11 introduces a memory model for C++, which means library support for threading and atomic operations. Some of these operations include (but aren't limited to) atomic loads/stores, compare-and-swap, atomic flags, promises, futures, locks, and condition variables.

### `std::async`
`std::async` runs the given function either asynchronously or lazily-evaluated, then returns a `std::future` which holds the result of that function call.

The first parameter is the policy which can be:

1. `std::launch::async` | `std::launch::deferred` It is up to the implementation whether to perform asynchronous execution or lazy evaluation.
2. `std::launch::async` Run the callable object on a new thread.
3. `std::launch::deferred` Perform lazy evaluation on the current thread.

```C++
int foo() {
  /* Do something here, then return the result. */
  return 1000;
}

auto handle = std::async(std::launch::async, foo);  // create an async task
auto result = handle.get();  // wait for the result
```

### `std::begin/end`
`std::begin` and `std::end` free functions were added to return begin and end iterators of a container generically. These functions also work with raw arrays which do not have begin and end member functions.
```C++
template <typename T>
int CountTwos(const T& container) {
  return std::count_if(std::begin(container), std::end(container), [](int item) {
    return item == 2;
  });
}

std::vector<int> vec = {2, 2, 43, 435, 4543, 534};
int arr[8] = {2, 43, 45, 435, 32, 32, 32, 32};
auto a = CountTwos(vec); // 2
auto b = CountTwos(arr);  // 1
```
