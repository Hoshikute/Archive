# Tasks: 修复 LoginUI 滚动列表与场景流程

## Phase 1: 鼠标可见性修复

- [x] 1.1 修改 GameApp.cs 启动时显示鼠标
  - [x] 1.1.1 在 StartGameLogic() 添加 Cursor.visible = true
  - [x] 1.1.2 设置 Cursor.lockMode = CursorLockMode.None
- [x] 1.2 确认 LoginUI.prefab 不包含隐藏鼠标的逻辑

## Phase 2: ServerItem 组件创建

- [x] 2.1 创建 ServerItem.cs 脚本
  - [x] 2.1.1 定义 UI 组件引用 (Text, Button)
  - [x] 2.1.2 实现 SetData() 方法
  - [x] 2.1.3 实现点击回调
- [x] 2.2 创建服务器列表 UI（改为代码动态创建，不使用独立 prefab）
  - [x] 2.2.1 创建服务器项模板
  - [x] 2.2.2 动态生成服务器按钮

## Phase 3: LoginUI 集成服务器列表

- [x] 3.1 修改 LoginUI.cs（改为动态创建，不修改 prefab）
  - [x] 3.1.1 添加服务器列表容器
  - [x] 3.1.2 实现动态按钮创建
  - [x] 3.1.3 实现服务器选择逻辑
  - [x] 3.1.4 实现选中状态视觉反馈
- [x] 3.2 点击服务器后加载 Game 场景

## Phase 4: 场景加载逻辑

- [x] 4.1 确认 Game.unity 场景位置 (`Assets/AssetRaw/Scenes/Game.unity`)
- [x] 4.2 修改 LoginUI.LoadGameScene()
  - [x] 4.2.1 实现 GameModule.Scene.LoadSceneAsync("Game")
  - [x] 4.2.2 关闭 LoginUI
- [x] 4.3 保存服务器信息到 GameData

## Phase 5: 验证

- [x] 5.1 编译验证
  - [x] 5.1.1 触发 Unity 编译
  - [x] 5.1.2 检查编译错误（应为 0 errors）✓
- [ ] 5.2 功能验证
  - [ ] 5.2.1 启动后鼠标可见
  - [ ] 5.2.2 LoginUI 显示服务器列表
  - [ ] 5.2.3 点击服务器加载 Game 场景
  - [ ] 5.2.4 进入 Game 场景后鼠标隐藏

---

**统计**: 已完成 20/24 个任务，运行验证 4 个

### 实现说明

由于 SuperScrollView 与 HotFix 程序集存在引用问题，改为使用代码动态创建按钮列表：
- 在 LoginUI.cs 中动态创建服务器列表容器和按钮
- 点击服务器按钮后直接加载 Game.unity 场景
- 鼠标在 LoginUI 阶段可见，进入 Game 场景后由 TestWindow 控制隐藏
