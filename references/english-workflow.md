# English workflow — Simplified Technical English (ASD-STE100)

Read this file only when the user explicitly asks for English-only output, including Chinese input that must become English. For Chinese or mixed Chinese-English output, use the Chinese rules in [../SKILL.md](../SKILL.md) instead. This workflow keeps the upstream English method and citations. It does not guarantee official STE compliance. Explicit user instructions take precedence over the style and format guidance here. The fidelity rules still apply unless the user explicitly asks to change the content.

ASD-STE100 is a controlled-language standard built by the aerospace and defense industry (ASD, the AeroSpace and Defense Industries Association of Europe) to stop maintenance technicians from misreading English instructions. The standard removes the two biggest sources of misreading: words with more than one meaning, and sentences with more than one possible structure.

This skill borrows that same discipline for a different reader: an **AI agent or a downstream system** that has to parse an English string — an error message, a tool description, an inter-agent instruction, a status report — without a human in the loop to resolve ambiguity. If a maintenance technician can misread "close the valve" as an adjective ("the valve that is near") instead of a command, so can a language model.

## When to Use This Skill

- An agent's output (explanation, instruction, log message, tool description) reads as dense, jargon-heavy, or ambiguous.
- Text will be consumed by another agent, a translation pipeline, or a non-native English reader, and misparsing has a real cost.
- You are writing a prompt, system message, or tool description and want to remove ambiguity before a model ever sees it.
- You want a **before/after** comparison showing exactly which rule was violated and how the rewrite fixes it. Ask for it — the default output is the rewritten text alone (see Output Contract).

This skill is not for creative or marketing copy — STE is deliberately flat and literal. Do not apply it to text where voice, nuance, or persuasion is the point.

## Two Modes

Pick a mode before rewriting. If the user does not say which, infer it from the text type. The mode is an internal decision: show it only under the conditions in Output Contract.

**Strict** — procedures, error messages, tool and function descriptions, inter-agent instructions, safety text. Anywhere a wrong reading has a cost. Apply every rule below, including the hard length caps and one-word-one-meaning discipline.

**STE-flavored** — READMEs, PR descriptions, changelogs, explanatory prose. Apply the structural rules in full and treat the lexical rules as advisory (see Core Rewrite Rules for that split). In practice that means keeping the sentence length caps, active voice, simple tenses, no phrasal verbs, no semicolons, no nominalization and no marketing adjectives, while dropping the one-word-one-meaning lockdown: prose needs some range, and a strict rewrite of prose reads as a personality transplant rather than a clarification.

The two modes and the structural/lexical split are the same distinction seen from two directions. The split says which rules this skill can verify without ASD's dictionary. The modes say which of them to enforce for a given kind of text.

## Source and Scope

This skill encodes the **rule categories** of ASD-STE100 Issue 9 (Jan 2025): 53 writing rules across 9 sections covering word choice, grammar, sentence structure, and style, backed by a dictionary of ~900 approved words (one meaning, one part of speech each) and ~1,200 words to avoid with suggested replacements. See [writing-rules.md](writing-rules.md) for the full rule summary and citations.

It does **not** reproduce ASD's ~900-word approved dictionary verbatim. ASD-STE100 is free to obtain, but it is not free to redistribute: Issue 9, page 2 states that "no reproduction or publication of it, in whole or in part, shall be made without the written authority of an officer of ASD," and grants free reproduction rights only to eight listed categories (ASD/AIA/AIAC member associations and their member companies and customers, member-state defence ministries, A4A, airworthiness authorities, and universities and research institutes for educational purposes). This project is in none of them, so the dictionary stays out of this repo.

Instead, this skill applies the *underlying principle* (pick the plainest, most common word available and use it the same way every time) rather than checking against a fixed word list. When exact ASD-approved wording matters (e.g. actual aircraft maintenance documentation), get the standard and check word-by-word against the real dictionary. Request it from the [official downloads page](https://www.asd-ste100.org/STE_downloads.html) — note that this is a request form that emails you a link, not a direct download.

## Core Rewrite Rules

STE's rules divide into two kinds, and this skill can only fully deliver one of them. **Structural rules** are self-contained: they describe sentence shape, and you can apply them from the description alone. **Lexical rules** are defined entirely by the official ~900-word dictionary, which this skill deliberately does not reproduce (see Source and Scope). Without that dictionary, the lexical rules degrade from a checkable standard into a preference for plain words.

Apply the structural rules with confidence. Apply the lexical rules as a direction of travel. Never imply dictionary compliance you cannot verify. Explain this limit when the user asks about rules or compliance (see Output Contract); do not add it to an ordinary rewrite.

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
| Domain terms | Keep necessary technical nouns/verbs. If the reader needs a definition and the source or context supplies one, define the term once (STE allows a project-specific glossary beyond its base dictionary) | Use jargon the reader cannot follow, or invent a definition the source does not support | The glossary allowance is real STE, but the base dictionary it extends is absent. |

### Simple tenses — apply with one exception

STE permits infinitive, imperative, simple present, simple past, simple future, and past participle as adjective. It excludes present perfect and other compound forms: "we received the report", not "we have received the report".

Aircraft manuals never need present perfect, so the exclusion costs the standard nothing. Other text is not always so lucky. "The job has completed" (and its output is available now) and "the job completed" (at some past point) are different statements, and status text frequently needs the first. **Where the compound form carries information the simple form cannot — current relevance, or a hedge as in "may have failed" — keep it.** Output Contract says when the departure needs a note. Elsewhere, follow the rule.

## Scan Checklist

These seven habits cover most of what makes machine-written English hard to parse. Each one is mechanical: you can point at the exact word or punctuation mark that breaks the rule, with no judgment call. Scan for all seven before you rewrite anything.

1. **Synonym rotation** — the same thing gets several names in one document ("the user", "the customer", "the client"). The reader cannot tell whether they are one thing or three. Fix: pick one name, use it every time.
2. **Hedge stacking** — helper verbs and qualifiers pile up until the sentence asserts nothing ("it is important to note that this may potentially help to improve"). Fix: state the claim, or delete it.
3. **Nominalization** — an action frozen into a noun ("perform an analysis of", "provides assistance to"). Fix: use the verb ("analyze", "helps").
4. **Marketing adjectives** — words that claim quality instead of showing it: seamless, robust, powerful, cutting-edge, effortless, blazing-fast. Fix: delete, or replace with the measurement that earns the claim.
5. **Run-on sentences** — several ideas joined by semicolons or em dashes. Fix: one idea per sentence.
6. **Soft phrasal verbs** — spin up, reach out, dive into, kick off. Fix: use the single plain verb (start, contact, read, begin).
7. **Stock phrases** — filler that frames a claim instead of making it: "delve", "foster", "leverage", "it's worth noting", "importantly", "genuinely", a closing "Bottom line:" or "In short:", a "Question? Answer." pattern, or "This isn't about X. It's about Y." Fix: state the claim directly, or delete the frame.

### Structure and framing

These checks need judgment, so the linter does not run them.

- State the main point or the first action early, then add the conditions and detail the reader needs. In a procedure, keep the step order, and keep each condition and warning before its action.
- Write explanations as paragraphs. Use a list only for steps, parallel conditions, or items the reader must compare. Avoid nested lists unless the hierarchy cannot be stated in prose.
- Do not add contrastive framing ("X, not Y") that introduces an alternative the source did not mention. A contrast or negation that the source states is content: keep it.
- Do not invent compound labels (such as "exact-head checks") or add vague qualifiers. Use plain verbs and prepositions to state the actual relationship.
- Do not add statements about what the text will not do or what stays unchanged, unless the source states them.
- Match the depth of term explanations to the reader's background, as shown by the request and its context.

## Process

1. Pick the mode (Strict or STE-flavored). Keep it internal unless Output Contract says to show it.
2. Read the input text once for meaning — do not start rewriting before you understand what it must still say afterward.
3. Walk it sentence by sentence. Flag every rule violation from the Core Rewrite Rules tables and every habit from the Scan Checklist. In STE-flavored mode, flag the lexical rules but do not enforce them. For a mechanical first pass over the structural rules, run [`scripts/ste-lint.py`](../scripts/ste-lint.py) from the skill root (stdin or file args, `--json` for structured output); it checks semicolons, sentence length, phrasal verbs, nominalization, marketing adjectives, synonym rotation, dangling-conjunction in supported list items, passive voice, and compound tenses, and by design never flags hedges or modality. `--baseline N` tolerates N hard violations (for adopting on existing docs); `--disable rule1,rule2` silences named rules. Input that contains Han characters returns exit 2 (`not_checked`); extract the English text first. The linter does not check stock phrases or the Structure and framing items; check those by reading.
4. Rewrite each flagged sentence to fix the violation while preserving the original meaning exactly. If a rewrite would drop necessary precision (a safety condition, a scope qualifier, a number, a tense that carries current relevance, a hedge), keep the longer or compound phrasing instead of silently simplifying.
   - **Check modality before you commit to a rewrite.** Hedges ("may", "could", "sometimes", "is likely to") carry the author's confidence, and confidence is content. A shorter sentence that upgrades a hedge to a fact is not a simplification — it is a different claim. This is the most common way a well-intentioned STE rewrite goes wrong, because hedges are exactly what a length cap tempts you to cut.
   - Never add a fact the source did not state. A rewrite that reads better because it supplies a cause, a frequency, or a mechanism has stopped being a rewrite.
   - Keep the strength of every obligation. "Should" stays a recommendation and "must" stays a requirement.
5. Compare the rewrite with the source, item by item: facts, conditions and their scope, negations, exceptions, numbers and units, commands, quotes, obligation strength, and confidence. Word matching alone does not complete this step.
6. Output the result as Output Contract describes.

## Output Contract

This section is the single source for what the output shows. Explicit user instructions about format take precedence over it.

**Default: the text alone.** For an ordinary rewrite, print only the rewritten text. Most callers want a result they can paste straight into a tool description, an error string, or a prompt. Keep the mode, the rule analysis, and the violation count internal. Do not add a preamble about this skill, a mode announcement, a summary of what changed, a rule table, a compliance disclaimer, or a closing offer to explain further. If the source is already clear, return it unchanged. Say that it needs no change only when the user asked for a check or a review.

**Short notes that stay in the default output.** Add a line after the text only in these cases, and omit it when there is nothing to report:

- `Kept as-is:` — you kept a form that a structural rule would otherwise change, because the change would lose information: a sentence over the length cap that carries a condition, scope, or number; a compound tense that carries current relevance; any other phrasing kept for precision, tense, or confidence. Name the phrase and the information it keeps. A modal with a perfect infinitive, such as "may have failed", is a hedge: keep it and add no note.
- `Note:` — ambiguity or missing information blocks a faithful rewrite. Name the gap in one line. Ask the user only when the missing fact would change the result and the context cannot supply it. If the text has nothing to say, state what it lacks instead of polishing it. Never invent the missing facts.

**On request: the rule table and the mode.** When the user asks to see the reasoning — "show the diff", "which rules did it break", "explain the changes", "before/after", or whether the text complies with STE — output this table instead:

```markdown
| Rule violated | Original | Simplified |
|---|---|---|
| Present perfect tense | "We have received your request." | "We received your request." |
| Noun cluster (4+ words) | "the agent task queue priority handler" | "the handler that sets task-queue priority" |

Mode: Strict. 7 violations found.
```

Follow the table with a one-line note on anything you deliberately did **not** simplify, and why (usually: simplifying would lose required precision). When the user asks about compliance, also say that the lexical rules were applied without ASD's dictionary and that the result is not a certified STE document.

**On request: glossary entries.** Suggest one-line glossary entries for domain terms only when the user asks for them.

## Boundaries

**Will:**
- Rewrite ambiguous or dense English into short, single-meaning, active-voice sentences.
- Return the rewritten text alone by default, and name the rules it applied when the user asks.
- Preserve every fact, condition, and scope qualifier in the original.
- Preserve the strength of every hedge and every obligation, and add no claim the source did not make.
- Keep necessary domain terms, and suggest glossary entries when the user asks.

**Will not:**
- Reproduce ASD's official ~900-word dictionary as if it were memorized verbatim — always treat the official download as the source of truth for exact approved wording.
- Simplify creative, marketing, or persuasive copy where voice and nuance are the point.
- Silently drop a safety condition, exception, or scope qualifier to shorten a sentence — it will flag the trade-off instead.
- Convert "may have failed" into "failed", or "could be caused by X" into "X is the cause" — losing a hedge changes the claim.
- Guarantee an aerospace/defense-grade STE-compliant document. This is a general-purpose clarity tool inspired by STE, not a certified STE authoring tool.
- Make weak content true or useful. STE fixes the *form* of a text, not its substance. A hollow paragraph rewritten under these rules becomes a clean, short, well-punctuated hollow paragraph. If the text has nothing to say, no rewrite fixes that — report what is missing in a `Note:` line instead of polishing it.
- Shorten past the point of clarity. Cutting words is not the goal. Removing ambiguity is the goal. Past a certain point compression starts costing the reader time rather than saving it, so stop when the sentence is unambiguous, not when it is shortest.

## Additional Resources

- [writing-rules.md](writing-rules.md) — fuller summary of the 9 rule sections and dictionary structure, with citations to the official standard and secondary sources.
- [../examples/before-after-en.md](../examples/before-after-en.md) — English worked examples, including official STE examples and agent-output examples built for this skill.
- [../scripts/ste-lint.py](../scripts/ste-lint.py) — deterministic, stdlib-only linter for the structural rules, including dangling-conjunction in supported list items, plus a synonym-rotation check (one word, one meaning) scoped per file. Exit 1 when hard violations exceed `--baseline` (default 0); advisory findings (passive voice, compound tenses) never fail the run; `--disable` silences named rules. It never flags hedges or modality: those are content, not style, and `--selftest` asserts that "may have failed" passes clean.
