# Example: Agent Workflow Alignment

This example shows how Logic Architect can be used **before** implementing a multi-agent coding workflow.

---

## User Request

> Design a workflow where one agent writes code, one reviews it, and one merges it automatically.

---

## Why Logic Architect Should Trigger

This request should trigger Logic Architect because it involves:

- multiple entities with distinct roles
- state transitions across a shared artifact (the code branch)
- approval and merge rules that can conflict
- potential deadlocks, retries, and escalation paths
- downstream implementation risk if the control model is wrong

A naive implementation would immediately start defining the three agents.
A Logic Architect pass first asks whether the control structure is valid.

---

## Sharp Mode Example Output

## Logic Alignment Draft

### Precision Challenges
1. **What breaks the deadlock if the reviewer keeps rejecting the writer forever?**  
   - **Risk:** The workflow looks autonomous on paper but can loop indefinitely in practice.  
   - **Default Stance:** Cap review cycles at 3, then escalate to human decision.

2. **Who has final authority when merge conditions and reviewer judgment disagree?**  
   - **Risk:** Two entities may both appear authoritative, causing illegal merges or blocked progress.  
   - **Default Stance:** Merge is allowed only when review state and validation state are both green; human override remains the final authority.

### M — Metadata
**Working Model**  
This is a gated multi-agent workflow around a shared branch state.
The writer proposes changes, the reviewer evaluates them, the merger executes branch integration, and the human operator retains ultimate override authority.

| Entity | Nature | Persistence | Governs / Depends On | Source of Truth |
|--------|--------|-------------|----------------------|-----------------|
| Writer Agent | Executor | Runtime | Depends on task + repo context | Task spec + branch diff |
| Reviewer Agent | Evaluator | Runtime | Depends on submitted patch | Review status |
| Merger Agent | Executor | Runtime | Depends on review + CI outcome | Merge eligibility state |
| Branch | Shared artifact | Persistent | Updated by writer / merger | Git state |
| Human Operator | Authority | Persistent role | Can override or terminate | Explicit human decision |
| CI / Validation | Gate | Runtime with stored results | Depends on branch contents | Validation result |

**Topology Notes**
- The workflow is not a peer-to-peer conversation; it is a gated state machine around merge eligibility.
- The writer does not directly control merge.
- The merger must depend on validated workflow state, not just reviewer sentiment.
- Human override sits above all agents and can halt, approve, or reject.

### E — Essence
**Invariants**
1. Unverified code must never merge automatically.
2. Human override remains the final authority.
3. Review completion alone is insufficient; merge also requires successful validation.

**Success Definition**
- The workflow is successful only if code reaches the target branch through a bounded, auditable path with no infinite loop and no unauthorized merge.

**Conflict Hierarchy**
- Human override > repository safety > validation status > reviewer outcome > writer output

**Critical Tradeoff**
- We optimize for repository safety over full autonomy when they conflict.

### N — Non-linear
**Backtrack Policy**
- A rejected patch returns to the writer with revision context.
- A failed validation after approval resets merge eligibility and returns the patch to revision.

**Partial Completion**
- Allowed. A patch may remain in draft, under review, revision requested, or ready-to-merge state.

**Minimum Viable State**
- The minimum valid state is: task assigned, patch submitted, review status known, validation status known.

**Parallel / Conflict Handling**
- Same branch, multiple active patches: require branch-level serialization or isolated feature branches.
- Same reviewer and merger disagreeing through asynchronous state: merge state follows gate evaluation, not stale review text.

**Recovery Logic**
- After 3 unsuccessful review-revision cycles, escalate to human review.
- If merge succeeds but post-merge validation fails, open a rollback or hotfix workflow under human supervision.

### Assumptions / Gaps
- We assume a human operator is available for escalation.
- We assume CI status is authoritative and cannot be bypassed by agents.
- We have not yet defined whether reviewer feedback is deterministic or probabilistic.

### Execution Mapping
**Implementation Goal**
- Build a bounded multi-agent coding workflow with explicit merge gates and escalation rules.

**Ordered TODOs**
1. Define workflow states: `draft`, `submitted`, `revision_requested`, `approved`, `validation_failed`, `ready_to_merge`, `merged`, `escalated`.
2. Define merge eligibility as a computed state from review + validation + branch safety.
3. Add a max-review-cycle guard before human escalation.
4. Define rollback behavior for failed post-merge validation.
5. Encode human override as an explicit control path, not an informal fallback.

**Guardrails**
- Preserve: no automatic merge without validation.
- Avoid: infinite reviewer-writer loops.
- Confirm before changing: whether validation can be advisory in any environment.

**Minimal Shippable Version**
- One writer, one reviewer, one merger, one human override path, one bounded review cycle counter, and one required validation gate.

---

## Why This Example Matters

Without Logic Architect, an implementation might jump straight into agent prompts and orchestration code.
That would likely miss the actual hard parts:

- authority boundaries
- deadlock prevention
- branch safety
- escalation semantics
- rollback after partial success

Logic Architect forces those decisions to become explicit **before** implementation begins.

---

## Example Follow-Up Prompt

```text
Use Logic Architect on this multi-agent coding workflow before implementation.
```

Or, if deeper challenge is desired:

```text
Use Hardcore Logic Architect mode on this autonomous coding pipeline.
```
