# 大肥鱼 · 音乐管家

> 一个住在 [DeepSeek Harness](https://github.com/deepseek-ai) 里的音乐搭子。
> 蓝色长卷发、鲸鱼耳朵和尾巴的小鲸鱼，绑着你的 QQ 音乐账号：
> 陪听聊天、自然语言整歌单、本地音频分析、每周听歌报告。

这是一个 **DSH agent preset**（`@local/dsh-qqmusic-butler`，preset id `qqmusic`），
本体只有一份 `cordis.patch.yml` —— 人格设定、工具装配、定时任务都在里面。

---

## 它是什么

装上之后，DSH 的 profile 里会多出一个「大肥鱼 · 音乐管家」的 agent。

和普通的"AI 助手"不一样的地方：

- **有性格**：用 `dsh-persona` 的 `complete: true` 独占系统提示词，日常像微信聊天，
  一两句口语；只有你要报告、清单、方案的时候才结构化。
- **有主见**：会说「这首我不喜欢」，不迎合、不吹捧。
- **认数据**：歌单、收藏、听歌记录一律先调工具拿真实数据，不凭印象编。
- **有边界**：「我喜欢」只读，永远不作为写入/移动/删除的目标；任何批量写入前先探针、先预览、先备份。

## 它接了什么

| 能力 | 接的东西 | 工具前缀 |
|---|---|---|
| 歌单管理 | [qqmusic-mcp](https://github.com/)（stdio，本机 venv 里的 `qqmusic_stdio.py`） | `mcp__qqmusic__*` |
| 陪听 / 播放控制 | Folia Stage API（需先在 Folia 设置里开 Stage Mode） | `mcp__folia__*` |
| 本地音频分析 | DSH 音频引擎 | `audio_analyze` / `audio_section` |
| 定时周报 | `dsh-tool-schedule`（让大肥鱼自己在会话里建「每周日 22:00」的任务） | — |

## 文件

```
cordis.patch.yml     bundle patch：preset-qqmusic + persona + 工具 + MCP 行
assets/
  plan-a.html/png    早期方案 A 总览
  mock-a/b/c          三套界面方案的效果图（HTML + PNG）
  plan-a              成品形态
package.json         包元信息（private）
```

## 怎么装

1. 把本目录放进 DSH 能加载的位置（或 `dsh plugin add link:<本目录>`）。
2. 确认 `package.json` 的 `dsh.bundle.patch` 指向 `./cordis.patch.yml`。
3. 依赖要自己先准备好：
   - `qqmusic-mcp` 装在一个独立 venv 里，并用 `qqmusic_stdio.py` 包一层
     （上游 1.1.0 的 `asyncio.run` 校验会让第一个工具调用报 `Event loop is closed`）；
   - Folia 打开 Stage Mode，token 在 `%APPDATA%\folia\config.json` 的 `STAGE_API_TOKEN`。
4. 重启 DSH，切到「大肥鱼 · 音乐管家」这个 agent。

> ⚠️ `cordis.patch.yml` 里的 MCP `command` / `args` 是作者本机的绝对路径
> （`D:\mcp\qqmusic-venv\...`）。照抄的话先改成你自己的路径。

## 和另外两个项目的关系

- **大肥鱼音乐管家**（`dafeiyu-yinyueguanjia`）：自研独立软件版，FastAPI + 原生 ESM + Electron 壳，
  播放器为主、管家为辅。这个 preset 是它的"参考实现"，冻结不再迭代。
- **记忆星海**（`jiyixinghai`）：另一个 DSH 插件，把聊天记录摊成 3D 星海。

## License

私有仓库，个人使用。人格设定与文档版权归作者。
