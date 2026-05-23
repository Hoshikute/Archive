# Change: 修复启动流程与鼠标控制

## Why

当前存在多个问题：

1. **Game 场景提前加载**：`ProcedureStartGame.StartGame()` 自动加载 Game 场景
2. **鼠标控制位置不当**：鼠标显示代码在 GameApp.cs 中，应该在 LoginUI 中
3. **业务逻辑分散**：启动相关业务代码应该集中在 LoginUI

## What Changes

### 1. 修改 ProcedureStartGame.cs

移除 Game 场景加载逻辑：

```csharp
// 修改后
private async UniTaskVoid StartGame()
{
    await UniTask.Yield();
    LauncherMgr.HideAllUI();
    GameModule.Camera.CleanupExtraCameras();
    // Game 场景由 LoginUI 点击服务器后加载
}
```

### 2. 修改 GameApp.cs

移除鼠标显示代码：

```csharp
// 修改后
private static void StartGameLogic()
{
    GameModule.Battle.EnsureInitialized();
    // 显示登录界面（鼠标控制在 LoginUI 中）
    GameModule.UI.ShowUIAsync<LoginUI>();
}
```

### 3. 修改 LoginUI.cs

鼠标显示逻辑已经在 `OnCreate()` 中：

```csharp
protected override void OnCreate()
{
    // 显示鼠标
    Cursor.visible = true;
    Cursor.lockState = CursorLockMode.None;
    // ...
}
```

### 正确流程

```
ProcedureStartGame
    │
    ├── 清理多余相机
    │
    ▼
GameApp.Entrance() → LoginUI 显示
    │
    ▼
LoginUI.OnCreate()
    │
    ├── 显示鼠标
    ├── 创建服务器列表
    │
    ▼
用户点击服务器按钮
    │
    ▼
LoginUI.LoadGameScene() → 加载 Game 场景
```

## Impact

### 修改文件

| 文件 | 修改内容 |
|------|---------|
| `ProcedureStartGame.cs` | 移除 Game 场景加载逻辑 |
| `GameApp.cs` | 移除鼠标显示代码 |

## Success Criteria

- [ ] 启动后只显示 LoginUI
- [ ] Game 场景不被自动加载
- [ ] 鼠标在 LoginUI 阶段可见
- [ ] 点击服务器后才加载 Game 场景
