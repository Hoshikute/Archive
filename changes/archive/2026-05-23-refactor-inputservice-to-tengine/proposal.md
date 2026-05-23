# Change: InputService 重构到 TEngine 规范

## Why

当前 `InputService` 存在以下问题：

1. **冗余封装**：大部分方法只是简单委托给 `GameModule.Input`，增加不必要的调用链
2. **单例模式不规范**：使用私有静态单例，而非 TEngine 推荐的 `Singleton<T>` 基类
3. **命名空间混乱**：`ThirdPersonController` 命名空间与项目结构不一致
4. **特定逻辑耦合**：`GetMoveHorizontalValue` 等方法包含平台特定逻辑，不应放在通用输入服务中

## What Changes

### 重构方案

**删除 InputService 单例，直接使用 `GameModule.Input`**

将 InputService 中的平台特定逻辑（如 GetMoveHorizontalValue）移到 Player 或 PlayerMovementFsmState 中。

### 修改范围

| 文件 | 修改内容 |
|-----|---------|
| `InputService.cs` | 删除或简化为静态工具类 |
| `Player.cs` | 移除 `InputService` 属性 |
| `PlayerFsmState.cs` | `inputServer` → 直接使用 `GameModule.Input` |
| 各 Player 状态类 | 同上迁移 API 调用 |

### API 映射

| 旧 API | 新 API |
|--------|--------|
| `inputServer.Move` | `GameModule.Input.Move` |
| `inputServer.Look` | `GameModule.Input.Look` |
| `inputServer.GetButtonDown(type)` | `GameModule.Input.GetButtonDown(type)` |
| `inputServer.Shift` | `GameModule.Input.GetButton(InputButtonType.Shift)` |

### 平台特定逻辑处理

`GetMoveHorizontalValue` 等属性包含 `#if UNITY_ANDROID` 条件编译，建议：

1. 移到 `PlayerMovementFsmState` 作为计算属性
2. 或创建独立的 `PlayerMovementUtils` 工具类

## Impact

### 受影响文件

- `Player/Controller/Service/GameService/InputService/InputService.cs`
- `Player/Controller/Core/Player/Player.cs`
- `Player/Controller/Core/StateMachine/State/PlayerFsmState.cs`
- `Player/Controller/Core/StateMachine/State/PlayerMovementFsmState.cs`
- 所有 Player 状态子类（约 15 个文件）

### 风险评估

| 风险 | 影响 | 缓解措施 |
|-----|------|---------|
| API 签名变化 | 所有输入调用点需修改 | 使用全局搜索替换 |
| 平台特定逻辑丢失 | Android 平台行为变化 | 将逻辑移到状态类中 |

## Success Criteria

- [ ] 删除或简化 InputService
- [ ] 所有输入调用迁移到 `GameModule.Input`
- [ ] 编译无错误
- [ ] 玩家输入功能正常（PC 和 Android）
