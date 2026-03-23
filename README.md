<div align="center">

<br>

# TOKEN SURGEON

### Audit your prompts. Name the waste. Cut it.

<br>

![Version](https://img.shields.io/badge/version-1.0.0-3B82F6?style=for-the-badge&labelColor=1E293B)
![License](https://img.shields.io/badge/license-MIT-22C55E?style=for-the-badge&labelColor=1E293B)
![Claude Code](https://img.shields.io/badge/Claude_Code-Skill-7C3AED?style=for-the-badge&labelColor=1E293B)
![Patterns](https://img.shields.io/badge/Waste_Patterns-10-F59E0B?style=for-the-badge&labelColor=1E293B)
![Rewrite](https://img.shields.io/badge/Rewrite-On_Demand-EF4444?style=for-the-badge&labelColor=1E293B)

<br>

> **Token Surgeon** is a [Claude Code](https://claude.ai/code) skill that scans any prompt against **10 named waste patterns**, scores its efficiency, quotes every offending line, and explains exactly why it's costing you tokens. It never rewrites until you say so.

<br>

</div>

---

## How It Works

Token Surgeon operates in two strict phases. Analysis always comes first.

<br>

```
  PHASE 1 — ANALYSIS                      PHASE 2 — REWRITE
  ─────────────────────────────────        ──────────────────────────────
  1. Estimate token count                  (only runs when you confirm)
  2. Scan for waste patterns
  3. Quote offending text                  1. Optimized prompt in code block
  4. Score efficiency (1–10)               2. Before / after token count
  5. Ask before doing anything             3. What was cut and why
```

<br>

---

## The 10 Waste Patterns

Every pattern has a name. When found, Token Surgeon quotes the exact text and explains the token cost.

<br>

| # | Pattern | What It Catches |
|:-:|---------|----------------|
| `01` | **Hedging Language** | `"if possible"` `"please try to"` `"feel free to"` `"you may want to"` |
| `02` | **Role Throat-Clearing** | Verbose persona intros that rarely affect output |
| `03` | **Restated Instructions** | The same rule expressed twice in different words |
| `04` | **Politeness Padding** | `"thank you"` `"please note that"` `"I appreciate your help"` |
| `05` | **Filler Affirmations** | `"Remember to always"` `"Make sure that"` `"It's very important that"` |
| `06` | **Dead Context** | Background the model doesn't need to complete the task |
| `07` | **Redundant Formatting** | Multiple instructions that say the same thing |
| `08` | **Negation Bloat** | Three `"don't"` clauses when one positive instruction covers all |
| `09` | **Over-Specified Examples** | More examples than the pattern needs to register |
| `10` | **Scope Creep Instructions** | Rules written for edge cases that appear 0.1% of the time |

<br>

---

## Efficiency Score

Every analysis ends with a score and a recoverable token estimate.

<br>

```
  Efficiency Score: 6/10
  Estimated recoverable tokens: ~47 (22%)
  Waste patterns found: Hedging Language, Dead Context, Negation Bloat
```

<br>

| Score | Rating | Meaning |
|:-----:|--------|---------|
| ![9-10](https://img.shields.io/badge/9--10-Tight-22C55E?style=flat-square) | **Tight** | Minor polish only |
| ![7-8](https://img.shields.io/badge/7--8-Good_Bones-84CC16?style=flat-square) | **Good Bones** | A few specific cuts available |
| ![5-6](https://img.shields.io/badge/5--6-Moderate-F59E0B?style=flat-square) | **Moderate Waste** | Real savings available |
| ![3-4](https://img.shields.io/badge/3--4-Bloated-F97316?style=flat-square) | **Significant Bloat** | Multiple patterns compounding |
| ![1-2](https://img.shields.io/badge/1--2-Rewrite-EF4444?style=flat-square) | **Needs Rewrite** | Start over |

<br>

---

## Before & After

A real example — a bloated system prompt put through Token Surgeon.

<br>

**Input** &nbsp; `~83 tokens`

```
You are a helpful, friendly, and knowledgeable assistant who always tries to provide
the best possible answers. Please try to respond in a clear and concise way. Make sure
that your responses are easy to read. Remember to always use bullet points when listing
things. Thank you for your help. Please note that the user is a software engineer.
Don't use jargon, don't use overly technical language, and don't assume prior knowledge.
```

<br>

**Token Surgeon Output**

```
Input tokens (estimated): ~83

  [02] Role Throat-Clearing
       "You are a helpful, friendly, and knowledgeable assistant who always tries
        to provide the best possible answers."
       Five adjectives, zero added constraint. "You are a helpful assistant." does
       the same work at 1/4 the cost.

  [01] Hedging Language
       "Please try to respond in a clear and concise way."
       "Please try to" softens without constraining. Either require it or remove it.

  [07] Redundant Formatting Instructions
       "respond in a clear and concise way. Make sure that your responses are
        easy to read."
       Same instruction, two forms. One covers both.

  [05] Filler Affirmations
       "Remember to always use bullet points"
       "Use bullet points" is the instruction. "Remember to always" is 3 wasted tokens.

  [04] Politeness Padding
       "Thank you for your help. Please note that"
       Models don't respond to social cues. These tokens do no work.

  [08] Negation Bloat
       "Don't use jargon, don't use overly technical language, and don't assume
        prior knowledge."
       Replace with: "Use plain language."

  Efficiency Score: 3/10
  Estimated recoverable tokens: ~61 (73%)
  Waste patterns found: Role Throat-Clearing, Hedging Language,
                        Redundant Formatting, Filler Affirmations,
                        Politeness Padding, Negation Bloat
```

<br>

**Output** &nbsp; `~22 tokens` &nbsp; ![Saved](https://img.shields.io/badge/saved-73%25-22C55E?style=flat-square&labelColor=1E293B)

```
You are a helpful assistant. Respond concisely using bullet points when listing.
The user is a software engineer — use plain language.
```

<br>

---

## Installation

Token Surgeon requires [Claude Code](https://claude.ai/code).

<br>

**Clone into your Claude Code skills directory:**

```bash
cd ~/.claude/skills
git clone https://github.com/natedemoss/token-surgen-claude-skill token-surgeon
```

**Restart Claude Code** (or open a new session). The skill loads automatically.

**Invoke it:**

```
/token-surgeon
```

Then paste any prompt. The analysis runs. Nothing is rewritten until you confirm.

<br>

---

## Triggers

Token Surgeon activates on `/token-surgeon` or contextually when you say:

```
"analyze this prompt"       "audit my system prompt"
"optimize my prompt"        "too many tokens in this"
"trim this prompt"          "this prompt is bloated"
```

<br>

---

## Design Philosophy

<br>

**Analysis before rewrite.**
Seeing the findings first lets you decide what to cut intentionally. Silent rewrites can remove constraints that mattered.

**Named patterns, not vibes.**
"This is verbose" is hard to act on. "Negation Bloat, lines 4–6" tells you exactly where to look and why it's a problem.

**Deletion over paraphrase.**
Paraphrasing for brevity changes meaning in subtle ways. Deletion is safer and produces cleaner prompts.

**No silent changes.**
If a cut is ambiguous, Token Surgeon flags it and asks rather than guessing.

<br>

---

<div align="center">

![Claude Code](https://img.shields.io/badge/Built_for-Claude_Code-7C3AED?style=for-the-badge&labelColor=1E293B)
![MIT](https://img.shields.io/badge/License-MIT-22C55E?style=for-the-badge&labelColor=1E293B)

<br>

*Cut what doesn't belong. Keep what does.*

<br>

</div>
