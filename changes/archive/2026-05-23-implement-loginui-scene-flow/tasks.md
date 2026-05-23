# Tasks: 实现 LoginUI 与场景流程

## Phase 1: Luban 配置生成

- [x] 1.1 检查 ServerData.txt 配置格式
- [x] 1.2 创建 ServerData 解析器（替代 Luban，项目无 Luban 配置）
- [x] 1.3 验证 ServerDataLoader 访问

## Phase 2: 创建 UI Prefab

- [x] 2.1 LoginUI.prefab 已存在（复用现有）
- [x] 2.2 复用现有 UI 节点结构
  - [x] 2.2.1 m_inputAccount 作为玩家名称输入
  - [x] 2.2.2 m_btnLogin 作为进入游戏按钮
- [x] 2.3 Prefab 位于 AssetRaw/UI/LoginUI.prefab

## Phase 3: 实现 LoginUI 逻辑

- [x] 3.1 创建 ServerData 数据类 (Config/ServerData.cs)
- [x] 3.2 重写 LoginUI.cs
  - [x] 3.2.1 添加 [Window] 特性和节点绑定
  - [x] 3.2.2 实现 ScriptGenerator() 节点绑定
  - [x] 3.2.3 实现 RegisterEvent() 注册按钮事件
  - [x] 3.2.4 实现 OnCreate() 初始化逻辑
  - [x] 3.2.5 实现 OnRefresh() 刷新服务器列表
- [x] 3.3 实现按钮事件处理
  - [x] 3.3.1 SelectServer() 选择服务器
  - [x] 3.3.2 GenerateRandomName() 生成随机名字
  - [x] 3.3.3 OnPlayClick() 进入游戏
- [x] 3.4 实现随机名字生成器

## Phase 4: 启动流程调整

- [x] 4.1 检查 GameApp 初始化逻辑
- [x] 4.2 修改为显示 LoginUI（GameApp.cs）
- [x] 4.3 实现场景加载逻辑
  - [x] 4.3.1 保存选中服务器到 GameData
  - [x] 4.3.2 当前场景为 main.unity，无需切换

## Phase 5: 验证

- [x] 5.1 Unity 编译无错误 ✓
- [ ] 5.2 启动后显示 LoginUI (需运行验证)
- [ ] 5.3 服务器列表正确显示 (需运行验证)
- [ ] 5.4 随机名字功能正常 (需运行验证)
- [ ] 5.5 点击进入后加载 Game 场景 (需运行验证)

---

**统计**: 已完成 16 个任务，待运行验证 4 个任务
