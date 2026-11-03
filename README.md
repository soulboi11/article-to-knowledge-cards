# make-knowledge-cards

`make-knowledge-cards` 是一个用于 Codex 的开源 skill，把用户粘贴的文章或本地 Markdown/TXT 文件转换成便于复习和自测的知识卡片。

## 解决的问题

普通摘要容易把多个概念挤在同一段里，也可能重复原文、遗漏条件或补充原文没有的信息。本项目专注于从素材中提取真正可独立学习的知识点：

- 一张卡片只讲一个知识点
- 合并重复或同义内容
- 保留原文中的条件和限定词
- 不编造人物、数字、机制、例子或结论
- 素材不足 5 个独立知识点时，只输出实际可支持的数量

## 主要功能

- 支持直接粘贴文章，或读取本地 `.md`、`.txt` 文件
- 提取定义、机制、原因、区别、条件、流程、规则和结论等知识单元
- 按学习顺序输出 5-8 张卡片；素材不足时自动减少数量
- 每张卡片包含标题、核心标识、简明解释、例子或自测
- 输出中文或与素材、用户要求一致的语言
- 输出格式为 Markdown，可直接用于阅读、复制和二次编辑

暂不支持网页抓取、PDF、图片/OCR、Anki 导出和图形界面。需要处理 URL 或 PDF 时，请先导出或粘贴为纯文本，并保存为 `.md` / `.txt`。

## 安装方法

将仓库中的 `skills/make-knowledge-cards/` 目录复制到 Codex 的 skills 目录。若设置了 `CODEX_HOME`，目标目录是 `$CODEX_HOME/skills/`；否则使用 `~/.codex/skills/`。

在项目根目录执行。PowerShell：

```powershell
$codexHome = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { Join-Path $HOME '.codex' }
$target = Join-Path $codexHome 'skills\make-knowledge-cards'
New-Item -ItemType Directory -Force -Path $target | Out-Null
Copy-Item -Path '.\skills\make-knowledge-cards\*' -Destination $target -Recurse -Force
```

macOS / Linux：

```bash
codex_home="${CODEX_HOME:-$HOME/.codex}"
mkdir -p "$codex_home/skills/make-knowledge-cards"
cp -R ./skills/make-knowledge-cards/. "$codex_home/skills/make-knowledge-cards/"
```

skill 本身不包含脚本或额外运行时依赖。安装后重新打开 Codex 会话，并使用 `$make-knowledge-cards` 调用。

## 使用方法

处理粘贴的文章：

```text
使用 $make-knowledge-cards 把下面的文章转换成知识卡片：
<粘贴文章>
```

处理本地文件：

```text
使用 $make-knowledge-cards 处理 ./notes/article.md
```

还可以指定输出语言或练习形式：

```text
使用 $make-knowledge-cards 处理 ./notes/article.txt，用中文输出，优先使用自测题。
```

## 输入输出示例

输入：

```text
咖啡因通过阻断腺苷受体发挥提神作用。腺苷在清醒时间逐渐积累，并与受体结合产生困倦感。咖啡因的结构与腺苷相似，会占据受体；它并不直接提供能量。长期摄入后突然减少时，可能出现头痛和疲劳等戒断症状。
```

输出：

```markdown
## 知识卡片（4 张）

原素材仅支持 4 个独立知识点，因此未凑足 5 张。

## 1. 咖啡因如何提神
- **核心标识**：占据腺苷受体
- **简明解释**：咖啡因的结构与腺苷相似，因此会占据腺苷受体并阻断腺苷发挥作用。
- **例子或自测**：自测：用原文中的结构相似性解释咖啡因为何能作用于腺苷受体。

## 2. 困倦感如何出现
- **核心标识**：腺苷积累与受体结合
- **简明解释**：清醒时间越长，腺苷积累越多；腺苷与受体结合后会产生困倦感。
- **例子或自测**：自测：原文怎样描述清醒时间与困倦感之间的联系？

## 3. 咖啡因不直接提供能量
- **核心标识**：不是能量来源
- **简明解释**：咖啡因通过受体机制发挥提神作用，它并不直接向身体提供能量。
- **例子或自测**：自测：咖啡因增加的是能量本身吗？（参考要点：不是，它通过受体机制起作用。）

## 4. 突然减少摄入后的反应
- **核心标识**：戒断症状
- **简明解释**：长期摄入后突然减少时，可能出现头痛和疲劳等戒断症状。
- **例子或自测**：自测：原文列举了哪两种可能出现的戒断症状？
```

## 验证

使用官方 skill 校验脚本检查结构：

```bash
python "$CODEX_HOME/skills/.system/skill-creator/scripts/quick_validate.py" skills/make-knowledge-cards
```
