# Effective C++

This is a summary of the book "Effective C++" which gives 55 specific ways to imporve your programs and designs by Scott Meyers.

## 1. View C++ as a federation of languages.
- Rules for effective C++ programming vary, depending on the part of C++ you are using.

## 2. Prefer consts, enums, and inlines to #defines.
- For simple constants, prefer const objects or enums to `#define`s.
- For function-like macros, prefer inline functions to `#define`s.

## 3. Use `const` whenever possible. 
- Declaring something const help compilers detect usage errors. const can be applied to objects at any scope, to function parameters and return types, and to member functions as a whole.
- Compilers enforce bitwise constness, but you should program using logical constness.
- When const and non-const member functions have essentially identical implementations, code duplication can be avoided by having the non-const version call the const version.

## 4. Make sure that the objects are initialized before they're used.
- Manually initialize objects of built-in type, because C++ only sometimes initializes them itself.
- In a constructor, prefer use of the member initialization list to assignment inside the body of the constructor. List data members in the initialization list in the same order they're declared in the class.
- Avoid initialization order problems across translation units by replacing non-local static objects with local static objects.

## 5. Know what functions C++ silently writes and calls.
- Compilers may implicityly generate a class's default constructor, copy constructor, copy assignment operator, and destructor.

## 6. Explicityly disallow the use of compiler-generated function you do not want.
- To disallow functionality automatically provided by compilerssss, declare the corresponding member functions `private` or `= delete` and give no implementations. Using a base class like Uncopyable is one way to do this.

## 7. Declare destructors virtual in polymorphic basic classes.
- Polymorphic base classes should declare virtual destructors. If a class has dany virtual functions, it should have a virtual destructor.
- Classes not designed to be base class or not designed to be used polymorhpically should not declare virtual destructors.

## 8. Prevent exceptions from leaving destructors.
- Destructors should never emit exceptions. If functions called in a destructor may throw, the destructor should catch any exceptions, then swallow them or terminate the program.
- If class clientss need to be able to react exceptions thron during an opreration, the class should provide a regular (i.e., non-destructor)function that performs the operation.

## 9. Never call virtual function during construction or destruction.
- Don't call virtual functions during construction or destruction, because such calls will never go to a more derived class than that of the currently executing constructor or destructor.

## 10. Have assignment operators return a reference to `*this`.
- To meet the chain of assignments like `x = y = z = 15;`.

## 11. Handle assignment to self in `operator=`.
- Make sure `operator=` is well-behaved when an object is assigned to itself. Techniques include comparing addresses of source and target objects, careful statement ordering, and copy-and-swap.
- Make sure that any function operating on more than one object behaves correctly if two or more of the objects are the same.

## 12. Copy all parts of an object.
- Copy functions should be sure to copy all of an object's data members and all of its base class parts.
- Don't try to implement one of the copying functions in terms of the other. Instead, put common functionality in a third function that both call.

## 13. Use objects to manage resources.
- To prevent resource leaks, use `RAII`(Resource Acquisition Is Initialization) objects that acquire resources in their constructors and release them in their destructors.
- Two commonly useful `RAII` classes are `std::shared_ptr` and `std::unique_ptr`. `std::shared_ptr` is usually the better choice, because its behavior when copied is intuitive.

## 14. Think carefully about copying behavior in resource-managing classes.
- Copying an `RAII` object entails copying the resource it manages, so the copying behavior of the resource determines the copying behavior of the `RAII` object.
- Common `RAII` class copying behaviors are disallowing copying and performing reference counting, but other behaviorss are possible.

## 15. Provide access to raw resource in resource-managing classes.
- `API`s often require access to raw resources, so each `RAII` class should offer a way to get at the resource it manages.
- Access may be via explicit conversion or implicit conversion. In general, explicit conversion is safer, but implicit conversion is more convenient for clients.

## 16. Use the same form in correspongding uses of `new` and `delete`.
- If you use `[]` in a new expression, you must use `[]` in the corresponding delete expression. If you don't use `[]` in a new expression, you mustn't use `[]` in the corresponding delete expression.

## 17. Store `new`ed objects in smart pointers in standalone statements.
- Store `new`ed objects in smart pointer in standlone statements. Failure to do this can lead to subtle resource leaks when exceptions are thrown.

## 18. Make interfaces easy to use correctly and hard to use incorrectly.
- Good interfaces are easy to use correctly and hard to use incorrectly. You should strive for these characteristics in all your interfaces.
- Ways to facilitate correct use include consistency in interfaces and behavioral compatibility with built-in types.
- Ways to prevent errors include creating new typess, restricting operations on types, constraining object values, and eliminating client resource management respoinsibilities.
- `std::shared_ptr` supports custom deleters. This preventss the cross-DLL problems, can be used to automatically unlock mutexes.

## 19. Treak class design as type design.
Before defining a new type, be sure to consider all these issues:

- How should objects of your new type be created and destroyed?
- How should object initialization differ from object assignment?
- What does it mean for objects of your new type to be passed by value?
- What are the restrictions on legal value for your new type?
- Does your new type fit into an inheritance graph?
- What kind of type conversions are allowed for your new type?
- What operators and functions make sense for the new type?
- What standard functions should be disallowed?
- Who should have access to the members of your new type?
- What is the "undeclared interface" of your new type?
- How general is your new type?
- Is a new type really what you need?

## 20. Prefer pass-by-reference-to-const to pass-by-value.
- Prefer pass-by-reference-to-const over pass-by-value. It's typically more efficient and it avoids the slicing problem.
- The rule doesn't apply to built-in types and `STL` iterator and function object types. For them, pass-by-value is usually appropriate.

## 21. Don't try to return a reference when you must return an object.
- Never return a pointer or reference to a local stack object, a reference to a heap-allocated object or a pointer or reference to a local static object i there is a chance that more than one such object will be needed.

## 22. Declare data members `private`.
- Declare data members `private`, it gives clients syntactically uniform access to data, affords fine-grained access control, allows invariants to be enforced, and offers class authors implementation flexibility.
- Protected is no more encapsulated than `public`.

## 23. Prefer non-member non-friend functions to member functions.
- Doing so increases encapsulation, packaging flexibility, and functional extensibility.

## 24. Declare non-member functions when type conversions should apply to all parameters.
- If you need type conversions on all parameters to a function (including the one that would otherwise be pointed to by the `this` pointer), the function must be a non-member.

## 25. Consider support for a non-throwing `swap`.
- Provide a `swap` member function when `std::swap` would be inefficient for your type. Make sure your swap doesn't throw exceptions.
- If you offer a member swap, also offer a non-member swap that calls the member. For classes(not templates), specialize `std::swap`, too.
- When calling swap, employ a using declaration for `std::swap`, then call swap without namespace qualification.
- It's fine to totally specialize `std` templates for user-defined bypes, but never try to add something completely new to std.

## 26. Postpone variable definitions as long as possible.
- It increase problem clarity and improves program efficiency.

## 27. Minimize casting.
C++ offers four new cast froms:

- `const_cast<T>(expression)`: used to cast away constness of objects.
- `dynamic_cast<T>(expression)`: used to perform "safe downcasting", i.e., to determine whether an object is of a particular type in an inheritance hierarchy.
- `reinterpret_cast<T>(expression)`: used for low-level casts that yield implementation-dependent results, e.g., casting a pointer to an int.
- `static_cast<T>(expression)`: used to force implicit conversions, e.g. non-const object to const object, int to double, etc.

- Avoid casts whenever practical, especially `dynamic_cast` in performance-sensitive code. If a design requires casting, try to develop a cast-free alternative.
- When casting is neccessary, try to hide it inside a function. Clients can then call the function instead of putting casts in their own code.
- Prefer C++-style casts to old-style casts. They are easier to see, and they are more specific aboud what they do.

## 28. Avoid returning "handle" to object internals
- Avoid returning handles (references, pointers, or iterators) to object internals. Not returning handles increases encapsulation, helps const member functions act cosnt and minimizes the creation of dangling handles.

## 29. Strive for exception-safe code.
- Exception-safe functions leak no resource and allow no data structure to become corrupted, even when exceptions are thrown. Such functions offer the basic, strong, or nothrow guarantees.
- The strong guarantee can often be implemented via copy-and-swap, but the strong guarantee is not practical for all functions.
- A function can usually offer a guarantee no stronger than the weakest guarantee of the functions it calls.

## 30. Understand the ins and outs of inlining.
- Limit most inlining to small, frequently called functions. This facilitates debugging and binary upgradability, minimizes potential code bloat, and maximizes the chances of greater program speed.
- Don't declare fucntion template inline just because they appear in header files.

## 31. Minimize compilation dependencies between files.
- The general idea behind minimizing compilation dependencies is to depend on declarations instead of definitions. Two approaches based on this idea are Handle classes and Interface classes.
- Library header files should exist in full and declaration-only forms. This applies regardless of whether templates are involved.

## 32. Make sure public inheritance models "is-a".
- Public inheritance means "is-a". Everything that applies to base classes must also apply to derived classes, because every derived class object `is a` base class object.

## 33. Avoid hiding inherited names.
- Names in derived classes hide names in base classes. Under public inheritance, this is never desirable.
- To make hidden names visible again, employ `using` declarations or forwarding functions.

## 34. Differentiate between inheritance of interface and inheritance of implementation.
- Inheritance of interface is different from inheritance of implementation. Under public inheritance, derived classes always inherit base class interfaces.
- Pure vitual functions specify inheritance of interface only.
- Simple (impure) virtual functions specify inheritance of interface plus inheritance of a default implementation.
- Non-virtual functions specify inheritance of interface plus inheritance of a mandatory implementation.

## 35. Consider alternatives to virtual functions.
Here are some alternatives of virtual function:

- Use the non-virtual interface idiom(NVI idiom), a form of the Template Method design pattern that wraps public non-virtual member functions around less accessible virtual functions.
- Replace virtual functions with `function pointer data member`, a stripped-down manifestation of the `Strategy` design pattern.
- Replace virtual functions with `std::function` data member, thus allowing use of any callable entity with a signature compatible with what you ned. This, too is a form of the `Strategy` design pattern.
- Replace virtual functions in one hierarchy with `virtual functions in another hierarychy`. This is the conventional implementation of the `Strategy` design pattern.

- Alternatives to virtual functions include the `NVI` idiom and various forms of the Strategy design pattern. The NVI idiom is itself an example of the Template Method design pattern.
- A disadvantage of moving functionality from a member function to a function outside the class is that the non-member function lacks access to the class's non-public members.
- `std::function` objects act like generalized function pointers. Such objects support all callable entities compatible with a given target signature.

## 36. Never redefine an inherited non-virtual function.
- The implementation of derived class will hide the one of base class, which may lead to different results when called by pointer of derived class and pointer of base class.

## 37. Never redefine a function's inherited default parameter value.
- Because default parameter values are statically bound, while virtual functions(the only functions you should be redefining) are dynamically bound.

## 38. Model "has-a" or "is-implemented-in-terms-of" through composition.
- Composition has meanings completely different from that of public inheritance.
- In the application domain, composition means "has-a". In the implementation domain, it means is-implemented-in-terms-of.

## 39. Use private inheritance judiciously.
- Private inheritance means is-implementation-in-terms of. It's usually inferior to composition, but it makes sense when a derived class needs access to protected base class members or needs to redefine inherited virtual functions.
- Unlike composition, private inheritance can enable the empty base optimization. This can be important for library developers who strive to minimize object sizes.

## 40. Use multiple inheritance judiciously.
- Multiple inheritance is more complex than single inheritance. It can lead to new ambiguity issues and to the need for virtual inheritance.
- Virtual inheritance imposes costs in size, speed, and complexity of initialization and assignment. It's most practical when virtual base classes have no idea.
- Multiple inheritance does have legitimate uses. On scenario involves combining public inheritance from an Interface class with private inheritance from a class that helps with implementation.

## 41. Understand implicit interfaces and compile-time polymorphism.
- Both classes and templates support interfaces and polymorphism.
- For classes, interfaces are centered on function signatures. Polymorphism occurs at runtime through virtual functions.
- For template parameters, interfaces are implicit and base on valid expression. Polymorphism occurs during compilation through template instantiation and function overloading resolution.

## 42. Understand the two meaning of typename.
- When declaring template parameters, `class` and `template` are interchangeable.
- Use `typename` to identify nested dependent type names, except in base class lists or as a base class identifier in a member initialization list.

## 43. Know how to access names in templatized base classes.
- In derived class templates, refer to names in base class templates via:
    - `this->` prefix;
    - `using` declaration;
    - explicit base class qualification.

## 44. Factor parameter-independent code out of templates.
- Templates generate multiple classes and multiple functions, so any template code not dependent on a template parameter causes bloat.
- Bloat due to non-type template parameters can often be eliminated by replacing template parameters with function parameters or class data members.
- Bloat due to type parameters can be reduced by sharing implementations for instantiation types with identical binary representations.

## 45. Use member function templates to accept "all compatible types".
- Use member function templates to generate functions that accept all compatible types.
- If you declare member templates for generalized copy construction or generalized assignment, you'll still need to declare the normal copy constructor and copy assignment operator, too.

## 46. Define non-member functions inside templates when type conversions are disired.
- When writting a class template that offers functions related to the template that support implicit type conversions on all parameters, define those functions as friends inside the class template.

## 47. Use traits classes for information about types.
- Traits classes make information about types available during compilation. They're implemented using templates specializations.
- In conjunction with overloading, traits classes make it possible to perform compile-time `if else` tests on types.

## 48. Beware of template metaprogramming.
- Template metaprogramming can shift work from runtime to compile-time, thus enabling earlier error detection and higher runtime performance.
- TMP can be used to generate custom code based on combinations of policy choices, and it can also be used to avoid generating code inappropriate for particular types.

## 49. Understand the behavior of the new-handler.
- `set_new_handler` allows you to specify a function to be called when memory allocation requests cannot be satisfied.
- Nothrow new is of limited utility, because it applies only to memory allocation; associated constructor calls may still throw exceptions.

## 50. Understand when it makes sense to replace `new` and `delete`.
There are many valid reasons for writting custom versions of `new` and `delete`:

- To detect usage errors.
- To collect statistics about the use of dynamically allocated memory.
- To increase the speed of allocation and deallocation.
- To reduce the space overhead of default memory management.
- To compensate for suboptimal alignment in the default allocator.
- To cluster related objects near one another.
- To obtain unconventional behavior.

## 51. Adhere to convention when writing `new` and `delete`.

- operator `new` should 
    - contain an infinite loop trying to allocate memory,
    - call the new-handler if it can't satisfy a memory request, 
    - handle requests from zero bytes,
    - class-specific versions should requests for larger blocks than expected.
- operator `delete` should do nothing if passed a pointer that is null. Class-specific versions should handle blocks that are larger than expected.

## 52. Write placement `delete` if you write placement `new`.

- When you write a placement version of operator new, be sure to write the corresponding placement version of operator delete. If you don't, your program may experience subtle, intermittent memory leaks.
- When you declare placement version of new and delete, be sure not to unintentionally hide the normal versions of those functions.

## 53. Pay attention to compiler warnings.

- Take compiler warnings seriously, and strive to compile warning-free at the maximum warning level supported by your compilers.
- Don't become dependent on compiler warnings, because different compilers warn about different things. Porting to a new compiler may eliminate warning messages you've come to rely on.

## 54. Familiarize yourself with the standard library.
- The primary standard C++ library functionality consists of:
    - STL,
    - iostreams,
    - locales,
    - C89 standard library.

## 55. Familiarize yourself with Boost.
- Boost is a community and website for the development of free, open source, peer-reviewed C++ libraries. Boost plays an influential role in C++ standardization.

## Reference
- [Effective C++](https://b-ok.asia/book/3487768/b70c18) 
