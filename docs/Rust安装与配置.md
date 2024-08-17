# Rust 安装与配置

## 安装 Rust
rustup 既是一个 Rust 安装器又是一个 Rust 版本管理工具。

- 在 Linux 或 macOS 上安装 rustup
`$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh`

- 在 Windows上安装rustup，请前往官网下载安装程序。

Rust还需要一个链接器（linker），将生成的所有机器代码以及相关的库链接成一个可执行文件。很可能你已经有一个了。如果你遇到了链接器错误，请尝试安装一个 C/C++编译器编译器，它通常包括一个链接器。C/C++编译器编译器很有用，因为一些常见的 Rust 包依赖于 C 代码。

- 在 macOS 上，你可以通过运行以下命令获得 C 语言编译器：`$ xcode-select --install`

- Linux 用户通常需要根据发行版（distribution）文档安装 GCC 或 Clang。

- 在 Windows 上需要安装 Visual Studio 2013 或其更新版本的 MSVC 构建工具。或者其他具有连接器的编译器（WinGW、Cygwin等） 。当被问及需要安装什么工作负载（Workload）的时候，请确保勾选了以下内容：

    - 使用 C++ 的桌面开发（“Desktop Development with C++”）
    - Windows 10（或 11）SDK
    - 英语语言包，以及其他你所需要的语言包

### 故障排除（Troubleshooting）

要检查是否正确安装了 Rust，打开命令行并输入：`$ rustc --version`

你应该可以看到按照以下格式显示的最新稳定版本的版本号、对应的 Commit Hash 和 Commit 日期：
`rustc x.y.z (abcabcabc yyyy-mm-dd)`

如：`rustc 1.77.1 (7cf61ebde 2024-03-27)`。这说明 Rust 已经安装成功了！

如果没看到，请按照下面说明的方法检查 Rust 是否在您的 %PATH% 系统变量中。

在 Windows CMD 中，请使用命令：`> echo %PATH%`

在 PowerShell 中，请使用命令：`> echo $env:Path`

在 Linux 和 macOS 中，请使用命令：`$ echo $PATH`

## Rust 工具链（Toolchain）

Toolchain 指一组Rust工具，包括编译器（rustc）、构建工具（cargo）、文档生成工具（rustdoc）、工具链管理器（rustup）、代码格式化工具（rustfmt） 等等。
Toolchain 用于管理和构建 Rust 代码，并且可以包括一个特定版本的 Rust 编译器和标准库，还包含一个默认是编译到本机平台的target。 工具链的版本可以是 "stable"（稳定版）、"beta"（测试版）或 "nightly"（每日构建版），每个版本都对应着不同的 Rust 编译器和特性。
Toolchain 默认存储在`~/.cargo/bin`这个目录下。

### Rustup常见命令

```shell
rustup install stable # 安装最新版本的 Rust，包括stable、 beta 和 nightly
rustup update # 更新脚本
rustup self uninstall # 卸载 Rust 和 rustup
rustup doc # 在浏览器中查看自带本地文档
rustup target list # 列出可用的 target
rustup target add x86_64-unknown-linux-gnu # # 安装一个新的 rustup target add <target>
```

任何时候，如果你拿不准标准库中的类型或函数的用途和用法，请查阅 API 文档！

### Cargo 常见命令

```shell
cargo new 项目名 # 支持中文项目名
cargo run 项目名 # 
cargo build
cargo test
cargo --list # 查看 cargo 提供的所有命令
```

## Rust 源代码编译过程
Rust 是一门 AOT 静态类型语言。
也是跨平台语言，支持跨平台编译，Rust的标准库提供的对多种系统调用的统一抽象。
Rust 的编译过程是相当复杂的，涉及多个阶段与子系统。以下是一个简化的概述：

1. 编译器前端
    1. 词法分析：首先，Rust 源代码会被分解成一系列的标记，或称为 “tokens”（例如关键字、标识符、字面量、符号等）。这个阶段也被称为 “词法分析” 或 “tokenizing”。
    2. 语法分析：接着，这些标记会被解析成一棵 AST 。在这个阶段，编译器会检查代码的语法，并确定如何将代码组织为表达式和语句。
    3. 语义分析：然后，编译器会进行语义分析，包括类型检查、借用检查等，确保代码满足 Rust 的语言规则。
2. 编译器后端（以LLVM+某一链接器为例）
    + MIR：接下来，编译器将 AST 转换为 MIR 。这是一个更简洁、更低级地表示，它使得后续的优化和分析步骤更容易进行。
    + 优化：在 MIR 阶段，编译器会执行一系列的优化，如移除死代码、简化表达式等。
    + 代码生成：然后，编译器将 MIR 转换为 LLVM IR 。这是一个一种介于C语言和汇编语言的表示，可以被 LLVM 的后端用来生成各种平台的机器代码。
    + 链接：最后，编译器使用链接器将生成的所有机器代码以及相关的库链接成一个可执行文件。

Rustc编译器中，1-3阶段编译过程都是Rust实现，后端可选用LLVM、GCC、CUDA、cranelift（Rust实现）等。建议运用LLVM是于生产阶段，cranelift编译速度快，可用于测试阶段。
Rustc 将Rust 源代码编译成机器代码,但是生成的这些机器代码通常是分散在多个对象文件中的。为了将这些对象文件以及所依赖的库文件合并成一个单一的可执行文件，
我们需要一个链接器,也就是7阶段。 7阶段Rust选择调用外部的链接工具，可能是 GNU 或 MSVC ，具体取决于目标平台和环境。
rustc为什么不自己实现第 7 阶段链接过程呢？ 因为Rust编译器团队贯彻了"不重复造轮子"（Don’t reinvent the wheel）这一编程理念。

# Rust 配置