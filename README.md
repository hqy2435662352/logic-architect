# Logic Architect

English | [简体中文](README.zh-CN.md)

A pre-execution reasoning skill that challenges hidden assumptions, aligns complex requirements, and maps them into execution-ready structures using the **M.E.N. framework**:

- **M — Metadata**
- **E — Essence**
- **N — Non-linear**

Logic Architect is designed for cases where executing too early would lock in flawed assumptions.

---

## Why this exists

Many complex requests sound clear on the surface, but are structurally incomplete underneath.

Typical failure modes include:

- hidden assumptions are never surfaced
- entities are named but not structurally modeled
- invariants are implied but not defined
- conflict priorities are unclear
- rollback, interruption, and partial completion are ignored
- execution starts before the logic is aligned

Logic Architect exists to solve that problem.

It acts as a **pre-execution reasoning layer**: before writing the prompt, code, workflow, or skill, it forces the logic to become explicit.

---

## What makes it different

Logic Architect is **not** a passive summarizer and **not** a compliant drafting assistant.

Its job is to:

- expose logical vacuums before implementation
- challenge high-risk assumptions with precision
- model the structure of the request through M.E.N.
- bridge analysis into execution through an explicit handover layer

Its core principle is:

> **Preserve epistemic sharpness; reduce interaction friction.**

That means:

- do not soften away critical logic problems
- do not challenge for theatrics
- prefer one decisive challenge over many shallow ones
- preserve rigor, but keep the output usable

---

## The M.E.N. Framework

### M — Metadata

Models what exists in the system.

Focus:

- entities
- persistence vs runtime state
- source of truth
- topology
- ownership / gravity
- structural relationships

### E — Essence

Defines the system’s non-negotiable logic.

Focus:

- invariants
- success definition
- conflict hierarchy
- tradeoff priority

### N — Non-linear

Models how the system behaves under interruption, mutation, and disorder.

Focus:

- rollback / backtrack
- partial completion
- minimum viable valid state
- concurrency and conflict handling
- recovery logic

---

## When to use Logic Architect

Use it when the request involves:

- multiple interacting entities with cross-entity constraints
- state transitions, rollback, inheritance, or partial completion
- concurrency, priority collisions, or permission hierarchy
- workflow / protocol / orchestration logic
- skill design, agent design, or business logic framing
- any requirement where a wrong early assumption would corrupt downstream execution

Typical use cases:

- designing an agent skill
- planning a multi-step workflow
- structuring a business rule system
- aligning requirements before coding
- refining a prompt into a robust execution framework
- defining system behavior before implementation

---

## When not to use it

Logic Architect is usually unnecessary for:

- simple drafting
- low-risk content generation
- straightforward summarization
- tasks where trial-and-error is cheaper than upfront alignment

---

## Modes

### Sharp Mode

Default mode.

Use when:

- the task is complex enough to need alignment
- but not so risky that full explicit confirmation is required

Behavior:

- 1–2 precision challenges
- compact M.E.N. output
- direct execution mapping
- proceed with explicit assumptions when appropriate

### Hardcore Mode

Use when:

- the task is architecture-defining
- ambiguity is dense
- downstream cost of error is high
- the user explicitly wants deeper challenge

Behavior:

- denser challenge layer
- fuller M.E.N. analysis
- explicit assumptions and gaps
- confirmation gate before formal execution

---

## Workflow

Logic Architect follows this structure:

```text
[Request]
→ [Evidence-First Reset]
→ [Precision Challenge]
→ [M.E.N. Analysis]
→ [Execution Mapping]
→ [Confirmation / Direct Continuation depending on risk]
```

### 1. Evidence-First Reset

Do not blindly import old patterns.

Instead:

- suspend pattern reuse by default
- reuse prior abstractions only when current evidence supports them
- prefer structural clarity over familiar analogy

### 2. Precision Challenge

Surface the 1–2 assumptions most likely to break the task if left unexamined.

Examples:

- scope ambiguity
- rigidity disguised as requirement
- undefined conflict priority
- missing rollback behavior
- unclosed success criteria

### 3. M.E.N. Analysis

Build the logic frame:

- what exists
- what must remain true
- what happens when reality gets messy

### 4. Execution Mapping

Translate the analysis into execution structure.

This usually includes:

- implementation goal
- ordered TODOs
- key assumptions
- failure points to guard
- minimal shippable version

---

## Output Shape

A typical output looks like this:

```md
## Logic Alignment Draft

### Precision Challenges
1. [High-leverage challenge]
2. [High-leverage challenge]

### M — Metadata
[working model, entities, source of truth, topology]

### E — Essence
[invariants, success definition, conflict hierarchy]

### N — Non-linear
[rollback, partial completion, minimum viable state, recovery]

### Assumptions / Gaps
[what is assumed, what remains unresolved]

### Execution Mapping
[implementation goal, TODOs, guardrails, minimal valid next step]
```

---

## Example

### Input

> Design a workflow where one agent writes code, one reviews it, and one merges it automatically.

### Logic Architect thinking direction

- Who has final authority?
- What breaks deadlock if review loops forever?
- What is the minimum state required before merge becomes legal?
- Is review feedback advisory or blocking?
- What happens if merge succeeds but post-merge validation fails?

### Example aligned output

- **M:** writer, reviewer, merger, human override; source of truth is the review state plus branch status
- **E:** unverified code must never merge automatically; human override remains final
- **N:** after 3 failed review cycles, escalate to manual decision instead of looping forever
- **Execution Mapping:** define review states, add max-iteration guard, require final validation gate before merge

---

## Repository Structure

```text
.
├── SKILL.md
├── README.md
├── LICENSE
└── examples/
    ├── example-agent-workflow.md
    └── example-business-logic.md
```

---

## How to use

You can invoke it with prompts like:

```text
Use Logic Architect on this requirement before execution.
```

```text
Run a M.E.N. analysis on this workflow.
```

```text
Use Hardcore Logic Architect mode for this system design.
```

---

## Design Philosophy

Logic Architect is built on one belief:

> Bad execution often begins as unchallenged structure.

So instead of optimizing for speed at the first step, it optimizes for **structural correctness before momentum**.

It does not try to be polite at the cost of precision. It tries to be precise enough that politeness does not need to hide the problem.

---

## Status

Current version: **v2 draft**

This repository is evolving toward a production-grade skill specification that is:

- sharp in reasoning
- explicit in structure
- usable in real interaction
- bridgeable into implementation

---

## Contributing

Suggestions, critiques, and structural improvements are welcome.

Good contributions include:

- better trigger logic
- stronger M.E.N. definitions
- better execution handover patterns
- improved examples
- sharper but cleaner challenge formulations

Please open an issue or submit a pull request.

---

## License

This project is released under the [MIT License](LICENSE).
