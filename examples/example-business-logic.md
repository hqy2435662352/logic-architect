# Example: Business Logic Alignment

This example shows how Logic Architect can be used to align business rules **before** writing application logic.

---

## User Request

> Design a subscription system where teams can start on a free plan, upgrade to Pro, add seats mid-cycle, and downgrade later. Teams should keep access to paid features until the billing period ends, but admins should also be able to remove unpaid seats immediately if an invoice fails.

---

## Why Logic Architect Should Trigger

This request should trigger Logic Architect because it includes:

- multiple entities with interacting authority and billing relationships
- state inheritance across billing periods
- conflicting rules around downgrade timing vs immediate enforcement
- partial completion and failed payment scenarios
- downstream implementation risk if "access", "plan", and "seat" are not clearly separated

A normal drafting assistant might summarize the request.
Logic Architect first asks whether the request is logically closed.

---

## Sharp Mode Example Output

## Logic Alignment Draft

### Precision Challenges
1. **Are "plan access" and "seat entitlement" governed by the same rule, or are they separate control planes?**  
   - **Risk:** If they are treated as one thing, downgrade logic and failed-payment enforcement will conflict immediately.  
   - **Default Stance:** Treat subscription feature access and seat entitlement as separate but related layers.

2. **When payment fails, what survives until period end and what is suspended immediately?**  
   - **Risk:** The current request contains two competing policies: graceful downgrade timing and immediate administrative enforcement.  
   - **Default Stance:** Paid feature access remains until the current paid period ends unless the account enters delinquent enforcement; unpaid excess seats may be restricted immediately after a defined grace rule.

### M — Metadata
**Working Model**  
This is not just a plan-switching system. It is a layered entitlement model composed of account subscription state, seat allocation state, billing state, and administrative authority.

| Entity | Nature | Persistence | Governs / Depends On | Source of Truth |
|--------|--------|-------------|----------------------|-----------------|
| Workspace / Team | Account container | Persistent | Owns subscription and seat pool | Account record |
| Subscription Plan | Commercial tier | Persistent by billing period | Governs feature eligibility | Subscription ledger |
| Billing Period | Time boundary | Persistent | Determines when changes take effect | Billing ledger |
| Seat Allocation | Usage entitlement | Mutable persistent state | Depends on purchased quantity + admin assignment | Seat ledger |
| Invoice | Billing event | Persistent | Depends on plan / seats / cycle | Billing system |
| Payment State | Account status | Mutable persistent state | Depends on invoice settlement | Payments ledger |
| Admin | Authority role | Persistent role | Can assign / revoke seats within rules | Role / permission system |

**Topology Notes**
- Plan tier governs feature eligibility.
- Seat allocation governs who may consume access.
- Billing period governs when non-immediate commercial changes become effective.
- Payment state can trigger an enforcement layer that temporarily overrides standard downgrade timing.

### E — Essence
**Invariants**
1. Feature access and seat entitlement must not be modeled as the same object.
2. Scheduled downgrades must preserve already-paid access until the current billing period ends, unless delinquency policy explicitly overrides this.
3. No active user may occupy a seat that is not backed by an allowed entitlement state.

**Success Definition**
- The system is successful only if plan changes, seat changes, and failed-payment enforcement can coexist without contradictory access outcomes.

**Conflict Hierarchy**
- Compliance / billing enforcement > paid entitlement window > admin seat operations > convenience of uninterrupted access

**Critical Tradeoff**
- We optimize for entitlement correctness over maximum user continuity when billing failure creates conflict.

### N — Non-linear
**Backtrack Policy**
- If a scheduled downgrade is canceled before period end, the future state is removed and current entitlements remain.
- If payment recovers during grace, suspended enforcement can be reversed without rewriting the subscription history.

**Partial Completion**
- Allowed. The account may be in states such as `scheduled_downgrade`, `payment_failed`, `grace_period`, `seat_overage`, or `restricted`.

**Minimum Viable State**
- The minimum valid state is: current plan, billing period boundary, purchased seat quantity, assigned seat count, and payment state.

**Parallel / Conflict Handling**
- Mid-cycle seat additions update entitlement immediately and affect the next invoice or prorated charge.
- A downgrade request and a payment failure arriving together are resolved by enforcement priority: delinquency policy first, scheduled commercial transition second.

**Recovery Logic**
- Failed payment enters a grace window or enforcement state according to policy.
- Excess unpaid seats can be restricted immediately based on deterministic seat selection rules.
- Once payment is recovered, the system restores the allowed entitlement state without inventing new commercial history.

### Assumptions / Gaps
- We assume the product supports a defined grace period for failed invoices.
- We assume unpaid excess seats can be deterministically identified.
- We have not yet defined whether seat removal uses admin selection, last-assigned ordering, or activity-based priority.

### Execution Mapping
**Implementation Goal**
- Build a subscription and seat-entitlement model that separates commercial plan timing from immediate enforcement controls.

**Ordered TODOs**
1. Split the model into four layers: subscription, billing period, seat entitlement, payment enforcement.
2. Define canonical account states and allowed transitions.
3. Encode scheduled downgrade as a future-effective change, not an immediate mutation of current access.
4. Define delinquency policy and grace-window behavior.
5. Define deterministic seat restriction rules for unpaid overage.
6. Add restoration rules for payment recovery.

**Guardrails**
- Preserve: plan access and seat entitlement remain distinct.
- Avoid: using a single boolean like `is_pro` to drive all access decisions.
- Confirm before changing: whether failed payment immediately revokes premium features or only restricts overage seats.

**Minimal Shippable Version**
- Free/Pro plans, scheduled downgrade at period end, mid-cycle seat add, one failed-payment grace rule, and deterministic unpaid-seat restriction.

---

## Why This Example Matters

Without Logic Architect, it is tempting to write a simple rule set like:

- plan = Pro → enable paid features
- payment failed → disable Pro
- seats > paid seats → remove some seats

That implementation looks simple, but it collapses distinct logic layers into one control switch.
The real challenge is not toggling a plan flag.
The real challenge is reconciling:

- time-based entitlement
- seat-based entitlement
- billing enforcement
- administrative authority
- recovery after payment failure

Logic Architect forces those layers to be modeled separately before code is written.

---

## Example Follow-Up Prompt

```text
Use Logic Architect on this subscription and seat-management system before implementation.
```

Or, for deeper review:

```text
Use Hardcore Logic Architect mode on this billing and entitlement workflow.
```
