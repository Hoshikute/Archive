# Design: 修复 LoginUI 滚动列表与场景流程

## 架构概览

```
启动流程
──────────────────────────────────────────────────────
main.unity (启动场景)
    │
    ▼
GameApp.Entrance()
    │
    ├── 初始化模块
    │
    ▼
GameModule.UI.ShowUIAsync<LoginUI>()
    │
    ├── 鼠标可见 ✓
    │
    ▼
LoginUI (服务器选择)
    │
    ├── LoopListView2 显示服务器列表
    ├── 用户选择服务器
    │
    ▼ (点击服务器项)
    │
    ├── 保存选中服务器
    ├── 加载 Game.unity 场景
    │
    ▼
Game.unity (游戏场景)
    │
    ├── TestWindow 显示
    ├── 鼠标隐藏
    │
    ▼
游戏运行
```

## 目录结构

```
Assets/
├── AssetRaw/
│   ├── Scenes/
│   │   └── Game.unity              # 游戏主场景
│   └── UI/
│       └── Prefabs/
│           ├── LoginUI.prefab      # 登录 UI (修改)
│           └── ServerItem.prefab   # 服务器列表项 (新建)
│
├── Scenes/
│   └── main.unity                  # 启动场景
│
└── GameScripts/HotFix/GameLogic/
    └── UI/LoginUI/
        ├── LoginUI.cs              # 登录逻辑 (修改)
        └── ServerItem.cs           # 列表项脚本 (新建)
```

## LoginUI 设计

### UI 层级结构

```
LoginUI (Canvas)
├── m_bg                              # 背景图
├── m_title                           # 标题 "选择服务器"
├── m_scroll_server                   # ScrollRect 容器
│   └── LoopListView2                 # SuperScrollView 组件
│       └── ServerItem (Template)     # 服务器项模板
│           ├── m_txt_name            # 服务器名称
│           ├── m_txt_address         # 服务器地址
│           └── m_btn_select          # 选择按钮
├── m_panel_player                    # 玩家面板
│   ├── m_inputAccount                # 玩家名称输入
│   └── m_btn_random_name             # 随机名字按钮
└── m_btn_play                        # 进入游戏按钮 (保留备用)
```

### 类设计

```csharp
[Window(UILayer.UI, "LoginUI")]
public class LoginUI : UIWindow
{
    // SuperScrollView
    private LoopListView2 m_loopListView;
    private ServerItem _selectedItem;

    // 生命周期
    protected override void ScriptGenerator();
    protected override void RegisterEvent();
    protected override void OnCreate();
    protected override void OnDestroy();

    // LoopListView2 回调
    private LoopListViewItem2 OnGetItemByIndex(LoopListView2 listView, int index);
    private void OnServerItemClick(ServerItem item);

    // 场景加载
    private async UniTaskVoid LoadGameScene();

    // 鼠标控制
    private void ShowCursor();
}
```

## ServerItem 设计

### 类设计

```csharp
public class ServerItem : MonoBehaviour
{
    [SerializeField] private Text m_txtName;
    [SerializeField] private Text m_txtAddress;
    [SerializeField] private Button m_btnSelect;

    private ServerData _data;
    private Action<ServerItem> _onSelect;

    public void SetData(ServerData data, Action<ServerItem> onSelect);
    public void SetSelected(bool selected);
}
```

### Prefab 结构

```
ServerItem (GameObject)
├── m_txt_name          # Text - 服务器名称
├── m_txt_address       # Text - 服务器地址
└── m_btn_select        # Button - 选择按钮
```

## SuperScrollView 集成

### LoopListView2 配置

```csharp
// 初始化
m_loopListView.InitListView(
    ServerDataLoader.ServerList.Count,  // Item 总数
    OnGetItemByIndex                     // Item 创建回调
);

// Item 创建回调
private LoopListViewItem2 OnGetItemByIndex(LoopListView2 listView, int index)
{
    var item = listView.NewListViewItem("ServerItem");
    var serverItem = item.GetComponent<ServerItem>();
    serverItem.SetData(ServerDataLoader.ServerList[index], OnServerItemClick);
    return item;
}
```

### 命名空间

```csharp
using SuperScrollView;
```

## 场景加载流程

### 修改前

```csharp
// GameApp.cs
private static void StartGameLogic()
{
    GameModule.Battle.EnsureInitialized();
    GameModule.UI.ShowUIAsync<LoginUI>();
}
```

### 修改后

```csharp
// GameApp.cs - 确保 main 场景不加载 Game 内容
private static void StartGameLogic()
{
    // 显示鼠标
    Cursor.visible = true;
    Cursor.lockState = CursorLockMode.None;
    
    GameModule.UI.ShowUIAsync<LoginUI>();
}

// LoginUI.cs - 点击服务器后加载 Game 场景
private async UniTaskVoid LoadGameScene()
{
    // 关闭 LoginUI
    GameModule.UI.CloseUI<LoginUI>();
    
    // 加载 Game 场景
    await GameModule.Scene.LoadSceneAsync("Game");
    
    // 进入场景后隐藏鼠标（由 TestWindow 控制）
}
```

## 鼠标控制策略

### 状态管理

| 阶段 | Cursor.visible | Cursor.lockState |
|------|---------------|------------------|
| main 场景 (LoginUI) | true | None |
| Game 场景 (TestWindow) | false | Locked |
| Game 场景 (按 Alt) | true | None |

### 实现位置

```csharp
// LoginUI.OnCreate() - 显示鼠标
Cursor.visible = true;
Cursor.lockState = CursorLockMode.None;

// TestWindow.OnCreate() - 隐藏鼠标
ApplyCursorState(false);
```

## 依赖项

| 依赖 | 状态 | 说明 |
|------|------|------|
| SuperScrollView | ✓ 已安装 | `Assets/Plugins/SuperScrollView/` |
| ServerDataLoader | ✓ 已存在 | `Config/ServerData.cs` |
| Game.unity | ✓ 已存在 | `Assets/AssetRaw/Scenes/Game.unity` |
| LoginUI.prefab | ✓ 已存在 | `Assets/AssetRaw/UI/LoginUI.prefab` |

## 实现顺序

```
Phase 1: 鼠标修复
├── GameApp.Entrance() 添加鼠标显示
└── 确认 LoginUI 显示时鼠标可见

Phase 2: ServerItem 创建
├── 创建 ServerItem.cs
├── 创建 ServerItem.prefab
└── 配置 UI 节点

Phase 3: LoginUI 集成 LoopListView2
├── 添加 LoopListView2 节点
├── 实现 OnGetItemByIndex 回调
├── 实现服务器列表显示
└── 实现点击选择逻辑

Phase 4: 场景加载
├── 实现 LoadGameScene()
├── 加载 Game.unity 场景
└── 验证场景切换

Phase 5: 验证
├── 编译检查
├── 运行验证
└── 鼠标状态验证
```
