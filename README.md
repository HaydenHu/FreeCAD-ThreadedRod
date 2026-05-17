# ThreadedRod - FreeCAD 螺丝柱外螺纹宏

通过 FreeCAD 自带 PartDesign Hole 的螺纹功能，在圆柱面上生成真实的外螺纹。
<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/4227b3d4-3e7a-4096-a72a-72f3f51a4f41" />

## 背景

FreeCAD PartDesign Hole 能生成带内螺纹的孔（包括真实 3D 螺纹几何），但没有直接的外螺纹工具。

ThreadedRod 利用 Hole 的螺纹能力：创建一个带螺纹孔的空心套筒，再将其与目标圆柱体做布尔差集，从而在圆柱外表面切出外螺纹。这比手工构造螺旋扫掠截面更稳定可靠。

## 安装

### 方法一：手动安装

1. 下载本仓库所有文件
2. 将 `.FCMacro` 和 `.svg` 文件复制到 FreeCAD 宏目录：
   - Windows: `%APPDATA%\FreeCAD\Macro\`
   - Linux: `~/.local/share/FreeCAD/Macro/`
   - macOS: `~/Library/Preferences/FreeCAD/Macro/`

### 方法二：克隆仓库

```bash
cd <FreeCAD Macro 目录>
git clone https://github.com/HaydenHu/FreeCAD-ThreadedRod.git .
```

## 添加到工具栏

1. 打开 FreeCAD，切换到 **Part** 或 **PartDesign** 工作台
2. 菜单 **Tools → Customize**，切换到 **Macro** 标签页
3. 选中 `ThreadedRod.FCMacro`：
   - 点击 `Pixmap` 旁的 `...`，选择 `ThreadedRod.svg`
   - 设置 **Menu text** 为 `螺丝柱外螺纹`
   - 点击 `Add`，在弹窗的 **Workbenches** 中勾选 `Part` 和 `PartDesign`
4. 切换到 **Toolbars** 标签页，将命令拖入工具栏
5. 点击 **Close**

## 使用方法

<img width="1280" height="688" alt="QQ20260517-192120" src="https://github.com/user-attachments/assets/bdd4013b-1adf-4d4a-9917-f908591d8dc2" />

1. 在 Part 或 PartDesign 工作台中选中一个圆柱面
2. 点击工具栏的 **螺丝柱外螺纹** 按钮
3. 在弹出的对话框中：
   - 选择螺纹规格（M2~M68，自动推荐匹配规格）
   - 调整螺距、螺纹长度、套筒壁厚等参数
   - 选择旋向（右旋/左旋）
4. 确认执行

## 工作原理

```
选中圆柱面
  ↓
创建临时 Body:
  ├─ AdditiveCylinder (半径 = 公称直径/2 + 壁厚)
  ├─ Sketch (定位孔心在圆心)
  └─ PartDesign::Hole (ModelThread=1)
       → Body.Shape = 内壁有螺纹的空心套筒
  ↓
将套筒移动到用户圆柱位置(对齐轴心)
  ↓
布尔切割: 用户圆柱 .cut(套筒) = 带外螺纹的螺丝柱
  ↓
删除临时对象
```

## 文件说明

| 文件 | 说明 |
|------|------|
| `ThreadedRod.FCMacro` | 螺丝柱外螺纹宏 |
| `ThreadedRod.svg` | 图标 |

## 兼容性

- FreeCAD 1.0+
- Part 和 PartDesign 工作台均支持
- Windows / Linux / macOS

## 许可

MIT License
