# Tasks: 修复 LoginUI 场景流程

## Phase 1: 清理 main.unity 场景

- [x] 1.1 删除残留 UI 元素
  - [x] 1.1.1 删除 `UIRoot/UICanvas/m_panel_server`
  - [x] 1.1.2 删除 `ServerItem`
- [x] 1.2 保存 main.unity 场景

## Phase 2: 验证代码逻辑

- [x] 2.1 确认 GameApp.cs 不加载 Game 场景
- [x] 2.2 确认 LoginUI.cs 点击后加载 Game 场景

## Phase 3: 验证

- [x] 3.1 编译验证
  - [x] 3.1.1 检查编译错误（应为 0 errors）✓
- [ ] 3.2 运行验证
  - [ ] 3.2.1 启动后显示 LoginUI
  - [ ] 3.2.2 服务器列表显示（本地/内网）
  - [ ] 3.2.3 点击服务器加载 Game 场景

---

**统计**: 已完成 6/9 个任务，运行验证 3 个

### 完成的修改

- 删除了 main.unity 场景中的残留 UI 元素：
  - `UIRoot/UICanvas/m_panel_server`
  - `ServerItem`
- 保存了 main.unity 场景
- 确认代码逻辑正确
