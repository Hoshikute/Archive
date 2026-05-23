# Design: 迁移 LockStepDemo 客户端代码

## 架构概览

**保持原有双层结构**：业务层(SyncClientLogic) 与 逻辑层(SyncGameLogic) 分离

```
源项目结构                          目标项目结构
─────────────────────────────────────────────────────────────────
LockStepDemo/Client/               Fire/UnityProject/
├── Assets/Script/                 ├── Assets/GameScripts/HotFix/
│   ├── SyncClientLogic/    ────► │   ├── GameLogic/Module/SyncClientLogic/   (表现层)
│   ├── SyncGameLogic/      ────► │   ├── GameLogic/Module/SyncGameLogic/     (逻辑层)
│   ├── Game/               ────► │   ├── GameLogic/Module/Game/              (角色系统)
│   ├── SyncFrameWork/      ────► │   ├── GameLogic/Module/FrameSync/         (已迁移核心)
│   └── Core/               ────► │   └── GameLogic/Common/                   (工具适配)
```

## 目录映射详细设计

### 1. SyncClientLogic (表现层)

```
GameLogic/Module/SyncClientLogic/
├── Component/
│   ├── PerfabComponent.cs      # Entity → GameObject 绑定
│   ├── AnimComponent.cs        # 动画控制
│   ├── HealthBarComponent.cs   # 血条显示
│   ├── BuffEffectComponent.cs  # Buff 特效
│   ├── PlayerCameraComponent.cs# 相机跟随
│   ├── SkillBehaviorComponent.cs
│   ├── OperationWindowComponent.cs
│   └── MonoComp/
│       ├── HardPointComponent.cs   # 角色挂点
│       ├── ElementCreatePoint.cs
│       └── MapComponent.cs
└── System/
    ├── CreatePerfabSystem.cs   # 创建视图
    ├── MovePerfabSystem.cs     # 位置同步
    ├── PlayerAnimSystem.cs     # 玩家动画
    ├── CameraSystem.cs         # 相机系统
    ├── HealthBarSystem.cs      # 血条系统
    ├── BuffBehaviorSystem.cs   # Buff 行为
    ├── SkillBehaviorSystem.cs  # 技能行为
    ├── InputSystem.cs          # 输入处理
    ├── ClientOperationSystem.cs
    ├── DestroyEffectSystem.cs
    ├── ResurgenceUISystem.cs
    └── SettlementUISystem.cs
```

### 2. SyncGameLogic (逻辑层)

```
GameLogic/Module/SyncGameLogic/
├── Component/
│   ├── AssetComponent.cs       # Prefab 资源名
│   ├── TransformComponent.cs   # 同步坐标
│   ├── LifeComponent.cs        # 生命值
│   ├── CampComponent.cs        # 阵营
│   ├── BlockComponent.cs       # 阻挡
│   ├── BlowFlyComponent.cs     # 击飞
│   ├── BuffComponent.cs        # Buff
│   ├── CDComponent.cs          # 冷却
│   ├── CollisionComponent.cs   # 碰撞
│   ├── FlyObjectComponent.cs   # 飞行物
│   ├── GameTimeComponent.cs    # 游戏时间
│   ├── InputComponent.cs       # 输入
│   ├── ItemComponent.cs        # 道具
│   ├── ItemCreatePointComponent.cs
│   ├── LifeSpanComponent.cs    # 生命周期
│   ├── RankComponent.cs        # 排名
│   ├── SkillStatusComponent.cs # 技能状态
│   └── ViewComponent.cs        # 视图标记
├── System/
│   ├── MoveSystem.cs           # 移动逻辑
│   ├── SkillSystem.cs          # 技能逻辑
│   ├── InitSystem.cs           # 初始化
│   ├── BlowFlySystem.cs        # 击飞系统
│   ├── BuffSystem.cs           # Buff 系统
│   ├── CollisionSystem.cs      # 碰撞系统
│   ├── CollisionDamageSystem.cs
│   ├── CreateItemSystem.cs     # 创建道具
│   ├── ItemSystem.cs           # 道具系统
│   ├── FireSystem.cs           # 开火系统
│   ├── FlyObjectCollisionSystem.cs
│   ├── GameSystem.cs           # 游戏系统
│   ├── LifeSpanSystem.cs       # 生命周期
│   ├── OperationSystem.cs      # 操作系统
│   ├── RankSystem.cs           # 排名系统
│   ├── ResurgenceSystem.cs     # 复活系统
│   └── SkillStatusSystem.cs    # 技能状态
├── Utils/
│   ├── SkillUtils.cs           # 技能工具
│   └── GameUtils.cs            # 游戏工具
└── World/
    └── DemoWorld.cs            # World 实现
```

### 3. Game (角色系统)

```
GameLogic/Module/Game/
├── BuffBase.cs                 # Buff 基类
├── Calculate.cs                # 计算工具
├── CharacterManager.cs         # 角色管理器
├── CommandRouteService.cs      # 指令路由
├── GameData.cs                 # 游戏数据
├── GameLogic.cs                # 游戏逻辑入口
├── PlayerControl.cs            # 玩家控制
├── SceneLoadManager.cs         # 场景加载
├── SyncService.cs              # 同步服务
├── Character/
│   ├── CharacterBase.cs        # 角色基类
│   ├── CharacterBaseProperty.cs# 角色属性
│   ├── CameraService.cs        # 相机服务
│   ├── Property/
│   │   └── BraveProperty.cs    # 勇者属性
│   ├── CharacterType/
│   │   └── Player.cs           # 玩家类型
│   ├── BaseComponent/
│   │   ├── ComponentBase.cs    # 组件基类
│   │   ├── AnimComponent.cs    # 动画组件
│   │   ├── AudioComponent.cs   # 音频组件
│   │   ├── ChangeWeaponComponent.cs
│   │   ├── CharacterMaterialComponent.cs
│   │   ├── CharacterUIComponent.cs
│   │   ├── EffectComponent.cs  # 特效组件
│   │   └── MoveComponent.cs    # 移动组件
│   ├── CharacterStatus/
│   │   ├── AttackStatus.cs     # 攻击状态
│   │   ├── BlowFlyStatus.cs    # 击飞状态
│   │   ├── BuffStatus.cs       # Buff 状态
│   │   ├── DieStatus.cs        # 死亡状态
│   │   ├── HurtStatus.cs       # 受击状态
│   │   ├── MonsterHurtStatus.cs
│   │   ├── PlayerHurtStatus.cs
│   │   ├── MoveStatus.cs       # 移动状态
│   │   ├── SkillStatus.cs      # 技能状态
│   │   ├── HurtModel.cs
│   │   ├── EffectManager.cs    # 特效管理
│   │   └── EffectStatus/
│   │       ├── DieEffectStatus.cs
│   │       ├── EffectStatusBase.cs
│   │       ├── MoveEffectStatus.cs
│   │       ├── SkillEffectStatus.cs
│   │       ├── StandardAttackStatus.cs
│   │       └── SubClass/
│   │           ├── MonsterAttackEffect.cs
│   │           ├── MonsterSkillEffect.cs
│   │           ├── PlayerAttackEffect.cs
│   │           ├── PlayerSkillEffect.cs
│   │           └── TrapAttackEffect.cs
│   └── SkillToken/
│       ├── SkillToken.cs
│       ├── SkillTokenAnimComp.cs
│       ├── SkillTokenMoveStatus.cs
│       ├── SkillTokenSkillStatus.cs
│       └── SkillTokenUIComponent.cs
├── Effect/
│   ├── CreatMesh.cs
│   ├── DisplayManager.cs
│   ├── FadInOut.cs
│   ├── Ghost.cs
│   ├── MaterialManager.cs
│   └── SM_UVScroller.cs
├── Fly/
│   ├── FlyObjectBase.cs
│   └── FlyObjectManager.cs
├── Item/
│   ├── Item.cs
│   └── ItemManager.cs
└── Model/
    └── SkillData.cs
```

### 4. Common (工具层)

```
GameLogic/Common/
├── GameObject/
│   └── GameObjectManager.cs    # 对象池 (适配 TEngine)
├── Pool/
│   └── PoolObject.cs           # 池化对象基类
├── Resource/
│   └── ResourceManager.cs      # 资源加载适配器
└── Timer/
    └── Timer.cs                # 计时器适配器
```

## 与现有 FrameSync 模块的关系

```
FrameSync/              # 已迁移的核心框架
├── Core/               # WorldBase, SyncRule
├── ECS/                # EntityBase, ComponentBase, SystemBase
├── Calc/               # SyncVector3, HashExtensions
├── Message/            # SyncMessage
└── Resource/           # FrameSyncResourceLoader

SyncClientLogic/        # 表现层 (本次迁移)
SyncGameLogic/          # 逻辑层 (本次迁移)
Game/                   # 角色系统 (本次迁移)
```

## TEngine 适配设计

### 资源加载适配

```csharp
// 源代码 (LockStepDemo)
GameObject go = ResourceManager.Load<GameObject>(gameObjectName);

// 适配器 (GameLogic/Common/Resource/ResourceManager.cs)
public static class ResourceManager
{
    public static T Load<T>(string path) where T : Object
    {
        // 同步加载 - TEngine 适配
        return GameModule.Asset.LoadAsset<T>(path);
    }

    public static void LoadAsync<T>(string path, Action<AsyncOperation, T> callback) where T : Object
    {
        // 异步加载 - TEngine 适配
        GameModule.Asset.LoadAssetAsync<T>(path).ContinueWith(asset =>
        {
            callback?.Invoke(null, asset);
        });
    }
}
```

### 对象池适配

```csharp
// 保持原有 GameObjectManager 实现
// 替换内部的 ResourceManager 调用为适配后的版本
public static class GameObjectManager
{
    public static GameObject CreateGameObject(string gameObjectName)
    {
        // 使用适配后的 ResourceManager
        GameObject goTmp = ResourceManager.Load<GameObject>(gameObjectName);
        // ... 其余逻辑保持不变
    }
}
```

## 命名空间规划

```csharp
namespace GameLogic.SyncClientLogic.Component { }
namespace GameLogic.SyncClientLogic.System { }
namespace GameLogic.SyncGameLogic.Component { }
namespace GameLogic.SyncGameLogic.System { }
namespace GameLogic.SyncGameLogic.Utils { }
namespace GameLogic.SyncGameLogic.World { }
namespace GameLogic.Game.Character { }
namespace GameLogic.Game.Effect { }
namespace GameLogic.Game.Fly { }
namespace GameLogic.Game.Item { }
namespace GameLogic.Game.Model { }
namespace GameLogic.Common.GameObject { }
namespace GameLogic.Common.Pool { }
namespace GameLogic.Common.Resource { }
namespace GameLogic.Common.Timer { }
```

## 迁移顺序

```
Phase 1: Common 工具层 (适配 TEngine)
├── ResourceManager.cs (适配器)
├── PoolObject.cs
├── GameObjectManager.cs
└── Timer.cs (适配器)

Phase 2: SyncGameLogic 组件
├── AssetComponent
├── TransformComponent
├── LifeComponent, CampComponent
└── 其他 Component

Phase 3: SyncGameLogic 系统
├── InitSystem
├── MoveSystem, SkillSystem
└──其他 System

Phase 4: SyncGameLogic 工具和 World
├── SkillUtils, GameUtils
└── DemoWorld

Phase 5: SyncClientLogic 组件
├── PerfabComponent
├── AnimComponent
├── HealthBarComponent
└── 其他 Component

Phase 6: SyncClientLogic 系统
├── CreatePerfabSystem
├── MovePerfabSystem
├── PlayerAnimSystem
└── 其他 System

Phase 7: Game 角色系统
├── CharacterBase, CharacterManager
├── CharacterStatus 状态机
├── SkillToken
├── Effect, Fly, Item
└── Model
```

## 文件统计

| 模块 | 文件数 | 说明 |
|------|--------|------|
| SyncClientLogic | 22 | 表现层 |
| SyncGameLogic | 41 | 逻辑层 |
| Game | 59 | 角色系统 |
| Common | 4 | 工具适配 |
| **总计** | **126** | |
