# Design: 修复 LoginUI 场景流程

## 问题分析

### 当前状态

main.unity 场景中存在以下残留对象：
```
UIRoot/UICanvas/m_panel_server    ← 残留的服务器面板
ServerItem                         ← 残留的服务器项
```

这些是之前测试时创建的，应该由 LoginUI.cs 动态创建。

### 正确流程

```
启动 main.unity
    │
    ▼
GameApp.Entrance()
    │
    ├── 显示鼠标
    │
    ▼
GameModule.UI.ShowUIAsync<LoginUI>()
    │
    ▼
LoginUI.OnCreate()
    │
    ├── 创建服务器列表容器 (m_panel_server)
    ├── 创建服务器按钮 (本地/内网)
    │
    ▼
用户点击服务器按钮
    │
    ▼
LoginUI.LoadGameScene()
    │
    ├── 保存选中服务器到 GameData
    ├── CloseUI<LoginUI>()
    │
    ▼
GameModule.Scene.LoadSceneAsync("Game")
    │
    ▼
Game.unity 场景加载
```

## 解决方案

### 1. 清理 main.unity 场景

需要删除以下对象：
- `UIRoot/UICanvas/m_panel_server`
- `ServerItem`

### 2. 确认代码逻辑

GameApp.cs 启动流程：
```csharp
private static void StartGameLogic()
{
    GameModule.Battle.EnsureInitialized();
    
    // 显示鼠标
    Cursor.visible = true;
    Cursor.lockState = CursorLockMode.None;
    
    // 显示登录界面（不加载 Game 场景）
    GameModule.UI.ShowUIAsync<LoginUI>();
}
```

LoginUI.cs 场景加载：
```csharp
private async UniTaskVoid LoadGameScene()
{
    // 保存服务器信息
    Game.GameData.ServerAddress = _selectedServer.Address;
    Game.GameData.ServerPort = _selectedServer.Port;
    
    // 关闭 LoginUI
    GameModule.UI.CloseUI<LoginUI>();
    
    // 加载 Game 场景
    await GameModule.Scene.LoadSceneAsync("Game");
}
```

### 3. 场景文件位置

| 场景 | 路径 | 用途 |
|------|------|------|
| main.unity | `Assets/main.unity` | 启动场景，LoginUI 显示 |
| Game.unity | `Assets/AssetRaw/Scenes/Game.unity` | 游戏主场景 |

## 验证检查点

### 启动验证

1. 启动后 main.unity 无 Game 相关内容
2. LoginUI 正确显示
3. 服务器列表显示 2 个选项（本地/内网）

### 交互验证

1. 点击本地服务器按钮
2. LoginUI 关闭
3. Game.unity 场景加载
4. 进入 Game 场景后鼠标隐藏
