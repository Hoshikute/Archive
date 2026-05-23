# Design: 修复 Game 场景 Player 加载

## 问题分析

### TPBattleContext 职责

`TPBattleContext.InitializeGameScene()` 负责：

```csharp
public async UniTask InitializeGameScene()
{
    // 步骤1：设置主相机
    SetupMainCamera();
    
    // 步骤2：动态加载 Player
    await LoadPlayerAsync();
}
```

### 当前流程缺失

```
LoginUI.LoadGameScene()
    │
    ├── GameModule.UI.CloseUI<LoginUI>()
    │
    ├── GameModule.Scene.LoadSceneAsync("Game")  ✓ 场景加载
    │
    └── [缺失] InitializeGameScene()              ✗ Player 未加载
```

## 解决方案

### 修改 LoginUI.cs

```csharp
private async UniTaskVoid LoadGameScene()
{
    Log.Info("[LoginUI] 进入游戏...");

    // 保存选中服务器
    if (_selectedServer != null)
    {
        Game.GameData.ServerAddress = _selectedServer.Address;
        Game.GameData.ServerPort = _selectedServer.Port;
        Game.GameData.PlayerName = _playerName;
    }

    // 关闭登录界面
    GameModule.UI.CloseUI<LoginUI>();

    // 加载 Game 场景
    await GameModule.Scene.LoadSceneAsync("Game");

    // 初始化 Game 场景（设置相机 + 加载 Player）
    await GameModule.TPBattleContext.InitializeGameScene();

    Log.Info("[LoginUI] 进入游戏完成");
}
```

### 正确流程

```
用户点击服务器按钮
    │
    ▼
LoginUI.LoadGameScene()
    │
    ├── 保存服务器信息到 GameData
    │
    ├── CloseUI<LoginUI>()
    │
    ▼
LoadSceneAsync("Game")
    │
    ▼
Game.unity 场景加载完成
    │
    ▼
TPBattleContext.InitializeGameScene()
    │
    ├── SetupMainCamera()
    │   └── 设置主相机
    │
    └── LoadPlayerAsync()
        └── 动态加载 Player Prefab
    │
    ▼
游戏运行
```

## 依赖关系

| 组件 | 说明 |
|------|------|
| `GameModule.Scene` | 场景加载 |
| `GameModule.TPBattleContext` | 游戏场景初始化 |
| `GameModule.Character` | Player 加载 |

## 验证

1. 启动游戏
2. 点击服务器按钮
3. 确认 Game 场景加载
4. 确认 Player 正确加载
