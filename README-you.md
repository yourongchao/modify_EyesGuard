# EyesGuard 项目开发与维护指南

本文档旨在帮助开发者（或用户）理解项目功能、搭建本地开发环境、构建可执行文件以及自定义休息提示词。

## 1. 项目功能简介

**EyesGuard** 是一款运行在 Windows 平台上的护眼工具，旨在**通过定时提醒用户休息来保护视力**。

**核心功能：**
*   **双重休息模式**：
    *   **小休息 (Short Break)**：高频短时的提醒（如每15分钟眨眼、远眺）。
    *   **大休息 (Long Break)**：低频长时的休息（如每1小时离开座位走动）。
*   **强制模式**：可设置无法跳过或推迟休息，强制用户暂停工作。
*   **高度可定制**：用户可自定义休息间隔、持续时间以及提示语。
*   **多语言支持**：支持中文、英语等多种语言。
*   **系统集成**：支持开机自启、托盘最小化、系统空闲检测（自动暂停计时）。

---

## 2. 构建与运行指南

如果您修改了代码（例如修改了 UI 或逻辑），需要重新构建项目才能生成新的可执行文件。以下是经过验证的构建步骤。

### 前置准备
*   **操作系统**: Windows 10 或 Windows 11。
*   **环境**: .NET Framework 4.8 开发包。
*   **工具**: PowerShell。

### 步骤一：还原依赖 (Restore Dependencies)
项目使用 `Paket` 管理依赖包。在首次构建或依赖变更时运行：

```powershell
# 运行 Paket 引导程序
.\.paket\paket.bootstrapper.exe

# 安装/还原依赖
.\.paket\paket.exe install
```

### 步骤二：处理进程占用
如果 EyesGuard 正在运行，构建会因文件被占用而失败。请先关闭应用：

```powershell
# 强制结束 EyesGuard 进程
taskkill /F /IM EyesGuard.exe
```

### 步骤三：构建项目 (Build)
由于项目包含 UWP 打包工程（StorePackage），直接使用 `dotnet build` 可能会因为缺少 UWP 环境而报错。
**推荐方案**：直接使用 `MSBuild` 构建主程序 `EyesGuard.csproj`。

在项目根目录下，运行以下命令（假设您已安装 Visual Studio 或 Build Tools，且 MSBuild 在路径中，或者使用 VS 的开发者 PowerShell）：

```powershell
# 定位到主项目目录并构建
msbuild .\Source\EyesGuard\EyesGuard.csproj /p:Configuration=Debug /p:Platform="Any CPU"
```

*注意：如果 `msbuild` 命令未找到，您可能需要从 Visual Studio 开发者命令提示符 (Developer Command Prompt for VS) 中运行，或者使用完整路径。*

### 步骤四：运行应用
构建成功后，可执行文件位于 `bin\Debug` 目录下：

```powershell
# 运行生成的程序
.\Source\EyesGuard\bin\Debug\EyesGuard.exe
```

### 总结构建命令

```powershell
& "E:\Software_Microsoft Visual Studio_2022Community\MSBuild\Current\Bin\MSBuild.exe" Source\EyesGuard\EyesGuard.csproj /p:Configuration=Debug /p:Platform="AnyCPU"

Source\EyesGuard\bin\Debug\EyesGuard.exe
```
第一个是将代码编译成可执行文件，第二个是运行可执行文件。

---

## 3. 如何添加休息提示词

您可以自定义小休息弹窗中显示的提示语（如“眨眨眼”、“喝杯水”）。这些内容存储在语言配置文件中。

### 修改步骤

1.  **找到语言文件**：
    打开文件：`Source\EyesGuard.Data\Languages\zh-CN.yml`

2.  **定位配置项**：
    找到 `ShortMessageSuggestions` 列表部分：

    ```yaml
    ShortMessageSuggestions:
    - 眨一眨眼
    - 往死里眨眼~
    - 遥望远方~
    # ...
    ```

3.  **添加新提示**：
    在列表中按相同格式添加您的句子。例如：

    ```yaml
    ShortMessageSuggestions:
    - 眨一眨眼
    - ...
    - 站起来活动一下颈椎      <-- 新增的内容
    - 去给杯子续杯水          <-- 新增的内容
    ```

4.  **生效**：
    保存文件后，**重启 EyesGuard** 即可生效。新的提示语将会在小休息弹窗中随机出现。

---

## 4. 常见问题记录

*   **pt-BR.yml 错误**：如果构建时提示找不到文件，请检查 `Source\EyesGuard.Data\Languages\` 下是否存在 `pt-BR.yaml`。如果是，请将其重命名为 `pt-BR.yml` 以匹配项目引用。
*   **MSB3026/3027 错误**：通常是因为程序正在运行，导致 DLL 或 EXE 无法被覆盖。请执行 `taskkill /F /IM EyesGuard.exe`。
