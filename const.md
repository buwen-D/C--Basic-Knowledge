#  `const`
---

### 1. `const` 修饰变量
`const` 直接修饰变量时，表示该变量是只读的，不可修改。通常在声明时就要初始化。
```cpp
const int num = 10;  // num 被初始化为 10，不可更改
// num = 20;  // 错误：num 是 const，不可更改
```

### 2. `const` 修饰指针
`const` 在指针中的位置会影响指针和它指向的值的性质，具体有以下几种情况：

#### (1) 指向常量的指针（Pointer to Const）
在这种情况下，指针指向的值不能被修改，但指针本身可以指向其他对象。常见的形式为 `const type* ptr`。
```cpp
const int* ptr = &num;  // *ptr 不可修改，但 ptr 本身可以指向其他地址
int anotherNum = 20;
ptr = &anotherNum;  // 合法，因为 ptr 可以重新赋值

// *ptr = 30;  // 错误：*ptr 是 const，不允许修改其值
```

**适用场景**：当不希望通过指针修改指向的内容时使用，例如读取一块数据时保证数据不被篡改。

#### (2) 常量指针（Const Pointer）
常量指针的地址是固定的，不能指向其他位置，但指针指向的值可以被修改（如果它本身不是 `const`）。常见的形式是 `type* const ptr`。
```cpp
int num1 = 10;
int* const ptr = &num1;  // ptr 是常量指针，不可重新指向其他地址

*ptr = 15;  // 合法，*ptr 可以被修改
// ptr = &anotherNum;  // 错误：ptr 是常量指针，不允许修改地址
```

**适用场景**：当指针始终指向同一个对象，并可能会修改该对象时使用。

#### (3) 指向常量的常量指针（Const Pointer to Const）
这种情况下，指针本身和它指向的值都不可修改。通常用 `const type* const ptr` 表示。
```cpp
const int num = 10;
const int* const ptr = &num;  // ptr 和 *ptr 都不能被修改

// *ptr = 20;  // 错误：*ptr 是 const
// ptr = &anotherNum;  // 错误：ptr 是 const
```

**适用场景**：需要一个指向特定对象的指针，并且不允许指针或它指向的对象被修改。

### 3. `const` 修饰引用
引用被 `const` 修饰后可以指向一个常量，即 **指向常量的引用（Reference to Const）**。这表示引用本身不能更改指向的内容，是一种只读引用。通常用于函数形参，避免传入对象的拷贝，同时防止函数内部对对象的修改。
```cpp
void func(const int& ref) {
    // ref 不能被修改
    // ref = 20;  // 错误：ref 是 const
}

int main() {
    int num = 10;
    func(num);
}
```

**适用场景**：函数参数是引用，既可以避免拷贝数据的开销，又可以确保函数内部不会修改传入的数据。

**注意**：没有 “`const` 引用”（即 `const reference`），因为引用本质上是对象的别名，不能直接被 `const` 修饰，只有引用指向的内容可以是 `const`。

### 4. `const` 修饰成员函数
在类的成员函数后加 `const`，表示该函数不会修改类的任何成员变量。使用 `const` 修饰成员函数的好处是，它可以确保成员函数不会意外修改对象的状态，并且可以被 `const` 对象调用。

```cpp
class MyClass {
public:
    MyClass(int val) : value(val) {}

    int getValue() const {
        return value;  // 合法，const 成员函数可以读取成员变量
    }

    void setValue(int val) {
        value = val;  // 合法，非 const 成员函数可以修改成员变量
    }

private:
    int value;
};

int main() {
    const MyClass obj(10);
    obj.getValue();  // 合法，const 对象可以调用 const 成员函数
    // obj.setValue(20);  // 错误，const 对象不能调用非 const 成员函数
}
```

#### const 成员函数的特性
- const 成员函数不能修改类的成员变量（除非成员变量被声明为 `mutable`）。
- const 成员函数可以在 `const` 对象上调用，而非 const 成员函数不能在 `const` 对象上调用。

### 5. `const` 与类中的成员变量
当类中的成员变量被 `const` 修饰时，该成员变量在构造函数初始化后不可更改。通常用于标记一些不可变的数据。

```cpp
class MyClass {
public:
    MyClass(int val) : value(val) {}

    int getValue() const { return value; }

private:
    const int value;  // const 成员变量，必须在构造函数中初始化
};
```

### 6. `const` 和 `mutable` 的配合使用
如果一个成员变量在 const 成员函数中需要修改，可以使用 `mutable` 关键字。`mutable` 允许在 `const` 成员函数中修改该变量的值，常用于计数器或缓存等成员。

```cpp
class MyClass {
public:
    MyClass(int val) : value(val) {}

    int getValue() const {
        ++accessCount;  // 合法，mutable 允许修改
        return value;
    }

private:
    int value;
    mutable int accessCount = 0;  // 允许在 const 成员函数中修改
};
```

---

### 总结
- **const 变量**：声明只读变量。
- **const 指针**：
  - `const type* ptr`：指向常量的指针，指向的值不可改。
  - `type* const ptr`：常量指针，指针地址不可改。
  - `const type* const ptr`：指向常量的常量指针，指针地址和指向的值都不可改。
- **const 引用**：用于函数形参，防止对象拷贝和修改。
- **const 成员函数**：不能修改对象的成员变量，并且可以在 const 对象上调用。
- **const 成员变量**：对象初始化后不可更改。
- **mutable 成员变量**：允许在 const 成员函数中修改的变量。
