# Preserve Realism · 写实素材与实景拼贴

为 AI 助手提供摄影保真与实景拼贴两种工作流：改写提示词、指导生成和编辑，并根据实际素材检查人物、材质、构图与视觉表达。

图片为主，兼顾视频。摄影工作流处理卡通化、塑料皮肤、纹理丢失与人物漂移；拼贴工作流用摄影主体、顺景撕纸、抽象延伸和留白表达情绪。

仓库版本：`0.2.0` · 许可证：[MIT](LICENSE)

| Skill | 版本 | 适用任务 |
| --- | --- | --- |
| [preserve-realism](skills/preserve-realism/SKILL.md) | `0.1.1` | 写实生成、真人编辑、自然纹理与人物构图 |
| [scene-zine-poster](skills/scene-zine-poster/SKILL.md) | `0.1.0` | 根据照片或场景及情绪，创作实景拼贴海报 |

两个 skill 可分别使用。海报中的摄影部分仍保持真实，新增纸面和插画区域可以有明确的平面风格。

## 快速使用

安装后，在支持相应调用方式的助手里提出任务：

```text
使用 preserve-realism 改写下面的提示词，让人物保持摄影真实感和自然皮肤细节。
原提示词：一个坐在咖啡馆窗边的人，半身构图，柔和窗光。
只给我可直接复制的中文提示词。
```

编辑已有照片时，提供原图和明确的修改要求：

```text
使用 preserve-realism。以我附的原图为身份参考，只换成街道背景。
保留面貌和可见皮肤纹理，必要的环境光可以调整。
先给编辑提示词；我提供结果后，再帮我对比检查。
```

希望人物更突出时，明确景别和背景权重，例如“腰部以上构图，人物放大，保留湖面与蓝天线索，背景取景收紧”。

实景拼贴海报：

```text
使用 scene-zine-poster，处理我附上的照片。
主题是湖边人物，情绪是宁静，人物要突出，横版 16:9。
保留人物的摄影质感，用顺应场景的撕纸、留白和一种强调色组织画面。
```

结果检查可以单独使用。提供原图和结果图时，说明它们各自的用途；如果只提供结果，助手可以检查摄影外观，但不能证明与某个原始人物一致。

## 能做什么

| 任务 | 输出 |
| --- | --- |
| 从零生成写实图片 | 主体、构图、光线和相关材质要求组成的提示词 |
| 编辑真人照片 | 修改项、保留项及必要的参考图和区域控制建议 |
| 改写已有提示词 | 保留画面意图，解决风格冲突与模糊要求 |
| 检查结果 | 基于实际可见素材的观察和修订建议 |
| 视频提示词与检查 | 动作、镜头、人物连续性约束及回看方法 |
| 实景拼贴海报 | 主体与情绪、撕口逻辑、摄影保留区、跨界图形和纸面留白 |

像素尺寸、可用清晰度、自然真实感和身份保真分别处理。提示词中写“8K”不会替代文件尺寸核对；生成性增强的新细节也不能当作原始纹理的真实恢复。

## 通用设计

核心使用标准 `SKILL.md` 与 Markdown 参考文档，不绑定单一图片模型，不依赖密钥或专有参数。格式依据 [Agent Skills 规范](https://agentskills.io/specification)。

支持原生 skill 的助手可以读取文件夹；其他聊天工具可以手动接收指令；仅接受生成提示词的工具则使用助手输出的文本和参考图。具体参数按工具实际能力适配。

**状态说明：**本地格式检查与实际视觉试用分别记录，见 [0.2.0 验证记录](docs/VALIDATION_V0.2.0.md)。首张云龙湖游客照曾出现主体偏小、背景取景过宽的问题，`0.1.1` 已补充相应的构图判断。符合开放格式不代表每个平台都具备原生 skill 功能。

## 安装

### Codex：项目内安装

将需要使用的 skill 完整文件夹复制到目标项目的 `.agents/skills/`。安装两种工作流时，分别复制 `skills/preserve-realism/` 和 `skills/scene-zine-poster/`。原有摄影 skill 的结构如下：

```text
你的项目/
└── .agents/
    └── skills/
        └── preserve-realism/
            ├── SKILL.md
            ├── LICENSE
            ├── agents/
            │   └── openai.yaml
            └── references/
                ├── image-workflows.md
                ├── video-consistency.md
                └── examples-and-review.md
```

从该项目启动 Codex，使用自然语言请求，或显式提及：

```text
$preserve-realism 帮我改写这个写实人物提示词，保留自然纹理。
```

海报工作流的路径为 `.agents/skills/scene-zine-poster/SKILL.md`，其余文件与参考文档一并复制，调用示例：

```text
$scene-zine-poster 使用这张照片创作实景拼贴海报，情绪是自由。
```

如果新增 skill 没有出现，重新启动客户端并核对加载位置。项目内加载路径及调用说明见 [Codex 官方文档](https://learn.chatgpt.com/docs/build-skills)。已有同名目录时，先比较内容再决定如何更新，避免直接覆盖个人修改。

### Claude Code：项目内安装

将需要的完整文件夹复制到目标项目的 `.claude/skills/`，目录名分别为 `preserve-realism` 和 `scene-zine-poster`。例如：

```text
/preserve-realism 帮我写一个自然、真实的人物摄影提示词。
/scene-zine-poster 用这张照片创作宁静情绪的实景拼贴海报。
```

加载与调用方式见 [Claude Code 官方文档](https://code.claude.com/docs/en/skills)。`agents/openai.yaml` 是可选的 OpenAI 界面元数据，核心规则不依赖它。

### 其他聊天或生成工具：手动使用

向能阅读文档的助手提供 `SKILL.md` 正文，以及本次任务所需的参考文件；可上传相关文件，也可以粘贴内容。只提供带路径的主文件，但助手无法读取对应文件时，需要同时提供参考内容。

然后说明：

```text
请依据我提供的 Preserve Realism 工作流处理下面的任务。
任务：给原照片换衣服，保持人物面貌和自然皮肤纹理。
只生成编辑提示词与必要的操作建议，先不执行生成。
```

把最终提示词和实际需要的参考图送入生成工具。模型支持哪些输入或参数，以该工具文档和界面为准。

## 文件结构

```text
preserve-realism-skill/
├── README.md
├── LICENSE
├── .gitignore
├── docs/
│   ├── PROJECT_PLAN.md
│   ├── HOW_IT_WORKS.md
│   ├── PUBLISH_TO_GITHUB.md
│   ├── TEST_CASES.md
│   ├── VALIDATION.md
│   ├── VALIDATION_V0.2.0.md
│   └── SCENE_ZINE_DESIGN.md
└── skills/
    ├── preserve-realism/
    │   ├── SKILL.md
    │   ├── LICENSE
    │   ├── agents/openai.yaml
    │   └── references/
    │       ├── image-workflows.md
    │       ├── video-consistency.md
    │       └── examples-and-review.md
    └── scene-zine-poster/
        ├── SKILL.md
        ├── LICENSE
        ├── agents/openai.yaml
        └── references/art-direction.md
```

各 skill 的参考文档按任务读取；仓库的说明与教程用于人类阅读，不需要全部加载进助手上下文。

## 如何继续改进

先用 [测试场景](docs/TEST_CASES.md) 检查触发、任务范围和素材依据，再用实际结果检验效果。反馈时记录任务、提示词、工具版本、可控设置、观察到的问题和素材覆盖范围。

修改关键决策规则，而不是给每次失败追加一长串禁用词。只有经过测试的兼容环境和效果案例，才标为已验证。

## 制作与发布教程

- [这个 skill 是怎样制作的](docs/HOW_IT_WORKS.md)：主文件、触发描述、参考文件与测试的作用。
- [发布到自己的 GitHub](docs/PUBLISH_TO_GITHUB.md)：网页上传步骤，以及可选 Git 方式。
- [项目策划](docs/PROJECT_PLAN.md)：需求、设计和第一版完成标准。
- [拼贴工作流的蒸馏说明](docs/SCENE_ZINE_DESIGN.md)：视觉判断与来源说明。

## 许可证

用户已为本项目选择 MIT。允许使用、修改和分发，包括商业用途，需要保留版权和许可声明。具体条款以 [LICENSE](LICENSE) 为准；许可摘要可参考 [MIT 说明](https://choosealicense.com/licenses/mit/)。

版权行采用项目署名 `Preserve Realism contributors`；维护者可在发布前换成希望公开使用的署名。以后增加示例素材时，单独说明素材来源和适用授权。

`scene-zine-poster` 根据用户提供的 Gathered Scenes Zine v1.3 规则独立整理，保留来源说明；它不是原项目的官方更新，仓库也未验证原帖中关于各平台功能的宣传。
