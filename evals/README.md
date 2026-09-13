# Benchmark & Evaluation Suite

This directory contains the testing methodology, evaluation metrics, and model compliance results for **`engz`**.

To ensure `engz` maintains zero-slop, execution-only output across different frontier AI models, we benchmark skill enforcement using real-world user queries.

---

## Test Benchmark #1: Complex Task Breakdown

### Test Prompt
> `/engz I need to learn React and build a portfolio in 3 weeks, what should I do?`

### Evaluation Criteria (Metrics)
Each model response is evaluated against five strict output criteria:

1. **Bullet Cap Enforcement:** Total top-level bullets $\le 10$.
2. **Imperative Verb Rule:** Every bullet opens with a direct command verb.
3. **Word Limit Constraint:** Every bullet is $\le 15$ words.
4. **Resource Syntax Accuracy:** Matches `[Action] : from [Source]` exactly with no trailing explanation.
5. **Zero Slop / Fluff:** No introductory text, conclusion, period punctuation slop, or hedging.

---

## Model Performance Scorecard

| Model | Score | Bullet Cap ($\le 10$) | Word Count ($\le 15$) | Resource Syntax | Status |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Claude Sonnet 5** *(Medium Effort)* | **98%** | Pass (10) | Pass | Pass | **PASSED** |
| **Gemini Pro 3.1** | **90%** | Pass (10) | Pass | Partial | **PASSED** |
| **GPT-5.6 Luna** | **60%** | **Fail (12)** | **Fail** | **Fail** | **FAILED** |

---

## Detailed Model Analysis

### 1. Claude Sonnet 5 (Passed — 98%)
* **Strengths:** Strictly respected the 10-bullet ceiling, successfully nested sub-steps with imperative verbs, used inline code (`npm create vite@latest`) appropriately, and accurately formatted resource links.
* **Minor Artifact:** Minor visual formatting variation based on platform rendering.

### 2. Gemini Pro 3.1 (Passed — 90%)
* **Strengths:** Respected the top-level bullet cap and adhered to imperative command openers throughout.
* **Minor Flaw:** Omitted the `: from [Source]` syntax on deployment options, returning generic items instead of source-linked tasks.

### 3. GPT-5.6 Luna (Failed — 60%)
* **Observed Failures:**
  * **Bullet Overcount:** Returned 12 top-level bullets, violating the hard cap of 10.
  * **Word Limit Leak:** Multiple bullets exceeded 18 words (e.g., full schedule breakdown).
  * **Syntax Corruption:** Appended conversational justification onto the resource syntax (`: from a public API before starting your portfolio.`).
  * **Punctuation Slop:** Added trailing periods (`.`) to bullet lines.

---

## Version Patch Log (v1.1.0 $\rightarrow$ v1.2.0)

Based on the failures observed in **GPT-5.6 Luna** and **Gemini Pro 3.1**, `engz` was updated to **v1.2.0** with the following anti-regression rules:

* Added explicit **`HARD CAP: Maximum 10 top-level bullets`** warning in pre-send self-check.
* Added **`No Trailing Periods`** rule to keep list lines raw and clean.
* Enforced **`Strict Resource Termination`**, forbidding any words after `[Source]`.

---

## Ongoing Retest Pipeline

We continuously re-evaluate `engz` as new models and model updates are released.

* [ ] Retest v1.2.0 against **Claude Sonnet 5**
* [ ] Retest v1.2.0 against **Gemini Pro 3.1**
* [ ] Retest v1.2.0 against **GPT-5.6 Luna**
