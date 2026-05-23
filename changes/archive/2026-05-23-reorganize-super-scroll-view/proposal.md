# Change: 整理 SuperScrollView 无限滚动组件目录结构

## Why

当前 SuperScrollView 直接放置在 `Assets/SuperScrollView/` 根目录下，不符合 TEngine 项目规范：

1. **目录位置不规范**：第三方 UI 组件库应放在 `Assets/Plugins/` 下
2. **示例代码混合**：Demo 目录与核心代码混在一起，影响项目整洁
3. **命名不符合 Unity Package 规范**：核心代码目录 `Scripts/` 应改为 `Runtime/`

## What Changes

### 1. 目录结构调整

| 当前结构 | 目标结构 |
|---------|---------|
| `Assets/SuperScrollView/` | `Assets/Plugins/SuperScrollView/` |
| `Scripts/` (核心代码) | `Runtime/` (符合 Unity Package 规范) |
| `Demo/` (示例) | `Assets/Demos/SuperScrollViewDemo/` |

### 2. 目标目录结构

```
Assets/
├── Plugins/
│   └── SuperScrollView/              # 核心库（移动后）
│       ├── Editor/                   # 编辑器扩展
│       ├── Runtime/                  # 核心代码（原 Scripts/）
│       └── package.json              # 版本信息
│
└── Demos/
    └── SuperScrollViewDemo/          # 示例（新建）
        ├── Prefabs/
        ├── Scenes/
        ├── Scripts/
        └── Texture/
```

### 3. 文件移动清单

| 操作 | 源路径 | 目标路径 |
|------|--------|---------|
| 移动 | `Assets/SuperScrollView/Scripts/` | `Assets/Plugins/SuperScrollView/Runtime/` |
| 移动 | `Assets/SuperScrollView/Editor/` | `Assets/Plugins/SuperScrollView/Editor/` |
| 移动 | `Assets/SuperScrollView/Demo/` | `Assets/Demos/SuperScrollViewDemo/` |
| 保留 | `Assets/SuperScrollView/Document.pdf` | `Assets/Plugins/SuperScrollView/Document.pdf` |
| 删除 | `Assets/SuperScrollView/` (空目录) | — |

## Impact

### 影响范围

- **编译影响**：移动后需要重新编译，确保命名空间正确
- **引用影响**：如有代码引用 SuperScrollView，需检查命名空间
- **Prefab 影响**：Demo 中的 Prefab 可能需要重新关联脚本

### 风险评估

| 风险 | 级别 | 缓解措施 |
|------|------|---------|
| 脚本引用丢失 | 低 | 使用 Unity 的 AssetDatabase.MoveAsset 保持引用 |
| Demo Prefab 脚本丢失 | 中 | 移动后检查 Prefab 关联 |
| Meta 文件变化 | 低 | Unity 会自动重新生成 |

## Success Criteria

- [ ] SuperScrollView 核心代码移动到 `Assets/Plugins/SuperScrollView/Runtime/`
- [ ] Editor 扩展移动到 `Assets/Plugins/SuperScrollView/Editor/`
- [ ] Demo 移动到 `Assets/Demos/SuperScrollViewDemo/`
- [ ] 原目录清理干净
- [ ] 编译无错误
- [ ] Demo 场景可正常运行
