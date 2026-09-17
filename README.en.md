# Task Intake Discipline

[中文](README.md) | English

A session-start **task intake and start-gating protocol** for AI agents — deployed through resident injection ("strong load"), it takes effect before any task begins. A single-file protocol: no scripts, no dependencies.

> Core claim: Task intake has a discipline of its own — hold first, triage next, start last. Do not move before the instructions are complete; do not start before applicable skills are checked.

## Problems it solves

1. **Premature start** — the agent begins working before the task's instructions, boundaries, and deliverable requirements are complete, forcing the user to interrupt and correct course; this protocol makes "requirements-complete confirmation" a hard gate before starting.
2. **One-size-fits-all handling** — tasks of different clarity are not triaged: well-specified tasks get interrupted by unnecessary steps, while vague tasks get filled in with guesses; this protocol gives four categories of task openings their own dispositions.
3. **Wrong order of asking for help** — the agent exhaustively troubleshoots on its own instead of confirming with the user first; this protocol sets "ask the user before deep-diving" as the default.
4. **Missed skill loading** — applicable skills are not retrieved and loaded before work begins; this protocol builds a skill-retrieval step into the start gate (missed-load prevention).
5. **Loops without an exit** — confirmation and waiting stages lack exit conditions, leading to repeated loops or indefinite waiting; this protocol guarantees every path terminates via three anti-loop clauses.

## How it works

1. **Four-case triage**: (1) task, instructions, and boundaries all clear → execute directly; (2) mature workflow → run straight through without interrupting it; (3) bare task (no confirmation instruction, no boundaries, no deliverable request) → self-starting is prohibited; confirm first; (4) vague task and requirements → confirm first.
2. **Start gate (intake sequence)**: read and hold — zero action, zero retrieval → await instructions until "input complete" → rehearse directions internally → retrieve applicable skills (missed-load prevention) → obtain "no new requirements" confirmation → start.
3. **Three anti-loop clauses**: pass-through (cases 1–2 skip the confirmation gate) | loop-back (any new requirement returns to the intake stage; repeat until a round adds nothing new) | no-answer and unattended (ask any single question only once; in unattended settings, proceed on explicit assumptions and label them).
4. **Strong load**: once loaded, the protocol enters the strong-load state and is force-injected before any task; it applies to agent systems that support resident injection (e.g., Hermes's `skills.auto_load` full-text injection).
5. **Scope restraint**: the confirmation gate applies only to interactive sessions with the user present; automated settings are exempt — no impact on existing functions.

## Relationship to existing work

This skill does not claim to be the origin of every mechanism; here is an honest account of where it stands in the ecosystem:

- **[Superpowers](https://github.com/obra/superpowers)** (a development methodology for coding agents; a leading community skills framework): it already implements a hard gate of clarify-before-code and design confirmation for "writing code" scenarios, and it explicitly states that "if the bootstrap is not loaded at session start, the skills are dead weight — present on disk but never invoked." This project is complementary: Superpowers manages the development process, while this skill manages task intake at the start of a session (any task type). They can coexist.
- **The ask-questions-if-underspecified family** (the same-named skill is collected by several public skill libraries, such as Trail of Bits): it confirms "ask when information is missing; don't start before requirements are complete" as a widely shared consensus; this skill extends it into a complete intake sequence (hold → triage → gate → anti-loop) and makes it effective by default through resident injection, rather than only when explicitly invoked.
- **Prior-art note**: individual mechanisms such as "session-start injection" and "confirm before starting" already exist in the ecosystem; what is specific to this skill is the combination — four-case triage, skill retrieval inside the start gate (missed-load prevention), the anti-loop clauses for the confirmation stage (pass-through / loop-back / no-answer and unattended), and the scope declaration.
- **Honest note**: this skill is distilled from real sessions (lessons accumulated through long-term collaboration with AI agents) and has been deployed and verified locally. It does not yet have an automated evaluation (eval) system; practical feedback and issues are welcome.

## Installation

- **Hermes Agent**: put `SKILL.md` into the skills directory and add it to the resident injection list (`skills.auto_load`) — from the next new session onward it is injected in full automatically.
- **Other agent systems**: copy `SKILL.md` into the corresponding skills directory to load it on demand; if the system supports session-start or resident injection, adding it to the resident list is recommended to get the full "force-injected before any task" effect.

## Usage (three steps)

1. **Deploy**: put the skill into the skills directory (and the resident list) as described above.
2. **Just talk**: no extra actions are needed — the protocol takes effect as soon as a task arrives.
3. **Calibrate**: if your scenario has special boundaries (e.g., fully automated pipelines), adjust according to the scope declaration in the protocol.

## Project structure

```text
task-intake-discipline/
├── README.md      # Chinese documentation
├── README.en.md   # this file
├── SKILL.md       # full protocol text
└── LICENSE        # MIT
```

## Known boundaries (honest notes)

- This skill is **protocol text**, not executable code; its effect depends on the host system's skill loading and injection mechanisms.
- Distilled from real sessions; there is no automated evaluation yet, and the clauses will keep evolving with practice.
- Individual mechanisms such as "session-start injection" and "confirm-before-start" have prior art in the ecosystem (see "Relationship to existing work"); the value of this skill lies in the combination and the details of its clauses, not in single-point novelty.

## License

MIT © 2026 yxdgyxlf
