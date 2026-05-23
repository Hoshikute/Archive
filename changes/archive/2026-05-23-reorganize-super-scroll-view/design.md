# Design: 整理 SuperScrollView 无限滚动组件目录结构

## 架构概览

```
移动前                                    移动后
────────────────────────────────────────────────────────────────────
Assets/SuperScrollView/                  Assets/Plugins/SuperScrollView/
├── Demo/                                ├── Editor/
│   ├── Prefabs/                         ├── Runtime/
│   ├── Scenes/                          │   ├── Common/
│   ├── Scripts/                         │   ├── GridView/
│   └── Texture/                         │   ├── ListView/
├── Editor/                              │   └── StaggeredGridView/
├── Scripts/                             └── Document.pdf
│   ├── Common/
│   ├── GridView/                        
│   ├── ListView/                        
│   └── StaggeredGridView/               Assets/Demos/SuperScrollViewDemo/
└── Document.pdf                         ├── Prefabs/
                                         ├── Scenes/
                                         ├── Scripts/
                                         └── Texture/
```

## 目录规范说明

### TEngine 第三方库规范

| 目录 | 用途 | 放置内容 |
|------|------|---------|
| `Assets/Plugins/` | 纯代码/DLL 类第三方库 | SuperScrollView 核心 |
| `Assets/Demos/` | 示例项目 | SuperScrollView Demo |
| `Assets/AssetArt/` | 美术资源类 | 不适用 |
| `Assets/TEngine/` | 框架核心 | 不适用 |

### Unity Package 规范

| 目录名 | 用途 |
|--------|------|
| `Runtime/` | 运行时代码（原 Scripts/） |
| `Editor/` | 编辑器扩展代码 |
| `Tests/` | 测试代码（本项目不涉及） |

## 核心代码结构

### Runtime 目录

```
Assets/Plugins/SuperScrollView/Runtime/
├── Common/
│   ├── ClickEventListener.cs      # 点击事件监听
│   ├── CommonDefine.cs            # 通用定义
│   └── ItemPosMgr.cs             # Item 位置管理
│
├── GridView/
│   ├── GridItemGroup.cs          # Grid Item 组
│   ├── LoopGridView.cs           # 循环网格视图（主组件）
│   ├── LoopGridViewItem.cs       # Grid Item
│   └── LoopGridItemPool.cs       # 对象池
│
├── ListView/
│   ├── LoopListView2.cs          # 循环列表视图（主组件）
│   ├── LoopListViewItem2.cs      # List Item
│   └── LoopListItemPool.cs       # 对象池
│
└── StaggeredGridView/
    ├── LoopStaggeredGridView.cs  # 瀑布流视图（主组件）
    ├── LoopStaggeredGridViewItem.cs
    ├── StaggeredGridItemGroup.cs
    └── StaggeredGridItemPool.cs
```

### Editor 目录

```
Assets/Plugins/SuperScrollView/Editor/
├── LoopGridViewEditor.cs         # GridView 编辑器
├── LoopListViewEditor2.cs        # ListView 编辑器
└── LoopStaggeredGridViewEditor.cs # StaggeredGridView 编辑器
```

## Demo 目录结构

```
Assets/Demos/SuperScrollViewDemo/
├── Prefabs/
│   ├── ButtonPanelBack.prefab
│   ├── ButtonPanelChat.prefab
│   ├── ButtonPanelDelete.prefab
│   └── ... (其他 Prefab)
│
├── Scenes/
│   ├── Menu/
│   │   └── Menu.unity
│   ├── ListView/
│   │   ├── ListViewSimpleTopToBottomDemo.unity
│   │   └── ... (其他场景)
│   ├── GridView/
│   ├── StaggeredView/
│   ├── TreeView/
│   ├── NestedView/
│   ├── Chat/
│   ├── Gallery/
│   ├── PageView/
│   ├── SpinView/
│   ├── DraggableView/
│   ├── ResponsiveView/
│   ├── SpecialGridView/
│   └── ListViewAnimation/
│
├── Scripts/
│   ├── Base/
│   ├── ButtonPanel/
│   ├── DataSource/
│   ├── Item/
│   ├── Res/
│   └── ViewDemo/
│
└── Texture/
    ├── background/
    ├── bubble/
    ├── button/
    ├── icon/
    └── pic/
```

## 移动策略

### 方案：Unity AssetDatabase.MoveAsset

使用 Unity API 移动资源，保持 GUID 和引用关系：

```csharp
// 伪代码示意
AssetDatabase.MoveAsset(
    "Assets/SuperScrollView/Scripts",
    "Assets/Plugins/SuperScrollView/Runtime"
);

AssetDatabase.MoveAsset(
    "Assets/SuperScrollView/Editor",
    "Assets/Plugins/SuperScrollView/Editor"
);

AssetDatabase.MoveAsset(
    "Assets/SuperScrollView/Demo",
    "Assets/Demos/SuperScrollViewDemo"
);
```

### 移动顺序

1. 创建目标目录结构
2. 移动 Runtime（原 Scripts）
3. 移动 Editor
4. 移动 Demo
5. 移动 Document.pdf
6. 删除空的原目录
7. 刷新 AssetDatabase

## 程序集定义

### 当前状态

SuperScrollView 未使用 asmdef，所有代码编译到默认程序集。

### 建议（可选）

为 SuperScrollView 添加 asmdef：

```
Assets/Plugins/SuperScrollView/
├── Runtime/
│   └── SuperScrollView.Runtime.asmdef
└── Editor/
    └── SuperScrollView.Editor.asmdef
```

**注意**：本次变更暂不添加 asmdef，保持原有编译方式。

## 命名空间

SuperScrollView 使用以下命名空间：

| 命名空间 | 包含类型 |
|---------|---------|
| `SuperScrollView` | 所有核心类型 |

移动后命名空间不变，无需修改代码。

## 验证检查点

### 移动后验证

1. **编译检查**：确保无编译错误
2. **Prefab 检查**：Demo Prefab 脚本关联正常
3. **场景检查**：Demo 场景可正常运行
4. **组件检查**：LoopListView2 等组件可正常添加

### 功能验证

| 组件 | 验证场景 |
|------|---------|
| LoopListView2 | `ListViewSimpleTopToBottomDemo.unity` |
| LoopGridView | `GridViewSimpleTopToBottomDemo.unity` |
| LoopStaggeredGridView | `StaggeredViewTopToBottomDemo.unity` |

## 依赖项

| 依赖 | 状态 |
|------|------|
| Unity Editor | 需要运行 |
| Coplay Unity MCP | 用于操作文件系统 |

## 回滚方案

如移动后出现问题：

1. Git 恢复：`git checkout -- Assets/SuperScrollView/`
2. 手动恢复：使用 Unity Project 窗口拖拽回原位置

## 实现顺序

```
Phase 1: 准备
├── 创建目标目录结构
└── 验证原目录内容

Phase 2: 核心代码移动
├── 移动 Scripts → Runtime
├── 移动 Editor
└── 移动 Document.pdf

Phase 3: Demo 移动
├── 创建 Demos 目录
└── 移动 Demo 内容

Phase 4: 清理
├── 删除空的原目录
└── 刷新 AssetDatabase

Phase 5: 验证
├── 编译检查
└── Demo 场景运行验证
```
