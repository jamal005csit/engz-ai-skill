<p align="center">
  <img src="engz_logo.png" width="300" alt="ENGZ Tactical Radio Logo">
</p>

<h1 align="center">engz-ai-skill</h1>

<p align="center">
  <strong>An execution-only AI skill that strips away conversational fluff and delivers pure, actionable checklists.</strong>
</p>

---

## 🛑 The Problem: AI "Slop"
Modern Large Language Models (LLMs) are incredibly powerful, but they suffer from extreme verbosity. When you need a quick technical roadmap, a project setup guide, or a list of next actions, LLMs bury the actual steps in paragraphs of conversational filler ("I'd be happy to help!", "Here is what you can do," "In conclusion..."). 

This wastes context window tokens, clutters the terminal, and slows down developers who just need to execute tasks.

## The Solution: `engz`
**`engz`** is a strict prompt rule/skill package designed for AI agents (Claude Code, Cursor, Gemini CLI, etc.). When invoked, it forces the AI into a rigid, execution-only mode. 

**Core Directives:**
- **Zero Fluff:** No introductions, no apologies, no hedging, and no concluding summaries.
- **Action-Oriented:** Every line is a bullet point that opens with an imperative command verb (e.g., *Install*, *Write*, *Deploy*).
- **Hard Limits:** Maximum of 10 top-level bullets, and strictly $\le 15$ words per line.
- **Strict Resource Linking:** Enforces a clean `[Action] : from [Source]` syntax with no trailing explanations.

---

## Status: Active Development (Ready to Use)
**`engz` is currently in active development but is fully tested and ready for production use in your daily workflows.** 
We are continuously benchmarking it against frontier models to patch "slop leaks" and refine the formatting constraints. Expect regular commits and version bumps as we find new ways models try to break the rules.

## 📊 Evaluation & Benchmarks (v1.2.0)

We benchmark `engz` using complex, multi-step user queries (e.g., *"I need to learn React and build a portfolio in 3 weeks, what should I do?"*) to ensure models strictly follow the output constraints.

| Model | v1.1.0 Score | v1.2.0 Score | Bullet Cap (≤ 10) | Word Count (≤ 15) | Resource Syntax | Status |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Claude 3.5 Sonnet** | 98% | **100%** | Pass (10) | Pass | Pass | **PASSED** |
| **Gemini 1.5 Pro** | 90% | **100%** | Pass (10) | Pass | Pass | **PASSED** |
| **ChatGPT (GPT-5.6)** | 60% | **85%** | **Fail (11)** | Pass | Pass | **PARTIAL** |

*Note: In the v1.2.0 patch, strict rule termination and trailing period removals were introduced, boosting performance across all major models.*

## 💻 Installation & Usage

1. Clone or download this repository.
2. Locate the `SKILL.md` file inside the `engz/` directory.
3. Import it into your preferred AI agent framework (Claude Code, OpenCode) or copy the rules into your `.cursorrules` file.
4. **Trigger:** Simply tell your AI: *"Use the engz skill"* or ask for *"next actions."*

---
**Author:** Jamal El-Shenawy  
**License:** MIT
