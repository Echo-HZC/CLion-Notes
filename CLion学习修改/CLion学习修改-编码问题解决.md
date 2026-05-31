# CLion 中文乱码终极解决方案（Windows + UTF-8）

> 彻底根治 CLion 在 Windows 下的中文乱码问题，覆盖编辑器、控制台、运行输出三个层面。

---

## 前置要求

**所有文件必须以 UTF-8 编码保存。**

检查/设置文件编码的方法：
- CLion 右下角状态栏显示当前文件编码，点击可切换
- 或：文件 → 设置 → 编辑器 → 文件编码

---

## 第一步：设置 CLion 编辑器编码

**路径**：`文件(File)` → `设置(Settings)` → `编辑器(Editor)` → `文件编码(File Encodings)`

| 设置项 | 值 |
|--------|-----|
| 全局编码 (Global Encoding) | `UTF-8` |
| 项目编码 (Project Encoding) | `UTF-8` |
| 属性文件默认编码 (Default encoding for properties files) | `UTF-8` |
| 自动转换为 ASCII 但显示为 Unicode (Transparent native-to-ascii conversion) | ✅ **勾选** |

> 勾选"自动转换为 ASCII 但显示为 Unicode"可确保 `.properties` 文件中的中文不被转义为 `\\uXXXX`。

---

## 第二步：设置运行/调试控制台编码

**路径**：`文件(File)` → `设置(Settings)` → `工具(Tools)` → `终端(Terminal)`

在 **环境变量 (Environment variables)** 中添加：

```
CHCP=65001
```

> `65001` 是 Windows 控制台的 UTF-8 代码页标识。设置后，CLion 内置终端和运行窗口将默认使用 UTF-8 编码。

---

## 第三步：禁用 PTY（伪终端）模式

**路径**：`帮助(Help)` → `编辑自定义属性(Edit Custom Properties...)`

在打开的文件中添加以下内容（如文件为空则直接添加）：

```properties
run.processes.with.pty=false
```

> **作用**：禁用 PTY 后，CLion 运行程序时使用标准管道而非伪终端，可避免 Windows PTY 层对编码的额外转换，从而解决部分乱码问题。
>
> 保存后 **重启 CLion** 生效。

---

## 补充：CMakeLists.txt 编码配置（可选）

如果上述三步后仍有乱码，可在 `CMakeLists.txt` 中追加编译器编码选项：

```cmake
# MinGW / GCC
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fexec-charset=UTF-8 -finput-charset=UTF-8")

# MSVC
add_compile_options("/utf-8")
```

---

## 验证步骤

创建一个测试文件 `test.cpp`：

```cpp
#include <iostream>

int main() {
    std::cout << "中文测试：Hello 世界" << std::endl;
    return 0;
}
```

运行后，若控制台输出 **"中文测试：Hello 世界"** 且无乱码，则配置成功。

---

## 配置汇总速查表

| 步骤 | 路径 | 关键设置 |
|------|------|----------|
| 1 | 设置 → 编辑器 → 文件编码 | 全部设为 UTF-8，勾选透明转换 |
| 2 | 设置 → 工具 → 终端 | 环境变量添加 `CHCP=65001` |
| 3 | Help → Edit Custom Properties | 添加 `run.processes.with.pty=false` |
| 重启 | — | 必须重启 CLion |

---

*觉得有用？点个 ⭐ 支持一下！*

