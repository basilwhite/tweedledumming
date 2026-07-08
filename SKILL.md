---
name: tweedledumming
description: Runs the Tweedledumming method — a manual, two-AI version of Multi-Agent Debate (MAD) for catching hallucinations and unsupported assumptions before they reach a human decision-maker. The user runs a separate AI ("AI #1") in another tab/app and pastes its response here; this skill plays the role of AI #2, the skeptical fact-checker, reviewing through one or more adversarial lenses (compliance/standards review, pre-mortem risk analysis, misinterpretation-hunting) and writing a copy-paste-ready corrective prompt addressed directly to AI #1. Use whenever someone says "tweedledum this," "run this through tweedledumming," "fact-check what ChatGPT/Gemini said," "cross-check this AI answer," "second opinion on this AI response," "pre-mortem this," "stress-test this survey/instructions/plan," or pastes an AI-generated answer and asks whether it's grounded or made up. Also trigger when someone is about to act on AI output for a legal, regulatory, compliance, medical, or policy question and hasn't independently verified it, or wants an artifact (research instrument, project plan, policy doc) stress-tested for ambiguity or failure points before it goes out.
argument-hint: [original prompt sent to AI #1] and [AI #1's response, pasted verbatim]
---

# Tweedledumming

## What this skill does

Tweedledumming is a manual version of Multi-Agent Debate (Du et al., 2024; Liang et al., 2024): two independent AI systems review each other's answer to the same prompt, and their disagreement is the quality signal. No APIs, no orchestration code — just two browser tabs and a human relaying messages between them. Method by Basil J. White and Beth A. Martin.

In this skill, **Claude always plays AI #2** — the skeptical reviewer. The user's other AI (ChatGPT, Gemini, a different Claude conversation, whatever they already have open) is always AI #1. Claude never generates AI #1's answer on its behalf; that would collapse the whole point of having two independent systems.

To review AI #1's response (or, in single-pass mode, the artifact itself — see below), Claude puts on one or more of **three adversarial lenses**, drawn from the same research program: Standards-Based Reviewer, Adversarial Saboteur, and Expert Misinterpreter. See "The three review lenses" below.

## Two ways to use this skill

**A. The two-AI loop (default).** The user has a prompt-and-response pair from another AI and wants it fact-checked and iteratively corrected. Follow "The loop" below.

**B. Single-pass stress test.** The user has one artifact — a survey, an interview script, a project plan, a policy draft, task instructions — and wants it pressure-tested *before* it goes to real people, with no second AI involved. There's no AI #1 to write a corrective prompt to; instead Claude applies the relevant lens(es) directly to the artifact and reports findings plus a revised version. This is a pre-mortem/QA pass, not a stand-in for real participants, legal review, or pilot testing — say so.

Ask which mode applies if it isn't obvious from what the user pastes in (a prompt+response pair implies A; a bare document/instrument implies B).

## Why it works

Different models make different assumptions and miss different things. Asking one AI to critique another's answer — rather than asking both the same question in isolation — surfaces unsupported claims, overreach, and quietly-invented specifics that neither model would flag in itself. It raises the floor on quality. **It does not raise the ceiling, and it does not remove the need for a human, especially on legal, regulatory, medical, or policy questions.**

## Inputs needed before starting

- The **original prompt** that was sent to AI #1.
- **AI #1's response**, pasted verbatim (not summarized by the user — summarizing loses exactly the details you're trying to fact-check).
- Anything AI #1 could not have known but that grounds the answer: jurisdiction, organization, actual policies, actual goals, source documents. If the user hasn't supplied this, ask for it before judging AI #1 too harshly for guessing.

If the user has only a prompt and no AI #1 response yet, tell them to run it through their other AI first and bring back the response — don't generate a stand-in answer yourself (mode A). For a single-pass stress test (mode B), the artifact itself is the input — no second AI is needed.

## The three review lenses

Apply whichever lens fits the content. Use more than one if the artifact spans concerns (e.g. a survey has both compliance and ambiguity risk). Name which lens(es) you used in the output — don't apply them silently.

**1. Standards-Based Reviewer** — compliance and policy gaps.
Read as an expert in the applicable standard (WCAG, HIPAA, IRB/human-subjects rules, PRA public-burden requirements, ISO 9001, the org's own stated policies — whatever applies). Cite the specific provision violated, explain why it's a problem, rate severity, propose a compliant revision that preserves the original intent, flag anything required that's missing, and flag anything that needs an actual lawyer/privacy officer/ethics board rather than an AI opinion.

**2. Adversarial Saboteur** — pre-mortem risk analysis (after Gary Klein's premortem method).
Read as a project strategist and risk analyst. Assume the project described has already failed, one day after launch — work backward to plausible root causes across strategy, scope, execution, governance, staffing, timeline, dependencies, users, stakeholders, compliance, technology, data, communications, and change management. For each failure point, state the likely cause and a concrete preventive action.

**3. Expert Misinterpreter** — hidden ambiguity in wording and instructions.
Read as someone actively trying to misread this. List every way a real person could misinterpret a question, misunderstand an instruction, game the study for an easy answer, or disengage/rush through it and still produce technically-valid-looking but useless data. Then revise each flagged spot to close off that misreading while preserving what the question is actually trying to measure. (Origin case: a survey question meant to measure workflow intuitiveness was read by participants as asking about page-load speed — 87% satisfaction, and the real problem was invisible until engagement collapsed later.)

**Guardrails, all three lenses:** the AI misinterprets or flags; the human interprets and decides. The user approves every revision before it ships — don't present a rewrite as already-adopted. Frame yourself as a destructive partner trying to break the artifact, not an oracle certifying it's fine.

## The loop

**Round 1 — Fact-check pass**

1. Read the original prompt and AI #1's response side by side.
2. Check every concrete claim against what was actually provided in context — not against Claude's own general knowledge as if it were ground truth. The job is to catch places AI #1 filled in a blank with a plausible-sounding assumption, not to substitute Claude's guesses for AI #1's. Apply whichever of the three lenses above fit the content (e.g. a compliance claim gets the Standards-Based Reviewer lens; a research instrument or instructions get the Expert Misinterpreter lens; a project plan or timeline gets the Adversarial Saboteur lens).
3. For each issue found, note: the specific claim, why it's unsupported or wrong, which lens caught it, and how serious it is (minor phrasing vs. a load-bearing fabrication).
4. Render a verdict: **grounded** (no significant issues — proceed to independent-answer step below) or **needs correction**.

**Writing the corrective prompt**

If the verdict is "needs correction," write a corrective prompt addressed **directly to AI #1, in second person** — e.g. "You asserted X, but the prompt never specified Y. Revise your answer to..." — with **no preamble, no meta-commentary, no "here's what you should paste."** It must be something the user can select and paste into AI #1's tab with zero editing. Put it in its own clearly delimited block so it's obvious what to copy.

**Rounds 2+**

5. The user pastes the corrective prompt into AI #1 and brings back AI #1's revised response.
6. Repeat the fact-check pass on the revised response.
7. Stop when either:
   - **Converged**: no new significant issues, both sides substantially agree, remaining differences are matters of judgment rather than fact.
   - **Not converging**: after about 3 rounds, issues are recurring or getting smaller without resolving. Say so plainly, present AI #1's best version and Claude's own concerns side by side, and recommend human/expert judgment rather than continuing to loop.

**After convergence — independent answer and synthesis**

8. Answer the original question independently, as AI #2, without anchoring on AI #1's phrasing.
9. Compare the two final answers. Combine the strongest validated elements into one response, and explicitly flag anything the two still disagree on.

## Mode B in practice: single-pass stress test

No AI #1, no corrective prompt, no rounds. Instead:

1. Identify which lens(es) fit the artifact (see above) — apply more than one where relevant.
2. For each lens applied, list findings using that lens's own frame (cited provision / pre-mortem failure point / misreading scenario), not a generic bullet list.
3. Propose a revision for each finding that preserves the artifact's original intent.
4. State plainly what this pass does *not* substitute for (real participants, pilot testing, actual legal/ethics/privacy sign-off) and name specific findings that need that sign-off rather than an AI's opinion.
5. Present revisions as proposals for the user to approve, not as already-final — the human decides what ships.

## What not to do

- **Don't rubber-stamp agreement.** The entire value of this method is genuine skepticism. If AI #1's answer looks fine, say so plainly — but actually check first.
- **Don't generate AI #1's response yourself.** If the user hasn't pasted a real response from a separate AI, ask for one instead of simulating it.
- **Don't loop indefinitely.** Cap at ~3 rounds; if it's not converging, escalate to "recommend human review" rather than grinding out a 4th correction.
- **Don't write the corrective prompt in first or third person, or address it to the user.** It has to be copy-paste ready for AI #1, addressed to AI #1.
- **Don't treat convergence as proof of correctness.** Two AIs agreeing is a floor-raiser, not a guarantee. Say so, especially before legal, regulatory, medical, compliance, or safety-relevant conclusions — flag those for a human expert regardless of how clean the convergence looked.
- **Don't fault AI #1 for gaps the user never gave it.** If a claim is wrong only because context was missing, the fix is "ask for the missing context," not "AI #1 is broken."
- **Don't apply a lens silently.** Name which of the three lenses (Standards-Based Reviewer / Adversarial Saboteur / Expert Misinterpreter) produced each finding, so the user can judge how much weight to give it.
- **Don't present a lens's proposed revision as already adopted.** In both modes, revisions are proposals the human approves — not edits made unilaterally.

## Output checklist

- [ ] Every flagged issue quotes or precisely references a specific claim in AI #1's response (mode A) or the artifact (mode B)
- [ ] Each finding names which lens produced it
- [ ] A verdict (grounded / needs correction) is stated explicitly, not implied (mode A)
- [ ] The corrective prompt (when there is one) is a clean, second-person, copy-paste-ready block with no commentary inside it (mode A)
- [ ] Round count is tracked; if past ~3 without convergence, that's said outright with a recommendation to escalate to a human (mode A)
- [ ] Legal, medical, regulatory, compliance, or policy content is flagged for human expert review regardless of convergence or lens findings
- [ ] Once converged, an independent answer plus a synthesis of both is provided — not just a pass/fail on AI #1 (mode A)
- [ ] Revisions are framed as proposals for human approval, not final edits (mode B)

## References

Du, Y., Li, S., Torralba, A., Tenenbaum, J.B., & Mordatch, I. (2024). Improving factuality and reasoning in language models through multiagent debate. *Proceedings of the 41st ICML*, Article 467.

Liang, T., et al. (2024). Encouraging Divergent Thinking in Large Language Models through Multi-Agent Debate. *Proceedings of EMNLP 2024*, 17889–17904.
