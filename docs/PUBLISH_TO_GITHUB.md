# 发布到自己的 GitHub

本教程对应 `preserve-realism-skill` 第一版，适合先用网页完成发布。操作依据在 2026-10-15 核对的 GitHub 官方文档。

`0.2.0` 增加 `skills/scene-zine-poster/` 及相关文档，实际目录以新版 README 为准。更新已有仓库时，上传新版中需要更新的内容，保留 `skills/` 层级。

## 1. 准备上传内容

下载项目压缩包并解压，打开 `preserve-realism-skill` 文件夹。仓库根目录应包含 `README.md`、`LICENSE`、`.gitignore`、`docs/` 和 `skills/`。

`skills/preserve-realism/` 是可供安装的 skill 文件夹；其他文件用于项目说明、许可和教程。

本项目已按你的选择采用 MIT。版权署名现在为 `Preserve Realism contributors`，可以在发布前改成你的公开名称或 GitHub 用户名。仓库根目录和 skill 文件夹内各有一份 `LICENSE`；改署名时同步更新。

MIT 的使用、修改和分发权限及保留声明要求，以许可证全文为准。[MIT 条款与摘要](https://choosealicense.com/licenses/mit/)

## 2. 创建仓库

登录 GitHub，打开 [创建仓库页面](https://github.com/new)，填写：

| 字段 | 建议内容 |
| --- | --- |
| Owner | 你的个人账号，或你选择的组织 |
| Repository name | `preserve-realism-skill`，也可改成你喜欢的名字 |
| Description | `A portable skill for photographic realism, face preservation, and natural texture.` |
| Visibility | 希望公开分享时选择 Public；也可先用 Private 整理 |
| Add README | 开启，创建后用本项目 README 替换初始内容 |
| License | 本地包已包含 MIT 文件，创建时可留空 |

点击 **Create repository**。官方流程见 [GitHub 仓库创建指南](https://docs.github.com/en/repositories/creating-and-managing-repositories/quickstart-for-repositories)。

## 3. 上传文件与文件夹

在仓库首页选择 **Add file → Upload files**。打开本地解压后的项目文件夹，将其中的文件与 `docs/`、`skills/` 文件夹拖入上传区域。

`choose your files` 打开的通常是文件选择框；文件夹使用 Windows 文件资源管理器拖入网页上传区域。

在预览列表中核对路径：

```text
README.md
LICENSE
.gitignore
docs/HOW_IT_WORKS.md
docs/PUBLISH_TO_GITHUB.md
docs/PROJECT_PLAN.md
docs/TEST_CASES.md
docs/VALIDATION.md
skills/preserve-realism/SKILL.md
skills/preserve-realism/LICENSE
skills/preserve-realism/agents/openai.yaml
skills/preserve-realism/references/image-workflows.md
skills/preserve-realism/references/video-consistency.md
skills/preserve-realism/references/examples-and-review.md
```

本地文件夹名称用于组织下载包；上传时选择它内部的内容，让 README 直接位于仓库根目录。

网页上传会提交所选文件，`.gitignore` 的存在不会替你过滤拖入网页的内容。当前包只有项目文档和 skill；后续新增素材时，选择适合公开分享的文件。

提交说明可填写：

```text
Add Preserve Realism skill v0.1.0
```

根据仓库权限与分支设置，提交到允许的分支，或选择新分支并创建 Pull Request。若使用新分支，合并后文件才进入默认分支。按钮名称可能显示 **Commit changes** 或 **Propose changes**。参考 [GitHub 文件上传指南](https://docs.github.com/en/repositories/working-with-files/managing-files/adding-a-file-to-a-repository)。

## 4. 检查仓库首页

上传或合并后确认：

- README 展示在首页，其中的相对链接可以打开。
- `skills/preserve-realism/SKILL.md` 存在，参考文件和许可文件齐全。
- LICENSE 显示预期署名和 MIT 条款；许可证识别以 GitHub 实际显示为准。
- 验证状态仍准确：本地文件检查与实际生成效果测试分别记录。

此时可以分享仓库页面。仓库网址就是你自己账号下的真实地址，无需编造一个作者名放进文档。

## 5. 告诉别人如何使用

可以这样介绍：

```text
我发布了 Preserve Realism，一个写实素材保真 skill。
它帮助改写提示词、指导真人照片编辑，并检查实际提供的素材。
用法在仓库 README 中：支持 skill 的助手安装完整文件夹，其他工具手动使用提示词工作流。
```

附上实际仓库链接。安装者可以下载仓库，将 `skills/preserve-realism/` 复制到对应助手的加载目录；README 已说明 Codex、Claude Code 和手动使用方式。

仓库公开后，Codex 用户也可以请求自带的 skill-installer 从你提供的实际 GitHub 地址安装该文件夹。安装目标与调用行为按使用者环境核对。[Codex 安装说明](https://learn.chatgpt.com/docs/build-skills)

GitHub 文件发布与产品插件目录分发是不同步骤。本教程完成前者；需要插件分发时再按目标产品准备打包配置。

## 6. 可选：用 Git 命令行上传

以下命令供你手动执行。先按网页步骤创建并初始化仓库，在一个合适的本地目录打开 PowerShell，然后粘贴从仓库复制的 HTTPS 地址：

```powershell
$repoUrl = Read-Host '粘贴你的仓库 HTTPS 地址'
git clone $repoUrl preserve-realism-publish
```

克隆目录名是本地名称。如果已有同名目录，换一个新的目录名即可。把解压包中的 `README.md`、`LICENSE`、`.gitignore`、`docs/` 和 `skills/` 复制到克隆目录内。

进入该目录，检查并提交本次内容：

```powershell
Set-Location -LiteralPath '.\preserve-realism-publish'
git status --short
git add -- README.md LICENSE .gitignore docs skills
git diff --cached --stat
git commit -m 'Add Preserve Realism skill v0.1.0'
git push
```

若 Git 要求设置作者身份，按你的公开署名和提交邮箱进行本仓库设置；若要求登录，按当前 Git 认证流程完成。分支受保护时，使用新分支和 Pull Request。

此方式从已初始化的远程仓库克隆，保留远程历史。后续更新也可在克隆目录修改、检查、提交和推送。[GitHub 命令行文件提交指南](https://docs.github.com/en/repositories/working-with-files/managing-files/adding-a-file-to-a-repository)

## 7. 第一次发布后的改进

用 [测试场景](TEST_CASES.md) 在你常用的助手中试用，记录真实环境和结果。完成实际图片或视频对比后，再补充合适的公开案例，并更新 [验证说明](VALIDATION.md)。

后续版本更新 `SKILL.md` 的版本元数据和 README，保留测试记录。只有准备好正式版本时，才需要另外创建 GitHub Release；创建仓库并上传文件已经可以分享和安装。
