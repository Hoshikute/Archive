# Change: 迁移 Timer 模块到 TEngine 标准实现

## Why

项目中存在两套 Timer 系统，造成代码冗余和维护负担：

1. **迁移的 Timer 模块**：`GameLogic.Common.Timer` + `ThirdPersonController.TickTimer`
2. **TEngine Timer**：`GameModule.Timer` - 框架标准实现

两套系统并存导致：
- 代码重复，增加维护成本
- API 不一致，增加学习成本
- 资源浪费（两个独立的更新循环）

## What Changes

### 删除重复文件

| 文件路径 | 说明 |
|---------|------|
| `GameLogic/Common/Timer/Timer.cs` | 旧定时器主类 |
| `GameLogic/Common/Timer/TimerEvent.cs` | 旧定时器事件 |
| `GameLogic/Player/.../TimerService/TimerService.cs` | MonoSingleton 定时器服务 |
| `GameLogic/Player/.../TimerService/TickTimer.cs` | 毫秒级线程定时器 |
| `GameLogic/Player/.../TimerService/GameTimerBase.cs` | 定时器基类 |

### 迁移引用代码

| 文件路径 | 修改内容 |
|---------|---------|
| `GameObjectManager.cs` | `Timer.DelayCallBack` → `GameModule.Timer.AddTimer` |
| `Player.cs` | 移除 `TimerService` 依赖 |
| `PlayerMovementFsmState.cs` | `timerService.AddTimer` → `GameModule.Timer.AddTimer` |
| `PlayerIdleState.cs` | 同上 |
| `PlayerMoveLoopState.cs` | 同上 |
| `PlayerMoveStartState.cs` | 同上 |
| `PlayerMoveEndState.cs` | 同上 |

## Impact

### API 映射

| 旧 API | TEngine API |
|--------|------------|
| `Timer.DelayCallBack(time, callback)` | `GameModule.Timer.AddTimer(callback, time)` |
| `Timer.CallBackOfIntervalTimer(time, callback)` | `GameModule.Timer.AddTimer(callback, time, isLoop: true)` |
| `Timer.DestroyTimer(timer)` | `GameModule.Timer.RemoveTimer(timerId)` |
| `TimerService.AddTimer(ms, callback)` | `GameModule.Timer.AddTimer(callback, ms/1000f)` |
| `TimerService.RemoveTimer(tid)` | `GameModule.Timer.RemoveTimer(timerId)` |

### 注意事项

1. **时间单位转换**：旧 TimerService 使用毫秒，TEngine Timer 使用秒
2. **回调签名**：旧 Timer 使用 `Action<object[]>`，TEngine 使用 `Action<Timer>` 或 `Action`
3. **循环模式**：旧 Timer 通过 `count` 参数控制，TEngine 通过 `isLoop` 参数

## Success Criteria

- [ ] 删除所有重复 Timer 文件
- [ ] 所有引用迁移到 `GameModule.Timer`
- [ ] 编译无错误
- [ ] 现有功能不受影响（玩家状态机、对象池延迟销毁等）
