# Design: InputService 重构到 TEngine 规范

## 设计目标

消除冗余封装层，直接使用 TEngine 的 `GameModule.Input`，同时保留平台特定逻辑。

## TEngine InputModule API 参考

```csharp
// 状态属性
GameModule.Input.Move           // Vector2 移动输入
GameModule.Input.Look           // Vector2 视角输入
GameModule.Input.Scroll         // Vector2 滚轮输入

// 按键检测
GameModule.Input.GetButtonDown(InputButtonType buttonType)
GameModule.Input.GetButton(InputButtonType buttonType)
GameModule.Input.GetButtonUp(InputButtonType buttonType)

// 按键类型枚举
InputButtonType.Move
InputButtonType.Look
InputButtonType.Attack
InputButtonType.Interact
InputButtonType.Crouch
InputButtonType.Jump
InputButtonType.Sprint
InputButtonType.Interactive
InputButtonType.Shift
InputButtonType.Scroll
InputButtonType.Lock
```

## 重构策略

### 方案：直接使用 GameModule.Input + 状态类扩展

**原则**：
- 通用输入直接使用 `GameModule.Input`
- 平台特定逻辑移到 `PlayerMovementFsmState` 作为扩展方法或属性

### 1. 平台特定逻辑迁移

将 `GetMoveHorizontalValue` 等属性移到 `PlayerMovementFsmState`：

```csharp
// 在 PlayerMovementFsmState 中添加
protected Vector2 GetMoveHorizontalValue
{
    get
    {
#if UNITY_ANDROID
        return GameModule.Input.Move;
#else
        Vector2 dir = GameModule.Input.Move;
        if (dir != Vector2.zero)
        {
            dir.y = 0;
            return dir.normalized;
        }
        return Vector2.zero;
#endif
    }
}
```

### 2. InputService 处理

**选项 A（推荐）：完全删除**
- 删除整个 InputService 类
- 所有调用点直接使用 `GameModule.Input`

**选项 B：保留为静态工具类**
- 保留平台特定逻辑的静态方法
- 移除单例模式

## 迁移步骤

### Step 1: 迁移 PlayerFsmState 及子类

```csharp
// 旧代码
protected InputService inputServer;
inputServer = player.InputService;
if (inputServer.GetButtonDown(InputButtonType.Jump))

// 新代码
if (GameModule.Input.GetButtonDown(InputButtonType.Jump))
```

### Step 2: 迁移 Player.cs

```csharp
// 删除
public InputService InputService { get; private set; }
InputService = InputService.Instance;
```

### Step 3: 迁移 PlayerMovementFsmState

```csharp
// 删除
protected InputService inputServer;

// 直接使用 GameModule.Input
if (GameModule.Input.GetButtonDown(InputButtonType.Lock))

// Shift 检测
if (GameModule.Input.GetButton(InputButtonType.Shift))
```

### Step 4: 处理平台特定逻辑

将 `GetMoveHorizontalValue` 等移到需要的状态类中。

## 文件修改清单

| 文件 | 修改 |
|-----|------|
| `InputService.cs` | 删除或保留为静态工具 |
| `Player.cs` | 移除 InputService 属性 |
| `PlayerFsmState.cs` | 移除 inputServer 字段 |
| `PlayerMovementFsmState.cs` | 直接使用 GameModule.Input |
| `PlayerIdleState.cs` | 同上 |
| `PlayerMoveLoopState.cs` | 同上 |
| `PlayerMoveStartState.cs` | 同上 |
| `PlayerMoveEndState.cs` | 同上 |
| `PlayerJumpState.cs` | 同上 |
| `PlayerClimbState.cs` | 同上 |
| `PlayerLedgeClimbState.cs` | 同上 |
| 其他状态类 | 同上 |

## 验证方案

1. **编译检查**：确保无编译错误
2. **功能测试**：
   - 玩家移动（WASD / 方向键）
   - 玩家跳跃（空格键）
   - 玩家蹲伏（C 键）
   - 玩家冲刺（Shift 键）
   - 锁定功能（Lock 键）
