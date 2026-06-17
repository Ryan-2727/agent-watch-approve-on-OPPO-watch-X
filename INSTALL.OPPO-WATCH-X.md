# Codex Desktop + ntfy + OPPO Watch X

这是当前仓库里整理好的 Windows/Android/Wear OS 终版接法，目标是：

- 审批通知只显示 `🤖 Codex 待批准`
- 不显示项目文件夹名
- 不显示 `git push --force` 这类动作摘要
- 只保留 `允许 / 拒绝` 两个按钮

## 用到的文件

- `watch_approve.py`
- `watch_done.py`
- `watch.env.example`
- `examples/codex/oppo-watch-x/hooks.windows.example.json`
- `examples/codex/oppo-watch-x/watch.env.example`

## 电脑端步骤

1. 在另一台电脑上克隆本仓库，或者只复制上面 5 个文件。
2. 建议把脚本放到固定目录，例如 `C:\Users\<YOU>\watch-hooks\`。
3. 复制 `examples/codex/oppo-watch-x/watch.env.example` 为 `watch.env`，填入你自己的 topic。
4. 复制 `examples/codex/oppo-watch-x/hooks.windows.example.json`，把里面两个路径改成你本机的真实路径：
   - Python 路径
   - `watch-hooks` 目录路径
5. 保存为 `C:\Users\<YOU>\.codex\hooks.json`。
6. 重启 Codex Desktop。
7. 在 Codex 里执行 `/hooks`，把 `PermissionRequest` 和 `Stop` 两个 hook 都 review + trust。
8. 在脚本目录执行：

```powershell
python .\watch_approve.py --doctor
```

## 手机端步骤

1. 安装 ntfy App。
2. 订阅 `NTFY_NOTIFY_TOPIC`。
3. 开启 ntfy 的即时推送。
4. 给 ntfy 关闭电池优化，允许后台常驻。

## 手表端步骤

1. 确认手表已镜像手机通知。
2. 确认 ntfy 的通知允许在手表显示。
3. 如果手表仍然不显示按钮，优先检查系统通知镜像策略，而不是 hook 本身。

## 推荐配置

这个组合就是你这次最终验证通过的“简洁版”：

```env
WATCH_TRANSPORT=ntfy
APPROVE_WAIT=240
APPROVE_TIMEOUT_DECISION=ask
WATCH_DANGER_ONLY=1
WATCH_PROTECT_SELF=1
WATCH_TERMINAL_BUTTON=0
WATCH_SHOW_CWD=0
WATCH_SHOW_DESC=0
```

## 测试

不要用 `codex exec` 测审批，因为它不会触发 `PermissionRequest`。

进入交互式 Codex 会话后，触发一次需要审批的联网或提权操作，手机/手表上应看到只有标题和两个按钮的通知。
