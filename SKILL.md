---
name: asd-ste100-zh
description: "默认用中文撰写或改写清晰、无歧义的技术说明、操作步骤、错误信息、技术报告和工具描述。保留术语、条件、风险、义务强度和事实；适用于简化技术输出、消除歧义或明确要求 asd-ste100-zh 的任务。不用于创意或营销文案，不提供 ASD-STE100 中文合规认证。"
metadata:
  version: 0.1.0
  upstream-version: 0.4.0
---

# 中文技术清晰写作（ASD-STE100 原则适配）

源自 [danyuchn/asd-ste100-skill](https://github.com/danyuchn/asd-ste100-skill)，基线版本 0.4.0，提交 `7d4a135a199a5d7447c4886bcd7ffe742a627bc9`。上游版权与 MIT 许可见 [LICENSE](LICENSE)。本项目是独立中文清晰写作适配，不是 ASD 官方中文标准、认证翻译或 ASD-STE100 符合性保证。

## 语言与适用范围

默认输出中文，适用于技术说明、操作步骤、技术报告、错误信息和工具描述。用户明确指定输出语言时，遵循该要求。不要擅自翻译代码、命令、路径、标识符、产品名称、英文原文引用或指定保留的英文片段；解释文字可用中文。领域术语有通行中文译法时可使用，必要时首次附英文；同一概念保持同一名称，不把不同概念合并成同一个词。

中文或中英混合输出使用下面的中文规则与流程。**跳过后面的英文规则、Strict / STE-flavored 模式及其英文示例**；其中的英语词数、词性、时态、短语动词、受控词典限制不适用于中文。不要把 20 words 换成 20 个汉字。仅当用户明确要求纯英文改写时，使用后面的英文分支。该分支也不能保证官方 STE 合规。

默认自然简洁，以消除歧义为目标，不逐句机械套用规则。操作说明和安全文本需要更严格地检查条件、动作和风险。

## 中文改写规则

- 先理解原意，再简化表达。保留事实、条件、否定、范围、例外、顺序、比较关系、数值、单位和标识符。没有依据时，不补充原因、频率、机制或操作。
- 明确谁做什么，优先使用直接动词，例如“检查日志”，而不是“进行日志的检查”。原文未说明执行者时，不凭空指定；能从上下文确定才补全。
- 每句集中表达一个动作或主要判断。必要条件写在对应动作前，例如“仅当备份完成且校验通过时，才删除源文件”。拆句时保留“且 / 或 / 仅当 / 除非”等逻辑关系，避免把条件写成无条件命令。
- 把风险与禁止事项放在相关操作前。保留风险的严重性、发生条件与可能性；不得为简短而删除安全限制。原文缺少必要安全条件时，标明缺失，不编造安全操作规程。
- 保留要求强度和置信度：“必须 / 不得”“应当 / 建议”“可以”“可能 / 通常 / 已确认”各有含义。不要把“应当”改成强制命令，把“可能”改成必然，或把“可以”改成“必须”。只有同义且不损失信息时才删除重复缓和语。
- 使用准确、常用的词；必要专业术语保留并按读者需要解释。不要为了口语化而换掉有特定技术含义的名称。保留表达时间、状态和不确定性所需的“已 / 正在 / 将 / 曾”等信息。
- 一段集中讨论一个主题。多步骤操作按执行顺序列出；并列条件保持其组合关系。优先拆开过密句子，但不设机械字数上限；中文分号并非一律禁止。精度优先于短句。
- 删除没有信息的套话；不要把未量化的评价改写成凭空编造的指标。对不清楚的原文，保留歧义并指出需要澄清的关键点，不猜测作者想法。

## 中文流程与输出

1. 阅读原文，确认主体、动作、条件、风险、范围和要求强度。写作任务以用户提供的事实为依据。
2. 按中文规则改写。参考 [examples/before-after.md](examples/before-after.md) 的中文示例；英文示例不是中文规则。
3. 将改写逐项对照原文，检查事实和逻辑是否保留，特别是条件的作用范围、否定、例外、数值单位、引用和置信度。仅靠词匹配不能完成这一步。
4. 默认只输出可直接使用的中文正文。不添加模式说明、违规数量或“合规 / PASS”结论。原文已经清楚时，保留原文；若用户询问检查结果，可说明无需修改。
5. 必须保留较长表达以避免信息损失时，可在正文后加一行“保留说明：……”。存在阻碍准确改写的歧义或缺失信息时，简短说明问题；必要时向用户澄清。
6. 用户要求对比或解释时，用“调整依据 / 原文 / 改写”表格说明中文规则与具体变化，避免宣称违反了官方中文 STE 规则。用户要求补充建议时，把新增建议明确标为建议，与原文改写分开。

`scripts/ste-lint.py` 仅是英语启发式检查器，不检查中文语义或中文合规性。含汉字输入的 CLI 检查会返回退出码 2（未检查），英文片段需单独提取后检查。其 `lint()` 内部函数仍是上游英语算法，不是中文检查接口。纯英文返回 0 也只表示配置的英语结构检查未发现硬问题，不代表保真、词典检查或官方符合性。不要为中文自动生成合规证书。

本仓库不提供官方规范全文或受控词典。MIT 覆盖本仓库可许可的代码与说明，不授予 ASD 官方规范、词典、PDF 或第三方材料的再分发权。需要官方符合性时，使用从官方合法取得的标准和适当的人工评审。

---

# English-only branch — Simplified Technical English (ASD-STE100)

以下内容保留上游英文方法与引用，仅用于用户明确要求的纯英文改写。中文与中英混合输出到此为止，不把下面的英语规则作为中文约束。

ASD-STE100 is a controlled-language standard built by the aerospace and defense industry (ASD, the AeroSpace and Defense Industries Association of Europe) to stop maintenance technicians from misreading English instructions. The standard removes the two biggest sources of misreading: words with more than one meaning, and sentences with more than one possible structure.

This skill borrows that same discipline for a different reader: an **AI agent or a downstream system** that has to parse an English string — an error message, a tool description, an inter-agent instruction, a status report — without a human in the loop to resolve ambiguity. If a maintenance technician can misread "close the valve" as an adjective ("the valve that is near") instead of a command, so can a language model.

## When to Use This Skill

- An agent's output (explanation, instruction, log message, tool description) reads as dense, jargon-heavy, or ambiguous.
- Text will be consumed by another agent, a translation pipeline, or a non-native English reader, and misparsing has a real cost.
- You are writing a prompt, system message, or tool description and want to remove ambiguity before a model ever sees it.
- You want a **before/after** comparison showing exactly which rule was violated and how the rewrite fixes it. Ask for it — the default output is the rewritten text alone (see Output Format).

This skill is not for creative or marketing copy — STE is deliberately flat and literal. Do not apply it to text where voice, nuance, or persuasion is the point.

## Two Modes

Pick a mode before rewriting. If the user does not say which, infer from the text type and state the choice in one line.

**Strict** — procedures, error messages, tool and function descriptions, inter-agent instructions, safety text. Anywhere a wrong reading has a cost. Apply every rule below, including the hard length caps and one-word-one-meaning discipline.

**STE-flavored** — READMEs, PR descriptions, changelogs, explanatory prose. Apply the structural rules in full and treat the lexical rules as advisory (see Core Rewrite Rules for that split). In practice that means keeping the sentence length caps, active voice, simple tenses, no phrasal verbs, no semicolons, no nominalization and no marketing adjectives, while dropping the one-word-one-meaning lockdown: prose needs some range, and a strict rewrite of prose reads as a personality transplant rather than a clarification.

The two modes and the structural/lexical split are the same distinction seen from two directions. The split says which rules this skill can verify without ASD's dictionary. The modes say which of them to enforce for a given kind of text.

## Source and Scope

This skill encodes the **rule categories** of ASD-STE100 Issue 9 (Jan 2025): 53 writing rules across 9 sections covering word choice, grammar, sentence structure, and style, backed by a dictionary of ~900 approved words (one meaning, one part of speech each) and ~1,200 words to avoid with suggested replacements. See `references/writing-rules.md` for the full rule summary and citations.

It does **not** reproduce ASD's ~900-word approved dictionary verbatim. ASD-STE100 is free to obtain, but it is not free to redistribute: Issue 9, page 2 states that "no reproduction or publication of it, in whole or in part, shall be made without the written authority of an officer of ASD," and grants free reproduction rights only to eight listed categories (ASD/AIA/AIAC member associations and their member companies and customers, member-state defence ministries, A4A, airworthiness authorities, and universities and research institutes for educational purposes). This project is in none of them, so the dictionary stays out of this repo.

Instead, this skill applies the *underlying principle* (pick the plainest, most common word available and use it the same way every time) rather than checking against a fixed word list. When exact ASD-approved wording matters (e.g. actual aircraft maintenance documentation), get the standard and check word-by-word against the real dictionary. Request it from the [official downloads page](https://www.asd-ste100.org/STE_downloads.html) — note that this is a request form that emails you a link, not a direct download.

## Core Rewrite Rules

STE's rules divide into two kinds, and this skill can only fully deliver one of them. **Structural rules** are self-contained: they describe sentence shape, and you can apply them from the description alone. **Lexical rules** are defined entirely by the official ~900-word dictionary, which this skill deliberately does not reproduce (see Source and Scope). Without that dictionary, the lexical rules degrade from a checkable standard into a preference for plain words.

Apply the structural rules with confidence. Apply the lexical rules as a direction of travel, and say so in your output rather than implying dictionary compliance you cannot verify.

### Structural rules — apply these

| Rule | Do | Don't |
|---|---|---|
| Active voice | "The agent deletes the file." | "The file is deleted (by the agent)." — unless the actor is genuinely unknown or irrelevant |
| No phrasal verbs (Rule 9.3) | "Remove the panel." / "Start the job." | "Take off the panel." / "Spin up the job." — a two-word verb has meanings the parts do not predict |
| One instruction per sentence | "Open the file. Read line 3." | "Open the file and read line 3, then check if it matches." |
| Sentence length | ≤20 words for instructions/procedures, ≤25 words for descriptions | Long compound/subordinate-clause sentences |
| No semicolons (Rule 8.1) | Split into separate sentences | Any semicolon at all — STE bans the mark outright, not only as a clause join. (Rule 8.1 permits every other standard punctuation mark. The em dash is *not* banned by STE, though it often signals a sentence that should be split.) |
| Noun clusters | ≤3 words stacked as a noun phrase ("fuel pump valve") | 4+ word noun stacks ("high pressure fuel pump inlet valve assembly") |
| No ellipsis | Keep the subject, verb, and article explicit even if it reads longer | Drop words to save space ("Files not backed up will be lost" → ambiguous which files) |
| Keep modality | "The request **may have** failed." stays "may have" | Promote a hedge to a fact ("The request failed.") or invent a certainty the source did not state |
| Paragraph limits | One topic per paragraph, ≤6 sentences | Multi-topic paragraphs |
| Lists for sequences | Use a numbered or bulleted list for 3+ steps or conditions | Bury a sequence inside one prose sentence |

### Lexical rules — direction of travel only

| Rule | Do | Don't | Why it is weaker here |
|---|---|---|---|
| One word, one meaning | Pick one verb for one action and reuse it every time (e.g. always "check", never mix "check"/"verify"/"confirm" for the same action) | Rotate synonyms for the same idea across a document | Consistency within a document is checkable. Which word is the *approved* one is not, without the dictionary. |
| One part of speech per word | "Apply oil to the valve" (oil = noun) | "Oil the valve" (oil = verb) | Whether "oil" is approved as a noun only is a dictionary fact. Prefer the noun form when both read equally well. Do not claim compliance. |
| Verb, not noun (Rule 3.7) | "Analyze the log." | "Perform an analysis of the log." — a noun form of an action makes the sentence longer and hides who acts | Rule 3.7 says "use an **approved** verb to describe an action." Preferring the verb form is safe to apply anywhere. Knowing which verb is the approved one needs the dictionary. |
| Domain terms | Keep necessary technical nouns/verbs, but define them once if not common English (STE allows a project-specific glossary beyond its base dictionary) | Use jargon without ever defining it | The glossary allowance is real STE, but the base dictionary it extends is absent. |

### Simple tenses — apply with one exception

STE permits infinitive, imperative, simple present, simple past, simple future, and past participle as adjective. It excludes present perfect and other compound forms: "we received the report", not "we have received the report".

Aircraft manuals never need present perfect, so the exclusion costs the standard nothing. Other text is not always so lucky. "The job has completed" (and its output is available now) and "the job completed" (at some past point) are different statements, and status text frequently needs the first. **Where the compound form carries information the simple form cannot — current relevance, or a hedge as in "may have failed" — keep it and flag the departure.** Elsewhere, follow the rule.

## Scan Checklist

These six habits cover most of what makes machine-written English hard to parse. Each one is mechanical: you can point at the exact word or punctuation mark that breaks the rule, with no judgment call. Scan for all six before you rewrite anything.

1. **Synonym rotation** — the same thing gets several names in one document ("the user", "the customer", "the client"). The reader cannot tell whether they are one thing or three. Fix: pick one name, use it every time.
2. **Hedge stacking** — helper verbs and qualifiers pile up until the sentence asserts nothing ("it is important to note that this may potentially help to improve"). Fix: state the claim, or delete it.
3. **Nominalization** — an action frozen into a noun ("perform an analysis of", "provides assistance to"). Fix: use the verb ("analyze", "helps").
4. **Marketing adjectives** — words that claim quality instead of showing it: seamless, robust, powerful, cutting-edge, effortless, blazing-fast. Fix: delete, or replace with the measurement that earns the claim.
5. **Run-on sentences** — several ideas joined by semicolons or em dashes. Fix: one idea per sentence.
6. **Soft phrasal verbs** — spin up, reach out, dive into, kick off. Fix: use the single plain verb (start, contact, read, begin).

## Process

1. Pick the mode (Strict or STE-flavored). Say which only when the user asked for the rule table — see Output Format.
2. Read the input text once for meaning — do not start rewriting before you understand what it must still say afterward.
3. Walk it sentence by sentence. Flag every rule violation from the Core Rewrite Rules tables and every habit from the Scan Checklist. In STE-flavored mode, flag the lexical rules but do not enforce them. For a mechanical first pass over the structural rules, run `scripts/ste-lint.py` (stdin or file args, `--json` for structured output); it checks semicolons, sentence length, phrasal verbs, nominalization, marketing adjectives, synonym rotation, dangling-conjunction in supported list items, passive voice, and compound tenses, and by design never flags hedges or modality. `--baseline N` tolerates N hard violations (for adopting on existing docs); `--disable rule1,rule2` silences named rules.
4. Rewrite each flagged sentence to fix the violation while preserving the original meaning exactly. If a rewrite would drop necessary precision (a safety condition, a scope qualifier, a number), keep the longer phrasing and flag it instead of silently simplifying.
   - **Check modality before you commit to a rewrite.** Hedges ("may", "could", "sometimes", "is likely to") carry the author's confidence, and confidence is content. A shorter sentence that upgrades a hedge to a fact is not a simplification — it is a different claim. This is the most common way a well-intentioned STE rewrite goes wrong, because hedges are exactly what a length cap tempts you to cut.
   - Never add a fact the source did not state. A rewrite that reads better because it supplies a cause, a frequency, or a mechanism has stopped being a rewrite.
5. Output the rewritten text (see Output Format). Keep the mode choice and the rule analysis internal unless the user asked to see them.
6. If the input already complies, say so — do not force changes onto compliant text.

## Output Format

**Default: the rewritten text, and nothing else.** Most callers want a result they can paste straight into a tool description, an error string, or a prompt. Print the simplified text on its own. Do not add a preamble about this skill, a mode announcement, a violation count, a summary of what changed, a rule table, or a closing offer to explain further.

The one permitted addition: if step 4 kept a longer phrasing on purpose, add a single line after the text, prefixed `Kept as-is:`, naming the phrase and the precision that would have been lost. Omit the line when there is nothing to report.

**On request: the rule table.** When the user asks to see the reasoning — "show the diff", "which rules did it break", "explain the changes", "before/after" — output this table instead:

```markdown
| Rule violated | Original | Simplified |
|---|---|---|
| Present perfect tense | "We have received your request." | "We received your request." |
| Noun cluster (4+ words) | "the agent task queue priority handler" | "the handler that sets task-queue priority" |

Mode: Strict. 7 violations found.
```

Follow the table with a one-line note on anything you deliberately did **not** simplify, and why (usually: simplifying would lose required precision).

## Boundaries

**Will:**
- Rewrite ambiguous or dense English into short, single-meaning, active-voice sentences.
- Return the rewritten text alone by default, and name the rules it applied when the user asks.
- Preserve every fact, condition, and scope qualifier in the original.
- Preserve the strength of every hedge, and add no claim the source did not make.
- Suggest a one-line glossary entry for domain terms that must stay.

**Will not:**
- Reproduce ASD's official ~900-word dictionary as if it were memorized verbatim — always treat the official download as the source of truth for exact approved wording.
- Simplify creative, marketing, or persuasive copy where voice and nuance are the point.
- Silently drop a safety condition, exception, or scope qualifier to shorten a sentence — it will flag the trade-off instead.
- Convert "may have failed" into "failed", or "could be caused by X" into "X is the cause" — losing a hedge changes the claim.
- Guarantee an aerospace/defense-grade STE-compliant document. This is a general-purpose clarity tool inspired by STE, not a certified STE authoring tool.
- Make weak content true or useful. STE fixes the *form* of a text, not its substance. A hollow paragraph rewritten under these rules becomes a clean, short, well-punctuated hollow paragraph. If the text has nothing to say, no rewrite fixes that — say so instead of polishing it.
- Shorten past the point of clarity. Cutting words is not the goal. Removing ambiguity is the goal. Past a certain point compression starts costing the reader time rather than saving it, so stop when the sentence is unambiguous, not when it is shortest.

## Additional Resources

- **`references/writing-rules.md`** — fuller summary of the 9 rule sections and dictionary structure, with citations to the official standard and secondary sources.
- **`examples/before-after.md`** — worked examples, including official STE examples and agent-output examples built for this skill.
- **`scripts/ste-lint.py`** — deterministic, stdlib-only linter for the structural rules, including dangling-conjunction in supported list items, plus a synonym-rotation check (one word, one meaning) scoped per file. Exit 1 when hard violations exceed `--baseline` (default 0); advisory findings (passive voice, compound tenses) never fail the run; `--disable` silences named rules. It never flags hedges or modality: those are content, not style, and `--selftest` asserts that "may have failed" passes clean.
