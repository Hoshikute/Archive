# Design: Timer 模块迁移到 TEngine

## 设计目标

将项目中重复的 Timer 实现统一迁移到 TEngine 标准的 `GameModule.Timer`，消除代码冗余。

## TEngine Timer API 参考

```csharp
// 添加计时器
int tid = GameModule.Timer.AddTimer(OnTick, time: 3f);                         // 单次
int tid = GameModule.Timer.AddTimer(OnTick, time: 1f, isLoop: true);           // 循环
int tid = GameModule.Timer.AddTimer(OnTick, time: 5f, isUnscaled: true);       // 非缩放时间

// 控制
GameModule.Timer.Stop(timerId);         // 暂停
GameModule.Timer.Resume(timerId);       // 恢复
GameModule.Timer.Restart(timerId);      // 重置
GameModule.Timer.RemoveTimer(timerId);  // 移除（销毁时必须调用）

// 查询
float left = GameModule.Timer.GetLeftTime(timerId);
bool running = GameModule.Timer.IsRunning(timerId);
```

## 迁移策略

### 1. GameLogic.Common.Timer → GameModule.Timer

**旧 API 签名**：
```csharp
// 延迟调用
TimerEvent DelayCallBack(float delayTime, TimerCallBack callBack, params object[] objs)

// 间隔调用
TimerEvent CallBackOfIntervalTimer(float spaceTime, TimerCallBack callBack, params object[] objs)

// 回调签名
delegate void TimerCallBack(params object[] objs);
```

**新 API 映射**：
```csharp
// 延迟调用
int tid = GameModule.Timer.AddTimer((timer) => {
    // 原 callback 逻辑
}, time: delayTime);

// 间隔调用
int tid = GameModule.Timer.AddTimer((timer) => {
    // 原 callback 逻辑
}, time: spaceTime, isLoop: true);
```

### 2. ThirdPersonController.TimerService → GameModule.Timer

**旧 API 签名**：
```csharp
// 添加定时器（毫秒）
int AddTimer(int time, Action taskCB, Action cancelCB = null, int count = 1)

// 移除定时器
void RemoveTimer(int tid)
```

**新 API 映射**：
```csharp
// 添加定时器（注意毫秒转秒）
int tid = GameModule.Timer.AddTimer((timer) => {
    taskCB?.Invoke();
}, time: time / 1000f);

// 移除定时器
GameModule.Timer.RemoveTimer(tid);
```

## 迁移步骤

### Step 1: 迁移 Player 类

移除 `TimerService` 依赖：

```csharp
// 删除
public TimerService TimerService { get; private set; }
TimerService = TimerService.Instance;

// 状态类直接使用 GameModule.Timer
```

### Step 2: 迁移 PlayerFsmState 及子类

```csharp
// 旧代码
timerService.AddTimer(50, () => OnLandToFall(fsm));

// 新代码
int tid = GameModule.Timer.AddTimer((timer) => OnLandToFall(fsm), time: 0.05f);
```

### Step 3: 迁移 GameObjectManager

```csharp
// 旧代码
global::GameLogic.Common.Timer.Timer.DelayCallBack(time, (object[] obj) => {
    DestroyGameObjectByPool(go);
});

// 新代码
GameModule.Timer.AddTimer((timer) => {
    DestroyGameObjectByPool(go);
}, time: time);
```

### Step 4: 删除旧 Timer 文件

确认无引用后删除：
- `Common/Timer/Timer.cs`
- `Common/Timer/TimerEvent.cs`
- `Player/.../TimerService/` 目录

## 风险评估

| 风险 | 影响 | 缓解措施 |
|-----|------|---------|
| 时间单位转换错误 | 定时器触发时间不对 | 仔细检查毫秒→秒转换 |
| 回调参数丢失 | 逻辑错误 | 使用闭包捕获变量 |
| TimerId 管理混乱 | 内存泄漏 | 确保状态离开时 RemoveTimer |

## 验证方案

1. **编译检查**：确保无编译错误
2. **功能测试**：
   - 玩家移动状态切换正常
   - 对象池延迟销毁正常
   - UI 刷新定时器正常（已在用 GameModule.Timer）
