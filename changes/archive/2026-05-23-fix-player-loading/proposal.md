# Change: 修复 Game 场景 Player 加载

## Why

进入 Game 场景后 Player 没有加载出来：

1. **Player 加载代码缺失**：`TPBattleContext.InitializeGameScene()` 没有被调用
2. **流程不完整**：`LoginUI.LoadGameScene()` 只加载场景，没有初始化游戏内容

## What Changes

### 修改 LoginUI.cs

在 `LoadGameScene()` 中添加 `InitializeGameScene()` 调用：

```csharp
private async UniTaskVoid LoadGameScene()
{
    // ... 保存服务器信息 ...
    
    GameModule.UI.CloseUI<LoginUI>();
    
    // 加载 Game 场景
    await GameModule.Scene.LoadSceneAsync("Game");
    
    // 初始化 Game 场景（设置相机 + 加载 Player）
    await GameModule.TPBattleContext.InitializeGameScene();
    
    Log.Info("[LoginUI] 进入游戏完成");
}
```

### 完整流程

```
LoginUI.LoadGameScene()
    │
    ├── 保存服务器信息
    │
    ├── CloseUI<LoginUI>()
    │
    ├── LoadSceneAsync("Game")
    │
    └── TPBattleContext.InitializeGameScene()
            │
            ├── SetupMainCamera()
            │
            └── LoadPlayerAsync()
```

## Impact

### 修改文件

| 文件 | 修改内容 |
|------|---------|
| `LoginUI.cs` | 添加 `InitializeGameScene()` 调用 |

## Success Criteria

- [ ] 进入 Game 场景后 Player 正确加载
- [ ] 主相机正确设置
- [ ] 编译无错误
