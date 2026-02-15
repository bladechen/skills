# Google C++ Style Guide 关键规则

本文档包含 Google C++ Style Guide 的核心规则，用于代码审查时参考。

## 1. 文件命名

- 源文件：`lower_case.cc`, `lower_case.cpp`, `lower_case.cu`, `lower_case.cuh`
- 头文件：`lower_case.h`, `lower_case.hpp`
- 特殊情况：协议缓冲区（`.proto`）、Googletest（`*_test.cc`, `_unittest.cc`）

## 2. 类型命名

- 类名：大写开头，驼峰命名 → `MyClass`, `HttpRequest`
- 结构体名：同 类名 → `MyStruct`, `Point3D`
- 类型别名（typedef/using）：`_t` 结尾 → `MyType_t`
- 枚举名：同 类名，枚举值用全大写下划线 → `enum class UrlTableError { kOk = 0, kOutOfMemory, kMalformedInput };`

## 3. 变量命名

- 普通变量：小写下划线 → `table_name`, `err_count`
- 类成员变量：末尾下划线 → `count_`, `name_`
- 常量：`k` 开头大驼峰 → `kDaysInWeek`, `kMaxConnections`

## 4. 函数命名

- 普通函数：大写开头驼峰 → `AddTableEntry()`, `GetName()`
- 存取函数：与变量名匹配 → `count()`, `set_count()`
- 命名空间内函数：可用小写下划线

## 5. 命名空间

- 命名空间名称：小写下划线
- 不要使用 `using namespace std;`
- 避免使用 using directive，可在函数内局部使用

```cpp
// ✅ 正确
namespace my_project {
namespace foo {
void Bar();
}
}

// ❌ 错误
using namespace std;
```

## 6. 行长度

- 目标：不超过 80 字符
- 硬限制：100 字符
- 例外：URL、路径、宏定义、模板参数

## 7. 缩进和格式

- 使用空格，不用 Tab
- 每次缩进 2 空格
- 条件语句格式：

```cpp
// ✅ 推荐
if (condition) {
  DoSomething();
} else {
  DoSomethingElse();
}

// ❌ 避免
if (condition)
  DoSomething();
```

- 指针/引用符号：靠左

```cpp
// ✅ 推荐
char* p;
const std::string& s;

// ❌ 避免
char *p;
const std::string &s;
```

## 8. 头文件保护

使用 `#pragma once`（推荐）或 `#ifndef` 保护：

```cpp
#pragma once
// 或
#ifndef PROJECT_PATH_FILE_H_
#define PROJECT_PATH_FILE_H_
// ...
#endif  // PROJECT_PATH_FILE_H_
```

## 9. 头文件包含顺序

1. 配对的头文件（如 `.cc` 对应 `.h`）
2. C 系统头文件（`cstdio`, `cstdlib` 等）
3. C++ 系统头文件（`iostream`, `vector` 等）
4. 其他库头文件
5. 项目内部头文件

```cpp
// foo.cc
#include "foo/public/foo.h"  // 配对头文件

#include <sys/types.h>       // C 系统头
#include <vector>            // C++ 系统头
#include <map>               // C++ 系统头

#include "base/basictypes.h" // 项目内部头
#include "base/port.h"       // 项目内部头
```

## 10. 作用域

- 尽量使用命名空间，而非前缀
- 避免使用全局变量
- 局部变量声明尽可能靠近使用位置

## 11. 类

- 公共/保护/私有顺序
- 构造函数、析构函数放最前面
- 成员函数按性质分组

```cpp
class Foo {
 public:
  Foo();
  ~Foo();

  // 公共方法
  void SomeMethod();

 private:
  // 私有成员
  int count_;
};
```

## 12. 智能指针

- 优先使用智能指针管理资源
- 使用 `std::unique_ptr` 独占所有权
- 使用 `std::shared_ptr` 共享所有权
- 避免使用 `std::auto_ptr`

```cpp
// ✅ 推荐
std::unique_ptr<Foo> foo = std::make_unique<Foo>();
void Process(std::unique_ptr<Foo> foo);

// ❌ 避免
std::auto_ptr<Foo> foo(new Foo());
```

## 13. 拷贝和移动

- 优先使用值传递或 const 引用，避免不必要的拷贝
- 考虑移动语义
- 五大原则：如果你需要自定义析构函数，通常也需要自定义拷贝/移动构造函数和赋值运算符

## 14. 异常

- **不推荐使用异常**（Google 风格）
- 使用错误码或日志进行错误处理

## 15. 类型

- 使用 `int64_t` 表示大整数
- 使用 `size_t` 表示大小
- 避免 `char*`，使用 `std::string`

## 16. 预处理宏

- 尽量避免宏，使用 const/enum/inline 替代
- 宏必须全大写下划线命名
- 使用 `#define` 保护

## 17. auto 关键字

- 使用 `auto` 避免类型重复书写
- 适用于迭代器、复杂类型

```cpp
auto it = map.find(key);
for (auto& item : items) {
  // ...
}
```

## 18. 常量

- 使用 `const` 修饰不会改变的成员函数
- 使用 `constexpr` 编译时常量
- 使用 `enum class` 替代枚举

## 19. 整数类型

- 使用 `int64_t` 而非 `long`
- 使用 `uint32_t` 而非 `unsigned int`
- 注意整数溢出

## 20. 注释

- 使用英语注释
- 函数注释：说明输入、输出、返回值
- 代码行尾注释：与代码间隔 1 空格

```cpp
// 这是一个函数说明
// Args:
///   x: 输入参数 x 的说明
///   y: 输入参数 y 的说明
/// Returns:
///   返回值的说明
int Add(int x, int y);
```

## 常见错误清单

1. ❌ 变量名大写开头 → 应该是 `my_var` 不是 `MyVar`
2. ❌ 类名小写开头 → 应该是 `MyClass` 不是 `myClass`
3. ❌ 使用 `using namespace std;` → 删除或移到函数内部
4. ❌ 指针符号靠右 → 使用 `char* p` 不是 `char *p`
5. ❌ 头文件无保护 → 添加 `#pragma once`
6. ❌ Raw 指针未说明所有权 → 使用智能指针
7. ❌ 全大写宏 → 使用 const/enum 替代
8. ❌ 硬编码 Magic Number → 使用命名常量
9. ❌ 循环内重复计算长度 → 循环外计算
10. ❌ 不必要的拷贝 → 使用 const ref 或 move
