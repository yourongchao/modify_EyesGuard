我将为您创建 `README-you.md` 文件，详细说明项目功能、构建流程以及如何自定义提示词。

### 1. 文件创建

* **文件名**: `README-you.md`

* **位置**: 项目根目录 (`F:\0-Project\EyesGuard\EyesGuard\`)

### 2. 内容大纲

文档将包含以下三个核心部分：

#### A. 项目功能简介

* 简述 EyesGuard 的核心目标：Windows 护眼工具。

* 主要功能：定时长/短休息提醒、强制休息模式、自定义时间间隔、多语言支持。

#### B. 构建与运行指南 (基于我之前的操作实践)

* **依赖还原**: 说明如何使用 `.paket/paket.bootstrapper.exe` 和 `paket install` 恢复 NuGet 包。

* **构建命令**:

  * 推荐使用 `MSBuild.exe` 直接编译主项目 (`Source/EyesGuard/EyesGuard.csproj`)，避开 Store 打包项目的依赖问题。

  * 提供具体的 PowerShell 构建脚本示例。

* **常见问题处理**:

  * **进程占用**: 提醒在构建前使用 `taskkill` 关闭正在运行的 EyesGuard 实例。

  * **文件重命名**: 提及之前遇到的 `.yaml` -> `.yml` 问题（虽已修复，但作为注意事项）。

#### C. 如何自定义休息提示词

* **文件路径**: 指向 `Source/EyesGuard.Data/Languages/zh-CN.yml`。

* **修改位置**: `ShortMessageSuggestions` 列表。

* **操作步骤**:

  1. 打开 YAML 文件。
  2. 在列表中按格式 `- 新的提示语` 添加内容。
  3. 保存并重启应用。

### 3. 下一步行动

* 获得您确认后，我将直接写入该文件。

