# Change: 迁移 LockStepDemo 客户端代码

## Why

将帧同步项目 (LockStepDemo) 的客户端代码完整迁移到本项目 (Fire)，当前迁移不完整，导致：

1. **渲染层缺失**：`SyncClientLogic` 整个目录未迁移，`PerfabComponent`、`AnimComponent`、`CreatePerfabSystem` 等核心渲染组件缺失
2. **游戏逻辑不完整**：`SyncGameLogic` 只迁移了部分组件，缺少 `AssetComponent`、`TransfromComponent`、`SkillSystem` 等
3. **角色系统缺失**：`Game/Character` 整个目录未迁移，角色状态机、属性系统、技能令牌等不存在
4. **工具代码缺失**：`GameObjectManager`、`PoolObject` 等对象池工具未迁移

## What Changes

### 1. 表现层 (SyncClientLogic → ClientLogic)

| 源路径 | 目标路径 | 内容 |
|--------|----------|------|
| `SyncClientLogic/Component/` | `FrameSync/ClientLogic/Component/` | PerfabComponent, AnimComponent, HealthBarComponent 等 |
| `SyncClientLogic/System/` | `FrameSync/ClientLogic/System/` | CreatePerfabSystem, MovePerfabSystem, PlayerAnimSystem 等 |

### 2. 游戏逻辑层 (SyncGameLogic → GameLogic)

| 源路径 | 目标路径 | 内容 |
|--------|----------|------|
| `SyncGameLogic/Component/` | `FrameSync/GameLogic/Component/` | AssetComponent, TransfromComponent, LifeComponent 等 |
| `SyncGameLogic/System/` | `FrameSync/GameLogic/System/` | MoveSystem, SkillSystem, InitSystem 等 |
| `SyncGameLogic/Utils/` | `FrameSync/GameLogic/Utils/` | SkillUtils, GameUtils |
| `SyncGameLogic/World/` | `FrameSync/GameLogic/World/` | DemoWorld |

### 3. 角色系统 (Game/Character → GameLogic/Character)

| 源路径 | 目标路径 | 内容 |
|--------|----------|------|
| `Game/Character/` | `GameLogic/Module/Character/` | 角色基类、状态机、属性系统 |
| `Game/Effect/` | `GameLogic/Module/Effect/` | 特效系统 |
| `Game/Fly/` | `GameLogic/Module/Fly/` | 飞行物系统 |
| `Game/Item/` | `GameLogic/Module/Item/` | 道具系统 |

### 4. 核心工具 (Core → 适配 TEngine)

| 源路径 | 目标路径 | 内容 |
|--------|----------|------|
| `Core/GameObject/` | `GameLogic/Common/GameObject/` | GameObjectManager (适配 TEngine 资源加载) |
| `Core/Pool/` | 使用 TEngine 对象池 | PoolObject (可能需要适配层) |
| `Core/Timer/` | 使用 TEngine Timer | Timer (可能需要适配层) |

## Impact

### 受影响文件

- 新增约 **80+ 文件**（从 LockStepDemo 迁移）
- 需要适配 TEngine 框架的代码：
  - `GameObjectManager` → 使用 `GameModule.Asset` 替代 `ResourceManager`
  - `Timer` → 使用 `GameModule.Timer` 替代
  - 命名空间调整

### 受影响功能

- 帧同步角色渲染
- 技能系统
- 角色状态机
- 对象池管理

## Migration Strategy

1. **直接复制**：不依赖 TEngine 的纯逻辑代码
2. **适配迁移**：依赖资源加载、计时器等需要适配 TEngine API
3. **重构合并**：与现有 EGamePlay 代码有重叠的部分需要合并

## Success Criteria

- [ ] 所有源项目文件迁移完成
- [ ] 编译无错误
- [ ] 现有功能不受影响
- [ ] 新迁移代码可通过单元测试（如有）
