# C# 语言特点

C# 是一门运行在.NET Core（CLR）上，既可 JIT，也可 AOT，拥有 GC，静态类型、对空格和缩进不敏感，纯粹面向对象的语言

## 关键字
保留关键字（Reserved Word）：在高级语言中已经定义过的字，使用者不能再将这些字作为变量名或过程名使用

上下文关键字（KWIC）：在特定位置才是关键字。 在其他上下文中，都会将其解释为标识符。

## 基本数据类型


C# 中，变量分为以下几种类型

- 值类型：值类型的变量存储数据。值类型隐式继承 ValueType 类型，但不能创建直接从 ValueType 继承的类。 
    - 简单类型
        - 有符号整型：`int`、`long`、`short`、`sbyte`
        - 无符号整型：`uint`、`ulong`、`ushort`、`byte`
        - 浮点型：`float`、`double`、`decimal`
        - 字符类型：`char`
        - 布尔类型：`bool`
        - 枚举类型：`enum`
          - 结构体类型：`struct`
- 引用类型：引用类型的变量存储对实际数据的引用。 引用类型也称为对象。
    - 类类型（内置引用类型）
        - `object`类:在 C# 的 CTS 中，所有类型（预定义类型、用户定义类型、引用类型和值类型）都是直接或间接从 Object 继承的。Object是System.Object类的别名。
所以对象（Object）类型可以被分配任何其它类型（值类型、引用类型、预定义类型或用户自定义类型）的值。但是，在分配值之前，需要先进行类型转换。
当一个值类型转换为对象类型时，则被称为装箱(Boxing)；另一方面，当一个对象类型转换为值类型时，则被称为拆箱(Unboxing)。除了装箱值类型外，无法将引用类型转换为值类型。
        - string类。用于表示文本字符的**顺序**集合，是一个字符序列（零个或更多 Unicode 字符）。是 System.String 类型的别名。
它是从对象（Object）类型派生的。字符串（String）类型的值可以通过两种形式进行分配：引号和@引号。@（称作“逐字字符串”）将转义字符串（\）当作普通字符对待。
@字符串中可以任意换行，换行符及缩进空格计算在字符串长度之内。
    - 数组类型：array
    - 接口类型：`interface`
    - 委托类型：`delegate`
    - 动态类型：`dynamic`。可以存储任何类型的值在动态数据类型变量中。 该类型的作用是绕过编译时类型检查， 改为在运行时解析这些操作，因此这些变量的类型检查是在运行时发生的。
dynamic 类型简化了对 COM API、动态 API（如：IronPython 库）和 HTML DOM 的访问。类型 dynamic 的变量会编译为 object 类型变量中。因此，dynamic 只在编译时存在，在运行时则不存在。

        !!! tip "dynamic 类型与 object 类型有区别"
            虽然 dynamic 类型会编译为 object 类型，但单纯的 object 类型会执行编译时类型检查。在运行时，dynamic 可以隐式的转换成任何类型，并且也可以从其他类型中转换回来。
            这导致 dynamic 类型会比 object 类型多一些用法。例如：`dyn = dyn + 3; obj = obj + 3; `前者可以编译，后者会报错。typeof 运算符无法用在动态类型上.

        !!! danger "使用 dynamic 类型将增加不安全风险"
            dynamic类型允许在运行时更改其类型，因此编译器不能保证这种转换的安全性。而且在运行时需要进行类型检查和转换，相对静态类型性能开销会比较大。
            由于dynamic可以访问任何类型的属性、方法、字段，如果使用不当，容易导致安全问题。

- 指针类型：指针类型仅可用于 unsafe 模式。*

void：空类型。对应部分语言中的单元类型（unit）即()。但在C#中该类型被阉割，功能受限。void 不能被用来声明变量。


## C# 程序的通用结构

C# 程序可由一个或多个文件组成。 每个文件都可以包含零个或零个以上的命名空间。 一个命名空间除了可包含其他命名空间外，还可包含类、结构、接口、枚举、委托等类型。 以下是 C# 程序的主干，它包含所有这些元素。

```C#
    // A skeleton of a C# program C# 程序的骨架 
using System;
namespace YourNamespace
{
    class YourClass // 类
    {
    }

    struct YourStruct // 结构
    {
    }

    interface IYourInterface // 接口
    {
    }

    delegate int YourDelegate(); // 委托

    enum YourEnum 
    {
    }

    namespace YourNestedNamespace
    {
        struct YourStruct 
        {
        }
    }

    class YourMainClass
    {
        static void Main(string[] args) 
        {
            //Your program starts here...
        }
    }
}
```