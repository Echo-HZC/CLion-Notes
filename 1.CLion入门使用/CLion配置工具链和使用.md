### 3.2 配置工具链（Toolchains）

CLion 需要至少一个工具链（编译器 + CMake + 调试器）。

#### 推荐方案：MinGW-w64（Windows 上最省心）

**步骤 1：下载 MinGW-w64**

建议下载 **UCRT64** 版本（兼容性好，支持 Windows 10/11）：

- 推荐来源：[MSYS2](https://www.msys2.org/) 或 [WinLibs](https://winlibs.com/)
- **WinLibs 直链方案**（更简单）：
  1. 访问 [https://winlibs.com/](https://winlibs.com/)
  2. 下载 `GCC *.*.* (with POSIX threads) + LLVM/Clang/LLD/LLDB` 的 **UCRT64** 版本（`zip` 包）
  3. 解压到无中文、无空格的路径，例如 `D:\\mingw64`

**步骤 2：在 CLion 中配置**

1. 打开 CLion，进入 **File → Settings → Build, Execution, Deployment → Toolchains**
2. 点击 **+** 添加新工具链，选择 `MinGW`
3. 填写路径：
   - **Environment**: `D:\\mingw64`（你解压的路径）
   - 如果路径正确，CLion 会自动检测下方的 `CMake`、`C Compiler`、`C++ Compiler`、`Debugger`
4. 点击 **Apply** → **OK**

**步骤 3：验证**

1. 新建项目：`File → New Project → C++ Executable`
2. 写一段简单的 `Hello, World!`
3. 点击右上角绿色三角运行（`Shift+F10`）
4. 下方 Run 窗口正常输出即表示配置成功


## ⚙️ 四、首次新建项目注意事项

### 4.1 项目类型选择

| 类型 | 说明 |
|------|------|
| `C++ Executable` | 普通可执行程序（最常用，初学者选这个） |
| `C++ Library` | 静态/动态库 |
| `C Executable` | 纯 C 语言项目 |

### 4.2 项目路径与命名

- **项目路径**：避免中文、空格、特殊符号！建议 `D:\\Projects\\hello-clion`
- **项目命名**：使用英文，例如 `hello-clion`、`cpp-learning`
- **标准**：选择 `c++17` 或 `c++20`（现代 C++ 标准）

### 4.3 自动生成的 CMakeLists.txt

新建项目后，CLion 会自动生成 `CMakeLists.txt`，初学者先不要删改，能编译运行后再学习修改。

---
