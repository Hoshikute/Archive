# Tasks: 整理 SuperScrollView 无限滚动组件目录结构

## Phase 1: 准备工作

- [x] 1.1 验证 SuperScrollView 当前目录结构
- [x] 1.2 检查是否有其他代码引用 SuperScrollView
- [x] 1.3 创建目标目录结构
  - [x] 1.3.1 创建 `Assets/Plugins/SuperScrollView/`
  - [x] 1.3.2 创建 `Assets/Demos/`

## Phase 2: 核心代码移动

- [x] 2.1 移动核心代码
  - [x] 2.1.1 移动 `Scripts/` → `Assets/Plugins/SuperScrollView/Runtime/`
  - [x] 2.1.2 验证 Runtime 目录结构完整
- [x] 2.2 移动编辑器扩展
  - [x] 2.2.1 移动 `Editor/` → `Assets/Plugins/SuperScrollView/Editor/`
  - [x] 2.2.2 验证 Editor 目录结构完整
- [x] 2.3 移动文档
  - [x] 2.3.1 移动 `Document.pdf` → `Assets/Plugins/SuperScrollView/`

## Phase 3: Demo 移动

- [x] 3.1 创建 Demo 目标目录
  - [x] 3.1.1 创建 `Assets/Demos/SuperScrollViewDemo/`
- [x] 3.2 移动 Demo 内容
  - [x] 3.2.1 移动 `Demo/Prefabs/`
  - [x] 3.2.2 移动 `Demo/Scenes/`
  - [x] 3.2.3 移动 `Demo/Scripts/`
  - [x] 3.2.4 移动 `Demo/Texture/`

## Phase 4: 清理

- [x] 4.1 删除原目录
  - [x] 4.1.1 删除 `Assets/SuperScrollView/` 空目录及 meta 文件
- [x] 4.2 刷新 Unity AssetDatabase

## Phase 5: 验证

- [x] 5.1 编译验证
  - [x] 5.1.1 触发 Unity 编译
  - [x] 5.1.2 检查编译错误（应为 0 errors）
- [x] 5.2 目录结构验证
  - [x] 5.2.1 确认 `Assets/Plugins/SuperScrollView/` 结构正确
  - [x] 5.2.2 确认 `Assets/Demos/SuperScrollViewDemo/` 结构正确
- [ ] 5.3 功能验证（可选）
  - [ ] 5.3.1 打开 `Menu.unity` 场景
  - [ ] 5.3.2 运行 Demo 确认正常

---

**统计**: 已完成 18/20 个任务，可选验证 2 个
