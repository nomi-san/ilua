# ilua
_Single header_ that supports **Lua interface** for C++

### features

- Fast and light code
- Support Lua 5.1 to 5.3
- With Lua 5.3 style

### how it works?

```
C lua_State* --+
               v
C++ class:  [this]
              ::
method calling  [this] + (arg, ...) -> CAPI: (L, arg, ...)
```

### usage

Add `ilua.h`, `compat-5.3.h`, `compat-5.3.c` to your project and...
```cpp
#include "ilua.h"
using namespace ILua;
   ...
```

### example

- **Simple module**

```cpp
static int myAdd(State& L) {                       // another way
    int a = L.toInt(1);     // get arg 1 -> int       |  auto a = L.to<int>(1);
    int b = L.toInt(2);     // get arg 2 -> int       |  auto b = L.to<int>(2);
    L.push(a + b);          // push int               |  L += (a + b);
    return 1;               // 1 return
}

extern "C"
int luaopen_mod(State& L) {
    L.reg("myAdd", myAdd);  // register myAdd as global function
    return 0;               // 0 return
}
```

```lua
require("mod")
print(myAdd)        --> function

ret = myAdd(4, 5)   
print(ret)          --> 9
```

