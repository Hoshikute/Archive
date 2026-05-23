# Tasks: Timer 模块迁移到 TEngine

## Phase 1: 迁移引用代码

### 1.1 迁移 Player.cs
- [x] 移除 `TimerService` 属性定义
- [x] 移除 `TimerService.Instance` 初始化
- [x] 检查是否有其他 TimerService 直接引用

### 1.2 迁移 PlayerFsmState.cs
- [x] 移除 `timerService` 字段
- [x] 确认 `SwitchState` 方法不依赖 Timer

### 1.3 迁移 PlayerMovementFsmState.cs
- [x] `timerService.AddTimer(50, ...)` → `GameModule.Timer.AddTimer(..., 0.05f)`
- [x] 存储 timerId 用于后续移除
- [x] 在 `OnLeave` 中调用 `RemoveTimer`

### 1.4 迁移 PlayerIdleState.cs
- [x] 添加 `private int _delayTid;` 字段
- [x] `timerService.AddTimer(50, ...)` → `GameModule.Timer.AddTimer(..., 0.05f)`
- [x] 在 `RemoveEventListening` 中调用 `GameModule.Timer.RemoveTimer(_delayTid)`

### 1.5 迁移 PlayerMoveLoopState.cs
- [x] `_moveLoopTid` 改用 `GameModule.Timer`
- [x] `timerService.AddTimer` → `GameModule.Timer.AddTimer`
- [x] `timerService.RemoveTimer` → `GameModule.Timer.RemoveTimer`

### 1.6 迁移 PlayerMoveStartState.cs
- [x] `_startMoveTid` 改用 `GameModule.Timer`
- [x] 同上迁移 API 调用

### 1.7 迁移 PlayerMoveEndState.cs
- [x] 添加 timerId 字段
- [x] `timerService.AddTimer(50, ...)` → `GameModule.Timer.AddTimer(..., 0.05f)`
- [x] 在 `RemoveEventListening` 中移除定时器

### 1.8 迁移 GameObjectManager.cs
- [x] 移除 `using GameLogic.Common.Timer;`
- [x] `Timer.DelayCallBack(time, ...)` → `GameModule.Timer.AddTimer(..., time)`
- [x] 两处调用：第 168 行和第 373 行

## Phase 2: 删除旧 Timer 文件

### 2.1 删除 Common/Timer 目录
- [x] 确认无引用后删除 `Common/Timer/Timer.cs`
- [x] 确认无引用后删除 `Common/Timer/TimerEvent.cs`
- [x] 删除 `Common/Timer/` 目录

### 2.2 删除 TimerService 目录
- [x] 删除 `Player/Controller/Service/GameService/TimerService/TimerService.cs`
- [x] 删除 `Player/Controller/Service/GameService/TimerService/TickTimer.cs`
- [x] 删除 `Player/Controller/Service/GameService/TimerService/GameTimerBase.cs`
- [x] 删除 `TimerService/` 目录

## Phase 3: 验证

### 3.1 编译验证
- [x] 运行 Unity 编译，确保无错误
- [x] 检查 IDE 无红色波浪线

### 3.2 功能验证
- [ ] 测试玩家移动状态切换
- [ ] 测试对象池延迟销毁
- [ ] 测试现有 UI 定时刷新（已使用 GameModule.Timer）

---

## 汇总

| 阶段 | 任务数 | 完成数 |
|-----|-------|-------|
| Phase 1 | 8 | 8 |
| Phase 2 | 2 | 2 |
| Phase 3 | 2 | 0 |
| **总计** | **12** | **10** |
