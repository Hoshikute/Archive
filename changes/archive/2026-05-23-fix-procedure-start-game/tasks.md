# Tasks: 修复启动流程与鼠标控制

## Phase 1: 修改 ProcedureStartGame.cs

- [x] 1.1 移除 `LoadSceneAsync("Game")` 调用
- [x] 1.2 移除 `TPBattleContext.InitializeGameScene()` 调用
- [x] 1.3 添加注释说明 Game 场景由 LoginUI 加载

## Phase 2: 修改 GameApp.cs

- [x] 2.1 移除 `Cursor.visible = true` 代码
- [x] 2.2 移除 `Cursor.lockState = CursorLockMode.None` 代码
- [x] 2.3 添加注释说明鼠标控制在 LoginUI 中

## Phase 3: 验证

- [x] 3.1 编译验证
  - [x] 3.1.1 检查编译错误（应为 0 errors）✓
- [ ] 3.2 运行验证
  - [ ] 3.2.1 启动后只显示 LoginUI
  - [ ] 3.2.2 鼠标可见
  - [ ] 3.2.3 Game 场景未被加载
  - [ ] 3.2.4 点击服务器后加载 Game 场景

---

**统计**: 已完成 7/10 个任务，运行验证 4 个

### 完成的修改

| 文件 | 修改内容 |
|------|---------|
| `ProcedureStartGame.cs` | 移除 Game 场景加载，只保留相机清理 |
| `GameApp.cs` | 移除鼠标显示代码，由 LoginUI 控制 |
