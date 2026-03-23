# Token Surgeon

**A Claude Code skill that audits your prompts for token waste — then cuts it.**

Token Surgeon scans any prompt or system prompt against 10 named waste patterns, scores its efficiency, quotes the offending text, and explains exactly why each flagged section costs tokens without adding value. It never rewrites until you ask.

---

## What It Does

| Phase | What Happens |
|-------|-------------|
| **Analysis** | Token count estimate, full waste pattern scan, efficiency score |
| **Review** | You see the findings before anything changes |
| **Rewrite** | On confirmation only — optimized prompt with before/after token delta |

The guiding principle: **deletion over paraphrase.** The goal is fewer tokens, not different words.

---

## The 10 Waste Patterns

Token Surgeon checks for each of these by name. When found, it quotes the exact text and explains the cost.

| # | Pattern | What It Catches |
|---|---------|----------------|
| 1 | **Hedging Language** | "if possible", "please try to", "feel free to", "you may want to" |
| 2 | **Role Throat-Clearing** | Verbose persona setup that rarely affects output |
| 3 | **Restated Instructions** | The same rule expressed twice in different words |
| 4 | **Politeness Padding** | "thank you", "please note that", "I appreciate your help" |
| 5 | **Filler Affirmations** | "Remember to always", "Make sure that", "It's very important that" |
| 6 | **Dead Context** | Background the model doesn't need to complete the task |
| 7 | **Redundant Formatting Instructions** | Multiple instructions that mean the same thing |
| 8 | **Negation Bloat** | "Don't do X, Y, Z" when one positive instruction covers all three |
| 9 | **Over-Specified Examples** | More examples than the pattern requires to register |
| 10 | **Scope Creep Instructions** | Rules written for edge cases that appear 0.1% of the time |

---

## Efficiency Score

Every analysis ends with a score and a summary:

```
Efficiency Score: 6/10
Estimated recoverable tokens: ~47 (22%)
Waste patterns found: Hedging Language, Dead Context, Negation Bloat
```

| Score | Meaning |
|-------|---------|
| 9–10 | Tight. Minor polish only. |
| 7–8 | Good bones. A few specific cuts available. |
| 5–6 | Moderate waste. Real savings available. |
| 3–4 | Significant bloat. Multiple patterns compounding. |
| 1–2 | Needs a full rewrite. |

---

## Example

**Input prompt (bloated):**

```
You are a helpful, friendly, and knowledgeable assistant who always tries to provide
the best possible answers. Please try to respond in a clear and concise way. Make sure
that your responses are easy to read. Remember to always use bullet points when listing
things. Thank you for your help. Please note that the user is a software engineer.
Don't use jargon, don't use overly technical language, and don't assume prior knowledge.
```

**Token Surgeon output:**

```
Input tokens (estimated): ~83

Patterns found:

  Role Throat-Clearing
  "You are a helpful, friendly, and knowledgeable assistant who always tries to
   provide the best possible answers."
  — Verbose persona intro. "You are a helpful assistant." costs 5 tokens and
    produces identical behavior.

  Hedging Language
  "Please try to respond in a clear and concise way."
  — "Please try to" softens without constraining. Either require it or don't.

  Redundant Formatting Instructions
  "respond in a clear and concise way. Make sure that your responses are easy to read."
  — Same instruction twice. One covers both.

  Filler Affirmations
  "Remember to always use bullet points"
  — "Use bullet points" is the instruction. "Remember to always" adds 3 tokens of noise.

  Politeness Padding
  "Thank you for your help. Please note that"
  — Models don't respond to social cues. These tokens do no work.

  Negation Bloat
  "Don't use jargon, don't use overly technical language, and don't assume prior knowledge."
  — Replace with: "Use plain language."

Efficiency Score: 3/10
Estimated recoverable tokens: ~41 (49%)
```

**After rewrite:**

```
You are a helpful assistant. Respond concisely using bullet points when listing.
The user is a software engineer — use plain language.
```

```
Before: ~83 tokens
After:  ~22 tokens
Saved:  ~61 tokens (73% reduction)
```

---

## Installation

Token Surgeon is a [Claude Code](https://claude.ai/code) skill. Skills are loaded automatically when placed in your Claude Code skills directory.

**Step 1** — Clone this repo into your Claude Code skills directory:

```bash
cd ~/.claude/skills
git clone https://github.com/natedemoss/token-surgen-claude-skill token-surgeon
```

**Step 2** — Restart Claude Code (or start a new session). The skill loads automatically.

**Step 3** — Invoke it:

```
/token-surgeon
```

Then paste any prompt to analyze.

---

## Usage

```
/token-surgeon
```

Paste a prompt. Token Surgeon analyzes it and stops. No rewrites happen without your explicit confirmation.

To trigger it contextually, you can also say things like:
- "analyze this prompt"
- "audit my system prompt"
- "too many tokens in this"
- "trim this down"

---

## Design Decisions

**Analysis before rewrite.** Seeing the findings first lets you decide what to cut and what to keep intentionally. Blind rewrites can silently remove constraints that mattered.

**Named patterns.** Generic feedback ("this is verbose") is hard to act on. Named patterns ("Negation Bloat, lines 4-6") tell you exactly where to look and why it's a problem.

**Deletion over paraphrase.** Paraphrasing for brevity often changes meaning in subtle ways. Deletion is safer and produces cleaner prompts.

**No silent changes.** If a cut is ambiguous, Token Surgeon flags it and asks rather than guessing.

---

## License

MIT
