# 宏定义 #define 和 const 常量

---

### 1. 宏定义 (`#define`) 和 `const` 常量

- **#define**：宏定义使用预处理指令 `#define` 来定义，只是简单的文本替换。
- **const**：`const` 是 C++ 语言中的常量声明，具有数据类型，属于编译器层面的常量处理。

**示例**：

```cpp
#define PI 3.14159     // 宏定义
const double pi = 3.14159;  // const 常量
```
  
> `#define PI 3.14159` --> 在编译前直接将 `PI` 替换成 `3.14159`。  
> `const double pi = 3.14159;` --> 编译器会分配内存并将 `pi` 作为 `double` 类型的常量。

---

### 2. 处理方式

- **#define**：在预处理阶段进行文本替换，不进行编译检查。
- **const**：在编译阶段处理，编译器会检查数据类型，并生成实际的机器代码。

**示例**：

```cpp
#define WIDTH 10
const int height = 20;

int area = WIDTH * height;
```

> 预处理器将代码中的 `WIDTH` 替换为 `10`，编译器在计算 `area` 时会检测 `height` 的类型，确保正确。

---

### 3. 类型安全

- **#define**：没有类型检查，可能导致类型相关错误，因为它只是纯粹的文本替换。
- **const**：有类型检查，编译器可以验证变量的类型，确保安全性。

**示例**：

```cpp
#define VALUE 3.14159
const double value = 3.14159;

int x = VALUE;      // 可能没有任何警告
int y = value;      // 可能会有类型转换的警告
```

> `#define` 会直接将 `VALUE` 替换为 `3.14159`，编译器无法检查 `VALUE` 的类型是否与 `int` 兼容。  
> `const` 常量 `value` 是 `double` 类型，编译器会检查并可能发出警告，避免隐式类型转换问题。

---

### 4. 内存分配

- **#define**：不分配内存，它只进行文本替换，因此没有内存地址。
- **const**：需要分配内存存储其值，有固定的内存地址，可以使用指针引用。

**示例**：

```cpp
#define SIZE 100
const int size = 100;

std::cout << &SIZE << std::endl;    // 错误：SIZE 没有内存地址
std::cout << &size << std::endl;    // 正确：size 是 const 常量，有内存地址
```

> `#define SIZE 100` 是简单的文本替换，没有内存地址。  
> `const int size = 100;` 则在内存中分配空间存储 `100`，因此 `size` 有内存地址。

---

### 5. 存储位置

- **#define**：宏定义的内容存储在代码段，每次使用都会进行文本替换。
- **const**：存储在数据段，只需存储一次，可避免重复。

**示例**：

```cpp
#define VALUE 10
const int value = 10;

int a = VALUE;
int b = VALUE;

int x = value;
int y = value;
```

> `#define VALUE 10` 每次使用都会替换为 `10`，多个 `VALUE` 实际上会增加编译后的代码大小。  
> `const int value = 10;` 编译器会将 `value` 的值存储在数据段，`x` 和 `y` 使用相同的内存地址，避免了重复的值。

---

### 6. 取消方式

- **#define**：可以使用 `#undef` 来取消宏定义，适合用在不同的条件编译环境中。
- **const**：一旦声明，`const` 常量在作用范围内不可取消。

**示例**：

```cpp
#define MAX 100
#undef MAX  // 取消 MAX 定义
const int max = 100;
// 无法取消 max 定义
```

> `#define MAX 100` 通过 `#undef` 可以移除 `MAX` 定义。  
> `const int max = 100;` 一旦定义，无法在作用域内取消。

---

### 总结表格

| 特性           | 宏定义 (`#define`)                            | `const` 常量                     |
|----------------|----------------------------------------------|----------------------------------|
| 本质           | 字符串替换                                    | 常量声明                         |
| 处理阶段       | 预处理器处理                                  | 编译器处理                       |
| 类型安全       | 无类型检查                                    | 有类型检查                       |
| 内存分配       | 不分配内存                                    | 需要分配内存                     |
| 存储位置       | 存储在代码段（每次替换）                      | 存储在数据段                     |
| 可取消性       | 可以通过 `#undef` 取消                        | 不可取消                         |

---

### 使用建议

- 使用 `const` 代替 `#define` 进行常量声明，这样可以确保类型安全并减少代码中的错误。
- 只有在需要条件编译时使用 `#define`。
- `#define` 可以用于定义编译器开关或条件编译的宏，但对于常量值，`const` 更安全、易维护。
