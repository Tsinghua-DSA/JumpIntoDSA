# 遵从不易出错的代码风格


!!! note "函数的使用"

    The number of errors in code correlates strongly with the amount of code and the complexity of the code. Both problems can be addressed by using more and shorter functions. Using a function to do a specific task often saves us from writing a specific piece of code in the middle of other code; making it a function forces us to name the activity and document its dependencies. If we cannot find a suitable name, there is a high probability that we have a design problem.
   
    A Tour of C++, Bjarne Stroustrup

!!! note "Subset of C++ Core Guidelines"

    [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)

    *They are meant to make code simpler and more correct/safer than most existing C++ code, without loss of performance. They are meant to inhibit perfectly valid C++ code that correlates with errors, spurious complexity, and poor performance.*

    You don’t have to know every detail of C++ to write good programs.

    Focus on programming techniques, not on language features.

    “Package” meaningful operations as carefully named functions.

    A function should perform a single logical operation.

    Keep functions short.

    Use overloading when functions perform conceptually the same task on different types.

    Understand how language primitives map to hardware.

    Use digit separators to make large literals readable.

    Avoid complicated expressions.

    Avoid narrowing conversions.

    Minimize the scope of a variable.

    Keep scopes small.

    Avoid “magic constants”; use symbolic constants.

    Prefer immutable data.

    Declare one name (only) per declaration.

    Keep common and local names short; keep uncommon and nonlocal names longer.

    Avoid similar-looking names.

    Avoid ALL_CAPS names.

    Prefer the {}-initializer syntax for declarations with a named type.

    Use auto to avoid repeating type names.

    Avoid uninitialized variables.

    Don’t declare a variable until you have a value to initialize it with.

    When declaring a variable in the condition of an **if**-statement, prefer the version with the implicit test against 0 or nullptr.

    Prefer range-**for** loops over **for**-loops with an explicit loop variable.

    Use **unsigned** for bit manipulation only.

    Keep use of pointers simple and straightforward.

    Use **nullptr** rather than 0 or NULL.

    Don’t say in comments what can be clearly stated in code.

    State intent in comments.

    Maintain a consistent indentation style.

