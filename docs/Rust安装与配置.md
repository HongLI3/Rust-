# Rust 安装
```python
print("word")
print("word")
print("word")
print("word")

```
## 安装 rustup
rustup既是一个 Rust 安装器又是一个版本管理工具。

- 在 Linux 或 macOS 上安装 rustup

  `$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh`

- 在 Windows上安装rustup，请前往官网下载安装程序。

Rust还需要一个链接器（linker），将生成的所有机器代码以及相关的库链接成一个可执行文件。很可能你已经有一个了。如果你遇到了链接器错误，请尝试安装一个 C/C++编译器编译器，它通常包括一个链接器。C/C++编译器编译器很有用，因为一些常见的 Rust 包依赖于 C 代码。

- 在 macOS 上，你可以通过运行以下命令获得 C 语言编译器：`$ xcode-select --install`

- Linux 用户通常需要根据发行版（distribution）文档安装 GCC 或 Clang。

- 在 Windows 上需要安装 Visual Studio 2013 或其更新版本的 MSVC 构建工具。或者其他具有连接器的编译器（WinGW、Cygwin等）

  当被问及需要安装什么工作负载（Workload）的时候，请确保勾选了以下内容：

  - 使用 C++ 的桌面开发（“Desktop Development with C++”）
  - Windows 10（或 11）SDK
  - 英语语言包，以及其他你所需要的语言包

## 故障排除（Troubleshooting）

要检查是否正确安装了 Rust，打开命令行并输入：`$ rustc --version`

你应该可以看到按照以下格式显示的最新稳定版本的版本号、对应的 Commit Hash 和 Commit 日期：
`rustc x.y.z (abcabcabc yyyy-mm-dd)` 

如：`rustc 1.77.1 (7cf61ebde 2024-03-27)`。这说明 Rust 已经安装成功了！

如果没看到，请按照下面说明的方法检查 Rust 是否在您的 %PATH% 系统变量中。

在 Windows CMD 中，请使用命令：`> echo %PATH%`

在 PowerShell 中，请使用命令：`> echo $env:Path`

在 Linux 和 macOS 中，请使用命令：`$ echo $PATH`

## 更新与卸载

命令行中运行如下更新脚本：`$ rustup update`

若要卸载 Rust 和 rustup，请在命令行中运行如下卸载脚本：
`$ rustup self uninstall`

## Rust 自带本地文档

安装程序也自带一份文档的本地拷贝，可以离线阅读。运行` rustup doc `在浏览器中查看本地文档。

任何时候，如果你拿不准标准库中的类型或函数的用途和用法，请查阅应用程序接口（application programming interface，API）文档！

# Rust 配置