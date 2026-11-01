# make-knowledge-cards

把一篇文章压缩成一组**结构化知识卡片**，用于学习、复习和分享。

## 它解决什么问题

长文章、课件、政策文件读起来信息密集，但事后很难回忆、也不方便随时复习。常见的笔记做法要么是整段抄录（没有提炼），要么凭印象总结（容易加入原文没有的内容、或硬凑条数）。

`make-knowledge-cards` 解决三件事：

- **提炼而不是抄录**：只挑出真正重要、可独立成立的知识点；
- **克制而不是编造**：严格只使用原文信息，不补充、不推测原文没有的事实，信息不够就少出几张，不硬凑数量；
- **一卡一点、便于复习**：每张卡片只讲一个知识点，并自带"例子 / 自测问题"，方便自我检测。

## 主要功能

- 输入**粘贴的文章正文**，或**本地 Markdown（`.md`）/ TXT（`.txt`）文件**，输出 **5~8 张知识卡片**；
- 每张卡片固定四部分：**标题、核心知识、简明解释、例子或自测问题**；
- 自动去重（同一知识点在文中反复出现只保留一次）；
- 一张卡片只讲一个知识点，不合并、不拆分凑数；
- 按知识价值取舍，信息不足时卡片少于 5 张并说明原因；
- 叙事/文学类文本只提取文中明确陈述的事实，不推断、不添加原文未明说的寓意；
- 默认在对话中直接输出卡片；需要时可保存为一个 Markdown 文件。

## 安装方法

本 Skill 是**纯提示词型**，无第三方依赖、无脚本需要执行。

1. 把整个 `make-knowledge-cards/` 目录复制到你所用 Agent 运行时的 skills 目录下，使最终路径形如：

   ```
   <你的 skills 目录>/make-knowledge-cards/SKILL.md
   ```

   常见位置例如 `<workspace>/.user_skills/` 或 `~/.codex/skills/`，以你运行时约定的目录为准。
2. 重启或刷新 Agent，使其重新扫描 skills。
3. 运行时会读取 `SKILL.md` 头部的 `name` 与 `description` 自动识别触发。
4. 若使用兼容 OpenAI Agents 的运行时，可直接加载 `make-knowledge-cards/agents/openai.yaml` 作为声明式 Agent 定义。

不需要 `pip install`，也不需要构建步骤。

## 使用方法

对 AI 助手说一句话，并附上内容来源（粘贴正文，或给出本地 `.md` / `.txt` 文件路径）：

- “把下面这篇文章做成知识卡片：……”
- “请用知识卡片整理一下 `examples/photosynthesis.txt`”
- “这篇文章讲几个要点，帮我做成复习卡片”

默认直接在回复里输出全部卡片。若要求保存，则生成一个 Markdown 文件，每张卡片以 `## 卡片 N：标题` 开头。

### 卡片格式

```
卡片 N：〈一句话标题〉

【核心知识】一句话，直接陈述该知识点是什么。

【简明解释】2~4 句，用更通俗的话把核心知识讲清楚。

【例子 / 自测问题】来自原文的例子，或一个能检验是否掌握的自测问题。
```

## 输入输出示例

### 输入：`examples/photosynthesis.txt`（节选）

> 光合作用是植物、藻类和一些细菌利用光能把二氧化碳和水合成有机物、并释放氧气的过程。…… 光合作用分为两个阶段。第一阶段叫光反应，发生在类囊体膜上，需要光照…… 第二阶段叫暗反应（卡尔文循环），发生在叶绿体基质中，不需要光……

### 输出：`examples/outputs/photosynthesis-cards.md`（节选）

```
卡片 4：两个阶段——光反应与暗反应

【核心知识】光合作用分两阶段：光反应在类囊体膜上进行、需要光，分解水并释放氧气、生成 ATP 和 NADPH；暗反应（卡尔文循环）在叶绿体基质中进行、不需要光，利用 ATP 和 NADPH 固定并还原二氧化碳生成有机物。

【简明解释】可以理解为两步接力：第一步"光反应"靠阳光把水拆开，顺便产出能量物质；第二步"暗反应"不用阳光，用第一步产出的能量把二氧化碳拼成葡萄糖。

【自测问题】光反应和暗反应分别发生在叶绿体的哪个部位？暗反应需要光照吗？
```

完整示例见 `examples/outputs/`，覆盖四类输入：

| 输入文件 | 文本类型 | 输出表现 |
| --- | --- | --- |
| `photosynthesis.txt` | 科普说明文 | 提炼出 7 张卡片 |
| `time-management.txt` | 含重复内容 | 去重后仅 4 张，不凑数 |
| `cat-sleep.txt` | 信息不足短文 | 仅 3 张，并说明原因 |
| `tortoise-hare.txt` | 叙事文 | 只取明说事实，不推断寓意 |

## 暂不支持的输入

- 网页 URL（不做网页抓取）；
- PDF；
- 导出 Anki 卡片；
- 图形界面操作。

遇到上述输入时会直接说明并拒绝，而不是勉强处理。

## 目录结构

```
first -skill/
├── README.md                          # 本说明
├── make-knowledge-cards/              # Skill 本体
│   ├── SKILL.md                       # Skill 定义与执行规则
│   └── agents/
│       └── openai.yaml                # 兼容 OpenAI Agents 的声明式 Agent 定义
└── examples/                           # 示例输入与对应输出
    ├── photosynthesis.txt / time-management.txt / cat-sleep.txt / tortoise-hare.txt
    └── outputs/                        # 四份输入对应的知识卡片
```

## 开发与校验

用官方 `skill-creator-for-work` 的 `init_skill.py` 初始化，`SKILL.md` 与 `agents/openai.yaml` 规则保持一致。校验：

```bash
python <skill-creator>/scripts/quick_validate.py make-knowledge-cards
```

输出 `Skill is valid!` 即通过。本项目仅做本地开发与测试，暂未初始化 Git，也未上传 GitHub。
