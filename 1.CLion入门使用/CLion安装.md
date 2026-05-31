# CLion 安装与配置指南

> 本指南基于 **Windows 10/11** 环境整理，面向 C++ 初学者，涵盖从下载到可正常编译运行的完整流程。

---

## 📥 一、下载 CLion

### 一：官网直接下载（免费使用30天）

1. 访问官网：[https://www.jetbrains.com/clion/download/](https://www.jetbrains.com/clion/download/)
2. 点击 **"Download"** 下载 Windows 版安装包（`.exe`）
3. 下载完成后双击运行安装程序

## 🛠️ 二、安装步骤

1. **运行安装程序**（`CLion-*.exe`）
2. **安装选项建议**：
   - ✅ `64-bit launcher`（创建桌面快捷方式）
   - ✅ `Add "Open Folder as Project"`（右键菜单快速打开项目）
   - ✅ `.cpp` / `.h` / `.hpp` 文件关联（可选，方便双击打开文件）
   - ✅ `Add to PATH`（可选，方便命令行启动，不强制）
3. **安装路径建议**：
   - 默认路径为 `C:\\Program Files\\JetBrains\\CLion *`
   - 如果 C 盘紧张，可改为 `D:\\JetBrains\\CLion`
4. 点击 **Install** 等待完成，勾选 **Run CLion** 结束安装

---

### 二：等待安装完成时正好注册JetBrains账号（非商业用途免费）
> JetBrains 对非商业用户免费，一次申请有效期通常为 1 年，可续期。
> 
# 1. 访问注册页面
打开浏览器，访问：[https://account.jetbrains.com/login],点击页面上的 Create account（创建账号）。

# 2. 填写注册信息
输入以下信息：
Email address：你的邮箱地址（建议使用常用邮箱）
Password：设置密码（至少8位，包含大小写字母和数字）
First name / Last name：你的姓名

# 3. 验证邮箱
提交后，JetBrains 会发送一封验证邮件到你的邮箱
打开邮箱，点击邮件中的 Verify email 链接完成验证

---

## 🔧 三、首次启动与工具链配置

CLion 本身只是编辑器，**必须配置 C++ 编译器工具链**才能编译运行代码。

### 3.1 首次启动向导

首次打开 CLion 会弹出配置向导：

1. **导入设置**：选择 `Do not import settings`（全新安装）
2. **主题选择**：Darcula（深色）或 Light（浅色），后续可在设置中更改
3. **插件选择**：初学者建议先跳过，后续按需安装
4. **激活**：
  在许可证窗口选择 Non-commercial use（非商业用途）
  点击 Log in to JetBrains Account...
  浏览器会弹出登录页面，输入刚才注册的邮箱和密码
  登录成功后，回到 CLion 点击 Activate（激活） 即可

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

### Q：试用期过了怎么办？

- 如果是学生，去申请教育授权（见上文方式 B）
- 或购买个人版授权（JetBrains 订阅制，约 ¥700~800/年，有时有折扣）

---

> 💡 **持续更新中**：如果你遇到本指南未覆盖的问题，欢迎提 Issue 或补充经验。
