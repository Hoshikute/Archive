# Tasks: 修复 Game 场景 Player 加载

## Phase 1: 修改 LoginUI.cs

- [x] 1.1 在 `LoadGameScene()` 中添加 `InitializeGameScene()` 调用
  - [x] 1.1.1 在 `LoadSceneAsync("Game")` 后添加 `await GameModule.TPBattleContext.InitializeGameScene()`

## Phase 2: 验证

- [x] 2.1 编译验证
  - [x] 2.1.1 检查编译错误（应为 0 errors）✓
- [ ] 2.2 运行验证
  - [ ] 2.2.1 启动后显示 LoginUI
  - [ ] 2.2.2 点击服务器加载 Game 场景
  - [ ] 2.2.3 Player 正确加载

---

**统计**: 已完成 3/4 个任务，运行验证 3 个
