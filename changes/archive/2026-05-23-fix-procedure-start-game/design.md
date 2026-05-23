# Design: 修复 ProcedureStartGame 场景加载

## 问题分析

### 当前启动流程

```
ProcedureLoadAssembly.AllAssemblyLoadComplete()
    │
    ├── ChangeState<ProcedureStartGame>()
    │       │
    │       ▼
    │   ProcedureStartGame.StartGame()
    │       │
    │       ├── LauncherMgr.HideAllUI()
    │       ├── GameModule.Camera.CleanupExtraCameras()
    │       ├── GameModule.Scene.LoadSceneAsync("Game")     ← 问题：自动加载 Game 场景
    │       └── GameModule.TPBattleContext.InitializeGameScene()
    │
    └── GameApp.Entrance()
            │
            └── GameModule.UI.ShowUIAsync<LoginUI>()
                    │
                    └── 用户看到 LoginUI（但 Game 场景已加载）
```

### 问题

`ProcedureStartGame.StartGame()` 和 `GameApp.Entrance()` 并行执行，导致 Game 场景被提前加载。

## 解决方案

### 修改 ProcedureStartGame.cs

移除 Game 场景加载逻辑，只保留相机清理：

```csharp
private async UniTaskVoid StartGame()
{
    await UniTask.Yield();
    LauncherMgr.HideAllUI();

    // 场景加载前清理多余相机
    GameModule.Camera.CleanupExtraCameras();

    // Game 场景由 LoginUI 点击服务器后加载
    // 不在此处加载
}
```

### 正确的流程

```
启动
    │
    ▼
ProcedureStartGame.StartGame()
    │
    ├── 清理多余相机
    │
    ▼
GameApp.Entrance()
    │
    ├── 显示鼠标
    │
    ▼
GameModule.UI.ShowUIAsync<LoginUI>()
    │
    ├── 用户选择服务器
    │
    ▼
LoginUI.LoadGameScene()
    │
    ├── GameModule.UI.CloseUI<LoginUI>()
    │
    ▼
GameModule.Scene.LoadSceneAsync("Game")
    │
    ▼
Game 场景加载完成
```

## 代码修改

### ProcedureStartGame.cs

```csharp
// 文件：Assets/GameScripts/Procedure/ProcedureStartGame.cs

private async UniTaskVoid StartGame()
{
    await UniTask.Yield();
    LauncherMgr.HideAllUI();

    // 场景加载前清理多余相机
    GameModule.Camera.CleanupExtraCameras();

    // Game 场景由 LoginUI 点击服务器后加载
    // LoginUI.LoadGameScene() 负责场景加载和初始化
}
```

## 验证

1. 启动游戏
2. 确认只显示 LoginUI
3. 确认 Game 场景未加载
4. 点击服务器按钮
5. 确认 Game 场景被加载
