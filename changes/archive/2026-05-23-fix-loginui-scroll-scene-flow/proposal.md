# Change: 修复 LoginUI 滚动列表与场景流程

## Why

当前 LoginUI 存在以下问题：

1. **鼠标被隐藏**：启动后鼠标即被隐藏，应该在 LoginUI 阶段显示鼠标，进入 Game 场景后才隐藏
2. **Game 场景提前加载**：Game.unity 场景直接被加载，应先停留在 main 场景，点击服务器后才加载
3. **服务器列表无 UI 展示**：ServerData 已加载但无可视化列表，需要使用 SuperScrollView 实现垂直滚动列表

## What Changes

### 1. 鼠标可见性修复

| 阶段 | 鼠标状态 |
|------|---------|
| main 场景 (LoginUI) | 可见 |
| Game 场景 | 隐藏（按 Alt 显示） |

### 2. 场景加载流程

```
当前流程: main 场景 → Game 内容已加载

目标流程: main 场景 → LoginUI → 选择服务器 → 加载 Game.unity 场景
```

### 3. LoginUI 滚动列表

使用 SuperScrollView LoopListView2 实现：

| 功能 | 描述 |
|------|------|
| 服务器列表 | 垂直滚动显示本地/内网服务器 |
| 点击选择 | 点击服务器项后加载 Game 场景 |
| 数据来源 | ServerDataLoader.ServerList |

### 4. 账号密码功能

暂不实现，保持现有 InputField 但不验证。

## Impact

### 修改文件

| 文件 | 修改内容 |
|------|---------|
| `LoginUI.cs` | 添加 LoopListView2 列表、场景加载逻辑、鼠标控制 |
| `LoginUI.prefab` | 添加 LoopListView2 节点结构 |

### 新增依赖

| 依赖 | 说明 |
|------|------|
| SuperScrollView | 已在 `Assets/Plugins/SuperScrollView/` |

## Success Criteria

- [ ] 启动后鼠标可见
- [ ] LoginUI 显示服务器列表（本地/内网）
- [ ] 点击服务器后加载 Game.unity 场景
- [ ] 进入 Game 场景后鼠标隐藏
- [ ] 编译无错误
