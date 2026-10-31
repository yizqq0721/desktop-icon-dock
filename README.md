# Desktop Icon Dock / 桌面图标 Dock

把桌面图标变成居中的 macOS 风格 Dock。默认使用完全透明的背景，只显示图标；也可以开启可选的液态玻璃面板。

![桌面图标 Dock 效果展示](images/dock-preview-transparent.jpg)

*实际效果：桌面图标以 macOS Dock 样式居中排列，悬停的图标会放大并显示名称。*

## 功能特性

- **macOS 风格悬停放大**：鼠标接近时图标平滑放大，按余弦曲线衰减，相邻图标同时向外让位。
- **透明背景（默认）**：只绘制图标与柔和投影，完全融入壁纸；Dock 窗口的透明区域允许鼠标穿透，不会挡住桌面右键和点击。
- **可选液态玻璃面板**：关闭「透明背景」后启用。面板包含壁纸模糊、乳白玻璃、高光描边和悬停光晕；背景从 Dock 上方的屏幕区域采样，支持 Wallpaper Engine 等动态壁纸。
- **名称标签**：悬停时在图标上方显示名称胶囊；透明背景下标签位置固定，不会随图标放大而跳动。
- **原生交互**：左键单击打开项目；右键弹出与桌面一致的资源管理器原生菜单（新建快捷方式、属性、删除等）。
- **自动同步桌面**：新增、删除、重命名、移动桌面项目会自动出现在 Dock 中，顺序与桌面网格位置一致。
- **隐藏原始桌面图标**：默认隐藏系统桌面上的原图标，只保留 Dock；卸载或禁用模组时自动恢复。
- **增量图标缓存**：按项目路径缓存 256px 图标，只有新项目需要重新提取，刷新快、不闪烁。

## 工作方式

Dock 不是重排系统桌面图标，而是读取桌面文件夹（Shell 的 IFolderView）内容后自行绘制，同时用 shell 命令隐藏原图标。

- **同步频率**：约每 3 秒检查一次桌面项目；在 Dock 右键菜单中执行命令后约 0.7 秒立即刷新。
- **排序**：按桌面网格坐标先上下、后左右排序，因此 Dock 顺序与桌面排列一致，新快捷方式会出现在对应的位置。
- **布局**：水平居中于主显示器底部、任务栏上方；图标增多时面板自动变宽，静止时图标垂直居中。
- **性能**：透明背景模式下用 50ms 间隔轮询鼠标位置，以支持图标间隙的悬停放大；空闲时 CPU 占用可忽略。

## 设置说明

| 设置 | 默认值 | 说明 |
| --- | --- | --- |
| 图标大小 | 48 | 100% DPI 下的基础像素大小，随系统缩放自动放大（150% 时约 72px） |
| 图标间距 | 10 | 静止时图标之间的像素间距 |
| 最大放大比例 | 175% | 悬停图标的最大倍数 |
| 效果半径 | 160 | 放大效果从光标向外衰减的距离（像素） |
| 相邻扩散 | 60% | 放大时相邻图标向外让位的程度 |
| 平滑速度 | 16 | 数值越大越跟手，越小越柔和 |
| 底部偏移 | 10 | Dock 与任务栏之间的间距（100% DPI 像素） |
| 透明背景 | 开 | 默认只显示图标；关闭后使用液态玻璃面板 |
| 玻璃不透明度 | 24% | 仅玻璃模式：乳白玻璃的强度 |
| 玻璃模糊 | 20 | 仅玻璃模式：背景模糊强度 |
| 显示名称 | 开 | 悬停时显示名称胶囊 |
| 隐藏原始桌面图标 | 开 | 隐藏系统桌面图标，卸载时恢复 |
| 排除项目 | 空 | 逗号分隔的显示名，匹配的项目不会进入 Dock |
| 背景刷新间隔 | 20 秒 | 仅玻璃模式：重新采样玻璃背景的间隔，0 表示不刷新 |

## 兼容性与限制

- 仅支持 x86-64 的 Windows 10 / 11，通过 Windhawk 注入 explorer.exe。
- 目前仅支持主显示器；Dock 固定在主显示器底部、任务栏上方。
- Dock 是位于普通窗口下方的分层窗口：全屏应用或其它窗口会盖住它，这是预期行为。
- 透明背景模式下，窗口透明像素点击穿透，图标本身仍可正常点击。

## 故障排查

- **卸载后桌面图标没有恢复**：在桌面右键 → 查看 → 勾选「显示桌面图标」；或重新加载模组后再卸载。
- **Explorer 崩溃后 Dock 消失或图标仍隐藏**：模组会随 explorer.exe 自动重新加载；若原图标仍隐藏，使用上面的方法恢复。
- **新快捷方式没有立即出现**：最多等待约 3 秒；在 Dock 右键菜单中创建的项目约 0.7 秒出现。
- **修改设置后未生效**：保存设置即自动重载并重建，无需重启 Explorer。
- **某个项目不想显示**：在「排除项目」中填入它的显示名（逗号分隔）。
*/
// ==/WindhawkModReadme==

// ==WindhawkModSettings==
/*
- IconSize: 48
  $name: Icon size
  $name:zh-CN: 图标大小
  $description: Base icon size in pixels (at 100% DPI scaling)
  $description:zh-CN: 图标的基础像素大小（100% DPI 缩放时）
- Spacing: 10
  $name: Icon spacing
  $name:zh-CN: 图标间距
  $description: Gap between icons at rest, in pixels
  $description:zh-CN: 静止状态下图标之间的像素间距
- MaxScale: 175
  $name: Maximum scale (%)
  $name:zh-CN: 最大放大比例（%）
  $description: How large the hovered icon grows (175 = 1.75x)
  $description:zh-CN: 鼠标悬停时图标的最大放大倍数（175 = 1.75 倍）
- EffectRadius: 160
  $name: Effect radius
  $name:zh-CN: 效果半径
  $description: Distance from the cursor where magnification fades out, in pixels
  $description:zh-CN: 放大效果从光标向外衰减的距离（像素）
- SpacingFactor: 60
  $name: Neighbor spread (%)
  $name:zh-CN: 相邻扩散（%）
  $description: How much neighboring icons move aside while magnifying
  $description:zh-CN: 放大时相邻图标向外让位的程度
- Smoothing: 16
  $name: Smoothing speed
  $name:zh-CN: 平滑速度
  $description: Higher is snappier, lower is softer
  $description:zh-CN: 数值越大越跟手，越小越柔和
- BottomOffset: 10
  $name: Bottom offset
  $name:zh-CN: 底部偏移
  $description: Gap between the dock and the taskbar, in pixels
  $description:zh-CN: Dock 与任务栏之间的间距（像素）
- TransparentBackground: true
  $name: Transparent background
  $name:zh-CN: 透明背景
  $description: Show only the icons and skip the glass panel
  $description:zh-CN: 只显示图标，不使用玻璃背景
- GlassOpacity: 24
  $name: Glass opacity (%)
  $name:zh-CN: 玻璃不透明度（%）
  $description: Strength of the milky glass tint
  $description:zh-CN: 玻璃乳白色调的强度
- GlassBlur: 20
  $name: Glass blur
  $name:zh-CN: 玻璃模糊
  $description: Backdrop blur strength
  $description:zh-CN: 背景模糊强度
- ShowLabels: true
  $name: Show labels
  $name:zh-CN: 显示名称
  $description: Show the name of the hovered item in a floating glass pill
  $description:zh-CN: 悬停时在浮动玻璃标签中显示名称
- HideDesktopIcons: true
  $name: Hide original desktop icons
  $name:zh-CN: 隐藏原始桌面图标
  $description: Hide the original desktop icons while the dock is active
  $description:zh-CN: 启用 Dock 时隐藏桌面上的原始图标
- ExcludeItems: ""
  $name: Excluded items
  $name:zh-CN: 排除项目
  $description: Comma separated display names to keep out of the dock
  $description:zh-CN: 用逗号分隔的显示名称，匹配的项目不会出现在 Dock 中
- BackdropRefreshSeconds: 20
  $name: Backdrop refresh interval (s)
  $name:zh-CN: 背景刷新间隔（秒）
  $description: How often the blurred glass backdrop is re-sampled
  $description:zh-CN: 多久重新采样一次玻璃背景
