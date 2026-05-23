# Tasks: 迁移 LockStepDemo 客户端代码

## Phase 1: Common 工具层适配

- [x] 1.1 创建目录结构 `GameLogic/Common/`
- [x] 1.2 创建 ResourceManager 适配器 (适配 TEngine 资源加载)
- [x] 1.3 迁移 PoolObject.cs
- [x] 1.4 迁移 GameObjectManager.cs (替换 ResourceManager 调用)
- [x] 1.5 创建 Timer 适配器 (适配 TEngine Timer)
- [x] 1.6 验证工具层编译通过

## Phase 2: SyncGameLogic 组件

- [x] 2.1 创建目录 `GameLogic/Module/SyncGameLogic/Component/`
- [x] 2.2 迁移 AssetComponent.cs
- [x] 2.3 迁移 TransformComponent.cs
- [x] 2.4 迁移 LifeComponent.cs
- [x] 2.5 迁移 CampComponent.cs
- [x] 2.6 迁移 BlockComponent.cs
- [x] 2.7 迁移 BlowFlyComponent.cs
- [x] 2.8 迁移 BuffComponent.cs
- [x] 2.9 迁移 CDComponent.cs
- [x] 2.10 迁移 CollisionComponent.cs
- [x] 2.11 迁移 FlyObjectComponent.cs
- [x] 2.12 迁移 GameTimeComponent.cs
- [x] 2.13 迁移 InputComponent.cs
- [x] 2.14 迁移 ItemComponent.cs
- [x] 2.15 迁移 ItemCreatePointComponent.cs
- [x] 2.16 迁移 LifeSpanComponent.cs
- [x] 2.17 迁移 RankComponent.cs
- [x] 2.18 迁移 SkillStatusComponent.cs
- [x] 2.19 迁移 ViewComponent.cs
- [x] 2.20 迁移 PlayerComponent.cs (补充)
- [ ] 2.21 验证组件编译通过

## Phase 3: SyncGameLogic 系统

- [x] 3.1 创建目录 `GameLogic/Module/SyncGameLogic/System/`
- [x] 3.2 迁移 InitSystem.cs
- [x] 3.3 迁移 MoveSystem.cs
- [x] 3.4 迁移 SkillSystem.cs
- [x] 3.5 迁移 BlowFlySystem.cs
- [x] 3.6 迁移 BuffSystem.cs
- [x] 3.7 迁移 CollisionSystem.cs
- [x] 3.8 迁移 CollisionDamageSystem.cs
- [x] 3.9 迁移 CreateItemSystem.cs
- [x] 3.10 迁移 ItemSystem.cs
- [x] 3.11 迁移 FireSystem.cs
- [x] 3.12 迁移 FlyObjectCollisionSystem.cs
- [x] 3.13 迁移 GameSystem.cs
- [x] 3.14 迁移 LifeSpanSystem.cs
- [x] 3.15 迁移 OperationSystem.cs
- [x] 3.16 迁移 RankSystem.cs
- [x] 3.17 迁移 ResurgenceSystem.cs
- [x] 3.18 迁移 SkillStatusSystem.cs
- [ ] 3.19 验证系统编译通过

## Phase 4: SyncGameLogic 工具和 World

- [x] 4.1 创建目录 `GameLogic/Module/SyncGameLogic/Utils/`
- [ ] 4.2 迁移 SkillUtils.cs
- [x] 4.3 迁移 GameUtils.cs
- [ ] 4.4 创建目录 `GameLogic/Module/SyncGameLogic/World/`
- [ ] 4.5 迁移 DemoWorld.cs
- [ ] 4.6 验证 SyncGameLogic 模块编译通过

## Phase 5: SyncClientLogic 组件

- [x] 5.1 创建目录 `GameLogic/Module/SyncClientLogic/Component/`
- [x] 5.2 迁移 PerfabComponent.cs
- [x] 5.3 迁移 AnimComponent.cs
- [x] 5.4 迁移 HealthBarComponent.cs
- [x] 5.5 迁移 BuffEffectComponent.cs
- [x] 5.6 迁移 PlayerCameraComponent.cs
- [x] 5.7 迁移 SkillBehaviorComponent.cs
- [x] 5.8 迁移 OperationWindowComponent.cs
- [x] 5.9 创建目录 `GameLogic/Module/SyncClientLogic/Component/MonoComp/`
- [x] 5.10 迁移 HardPointComponent.cs
- [x] 5.11 迁移 ElementCreatePoint.cs
- [x] 5.12 迁移 MapComponent.cs
- [ ] 5.13 验证组件编译通过

## Phase 6: SyncClientLogic 系统

- [x] 6.1 创建目录 `GameLogic/Module/SyncClientLogic/System/`
- [x] 6.2 迁移 CreatePerfabSystem.cs
- [x] 6.3 迁移 MovePerfabSystem.cs
- [x] 6.4 迁移 PlayerAnimSystem.cs
- [x] 6.5 迁移 CameraSystem.cs
- [x] 6.6 迁移 HealthBarSystem.cs
- [x] 6.7 迁移 BuffBehaviorSystem.cs
- [x] 6.8 迁移 SkillBehaviorSystem.cs
- [x] 6.9 迁移 InputSystem.cs
- [x] 6.10 迁移 ClientOperationSystem.cs
- [x] 6.11 迁移 DestroyEffectSystem.cs
- [x] 6.12 迁移 ResurgenceUISystem.cs
- [x] 6.13 迁移 SettlementUISystem.cs
- [ ] 6.14 验证 SyncClientLogic 模块编译通过

## Phase 7: Game 角色系统

### 7.1 基础文件
- [x] 7.1.1 创建目录 `GameLogic/Module/Game/`
- [x] 7.1.2 迁移 BuffBase.cs
- [x] 7.1.3 迁移 Calculate.cs
- [x] 7.1.4 迁移 CharacterManager.cs
- [ ] 7.1.5 迁移 CommandRouteService.cs (简化版本，需要网络模块支持)
- [x] 7.1.6 迁移 GameData.cs
- [x] 7.1.7 迁移 GameLogic.cs
- [x] 7.1.8 迁移 PlayerControl.cs
- [x] 7.1.9 迁移 SceneLoadManager.cs
- [x] 7.1.10 迁移 SyncService.cs

### 7.2 Character 角色
- [x] 7.2.1 创建目录 `GameLogic/Module/Game/Character/`
- [x] 7.2.2 迁移 CharacterBase.cs
- [x] 7.2.3 迁移 CharacterBaseProperty.cs
- [x] 7.2.4 迁移 CameraService.cs
- [x] 7.2.5 创建目录 `Character/Property/`，迁移 BraveProperty.cs
- [x] 7.2.6 创建目录 `Character/CharacterType/`，迁移 Player.cs

### 7.3 BaseComponent 组件
- [x] 7.3.1 创建目录 `Character/BaseComponent/`
- [x] 7.3.2 迁移 ComponentBase.cs
- [x] 7.3.3 迁移 AnimComponent.cs
- [x] 7.3.4 迁移 AudioComponent.cs
- [x] 7.3.5 迁移 ChangeWeaponComponent.cs
- [x] 7.3.6 迁移 CharacterMaterialComponent.cs
- [x] 7.3.7 迁移 CharacterUIComponent.cs
- [x] 7.3.8 迁移 EffectComponent.cs
- [x] 7.3.9 迁移 MoveComponent.cs

### 7.4 CharacterStatus 状态机
- [x] 7.4.1 创建目录 `Character/CharacterStatus/`
- [x] 7.4.2 迁移 AttackStatus.cs
- [x] 7.4.3 迁移 BlowFlyStatus.cs
- [x] 7.4.4 迁移 BuffStatus.cs
- [x] 7.4.5 迁移 DieStatus.cs
- [x] 7.4.6 迁移 HurtStatus.cs
- [x] 7.4.7 迁移 MonsterHurtStatus.cs (合并到 HurtStatus.cs)
- [x] 7.4.8 迁移 PlayerHurtStatus.cs (合并到 HurtStatus.cs)
- [x] 7.4.9 迁移 MoveStatus.cs
- [x] 7.4.10 迁移 SkillStatus.cs
- [x] 7.4.11 迁移 HurtModel.cs
- [x] 7.4.12 迁移 EffectManager.cs

### 7.5 EffectStatus 特效状态
- [x] 7.5.1 创建目录 `Character/CharacterStatus/EffectStatus/`
- [x] 7.5.2 迁移 DieEffectStatus.cs
- [x] 7.5.3 迁移 EffectStatusBase.cs
- [x] 7.5.4 迁移 MoveEffectStatus.cs
- [x] 7.5.5 迁移 SkillEffectStatus.cs
- [x] 7.5.6 迁移 StandardAttackStatus.cs
- [x] 7.5.7 创建目录 `EffectStatus/SubClass/`
- [x] 7.5.8 迁移 MonsterAttackEffect.cs
- [x] 7.5.9 迁移 MonsterSkillEffect.cs
- [x] 7.5.10 迁移 PlayerAttackEffect.cs
- [x] 7.5.11 迁移 PlayerSkillEffect.cs
- [x] 7.5.12 迁移 TrapAttackEffect.cs

### 7.6 SkillToken 技能令牌
- [x] 7.6.1 创建目录 `Character/SkillToken/`
- [x] 7.6.2 迁移 SkillToken.cs
- [x] 7.6.3 迁移 SkillTokenAnimComp.cs
- [x] 7.6.4 迁移 SkillTokenMoveStatus.cs
- [x] 7.6.5 迁移 SkillTokenSkillStatus.cs
- [x] 7.6.6 迁移 SkillTokenUIComponent.cs

### 7.7 Effect 特效系统
- [x] 7.7.1 创建目录 `Game/Effect/`
- [x] 7.7.2 迁移 CreatMesh.cs
- [x] 7.7.3 迁移 DisplayManager.cs
- [x] 7.7.4 迁移 FadInOut.cs
- [x] 7.7.5 迁移 Ghost.cs
- [x] 7.7.6 迁移 MaterialManager.cs
- [x] 7.7.7 迁移 SM_UVScroller.cs

### 7.8 Fly 飞行物系统
- [x] 7.8.1 创建目录 `Game/Fly/`
- [x] 7.8.2 迁移 FlyObjectBase.cs
- [x] 7.8.3 迁移 FlyObjectManager.cs

### 7.9 Item 道具系统
- [x] 7.9.1 创建目录 `Game/Item/`
- [x] 7.9.2 迁移 Item.cs
- [x] 7.9.3 迁移 ItemManager.cs

### 7.10 Model 数据模型
- [x] 7.10.1 创建目录 `Game/Model/`
- [x] 7.10.2 迁移 SkillData.cs

### 7.11 验证
- [ ] 7.11.1 验证 Game 模块编译通过
- [ ] 7.11.2 验证所有模块整体编译通过

## Phase 8: 最终验证

- [ ] 8.1 检查所有命名空间正确
- [ ] 8.2 检查所有依赖引用正确
- [ ] 8.3 运行 Unity 编译，确保无错误
- [ ] 8.4 检查与现有 EGamePlay 代码的兼容性
- [ ] 8.5 更新相关文档

---

**统计**: 已迁移 117 个文件
- Common: 5 文件 ✓
- SyncGameLogic: 37 文件 ✓
- SyncClientLogic: 22 文件 ✓
- Game: 53 文件 ✓

**待完成**: 编译验证
