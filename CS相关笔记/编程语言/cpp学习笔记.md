# extern 的使用方法
在 **C++** 中，`extern` 关键字的作用主要是**声明而非定义变量或函数**，用于**跨文件共享全局变量或函数**。它告诉编译器：“这个标识符的定义在别的地方（通常在其他源文件中）。

# const 相关的知识
## 2.4.3 顶层 const
这里也可以使用螺旋法则来理解,具体操作可以google一下

```cpp
int i = 0;
const int ci = 0;
const int &ci = i; // 正确, const int& 可以绑定到一个普通 int 上
int &r = ci;       // 错误, int& 不能绑定到一个 const int 上
```

## 2.4.4 constexpr 和常量表达式
```cpp
constexpr int *q = nullptr; 
// q 是一个指向整数的常量指针, 也就是指针的地址值不能修改
// 或者说, 就是一个顶层const
// 相当于以下的表达式
int *const q = nullptr;
```

```cpp
const int *p = nullptr;
// 而这个则表示指向 const int 的指针
```

常量表达式是指不会改变并在编译过程中就能得到计算结果的表达式.

```cpp
int staff_size = get_size(); // staff_size的值要在运行时得到,所以不为常量表达式

constexpr staff_size = get_size(); // 要在get_size()是常量函数的前提下才成立
```

# 2.5
## 2.5.1 类型别名
在cpp中用using代替typedef

```cpp
using INT = int; <==> typedef int INT; 
```

```shell
[head] -> newNode[next] -> 
```
# 拷贝
在cpp中,一个对象被复制有两种常见的情况,这两种方式都会触发拷贝构造函数.
- 拷贝构造
```cpp
CircularList<int> a;
CircularList<int> b = a;
```
- 拷贝赋值
```cpp
CircularList<int> a,b;
b = a;
```
## 为什么要禁用拷贝
如果你的类中含有裸指针,默认的拷贝操作就会触发浅拷贝,容易触发多重释放.
所以我们要禁用拷贝.

```cpp
CircularList(const CircularList&) = delete;
CircularList& operator=(const CircularList&) = delete;
```