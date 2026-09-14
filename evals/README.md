# Benchmark & Evaluation Suite

This directory contains the testing methodology, evaluation metrics, and model compliance results for **`engz`** (v2.0.0).

To ensure `engz` maintains zero-slop, execution-only output across different frontier AI models, we benchmark skill enforcement using real-world user queries.

---

## Test Benchmark #1: Complex Task Breakdown

### Test Prompt
> `/engz I need to learn React and build a portfolio in 3 weeks, what should I do? Use the engz skill.`

### Evaluation Criteria (v2.0.0 Metrics)
Each model response is evaluated against six strict output criteria defined in `SKILL.md` (v2.0.0):

1. **Bullet Cap & Nesting Enforcement:** Top-level bullets $\le 10$. Sub-steps must be nested exactly one level under parent bullets if grouping.
2. **Imperative Verb Rule:** Every bullet opens with a direct command verb.
3. **Word Limit & Line Clarity:** Target $\le 15$ words per bullet without creating ambiguity (clarity prioritized over over-compression).
4. **Clean Resource Syntax & Termination:** Matches `[Action] : from [Source]` with clean termination (zero trailing text after the source name).
5. **Zero Trailing Periods:** Clean line endings with no trailing period punctuation (`.`).
6. **Zero Slop / Fluff:** No intro/outro text, no headers, no bolding/italics, no hedging, and zero forbidden conversational phrases.

---

## Model Performance Scorecard (v2.0.0 Retest)

| Model | Score (v1.2.0) | Score (v2.0.0) | Bullet Cap ($\le 10$) | Word Count ($\le 15$) | Resource Syntax | Zero Periods | Status |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Gemini Pro 3.1** | 100% | **100%** | Pass (7) | Pass | Pass | Pass | **PASSED** |
| **Claude Sonnet 5** *(Medium Effort)* | 100% | **95%** | **Fail (11)** | Pass | Pass | Pass | **PASSED** |
| **ChatGPT (GPT-5.6)** | 85% | **90%** | Pass (10)* | Pass | **Fail (Leak)** | Pass | **PARTIAL** |

*\*ChatGPT utilized sub-bullet grouping (`↳`) to fit all steps into 10 lines total, complying with the overall line count constraint.*

---

## Detailed Model Retest Analysis

### 1. Gemini Pro 3.1 — 100% (Flawless Execution)
* **Execution:** Perfect adherence to all v2.0.0 directives.
* **Highlights:** Exactly 7 top-level bullets, concise line lengths (5–8 words), perfectly terminated resource tags (`: from React Docs`, `: from Next.js`, `: from Tailwind CSS`, `: from Vercel`, `: from GitHub`), zero trailing periods, and zero slop.

### 2. Claude Sonnet 5 — 95% (Near Perfect)
* **Execution:** Excellent line brevity (7–11 words per bullet), strong imperative verbs, clean resource tags (`: from official react.dev learn tutorial`, `: from Vercel`), and zero trailing periods. Correctly applied a `Note:` bullet for scope control.
* **Minor Leak:** Output 11 top-level bullets total (10 action bullets + 1 `Note:` bullet), slightly breaching the hard ceiling cap ($\le 10$).

### 3. ChatGPT (GPT-5.6) — 90% (Improved Structure)
* **Fixed in v2.0.0:** Fully eliminated trailing period punctuation across all lines and adopted sub-item nesting (`↳`) to structure complex learning phases into 10 total lines.
* **Remaining Leaks:** Appended trailing text after a resource tag (`: from GitHub and update LinkedIn...` violates the clean termination rule) and had 1 line slightly exceed the word count target (16 words).

---

## Version History & Fix Log

* **v1.0.0:** Initial draft layout and prompt contract.
* **v1.1.0:** Conformed to 2026 AgentSkills YAML specification.
* **v1.2.0:** Added strict rule termination (`HARD CAP`), eliminated trailing period punctuation slop, and standardized resource formatting.
* **v2.0.0:** Added single-level nested bullet support for grouping, clarity-first word count guidance, clean resource termination checks, zero trailing period rules, and structured `Note:` bullet handling.
