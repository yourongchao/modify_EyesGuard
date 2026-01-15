# 优化 EyesGuard 短休息弹窗功能

根据您的需求，我将对短休息弹窗（Short Break Window）进行界面升级和功能增强。

## 修改文件
1.  **`Source/EyesGuard/Views/Windows/ShortBreakWindow.xaml`**: 修改界面布局，增加关闭按钮和延迟按钮。
2.  **`Source/EyesGuard/Views/Windows/ShortBreakWindow.xaml.cs`**: 处理新按钮的点击事件。
3.  **`Source/EyesGuard/App.TimingAndControl.xaml.cs`**: 增加“取消休息”和“延迟休息”的核心逻辑。

## 详细实施步骤

### 1. 核心逻辑扩展 (`App.TimingAndControl.xaml.cs`)
在 `App` 类中添加两个新方法：
*   **`CancelShortBreak()`**: 用于处理关闭按钮逻辑。
    *   停止当前休息倒计时。
    *   关闭窗口。
    *   重置并重新开始正常的休息间隔倒计时。
*   **`DelayShortBreak(TimeSpan duration)`**: 用于处理延迟按钮逻辑。
    *   停止当前休息倒计时。
    *   关闭窗口。
    *   将下一次休息的倒计时（`NextShortBreak`）设置为 5 分钟。
    *   启动倒计时。

### 2. 界面布局重构 (`ShortBreakWindow.xaml`)
*   **Grid 列定义调整**: 将原来的 3 列扩展为 4 列，新增一列用于放置右侧按钮。
*   **新增关闭按钮**:
    *   位于右上角 (Grid.Column="3")。
    *   尺寸 24x24，使用 FontAwesome 的 `Close` 图标。
    *   添加 ToolTip: "关闭"。
    *   样式：透明背景，绿色图标，鼠标悬停变色。
*   **新增延迟按钮**:
    *   位于右侧垂直居中 (Grid.Column="3")。
    *   内容："延迟5分钟"。
    *   样式：与现有绿色主题一致的扁平化按钮。
*   **过渡动画**: 为按钮添加简单的透明度变化（Opacity）作为点击反馈。

### 3. 事件交互 (`ShortBreakWindow.xaml.cs`)
*   **关闭按钮点击**: 调用 `((App)Application.Current).CancelShortBreak()`。
*   **延迟按钮点击**: 
    *   显示短暂提示 "已延迟5分钟" (可选，通过 Toast 或改变按钮文字实现，鉴于窗口即将关闭，可能直接关闭更流畅)。
    *   调用 `((App)Application.Current).DelayShortBreak(TimeSpan.FromMinutes(5))`。

## 验证计划
1.  **功能测试**:
    *   点击“关闭”：窗口立即消失，主程序恢复倒计时。
    *   点击“延迟5分钟”：窗口消失，主程序显示下一次休息在 5 分钟后。
2.  **界面测试**:
    *   检查按钮布局是否拥挤。
    *   确认图标显示正常。
    *   多显示器测试（基于现有窗口逻辑，应保持在屏幕中心）。
