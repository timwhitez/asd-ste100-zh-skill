# asd-ste100-zh — 中文技术清晰写作

基于 [danyuchn/asd-ste100-skill](https://github.com/danyuchn/asd-ste100-skill) 的最小中文适配。默认输出自然、简洁的中文，适用于技术说明、操作步骤、技术报告、错误信息和工具描述。保留清晰术语、明确主体与动作，并把必要条件和风险放在对应操作前。

这是独立中文清晰写作适配，**不是 ASD 官方中文标准、认证翻译或 ASD-STE100 符合性保证**。没有可测的“80% 合规”评分。需要官方符合性时，使用合法取得的官方标准和适当的人工评审。

## 安装

安装到 Codex 的技能目录（已存在同名目录时，先检查，勿覆盖）：

```bash
git clone https://github.com/timwhitez/asd-ste100-zh-skill.git "${CODEX_HOME:-$HOME/.codex}/skills/asd-ste100-zh"
```

Claude Code 可使用：

```bash
git clone https://github.com/timwhitez/asd-ste100-zh-skill.git ~/.claude/skills/asd-ste100-zh
```

仓库根目录就是技能目录，入口为 [SKILL.md](SKILL.md)，技能名为 `asd-ste100-zh`。也可把整个仓库复制到项目的 `.agents/skills/asd-ste100-zh/`；保留 `scripts/`、`references/`、`examples/` 和 `LICENSE` 的相对位置。安装后重新加载客户端的技能列表。以上步骤仅说明安装，没有执行全局安装。

## 用法

```text
用 $asd-ste100-zh 改写下面的技术说明，默认中文输出。
把这些操作步骤写清楚，保留全部条件、风险、数值和单位。
用 asd-ste100-zh 消除歧义，并给出原文与改写的对比。
```

默认只返回可直接使用的正文。需要解释时，要求“说明修改依据”或“展示前后对比”。英文片段、代码、命令、路径、标识符和原文引用不会擅自翻译。用户明确要求纯英文改写时，才使用保留的上游 English-only 分支（Strict / STE-flavored）。

| 原文 | 改写 |
|---|---|
| 在备份完成且校验通过的情况下应当删除源文件，不过只适用于 /tmp/cache。 | 仅当备份完成且校验通过时，应当删除 `/tmp/cache` 中的源文件。 |
| 请求可能因为客户端版本过旧而失败，目前还没有确认原因。 | 请求可能失败。客户端版本过旧可能是原因。目前尚未确认原因。 |

更多示例见 [examples/before-after.md](examples/before-after.md) 的中文部分。

## 中文适配的边界

- 取消中文不适用的英文词数、词性、时态、短语动词和受控词典要求；不把 20 words 机械换成 20 个汉字，也不一律禁止中文分号。
- 保留条件的组合关系、顺序、否定、例外、范围、风险、数值单位、标识符，以及“必须 / 应当 / 可以”“可能 / 通常 / 已确认”的强度与置信度。
- 不凭空补充原因、频率、机制、指标或操作。原文缺失必要信息时指出问题。新增建议需要用户要求，并与改写分开标明。
- 不用于创意或营销文案，不处理图像或视频生成，不保证改写后的事实为真。

## 英语检查器

保留的 [scripts/ste-lint.py](scripts/ste-lint.py) 仅检查英语启发式结构模式，**不检查中文**，不检查语义保真，也不检查官方词典。CLI 输入含汉字时，整个输入返回退出码 **2** 与 `not_checked`，避免把中文未检查误当成功；混合文本中的英语需要单独提取后检查。代码或引用里的汉字也会触发该保守边界，它不是完整语言识别器。内部 `lint()` 函数保留上游英语算法，没有这层 CLI 保护，不得作为中文检查接口。

纯英文退出码 0 仅表示配置的检查未发现硬问题；退出码 1 表示硬问题超出 baseline。两者都不是官方符合性或语义保真结论。

```bash
python3 scripts/ste-lint.py --selftest
python3 scripts/ste-lint.py examples/linter-edge-cases.md --json
```

第二条用于检查上游的故意错误样例，预期退出码 1、两条 `dangling-conjunction` 发现。简明验证记录见 [VALIDATION.md](VALIDATION.md)。

## 来源与许可

- 上游：[danyuchn/asd-ste100-skill](https://github.com/danyuchn/asd-ste100-skill)
- 锁定基线：[`7d4a135a199a5d7447c4886bcd7ffe742a627bc9`](https://github.com/danyuchn/asd-ste100-skill/tree/7d4a135a199a5d7447c4886bcd7ffe742a627bc9)，SKILL.md v0.4.0。
- 上游 SKILL.md blob：`35579a421ff9b93452ecaf9dd01bbe6e51decff3`。
- 上游 Git 历史与原 [MIT LICENSE](LICENSE) 完整保留：Copyright (c) 2026 Dustin Yuchen Teng。新增中文说明与代码改动同样以 MIT 许可发布。
- [references/writing-rules.md](references/writing-rules.md) 保留上游英文规则类别概述与来源，仅供英文分支参考；不是官方全文或当前词典的核验记录。
- [ASD 官方网站](https://www.asd-ste100.org/)与 [FAQ](https://www.asd-ste100.org/STE_faq.html)。本仓库不打包官方规范 PDF、完整词典或未知许可正文。仓库 MIT 许可不授予这些第三方材料的再分发权。
