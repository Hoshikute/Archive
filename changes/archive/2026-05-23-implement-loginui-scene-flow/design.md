# Design: 实现 LoginUI 与场景流程

## 架构概览

```
启动流程
──────────────────────────────────────────────────────
Launch Scene
    │
    ▼
GameApp (HotFix 入口)
    │
    ├── 初始化模块
    │
    ▼
GameModule.UI.ShowUIAsync<LoginUI>()
    │
    ▼
LoginUI (选服界面)
    │
    ├── 显示服务器列表 (从 ServerData 配置读取)
    ├── 随机名字生成
    │
    ▼ (用户点击进入)
    │
    ├── 保存选中服务器信息
    ├── 启动网络连接 (可选)
    │
    ▼
GameModule.Scene.LoadSceneAsync("Game")
    │
    ▼
Game Scene (游戏主场景)
```

## 目录结构

```
Assets/
├── AssetRaw/
│   ├── UI/
│   │   └── Prefabs/
│   │       └── LoginUI.prefab        # UI Prefab (需创建)
│   └── FrameSync/
│       └── Configs/
│           └── Data/
│               └── ServerData.txt    # 服务器配置 (已存在)
│
└── GameScripts/
    └── HotFix/
        └── GameLogic/
            └── UI/
                └── LoginUI/
                    └── LoginUI.cs    # UI 逻辑 (重写)
```

## LoginUI 设计

### UI 层级结构

```
LoginUI (Canvas)
├── m_bg                              # 背景图
├── m_title                           # 标题
├── m_panel_server                    # 服务器面板
│   ├── m_btn_local                   # 本地服务器按钮
│   └── m_btn_lan                     # 内网服务器按钮
├── m_panel_player                    # 玩家面板
│   ├── m_txt_name                    # 玩家名称
│   └── m_btn_random_name             # 随机名字按钮
└── m_btn_play                        # 进入游戏按钮
```

### 类设计

```csharp
[Window(UILayer.UI, "LoginUI")]
public class LoginUI : UIWindow
{
    // UI 节点绑定 (ScriptGenerator)
    private Button m_btn_local;
    private Button m_btn_lan;
    private Button m_btn_random_name;
    private Button m_btn_play;
    private Text m_txt_name;

    // 状态
    private ServerInfo _selectedServer;
    private string _playerName;

    // 生命周期
    protected override void ScriptGenerator();
    protected override void RegisterEvent();
    protected override void OnCreate();
    protected override void OnRefresh();

    // 事件处理
    private void OnLocalServerClick();
    private void OnLanServerClick();
    private void OnRandomNameClick();
    private void OnPlayClick();
}
```

### 服务器信息数据结构

```csharp
public class ServerInfo
{
    public string Id;        // 服务器 ID
    public string Address;   // IP 地址
    public int Port;         // 端口
    public string Name;      // 显示名称
}
```

## 场景流程设计

### Launch 场景

负责框架初始化，然后显示 LoginUI：

```csharp
// GameApp.cs (HotFix 入口)
public class GameApp : MonoBehaviour
{
    async UniTaskVoid Start()
    {
        await InitializeModules();
        await GameModule.UI.ShowUIAsync<LoginUI>();
    }
}
```

### Game 场景

游戏主场景，包含：
- Player (玩家控制器)
- 地图
- 游戏逻辑模块

## Luban 配置集成

### ServerData 配置

当前 `ServerData.txt` 内容：

```
id	Address	port	name
1	192.168.89.146	7500	内网
Address	127.0.0.1	7500	本地
```

### Luban 生成代码 (需生成)

```csharp
// TServerData 表访问
var serverList = Tables.Instance.TbServerData.DataList;
foreach (var server in serverList)
{
    // server.Id, server.Address, server.Port, server.Name
}
```

## 网络连接流程

```
LoginUI.OnPlayClick()
    │
    ├── 保存选中服务器到 GameData
    │   GameData.ServerAddress = _selectedServer.Address
    │   GameData.ServerPort = _selectedServer.Port
    │   GameData.PlayerName = _playerName
    │
    ├── (可选) 启动网络连接
    │   await GameModule.Network.ConnectAsync(...)
    │
    ▼
GameModule.Scene.LoadSceneAsync("Game")
```

## 按钮命名规范

| 按钮 | 命名 | 前缀规范 |
|------|------|---------|
| 本地服务器 | m_btn_local | btn + 下划线 |
| 内网服务器 | m_btn_lan | btn + 下划线 |
| 随机名字 | m_btn_random_name | btn + 下划线 |
| 进入游戏 | m_btn_play | btn + 下划线 |
| 名称文本 | m_txt_name | txt + 下划线 |

## 依赖项

| 依赖 | 状态 | 说明 |
|------|------|------|
| ServerData.txt | ✓ 已存在 | 需生成 Luban 代码 |
| NetworkModule | ⚠ 未注册 | 需先修复命名空间 |
| LoginUI.cs | 空壳 | 需重写 |
| LoginUI.prefab | ✗ 不存在 | 需创建 |

## 实现顺序

```
Phase 1: Luban 配置
├── 生成 ServerData Luban 代码
└── 验证配置读取

Phase 2: UI Prefab
├── 创建 LoginUI.prefab
└── 配置节点结构

Phase 3: LoginUI 逻辑
├── 实现 LoginUI.cs
├── 服务器列表显示
├── 随机名字生成
└── 场景加载逻辑

Phase 4: 启动流程
├── 调整 GameApp 初始化
└── 验证完整流程
```
