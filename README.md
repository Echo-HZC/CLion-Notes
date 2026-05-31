# CLion 学习配置指南（Windows）

> 面向 **C/C++ 初学者** 的 CLion 配置教程，解决 Windows 环境下的常见痛点，涵盖从下载安装到可正常编译运行的完整流程。

---

## 📋 本仓库解决的问题

| 问题 | 说明 |
|------|------|
| ❓ 不会安装 CLion | 从官网下载到安装完成的详细步骤 |
| 💰 许可证问题 | JetBrains 非商业账号注册教程（免费使用） |
| 🔧 工具链配置 | MinGW-w64 下载与 CLion 工具链配置 |
| 📝 中文乱码 | 编辑器、控制台、独立运行 exe 的乱码根治方案 |
| 🎯 多 main 函数 | 学习阶段不用为每个练习新建项目，自动收集所有 `.cpp` 编译 |
| 🖥️ exe 无法独立运行 | 静态链接运行时库，解决缺少 DLL 问题 |

---

## 🚀 快速开始

### 1. 安装 CLion

- [📥 下载与安装指南](docs/01-CLion安装与配置指南.md)
  - 官网下载安装包
  - 安装选项建议
  - 首次启动向导

### 2. 注册 JetBrains 账号（非商业免费）

- [🔑 非商业账号注册教程](docs/01-CLion安装与配置指南.md#二等待安装完成时正好注册jetbrains账号非商业用途免费)
  - 访问 [account.jetbrains.com](https://account.jetbrains.com/login) 创建账号
  - 邮箱验证
  - CLion 中选择 **Non-commercial use** 激活

### 3. 配置工具链

- [⚙️ 工具链配置（MinGW-w64）](docs/01-CLion安装与配置指南.md#32-配置工具链toolchains)
  - 下载 MinGW-w64（推荐 UCRT64 版本）
  - CLion 中配置 `File → Settings → Toolchains`
  - 验证：新建项目运行 Hello, World!

### 4. 复制万能 CMakeLists.txt

将本仓库的 [`CMakeLists.txt`](CMakeLists.txt) 复制到你的项目根目录，替换默认生成的文件：

**功能特性：**
- ✅ 自动递归收集所有 `.cpp` 文件（排除 `cmake-build` 目录）
- ✅ 每个 `.cpp` 自动编译为独立可执行文件
- ✅ 保留目录结构输出到 `runtime/` 文件夹
- ✅ 静态链接 GCC 运行时库（解决 exe 独立运行缺 DLL）
- ✅ 强制 UTF-8 编码（解决中文乱码）
- ✅ C++20 标准

**使用方法：**
1. 复制 [`CMakeLists.txt`](CMakeLists.txt) 到项目根目录
2. 在任意位置新建 `.cpp` 文件写代码
3. 点击 **构建 → 重新构建项目**
4. 每个 `.cpp` 会自动生成对应的 `.exe`

### 5. 根治中文乱码

- [🔤 中文乱码完整解决方案](docs/03-CLion中文乱码解决方案.md)
  - 编辑器编码设置为 UTF-8
  - 终端环境变量 `CHCP=65001`
  - 禁用 PTY 模式

---

---


## ⚠️ 重要注意事项

1. **路径不要有中文或空格**
   - 安装路径：`D:\\JetBrains\\CLion` ✔️，`D:\\编程软件\\CLion` ❌
   - 项目路径：`D:\\Projects\\hello-clion` ✔️，`D:\\我的项目\\hello clion` ❌

2. **文件编码必须是 UTF-8**
   - CLion 右下角状态栏可查看/切换当前文件编码
   - 设置路径：`文件 → 设置 → 编辑器 → 文件编码 → 全局编码 UTF-8`

3. **非商业许可证**
   - 有效期 1 年，只要 6 个月内使用过会自动续期
   - 仅限学习/开源贡献/内容创作等非商业用途

4. **多 main 方案仅限学习使用**
   - 正式项目请遵循一个可执行目标一个 `main()` 的规范

5. **禁用 PTY 后需重启 CLion**
   - 路径：`帮助 → 编辑自定义属性 → 添加 run.processes.with.pty=false`

---

## 💡 常见问题速查

**Q: 为什么 exe 文件双击运行提示缺少 DLL？**  
A: 因为默认是动态链接 GCC 运行时库。使用本仓库的 [`CMakeLists.txt`](CMakeLists.txt) 已配置静态链接（`-static-libgcc -static-libstdc++`），重新构建后即可独立运行。

**Q: 为什么 CLion 内运行正常，但单独运行 exe 中文乱码？**  
A: CLion 内运行和独立运行的控制台环境不同。解决方案：
- 方案一：在代码开头加 `system("chcp 65001");`（临时）
- 方案二：Windows 控制面板 → 区域 → 管理 → 勾选"Beta 版：使用 Unicode UTF-8"（永久）
- 方案三：使用 [`CMakeLists.txt`](CMakeLists.txt) 中的 `-fexec-charset=UTF-8` 编译选项

**Q: 一个项目里怎么放多个练习文件？**  
A: 使用本仓库的 [`CMakeLists.txt`](CMakeLists.txt)，在项目中任意位置新建 `.cpp` 文件即可，会自动为每个文件生成独立的可执行目标。

---

> 觉得有用？点个 ⭐ 支持一下！
> 
> 如有问题欢迎提交 Issue 或讨论。
