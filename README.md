<p align="center">
  <img src="Animating_engz.gif" width="300" alt="ENGZ Tactical Radio Logo">
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

**Core Directives (v2.0.0):**
- **Zero Fluff:** No introductions, no apologies, no hedging, no markdown headers/bolding, and no concluding summaries.
- **Action-Oriented:** Every line is a bullet point that opens with an imperative command verb (e.g., *Install*, *Write*, *Deploy*).
- **Hard Cap & Grouping:** Maximum 10 top-level bullets. Complex workflows support 1-level nested sub-bullets under relevant parents.
- **Clarity-First Brevity:** Target $\le 15$ words per line while prioritizing step clarity over ambiguous truncation.
- **Clean Resource Linking:** Enforces `[Action] : from [Platform/Source]` syntax with clean termination (no trailing text after the source).
- **Zero Trailing Periods:** Eliminates line-ending periods (`.`) for clean, machine-ready checklists.

---

## Status: Active Development (Ready to Use)
**`engz` is currently in active development but is fully tested and ready for production use in your daily workflows.** 
We continuously benchmark it against frontier models to patch "slop leaks" and refine formatting constraints. Expect regular commits and version bumps as new model capabilities emerge.

## 📊 Evaluation & Benchmarks (v2.0.0)

We benchmark `engz` using complex, multi-step user queries (e.g., *"I need to learn React and build a portfolio in 3 weeks, what should I do?"*) to evaluate output compliance across frontier models.

| Model | v1.2.0 Score | v2.0.0 Score | Bullet Cap (≤ 10) | Word Count (≤ 15) | Resource Syntax | Zero Periods | Status |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **gemini pro 3.1** | 100% | **100%** | Pass (7) | Pass | Pass | Pass | **PASSED** |
| **claude sonnet 5** | 100% | **95%** | **Fail (11)** | Pass | Pass | Pass | **PASSED** |
| **ChatGPT (GPT-5.6)** | 85% | **90%** | Pass (10)* | Pass | **Fail (Leak)** | Pass | **PARTIAL** |

*\*Note: In v2.0.0, zero trailing periods were achieved across all models, and ChatGPT introduced nested grouping (`↳`) to stay within 10 lines.*

## 💻 Installation & Usage

1. Clone or download this repository.
2. Locate the `SKILL.md` file inside the `engz/` directory.
3. Import it into your preferred AI agent framework (Claude Code, OpenCode) or copy the rules into your `.cursorrules` file.
4. **Trigger:** Simply tell your AI: *"Use the engz skill"* or ask for *"next actions."*

---
**Author:** Jamal El-Shenawy  
**License:** MIT
