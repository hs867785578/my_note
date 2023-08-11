# Effective Modern C++

This is a summary of the book "Effective Modern C++" which gives 42 specific ways to imporve your programs and designs by Scott Meyers.

## 1. Understand template type deduction.
- During template type deduction, arguments that are references are treated as non-reference, i.e., their reference-ness is ignored.
- When deducing types for universal reference parameters, lvalue arguments get special treatment.
- when deducing types for by-value parameters, `const` and/or `volatile` arguments are treated as non-const and non-volatile.
- During template type deduction, arguments that are array or function names decay to pointers, unless they're used to initialize references.

## 2. Understand auto type deduction.
- `auto` type deduction is usually the same as template type deduction, buy `auto` type deduction assumes that a braced initializer represents a `std::initializer_list`, and template type deduction doesn't.
- `auto` in a function return type or a lambda parameter implies template type deduction, not auto type deduction.

## 3. Understand `decltype`.
- `decltype` almost always yields the type of a variable or expression without any modifications.
- For lvalue expressions of type T other than names, decltype always reports a type of `T&`.
- C++14 supports `decltype(auto)`, which, like auto, deduces a type from its initializer, but it performs the type deduction using the `decltype` rules.

## 4. Know how to view deduced types.

- Deduced types can often be seen using:
    - IDE editors,
    - compiler error messages,
    - Boost TypeIndex library.
- The results of some tools may be neither helpful nor accurate, so an understanding of C++'s type deduction rules remains essential.

## 5. Prefer `auto` to explicit type declarations.
- The advances are:
    - `auto` variables must be initialized,
    - immune to type mismatches that can lead to portability or efficiency problems,
    - can ease the process of refactoring,
    - require less typing than variables with explicitly specified types.
- `auto`-typed variables are subject to the pitfalls described in iterm 2 and 6.

## 6. Use the explicitly typed initializer idiom when auto deduces undesired types.
- "Invisible" proxy types can cause auto to deduce the "wrong" type for an initializing expression.
- The explicitly typed initializer idiom forces auto to deduce the type you want it to have.

## 7. Distinguish between `()` and `{}` when creating objects.
- Braced initialization is the most widely usable initialization syntax, it prevents narrowing conversions, and it's immune to C++'s most vexing parse.
- During constructor overload resolution, braced initializers are matched to `std::initializer_list` parameters if at all possible, even if other constructors offer seemingly better matches.
- An example of where the choice between parentheses and braces can make a significant difference is creating a `std::vector<numeric type>` with two arguments.
- Choosing between parentheses and braces for object creation inside templates can be challenging.

## 8. Prefer `nullptr` to `0` and `NULL`.
- Avoid overloading on integral and pointer types.

## 9. Prefer alias declarations to typedefs.
- `typedef`s don't support templatization, but alias declarations do.
- Alias templates avoid the `::type` suffix and, in templates, the "typename" prefix often required to refer to  typedefs.
- C++14 offers alias templates for all the C++11 type traits transformations.

## 10. Prefer `scoped enums` to `unscoped enums`.
- C++98-style `enum`s are now known as unscoped enums.
- Enumerators of scoped enums are visible only within the enum. They convert to other types only with a cast.
- Both scoped and unscoped enums support specification of the underlying type. The default underlying type for scoped enums is int. Unscoped enums have no default underlying type.
- Scoped enums may always be forward-declared. Unscoped enums may be forward-declared only if their declaraation specifies an underlying type.

## 11. Prefer deleted functions to private underfined ones.
- Any function may be deleted, including non-member functions and template instantiations.

## 12. Declare overriding functions `override`.
- Declare overriding functions `override`.
- Member function reference qualifiers make it possible to treat lvalue and rvalue objects(`*this`) diffrently.

## 13. Prefer `const_iterator`s to `iterator`s.
- In maximally generic code, prefer non-member versions of begin, end, rbegin, etc, over their member function counterparts.

## 14. Declare functions `noexcept` if they won't emit exceptions.
- `noexcept` is part of a function's interface, and that means that callers may depend on it.
- `noexcept` functions are more optimizable than non-noexcept functions.
- `noexcept` is particularly valuable for the move operations, swap, memory deallocation functions and destructions.
- Most functions are exception-neutral rather than `noexcept`.

## 15. Use `constexpr` whenever possible.
- `constexpr` objects are `const` and are initialized with values known during compilation.
- `constexpr` functions can produce compile-time results when called with arguments whose values are known during compilation.
- `constexpr` objects and functions may be used in a wider range of contexts than non-constexpr objects and functions.
- `constexpr` is part of an object's or function's interface.

## 16. Make `const` member functions thread safe.
- Make `const` member functionsthreadsafe unless you're certain they'll never be used in a concurrent context.
- Use of `std::atomic` variables may offer better performance than a mutex, but they're suited for manipulation of only a single variable or memory location.

## 17. Understand special member function generation.
- The special member functions are those compilers may generate on their own:
    - default constructor,
    - destructor,
    - copy constructor,
    - copy operator,
    - move constructor,
    - move operator
- Move operations are generated only for classes lacking explicitly declared move operations, copy operations and a destructor.
- The copy operations are generated only for classes lacking an explicitly declared copy operations, and it's deleted if a move operation is declared.
- Member function templates never suppress generation of special member functions.

## 18. Use `std::unique_ptr` for exclusive-ownership resource management.
- `std::unique_ptr` is a small, fast, move-only smart pointer for managing resources with exclusive-ownership semantics.
- By default, resource destruction takes place via `delete`, but custom deleters can be specified. Stateful deleters and function pointers as deleters increase the size of `std::unique_ptr` objects.
- Converting a `std::unique_ptr` to a `std::shared_ptr` is easy.

## 19. use `std::shared_ptr` for shared-ownership resource management.
- `std::shared_ptr`s offer convenience approaching that of garbage collection for the shared lifetime management of arbitrary resources.
- Compared to `std::unique_ptr`, `std::shared_ptr` objects are typically twice as big, incur overhead for control blocks, and require atomic reference count manipulations.
- Default resource destruction is via `delete`, but custom deleters are supported. The type of the deleter has no effect on the type of the `std::shared_ptr`.
- Avoid creating `std::shared_ptr`s from viariable of raw pointer type.

## 20. Use `std::weak_ptr` for `std::shared_ptr`-like pointers that can dangle.
- Use `std::weak_ptr` for `std::shared_ptr`-like pointers that can dangle.
- Potential use cases for `std::weak_ptr` includes caching, observer lists, and the prevention of `std::shared_ptr` cycles.

## 21. Prefer `std::make_unique` and `std::make_shared` to direct use of `new`.
- Compared to direct use of `new`, `make` functions eliminate source code duplication, improve exception safety, and, for `std::make_shared` and `std::allocate_shared`, generate code that's smaller and faster.
- Situations where use of `make` functions is inappropriate include the need to specify custom deleters and a desire to pass braced initializers.
- For `std::shared_ptr`s, additional situations where `make` functions may be ill-advised include:
    - classes with custom memory management,
    - systems with memory concerns, very large objects, and `std::weak_ptr`s that outlive the corresponding `std::shared_ptr`s.

## 22. When using the Pimpl(Pointer to implementation) Idiom, define special member functions in the implementation file.
- The Pimpl Idiom decreases build times by reducing compilation dependencies between class clients and class implementations.
- For `std::unique_ptr pImpl` pointers, declare special member functions in the class header, but implement them in the implementation file. Do this even if the default function implementations are acceptable.
- The above advice applies to `std::unique_ptr`, but no to `std::shared_ptr`.

## 23. Understand `std::move` and `std::forward`.
- `std::move` performs an uncoditional cast to an rvalue. In and of itself, it doesn't move anything.
- `std::forward` casts its argument to an rvalue only if that argument is bound to an rvalue.
` Neither `std::move` nor `std::forward` do anything at runtime.

## 24. Distinguish universal references from rvalue references.
- If a function template parameter has type `T&&` for a deduced type `T`, or if an object is declared using `auto&&`, the parameter or object is a universal reference.
- If the form of the type declaration isn't precisely `type&&`, or if type deduction does not occur, `type&&` denotes an rvalue reference.
- Universal references correspond to rvalue references if they're initialized with rvalues. The correspond to lvalue references if they're initialized with lvalues.

## 25. Use `std::move` on rvalue references, `std::forward` on universal references.
- Apply `std::move` to rvalue references and `std::forward` to universal references the last time each is used.
- Do the same thing for rvalue references and universal references being returned from functions that return by value.
- Never apply `std::move` or `std::forward` to local objects if they would otherwise be eligible for the return value optimization.

## 26. Avoid overloading on universal references.
- Overloading on universal references almost always leads to the universal reference overload being called more frequently than expected.
- Perfect-forwarding constructors are especially problematic, because they're typically better matches than copy constructors for non-cost lvalues, and they can hijack derived class calls to base class copy and move constructors.

## 27. Familiarize yourselff with alternatives to overloading on universal references.
- Alternatives to the combination of universal references and overloading include:
    - the use of distinct function names,
    - passing parameters by lvalue-reference-to-cost,
    - passing parameters by value,
    - using tag dispatch.
- Constraining template via `std::enable_if` permits the use of universal references and  overloading together, but it controls the conditions under which compilers may use the universal reference overloads.
- Universal reference parameters often have efficiency advantages, but they typically have usability diadvantages.

## 28. Understand reference collapsing.
- Reference collapsing occurs in four contexts:
    - template instantiation,
    - `auto` type generation,
    - creation and use of `typedef`s and alias declarations,
    - `decltype`
- When compilers generate a reference to a reference in a reference collapsing context, the result becomes a single reference. If either of the original references is an lvalue reference, the result is an lvalue reference. Otherwise it's an rvalue reference.
- Universaal references are rvalue references in contexts where type deduction distinguishes lvalues from rvalues and where reference collapsing occurs.

## 29. Assume that move operations are not present, not cheap, and not used.
- Assume that move operations are not present, not cheap, and not used.
- In code with known types or support for move semantics, there is no need for assumptions.

## 30. Familiarize yourself with perfect forwarding failure cases.
- Perfect forwarding fails when template type deduction fails or when it deduces the wrong type.
- The kinds of arguments that lead to perfect forwarding failure are:
    - braced initializers,
    - null pointers expressed as `0` or `NULL`,
    - declaration-only integral `const static` data members,
    - template and overloaded function names,
    - bitfields.

## 31. Avoid default capture modes.
- Default by-reference capture can lead to dangling references.
- Default by-value capture is susceptible to dangling pointers(especially `this`), and it misleadingly suggests that lambdas are self-contained.

## 32. Use init capture to move objects into closures.
- Use C++14's init capture to move objects into closures.
- In C++11, emulate init capture via:
    - hand-written classes,
    - `std::bind`

## 33. Use `decltype` on `auto&&` parameters to `std::forward` them.
- Use `decltype` on `auto&&` parameters to `std::forward` them.

## 34. Prefer lambda to `std::bind`.
- Lambdas are more readable, more expressive, and may be more efficient than using `std::bind`.
- In C++11 only, `std::bind` may be useful for implementing move capture or for binding objects with templatized function call operators.

## 35. Prefer task-based programming to thread-based.
- The `std::thread` API offers no direct way to get return values from asynchronouly run functions, and if those functions throw, the program is terminated.
- Threadd-base programming calls for manual management of thread exhaustion, oversubscription, load balancing, and adaptation to new platform.
- Task-based programming via `std::async` with the default launch policy handles most of these issues for you.

## 36. Specify `std::launch::async` if asynchronicity is essential.
- The default launch policy for `std::async` permits both asynchronous and synchronous task execution.
- This flexibility leads to uncertainty when accessing `thread_local`s, implies that the task may never execute, and affects program logic for timeout-based wait calls.
- Specify `std::launch::async` if asynchronous task execution is essential.

## 37. Make `std::threads` unjoinable on all paths.
- Make `std::thread`s unjoinable on all paths.
- join-on-destruction can lead to difficult-to-debug performance anomalies.
- detach-on-destruction can lead to difficult-to-debug undefined behavior.
- Declare `std::thread` objects last in lists of data members.

## 38. Be aware of varying thread handle destructor behavior.
- Future destructors normally just destroy the future's data members.
- The final future referring to a shared state for a non-deferred task launched via `std::async` blocks until the task completes.

## 39. Consider `void` function for one-shot event communication.
- For simple event communication, condvar-based designs require a superfluous mutex, impose constraints on the relative progress of detecting and reacting tasks, and require reacting tasks to verify that the event has taken place.
- Design employing a flag avoid those problems, but are based on polling, not blocking.
- Using `std::promise`s and futures dodges these issues, but the approach uses heap memory for shared states, and it's limited to one-shot communication.

## 40. Use `std::atomic` for concurrency, `volatile` for special memory.
- `std::atomic` is for data accessed from multiple threads without using mutexes, It's a tool for writing concurrent software.
- `volatile` is for memory where reads and writes should not be optimized away. It's a tool for working with special memory.

## 41. Consider pass by value for copyable parameters that are cheap to move and always copied.
- For copyable, cheap-to-move parameters that are always copied, pass by value may be nearly as efficient as pass by reference, it's easier to implement, and it can generate less object code.
- Copying parameters via construction may be significantly more expensive than copying them via assignment.
- Pass by value is suject to the slicing problem, so it's typically inappropriate for base class parameter types.

## 42. Consider emplacement instead of insertion.
- In principle, emplacement functions should sometimes be more efficient than their insertion counterparts, and they should never be less efficient.
- In practice, they're most likely to be faster when:
    - the value being added is constructed into the container, not assigned;
    - the argment types passed differ from the type held by the container;
    - the container won't reject the value being added due to it being a duplicate.
- Emplacement functions may perfrom type conversions that would be rejected by insertion functions.

## Reference
- [Effective Modern C++](https://b-ok.asia/book/2468699/d9aa73)
