# 这个 skill 是怎样制作的

这是第一版 `preserve-realism` 的制作讲解，帮助你以后自己修改或制作新的 skill。

## 1. 先定义可观察的工作

项目目标包括写提示词、指导编辑和检查素材。每项工作都有可观察的输出，不能把“保留真实感”写成无法验证的效果承诺。

例如，“换背景时仍然像本人”需要原图、结果图、修改范围和具体对比。“变高清”则要分别核对输出尺寸、主体细节及有没有新瑕疵。

这些决策已经在 [项目策划](PROJECT_PLAN.md) 中固定下来。

## 2. 理解必需的文件

一个 skill 的最小结构是一个文件夹和其中的 `SKILL.md`。元数据和指令写在同一个文件里，详细的条件流程可放进参考文件。格式见 [Agent Skills 规范](https://agentskills.io/specification)。

本项目的主文件是 [SKILL.md](../skills/preserve-realism/SKILL.md)。它的开头包含：

```yaml
---
name: preserve-realism
license: MIT
description: >-
  Preserve photographic realism in image generation, photo editing, and video
  prompting. Rewrite prompts, guide edits, and review supplied results for
  cartoonized faces, plastic skin, lost texture, identity drift, and temporal
  consistency. Use for 写实图片、真人保真、皮肤纹理、去塑料感 and 视频人物一致性;
  respect intentional stylization and region-specific styles.
metadata:
  version: "0.1.0"
---
```

- `name`：skill 的名称，与文件夹名称一致。
- `description`：说明它做什么、什么时候适合使用，帮助助手选择它。
- `license`：声明采用的许可证，完整条款放在仓库根目录。
- `metadata.version`：记录当前项目版本，本版为 `0.1.0`。

修改这几个字段不等于改变实际行为；正文和参考文件才规定如何处理任务。

## 3. 把正文写成决策规则

重点写那些会改变助手选择的信息：

| 决策问题 | 本项目的处理 |
| --- | --- |
| 用户没有指定模型 | 先给通用提示词，具体参数按需核对 |
| 编辑人物照片 | 区分修改项、稳定身份和允许变化的状态 |
| 原图很模糊 | 区分可见细节与推测补充，不承诺真实恢复 |
| 没有结果素材 | 给待执行清单，不报告“通过检查” |
| 视频只有截图 | 可以检查这些帧，连续运动仍无法确认 |
| 用户明确要求卡通 | 尊重风格；混合风格按区域表达 |

“认真工作”“提高质量”很难帮助助手选择；上面的条件与对应行动才便于执行和测试。

## 4. 将条件流程放进参考文件

本项目用三份文件支持不同任务：

1. [图片工作流](../skills/preserve-realism/references/image-workflows.md)：景别、材质、真人编辑、尺寸与增强。
2. [视频一致性](../skills/preserve-realism/references/video-consistency.md)：动作、镜头、时间变化及检查覆盖范围。
3. [示例与检查](../skills/preserve-realism/references/examples-and-review.md)：输入输出例子、可见证据与修订方法。

主文件明确何时读取它们。新加内容时，先判断它属于共用规则还是某个分支；避免在多个文件重复同一套要求。

第一版没有执行脚本，因为现阶段的工作可以用指令、现有工具和人工查看完成。以后出现稳定、重复的文件处理需求，再考虑脚本。

## 5. 理解可选的界面元数据

[agents/openai.yaml](../skills/preserve-realism/agents/openai.yaml) 包含显示名称、简短介绍和默认任务提示。

核心规则不依赖它，其他助手按自身能力处理。它没有声明额外工具依赖，也没有关闭默认自动选择。OpenAI 产品中的相关机制见 [官方 skill 文档](https://learn.chatgpt.com/docs/build-skills)。

## 6. 怎么检查修改是否合理

分开做两类检查：

- **文件检查**：元数据能解析、名称一致、参考文件存在、指令没有未完成模板、文本编码正确。
- **行为与效果检查**：用新请求观察是否选对流程，再检查实际生成或编辑的素材。

使用 [测试场景](TEST_CASES.md) 进行实际试用。格式检查通过并不证明工具都兼容，也不证明画质一定改善。

拥有 Codex 自带 skill-creator 的环境，可以调用其 `quick_validate.py` 检查 skill 文件夹。该脚本的位置以当地安装为准；它不是本项目必需的运行依赖。

对于原图和改写后的提示词，尽可能在相同条件下比较多个生成结果。记录模型版本、参考素材和实际设置，而不是只保留最好的一张。

## 7. 怎样做一次有效修改

例如，测试发现“把原人物改为微笑”时，助手写出了“表情完全不变”。

应修正的是：区分稳定身份与表情需要的变化，并增加相应测试。没有必要据此让所有任务都先问十个问题，或禁止所有面部编辑。

以后遇到问题，按这个顺序处理：

1. 记录具体输入、输出和可观察的问题。
2. 找到导致错误选择的规则或遗漏条件。
3. 修改相关位置，保持其他有效规则。
4. 重做相关场景，确认没有损害已有用途。
5. 有实际效果记录后，再更新 README 的验证状态。

## 8. 怎样准备发布

skill 本体和 GitHub 说明承担不同作用：前者给助手执行，后者帮助读者理解、安装和贡献。

本次已经制作了 README、MIT 许可证、测试场景和发布教程。下一步按 [GitHub 发布教程](PUBLISH_TO_GITHUB.md) 创建仓库并上传，上传时保持现有目录关系。

发布前你还可以调整名字、版权署名和说明文字。不要把未经测试的效果示例改写成已经验证的结论。
