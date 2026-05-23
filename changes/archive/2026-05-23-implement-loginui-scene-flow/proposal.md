# Change: 实现 LoginUI 与场景流程

## Why

当前项目缺少登录界面，启动后直接进入 Game 场景，无法选择服务器。需要实现：

1. **启动流程不完整**：没有登录/选服界面，直接进入游戏
2. **服务器配置未使用**：`ServerData.txt` 已存在但无 UI 展示
3. **网络模块未连接**：网络模块需要用户选择服务器后才启动

## What Changes

### 1. LoginUI 界面实现

| 功能 | 描述 |
|------|------|
| 服务器列表 | 显示可用服务器（本地/内网） |
| 随机名字 | 生成随机玩家名称 |
| 进入游戏 | 连接选中的服务器并加载场景 |

### 2. 启动流程调整

```
当前流程: 启动 → Game 场景

目标流程: 启动 → LoginUI → 选择服务器 → 连接网络 → Game 场景
```

### 3. 场景结构调整

| 场景 | 用途 |
|------|------|
| Launch | 启动场景，初始化框架，显示 LoginUI |
| Game | 游戏主场景，包含玩家、地图等 |

## Impact

### 新增文件

- `Assets/GameScripts/HotFix/GameLogic/UI/LoginUI/LoginUI.cs` (重写)
- `Assets/AssetRaw/UI/Prefabs/LoginUI.prefab` (UI Prefab)

### 修改文件

- 场景启动逻辑（调整初始化顺序）
- 网络模块连接时机（由 LoginUI 触发）

### 依赖项

- ServerData 配置（已存在）
- NetworkModule（需先完成注册）
- Luban 配置代码生成

## Success Criteria

- [ ] 启动后显示 LoginUI 界面
- [ ] 服务器列表正确显示（本地/内网）
- [ ] 点击服务器后能加载 Game 场景
- [ ] 随机名字功能正常工作
- [ ] 编译无错误
