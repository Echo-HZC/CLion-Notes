# CLion 学习配置指南（Windows）

> 面向 C/C++ 初学者的 CLion 配置教程，解决 Windows 环境下的常见痛点。

---

## ⚠️ 适用范围

- **操作系统**：Windows
- **目标用户**：C/C++ 学习者（非生产开发）
- **IDE**：CLion

---

## ✅ 已实现功能

### 1. 解决中文乱码问题

**问题**：CLion 使用 UTF-8 编码，但 Windows 控制台默认使用 GBK，导致中文输出乱码。

**解决方案**：在 `CMakeLists.txt` 中添加编译选项，强制使用 UTF-8 编码：

```cmake
# 在 CMakeLists.txt 顶部添加
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fexec-charset=GBK")
# 或针对 MSVC
add_compile_options("/source-charset:utf-8" "/execution-charset:gbk")
```

> 💡 **注意**：此配置解决的是 CLion 内运行时的乱码。exe 文件独立运行时仍可能乱码（见下方未解决问题）。

---

### 2. 解决"一个项目只能有一个 main 函数"问题

**问题**：CLion 默认要求每个可执行目标对应一个 `main()`，学习时每个练习都要新建项目，非常繁琐。

**解决方案**：使用 `add_executable()` 为每个练习单独创建目标，或通过宏批量添加：

```cmake
# 方案 A：手动添加每个练习
add_executable(exercise1 src/ex1.cpp)
add_executable(exercise2 src/ex2.cpp)

# 方案 B：自动扫描 src 目录下的所有 cpp 文件（推荐）
file(GLOB EXERCISE_SOURCES "src/*.cpp")
foreach(source_file ${EXERCISE_SOURCES})
    get_filename_component(exec_name ${source_file} NAME_WE)
    add_executable(${exec_name} ${source_file})
endforeach()
```

> ⚠️ **开发人员慎用**：这种多 main 的方式不适合正式项目，仅用于学习阶段快速验证代码。

---

### 3. 解决 exe 程序无法运行问题

**问题**：Windows 下直接运行 CLion 编译的 exe 时，提示缺少 DLL 或无法找到依赖。

**解决方案**：修改 `CMakeLists.txt`，将可执行文件输出到指定目录，并复制所需 DLL：

```cmake
# 设置输出目录
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/running)

# 如果需要复制 DLL，可以添加自定义命令
# add_custom_command(TARGET your_target POST_BUILD
#     COMMAND ${CMAKE_COMMAND} -E copy_if_different
#     "path/to/your.dll"
#     $<TARGET_FILE_DIR:your_target>)
```

编译后，所有 `.exe` 文件会自动生成在项目根目录的 `running/` 文件夹中，方便直接双击运行。

---

## ❌ 未解决问题

| 问题 | 说明 | 可能的解决方向 |
|------|------|----------------|
| **exe 独立运行乱码** | 脱离 CLion 运行 exe 时，中文仍显示乱码 | 修改 Windows 控制台默认代码页为 UTF-8，或在程序开头调用 `system("chcp 65001")` |

### 临时解决方案（在代码中添加）

```cpp
#include <stdlib.h>

int main() {
    system("chcp 65001");  // 切换控制台为 UTF-8
    // 你的代码...
    return 0;
}
```

> 注意：`system()` 调用有安全风险，仅用于学习测试。

---

## 📁 完整 CMakeLists.txt 示例

```cmake
cmake_minimum_required(VERSION 3.20)
project(CLionStudy)

set(CMAKE_CXX_STANDARD 17)

# 1. 解决中文乱码（CLion 内运行）
if(MSVC)
    add_compile_options("/source-charset:utf-8" "/execution-charset:gbk")
else()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fexec-charset=GBK")
endif()

# 2. 设置 exe 输出目录
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/running)

# 3. 自动添加 src 目录下所有 cpp 为独立可执行文件
file(GLOB SOURCES "src/*.cpp")
foreach(source ${SOURCES})
    get_filename_component(name ${source} NAME_WE)
    add_executable(${name} ${source})
endforeach()
```

---

## 🛠️ 推荐目录结构

```
CLion-Study/
├── CMakeLists.txt
├── src/
│   ├── hello.cpp
│   ├── loop.cpp
│   └── array.cpp
├── running/          # 自动生成的 exe 目录
└── README.md
```

---

## 📌 补充建议

1. **控制台 UTF-8 永久设置**（Windows 10+）：
   - 控制面板 → 区域 → 管理 → 更改系统区域设置 → 勾选"Beta 版：使用 Unicode UTF-8 提供全球语言支持"
   - 重启后，控制台默认使用 UTF-8，可解决 exe 乱码问题

2. **更优雅的 exe 乱码方案**：
   在代码中使用宽字符版本：`wprintf()`、`std::wcout`，并设置控制台输出代码页

---

*觉得有用？点个 ⭐ 支持一下！*

