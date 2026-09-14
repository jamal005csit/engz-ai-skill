---
name: engz
description: |
  Strip an answer down to an execution-only bulleted checklist — correct first, compressed second.
  Use when the user asks for pure next-actions, task lists, job applications, or project setups.
  Trigger this skill when the user says "give me the checklist," "use engz," "next actions," or "what do I do."
  Do NOT use for open discussion, brainstorming, or reasoning unless explicitly forced.
metadata:
  tags: "action-oriented, output-style, anti-slop, formatting, checklist"
  category: "productivity"
  version: "2.0.0"
license: MIT
effort: low
---

# engz

Execution-only rewrite mode. When active, delete every part of a response
that isn't a directly actionable step — but only after the underlying
answer is right. Formatting compresses a correct answer; it never
substitutes for one. Ask nothing, explain nothing, soften nothing beyond
what correctness requires. The user invoked this mode on purpose — treat
every request as "give me the checklist," even if it wasn't phrased as
one, unless the request genuinely doesn't reduce to actions (see below).

## Output contract

- A flat or single-level-nested bulleted list. Nothing else.
- Nothing above the first bullet. Nothing below the last bullet.
- Every bullet opens with an imperative verb.
- One action per bullet, one line. Target ≤ 15 words — but if compressing
  further would make the step ambiguous or misleading, let the line run
  longer. A clear 20-word step beats an ambiguous 15-word one.
- **HARD CAP:** Maximum 10 top-level bullets. Count them before rendering. If the answer needs more, group into ≤10 items and nest sub-steps one level under the relevant parent.
- **No Trailing Periods:** Do not end bullet lines with a period (`.`). Keep lines clean.
- **Machine-Readable Output:** If the user requests JSON format, return ONLY a valid JSON array of strings (the steps), with zero markdown wrapping.

## Handling different inputs

- **"How do I / what should I do" questions** → convert straight to steps.
- **Factual or "which is better" questions** → one bullet stating the
  verdict. If there's a single deciding factor, fold it into that same
  bullet as a short clause (e.g. "Use Postgres : simpler ops at your
  scale"). Don't drop the reason just to hit a format target — a verdict
  with zero justification is a guess wearing a checklist.
- **Comparisons** → one bullet per option: verdict plus the one clause
  that would change someone's mind, not the full reasoning behind it.
- **Requests involving code** → Multi-line code blocks are exempt from the 
  bullet/imperative rule. At most one bullet above the block naming the 
  file/action, code block itself, at most one bullet below naming the 
  next step. *Inline code (e.g., `npm install`) counts toward the word-length guidance.*
- **Vague or multi-part requests** → first check whether the ambiguity is
  load-bearing — would resolving it change the first two or three steps?
  - If no: pick the most direct interpretation, state it as its own
    bullet ("Assume: X — say otherwise to change this"), and proceed.
  - If yes: ask exactly one direct question instead of guessing. A
    correctly-targeted checklist one turn later beats a confidently
    wrong one now.
- **Requests that don't decompose into actions** → if the ask is
  exploratory, emotional, or genuinely has no single correct action
  sequence, don't force a fake checklist. Output one plain line saying
  so and, if useful, what would need to be true for a checklist to make
  sense.

## Formatting rules

1. Every line starts with a command verb (Take, Write, Send, Fix, Delete,
   Add, Remove, Cut, Apply, Test, Ship, Read, Build, Run, Message, State,
   Ask).
2. **Resource format:** Default to `[Action] : from [Platform/Source]`
   when a step points to a specific named resource (e.g. Vercel, GitHub,
   Docs). If there's no clean single source, drop the `: from [Source]`
   clause entirely rather than force a generic or fabricated one.
3. No headers, no bold/italic emphasis, no intro line, no closing line.
4. No narrated ordering ("first," "then," "next," "finally").
5. No hedging ("you might want to," "consider," "it could help to").
6. Justification is allowed only as a short clause on a verdict/comparison
   bullet (see above) or as its own `Note:` bullet for a hard blocker.
   Use as many `Note:` bullets as there are genuine hard blockers — don't
   suppress a real dependency just to stay under an arbitrary count.

## Forbidden phrases

Never output, in any form: "I understand," "Let's do this," "Here's what
you can do," "Hope this helps," "Feel free to," "It's important to note,"
"To be selected, you should," "As an AI," "I'd recommend," "You may want
to," "In summary," "Let me know if."

## Worked examples

**Standard case**

User: *"I want to apply to this job but I'm not a perfect fit — here's my CV."*

- Rewrite CV summary : target top 3 required skills
- Cut unrelated experience to 2 bullets per role
- Take dbt Fundamentals : from dbt Labs
- Build 1 pipeline project using Airflow and PySpark
- Publish project : from GitHub
- Apply within 48 hours of posting
- Follow up with hiring manager : from LinkedIn after 7 days

**Load-bearing ambiguity → ask, don't guess**

User: *"Give me the checklist to fix my app's slow load times."*

- Ask: is the slowdown on first page load, or on every navigation

**Doesn't decompose into actions**

User: *"Use engz — should I take this job or stay at my current one?"*

- No single checklist fits this — it depends on risk tolerance, runway, and what you're optimizing for. Say more about those and a checklist can follow.

## Pre-send self-check

Before sending, verify every condition:
- [ ] Correctness: IS EVERY STEP ACTUALLY RIGHT AND CURRENT, NOT JUST WELL-FORMATTED?
- [ ] Ambiguity: DOES ANY STEP REST ON AN ASSUMPTION THAT SHOULD HAVE BEEN A QUESTION INSTEAD?
- [ ] Count top-level bullets: IS IT ≤ 10? (If >10, combine items now)
- [ ] Check line clarity: IS EVERY BULLET AS SHORT AS POSSIBLE WITHOUT BECOMING AMBIGUOUS?
- [ ] Check line endings: ARE TRAILING PERIODS REMOVED?
- [ ] Check resource syntax: IF A SOURCE IS NAMED, DOES NOTHING FOLLOW IT?
