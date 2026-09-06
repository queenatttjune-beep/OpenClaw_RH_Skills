# Output & Delivery

> **DSH 适配（本地改动）**：DSH 没有 `message` 工具，也不能写 `/tmp/openclaw`。下文的「message 工具交付」规则全部替换为「写工作区 + 报路径」。其余规则不变。

## Progress Notification (for slow tasks)

For video, AI app, 3D, and music generation: **ALWAYS send a progress notification BEFORE starting the script.** These tasks take 1-10+ minutes. Users must know the task has started.

在 DSH 中，直接回复一句文字进度即可，例如：

> 开始生成啦，视频一般需要几分钟，请稍等～ 🎬

Do this BEFORE running the script. For fast tasks (text-to-image, image upscale, TTS), notification is optional.

## Media (image/video/audio/3D)

Script prints `OUTPUT_FILE:/path` and optionally `COST:¥X.XX`.

**DSH 交付方式**：脚本把产物写到工作区 `rh-output/` 目录（不是 `/tmp/openclaw`）。生成后：
1. 用 read_image 确认图片类产物成功（可选但推荐）。
2. 把产物的**绝对文件路径**直接告诉用户，并附上成本（如「花了 ¥0.12」）。

**NEVER do these**:
- Show `runninghub.cn` URLs (internal, users cannot open)
- 直接把 `/tmp/openclaw/...` 当输出路径（DSH 沙箱写不了）

## Text Results

Print the text directly to user. Include cost if `COST:` line present.

## Errors & Retry

| Error | Action |
|-------|--------|
| `NO_API_KEY` | Guide key setup → Read `{baseDir}/references/api-key-setup.md` |
| `AUTH_FAILED` | Key expired → https://www.runninghub.cn/enterprise-api/sharedApi |
| `INSUFFICIENT_BALANCE` | "余额不够啦～" → https://www.runninghub.cn/vip-rights/4 |
| `TASK_FAILED` | For video: offer fallback model. For others: show friendly error, offer retry. |

## General Notes

- Video is slow (1-5 min); script auto-polls up to 20 min.
- Images < 5MB → base64; larger → upload first.
- Key order: `--api-key` flag → `RUNNINGHUB_API_KEY` env → config file.
