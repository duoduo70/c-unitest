在 C 文件中，你只需这样写：

```c
#include <unitest.h>
unitest {
        return 0;
}
```

若符合我们想要的结果，则应返回 0 ，否则一般返回 1 。
```c
// unitest.h 之定义：
#define unitest static inline __attribute__((always_inline)) int _unitest_##__COUNTER__()
```
如果你使用 O1 ，则 unitest 不会被编译进代码。
如果你使用 O0 ，这些函数可能会被某些编译器编译进代码。你使用的编译器可能出现我们所不希望的行为。
那么，有这样一种替代定义：
```
#define UNITEST(code)

UNITEST({return 0;})
```
这仍然需要编译器至少支持代码块扩展（常见编译器，包括 TCC 都是支持的）
你需要略微修改 `unitest.py` 以实现这个替代定义。

我编写的脚本中 unitest 不支持使用宏自动生成，但上述第一个定义在宏展开后仍然包含可检索的语义，在略微对程序进行修改后你就能让单元测试支持使用宏自动生成。

该脚本是可扩展的，因为该脚本实际上包含一个微型 C 语言词法分析器。
