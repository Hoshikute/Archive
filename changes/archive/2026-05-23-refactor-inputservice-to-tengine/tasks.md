# Tasks: InputService 重构到 TEngine 规范

## Phase 1: 迁移核心类

### 1.1 迁移 Player.cs
- [x] 移除 `InputService` 属性定义
- [x] 移除 `InputService.Instance` 初始化

### 1.2 迁移 PlayerFsmState.cs
- [x] 移除 `inputServer` 字段
- [x] 移除 `inputServer` 初始化

### 1.3 迁移 PlayerMovementFsmState.cs
- [x] 将 `inputServer.Move` 改为 `GameModule.Input.Move`
- [x] 将 `inputServer.Shift` 改为 `GameModule.Input.GetButton(InputButtonType.Shift)`
- [x] 将 `inputServer.GetButtonDown(...)` 改为 `GameModule.Input.GetButtonDown(...)`
- [x] 添加 `GetMoveHorizontalValue` 扩展属性（平台特定逻辑）

## Phase 2: 迁移状态类

### 2.1 迁移 PlayerIdleState.cs
- [x] `inputServer.GetButtonDown(InputButtonType.Jump)` → `GameModule.Input.GetButtonDown(...)`
- [x] `inputServer.GetButtonDown(InputButtonType.Crouch)` → 同上
- [x] `inputServer.Move` → `GameModule.Input.Move`

### 2.2 迁移 PlayerMoveLoopState.cs
- [x] `inputServer.GetButtonDown(InputButtonType.Jump)` → `GameModule.Input.GetButtonDown(...)`
- [x] `inputServer.GetButtonDown(InputButtonType.Crouch)` → 同上
- [x] `inputServer.Move` → `GameModule.Input.Move`

### 2.3 迁移 PlayerMoveStartState.cs
- [x] `inputServer.GetButtonDown(InputButtonType.Jump)` → `GameModule.Input.GetButtonDown(...)`
- [x] `inputServer.GetButtonDown(InputButtonType.Crouch)` → 同上
- [x] `inputServer.Move` → `GameModule.Input.Move`

### 2.4 迁移 PlayerMoveEndState.cs
- [x] `inputServer.GetButtonDown(InputButtonType.Jump)` → `GameModule.Input.GetButtonDown(...)`
- [x] `inputServer.GetButtonDown(InputButtonType.Crouch)` → 同上
- [x] `inputServer.Move` → `GameModule.Input.Move`

### 2.5 迁移其他状态类
- [x] `PlayerJumpState.cs` - `inputServer.Move`
- [x] `PlayerClimbState.cs` - `inputServer.Move`
- [x] `PlayerLedgeClimbState.cs` - `inputServer.Move`, `inputServer.GetButtonDown`
- [x] `PlayerOutPlaceJumpState.cs` - `inputServer.Move`
- [x] `PlayerMoveToWallState.cs` - `inputServer.Move`
- [x] `PlayerPlatformerUpState.cs` - 检查是否有输入调用
- [x] `PlayerFallLoopState.cs` - 检查是否有输入调用
- [x] `PlayerLandState.cs` - 检查是否有输入调用
- [x] `PlayerLockIdleState.cs` - 检查是否有输入调用

### 2.6 迁移 StateBase.cs（遗留代码）
- [x] 移除 `inputServer` 字段
- [x] 移除 `inputServer` 初始化

## Phase 3: 清理

### 3.1 处理 InputService.cs
- [x] 检查是否还有其他引用
- [x] 删除文件或保留为静态工具类

### 3.2 验证
- [x] 运行 Unity 编译，确保无错误
- [ ] 测试玩家移动功能
- [ ] 测试玩家跳跃功能
- [ ] 测试玩家蹲伏功能
- [ ] 测试玩家冲刺功能

---

## 汇总

| 阶段 | 任务数 | 说明 |
|-----|-------|------|
| Phase 1 | 3 | 迁移核心类 |
| Phase 2 | 6 | 迁移状态类 |
| Phase 3 | 2 | 清理验证 |
| **总计** | **11** | |
