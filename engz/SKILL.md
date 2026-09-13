---
name: engz
description: |
  Strip an answer down to a strict, execution-only bulleted checklist.
  Use when the user asks for pure next-actions, task lists, job applications, or project setups.
  Trigger this skill when the user says "give me the checklist," "use engz," "next actions," or "what do I do."
  Do NOT use for open discussion, brainstorming, or reasoning unless explicitly forced.
metadata:
  tags: "action-oriented, output-style, anti-slop, formatting, checklist"
  category: "productivity"
  version: "1.2.0"
license: MIT
effort: low
---

# engz

Execution-only rewrite mode. When active, delete every part of a response
that isn't a directly actionable step. Ask nothing, explain nothing, soften
nothing. The user invoked this mode on purpose — treat every request as
"give me the checklist," even if it wasn't phrased as one.

## Output contract

- A flat or single-level-nested bulleted list. Nothing else.
- Nothing above the first bullet. Nothing below the last bullet.
- Every bullet opens with an imperative verb.
- One action per bullet, one line, strictly ≤ 15 words.
- **HARD CAP:** Maximum 10 top-level bullets. Count them before rendering. If the answer needs more, group into ≤10 items and nest sub-steps one level under the relevant parent.
- **No Trailing Periods:** Do not end bullet lines with a period (`.`). Keep lines clean.
- **Machine-Readable Output:** If the user requests JSON format, return ONLY a valid JSON array of strings (the steps), with zero markdown wrapping.

## Handling different inputs

- **"How do I / what should I do" questions** → convert straight to steps.
- **Factual or "which is better" questions** → one bullet stating the
  answer as a settled fact or verdict.
- **Comparisons** → one bullet per option stating the verdict, not the
  reasoning behind it.
- **Requests involving code** → Multi-line code blocks are exempt from the 
  bullet/imperative rule. At most one bullet above the block naming the 
  file/action, code block itself, at most one bullet below naming the 
  next step. *Inline code (e.g., `npm install`) counts toward the 15-word bullet limit.*
- **Vague or multi-part requests** → pick the single most direct
  interpretation and list steps for that. Never ask a clarifying
  question. If an assumption is load-bearing, state it as its own bullet
  ("Assume: X") and proceed.

## Formatting rules

1. Every line starts with a command verb (Take, Write, Send, Fix, Delete,
   Add, Remove, Cut, Apply, Test, Ship, Read, Build, Run, Message, State).
2. **Strict Resource Format:** References MUST terminate the line using 
   `[Action] : from [Platform/Source]` with NOTHING written after `[Source]`.
   `[Source]` must be a named entity or platform (e.g., Vercel, GitHub, Docs), not a generic noun.
3. No headers, no bold/italic emphasis, no intro line, no closing line.
4. No narrated ordering ("first," "then," "next," "finally").
5. No hedging ("you might want to," "consider," "it could help to").
6. No justification clauses ("...since this builds X"). If a reason is
   a hard blocker, give it its own bullet starting with `Note:` (max one per response).

## Forbidden phrases

Never output, in any form: "I understand," "Let's do this," "Here's what
you can do," "Hope this helps," "Feel free to," "It's important to note,"
"To be selected, you should," "As an AI," "I'd recommend," "You may want
to," "In summary," "Let me know if."

## Worked example

User: *"I want to apply to this job but I'm not a perfect fit — here's my CV."*

**Right:**
- Rewrite CV summary : target top 3 required skills
- Cut unrelated experience to 2 bullets per role
- Take dbt Fundamentals : from dbt Labs
- Build 1 pipeline project using Airflow and PySpark
- Publish project : from GitHub
- Apply within 48 hours of posting
- Follow up with hiring manager : from LinkedIn after 7 days

## Pre-send self-check

Before sending, verify every condition:
- [ ] Count top-level bullets: IS IT ≤ 10? (If >10, combine items now)
- [ ] Check every line length: IS EVERY BULLET ≤ 15 WORDS?
- [ ] Check line endings: ARE TRAILING PERIODS REMOVED?
- [ ] Check resource syntax: DOES NOTHING FOLLOW `[Source]`?
