# Benchmark & Evaluation Suite

This directory contains the testing methodology, evaluation metrics, and model compliance results for **`engz`**.

To ensure `engz` maintains zero-slop, execution-only output across different frontier AI models, we benchmark skill enforcement using real-world user queries.

---

## Test Benchmark #1: Complex Task Breakdown

### Test Prompt
> `/engz I need to learn React and build a portfolio in 3 weeks, what should I do? Use the engz skill.`

### Evaluation Criteria (Metrics)
Each model response is evaluated against five strict output criteria:

1. **Bullet Cap Enforcement:** Total top-level bullets $\le 10$.
2. **Imperative Verb Rule:** Every bullet opens with a direct command verb.
3. **Word Limit Constraint:** Every bullet is $\le 15$ words.
4. **Resource Syntax Accuracy:** Matches `[Action] : from [Source]` exactly with clean termination.
5. **Zero Slop / Fluff:** No intro/outro text, no trailing period punctuation, no hedging.

---

## Model Performance Scorecard (v1.2.0 Retest)

| Model | Score (v1.1.0) | Score (v1.2.0) | Bullet Cap ($\le 10$) | Word Count ($\le 15$) | Resource Syntax | Status |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Claude Sonnet 5** *(Medium Effort)* | 98% | **100%** | Pass (10) | Pass | Pass | **PASSED** |
| **Gemini Pro 3.1** | 90% | **100%** | Pass (10) | Pass | Pass | **PASSED** |
| **ChatGPT (GPT-5.6)** | 60% | **85%** | **Fail (11)** | Pass | Pass | **PARTIAL** |

---

## Detailed Model Retest Analysis

### 1. Claude Sonnet 5 — 100% (Flawless)
* **Execution:** Perfect adherence to all 5 criteria. 
* **Highlights:** Exactly 10 bullets, correctly included a `Note:` bullet for state management scope, clean resource tags (`: from freeCodeCamp`, `: from React Docs`, `: from Figma Community`, `: from Vercel`, `: from Namecheap`), and zero trailing period slop.

### 2. Gemini Pro 3.1 — 100% (Flawless)
* **Execution:** Fixed all resource syntax issues from v1.1.0.
* **Highlights:** Exactly 10 bullets, perfectly concise line lengths (5–7 words per line), clean resource tags (`: from React Docs`, `: from GitHub`, `: from Vercel`), and zero trailing period slop.

### 3. ChatGPT (GPT-5.6) — 85% (Major Improvement)
* **Fixed in v1.2.0:** Completely eliminated trailing periods, respected the 15-word bullet limit, and fixed resource tag syntax.
* **Remaining Leak:** Generated **11 top-level bullets**, missing the hard cap ceiling by 1 item.

---

## Version History & Fix Log

* **v1.0.0:** Initial draft layout and prompt contract.
* **v1.1.0:** Conformed to 2026 AgentSkills YAML specification.
* **v1.2.0:** Added strict rule termination (`HARD CAP`), eliminated trailing period punctuation slop, and standardized resource formatting.
