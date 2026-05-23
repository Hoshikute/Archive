# Change: 修复 LoginUI 场景流程

## Why

当前启动流程存在问题：

1. **main.unity 场景有残留 UI**：`m_panel_server` 和 `ServerItem` 残留在场景中，这些应该由 LoginUI 代码动态创建
2. **Game 场景可能被提前加载**：需要确保只有点击服务器按钮后才加载 Game.unity 场景
3. **LoginUI 应该完整显示**：启动后应该先看到完整的 LoginUI 界面

## What Changes

### 1. 清理 main.unity 场景

移除场景中残留的 UI 元素：
- `UIRoot/UICanvas/m_panel_server`
- `ServerItem`

这些由 LoginUI.cs 动态创建，不应在场景中预置。

### 2. 确认场景加载流程

```
正确流程:
main.unity → GameApp.StartGameLogic() → LoginUI 显示 → 点击服务器 → 加载 Game.unity
```

### 3. 验证 LoginUI 显示

- LoginUI.prefab 正确加载
- 服务器列表动态创建
- 点击服务器按钮触发场景加载

## Impact

### 修改文件

| 文件 | 修改内容 |
|------|---------|
| `Assets/main.unity` | 移除残留 UI 元素 |

### 验证项

- [ ] main.unity 无残留 UI
- [ ] 启动后显示 LoginUI
- [ ] 点击服务器加载 Game 场景

## Success Criteria

- [ ] 启动后先显示 LoginUI
- [ ] 服务器列表正确显示（本地/内网）
- [ ] 点击本地服务器按钮后加载 Game.unity 场景
- [ ] 编译无错误
