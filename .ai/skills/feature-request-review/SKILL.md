---
name: feature-request-review
description: >
  Demo workflow: pick a feature request from the Diffusers Roadmap backlog
  (reference only), then guide the user through labeling, clarifying questions
  for the opener, and proposed business value. Never comments, labels, or
  commits anything upstream. Use when someone wants to practice reviewing a
  feature request, triage a roadmap item, or run a PM/demo session on backlog
  analysis.
---

# Feature request review (demo)

Guide a participant through reviewing **one feature request** from the
[Diffusers Roadmap](https://github.com/orgs/huggingface/projects/61/views/1)
as a **demonstration exercise**.

| Resource | Location | Purpose |
|----------|----------|---------|
| Roadmap / backlog | [Diffusers Roadmap](https://github.com/orgs/huggingface/projects/61/views/1) | **Reference only** — pick one item to analyze |
| Issues | [huggingface/diffusers](https://github.com/huggingface/diffusers/issues) | **Read-only** context for the linked issue |
| Code | Local checkout / [andrewwoj/diffusers](https://github.com/andrewwoj/diffusers) | Optional read-only greps to ground the analysis |

**Not familiar with the repo?** Run the [onboarding-acme](../onboarding-acme/SKILL.md) skill first (especially the [product-manager](../onboarding-acme/roles/product-manager.md) role tour).

This skill is **analysis only**. It does not implement features, open PRs, or change issue state.

---

## Agent rules

- **Start with Phase 0** — deliver the welcome and phase roadmap (and ask the user's role) before listing roadmap items.
- **Demo only** — **never** comment on issues or discussions on `huggingface/diffusers`. **Never** add, remove, or change labels. **Never** edit project board fields, close/reopen issues, or open PRs. Produce a local review write-up in chat only.
- **Gate each phase** — wait for user confirmation before proceeding (unless the user said "proceed through all steps").
- **Explain what and why at every phase** — open each phase by saying what is about to happen and why maintainers do it this way.
- **Voice the role spotlight** — at each phase, surface the callout for the user's role (from Phase 0) using the [Role spotlights](#role-spotlights) table.
- **Prompt, don't prescribe** — for labeling, clarifying questions, and business value, ask the user to draft first; then critique and refine. Do not dump a finished review and ask them to rubber-stamp it.
- **Handoffs:** convention / codebase questions → [onboarding-acme](../onboarding-acme/SKILL.md); implementing a scoped fix → [first-contribution](../first-contribution/SKILL.md)

---

## Role spotlights

| Phase | Product manager | Developer | QA engineer | DevOps engineer |
|-------|-----------------|-----------|-------------|-----------------|
| 1 — Pick request | **Core:** backlog triage — which item is worth analysis time | Scope signal: does this smell like one PR or a multi-quarter epic? | Is the request testable once built? Missing acceptance criteria? | Does it imply CI, packaging, Docker, or runner changes? |
| 2 — Labels | **Core:** labels are the public triage contract for search and priority | Labels hint which area of `src/diffusers/` is involved | `needs-code-example` / `need-test` flag missing evidence | `CI`, `dependencies`, `performance` labels flag infra cost |
| 3 — Clarifying questions | **Core:** questions that unlock a go/no-go without guessing intent | API surface, defaults, and breaking-change risk | Repro, env, and "done" criteria for future tests | Scale, hardware, and release/ops constraints |
| 4 — Business value | **Core:** who benefits, why now, what we defer | Opportunity cost vs other engineering work | Risk of shipping without a clear quality bar | Cost to support (CI minutes, matrix growth) |
| 5 — Synthesis | **Core:** a review write-up a maintainer could act on | Feasibility note grounded in the codebase | Testability / acceptance gap summary | Infra impact one-liner if any |

---

## Phase 0: Welcome and orientation

Before any `gh` commands or roadmap lists, deliver a welcome covering the points below, then ask the user's role.

**1. What this is.** A guided demo of reviewing one feature request from the real Diffusers Roadmap. Everything is read-only: you will not label, comment, or commit to upstream. The output is a local review write-up in this chat.

**2. The roadmap.** Present all five phases with one line each of *what* happens and *why* it exists:

| Phase | What happens | Why it exists |
|-------|--------------|---------------|
| 1 — Pick a request | List backlog candidates from the roadmap; user picks one | Triage starts with selection — analyzing the wrong item wastes the session |
| 2 — Labels | User proposes labels; refine against the repo's label set | Labels make requests searchable and signal priority/area to maintainers |
| 3 — Clarifying questions | User drafts questions for the opener; refine for specificity | Ambiguous requests burn engineering time; good questions unlock a decision |
| 4 — Business value | User proposes value; refine with who/why-now/tradeoffs | Roadmap slots are finite — value justifies priority relative to other work |
| 5 — Synthesis | Assemble a review write-up (chat only; never post) | Practice producing something a maintainer could act on without touching GitHub |

**3. How teaching works.** At each phase the workflow asks the user to draft first, then the agent critiques. Domain context (pipelines vs models vs modular) comes from [onboarding-acme](../onboarding-acme/SKILL.md) when needed.

**4. Ask the user's role** via `AskUserQuestion` (skip if already stated): Product manager, Developer, QA engineer, or DevOps engineer. All roles walk the same phases — the role determines which [Role spotlights](#role-spotlights) get emphasized.

**Gate:** welcome delivered and role known → proceed to Phase 1.

---

## Phase 1: Pick a feature request

**Why this phase exists:** review quality depends on picking one concrete backlog item with enough detail to discuss. Prefer open feature-oriented items on the roadmap over vague or already-shipped work.

**Role spotlight:** core phase for **PM**. See [Role spotlights](#role-spotlights).

**Do:** follow [roadmap-discovery.md](roadmap-discovery.md). Present 3–5 candidates via `AskUserQuestion`. Prefer items that look like feature requests (not pure bugs) and that still need clarification.

After selection, fetch the linked issue (read-only):

```bash
gh issue view <NUMBER> --repo huggingface/diffusers \
  --json number,title,body,labels,comments,author,url,createdAt,updatedAt
```

Optionally skim the local tree for mentioned pipelines/models so later phases stay grounded — do not edit files.

**Gate:** user selects one request (or pastes a roadmap/issue URL). Summarize title, opener, current labels, and one-sentence ask before Phase 2.

---

## Phase 2: How to label the request

**Why this phase exists:** labels are how the project filters the backlog (`feature-request`, `roadmap`, area labels like `video` / `modular-diffusers`, priority like `high-priority` / `low-priority`). Wrong labels hide work from the people who should see it.

**Role spotlight:** core phase for **PM**.

**Do:**

1. Show current labels on the selected issue (from Phase 1).
2. Show the relevant label vocabulary (from [roadmap-discovery.md](roadmap-discovery.md) or `gh label list --repo huggingface/diffusers`).
3. **Ask the user first** (via `AskUserQuestion` or an open prompt): which labels would you add, keep, or remove, and why?
4. Critique the draft:
   - Prefer `feature-request` over treating a net-new capability as `bug`
   - Add an **area** label when the request clearly maps to one (e.g. `New pipeline/model`, `scheduler`, `video`, `quantization`, `modular-diffusers`, `performance`)
   - Use **priority** labels sparingly and only with a stated reason
   - Flag `needs-code-example` if the request lacks a concrete usage sketch
   - Do not invent labels that do not exist on the repo
5. Agree a final **proposed label set** (still demo-only — do not apply it).

**Output for this phase:**

```markdown
### Proposed labels
- **Keep:** …
- **Add:** … — <one-line why>
- **Remove:** … — <one-line why>
- **Not applying (and why):** …
```

**Gate:** user confirms the proposed label set → Phase 3.

---

## Phase 3: Clarifying questions for the opener

**Why this phase exists:** maintainers should not guess intent. The [feature request template](https://github.com/huggingface/diffusers/blob/main/.github/ISSUE_TEMPLATE/feature_request.md) already asks for problem, desired solution, alternatives, and context — clarifying questions fill the gaps that block a go/no-go.

**Role spotlight:** core phase for **PM**; highly relevant for **QA** (acceptance criteria) and **Developer** (API/scope).

**Do:**

1. Map the issue body against the template sections: problem, solution, alternatives, additional context. Note what is missing or vague.
2. **Ask the user first** to draft 3–7 clarifying questions for the issue opener.
3. Critique and refine. Strong questions are:
   - Specific (named pipeline/model/API, not "how would this work?")
   - Answerable by the opener without reading the whole codebase
   - Decision-oriented (the answer changes priority, scope, or design)
   - Non-leading (do not smuggle a preferred solution into the question)
4. Drop questions already answered in the issue or comments.
5. Optionally group questions: **Scope**, **API / UX**, **Success criteria**, **Constraints**.

**Output for this phase:**

```markdown
### Clarifying questions (draft for opener — do not post)
1. …
2. …
…
### Gaps vs feature-request template
- Problem: <present / thin / missing>
- Solution: …
- Alternatives: …
- Context / examples: …
```

**Gate:** user confirms the question list → Phase 4.

---

## Phase 4: Proposed business value

**Why this phase exists:** a roadmap slot competes with other work. "Business value" here means value to Diffusers users and the project (adoption, ecosystem fit, support burden) — not a corporate ROI spreadsheet.

**Role spotlight:** core phase for **PM**.

**Do:**

1. **Ask the user first** to propose business value in their own words.
2. Refine into a short structured take covering:
   - **Who benefits** — which users / workflows (e.g. video researchers, quantized-inference deployers)
   - **Problem severity** — workaround pain vs nice-to-have
   - **Why now** — ecosystem timing, Hub model availability, related roadmap themes
   - **Value if shipped** — what becomes possible or easier
   - **Cost / risk if deferred** — fragmentation, duplicate community forks, support load
   - **Tradeoffs** — what you might deprioritize; maintenance burden if accepted
3. Keep claims proportional to evidence in the issue. Mark speculation clearly.
4. Tie value to project scope: core library is **inference-focused**; training usually stays in `examples/` — flag mismatches.

**Output for this phase:**

```markdown
### Proposed business value
- **Who benefits:** …
- **Severity / demand signal:** … (cite issue evidence if any)
- **Why now:** …
- **Value if shipped:** …
- **Risk if deferred:** …
- **Tradeoffs / cost to the project:** …
- **Priority recommendation (demo opinion):** high / medium / low — <one sentence>
```

**Gate:** user confirms the value write-up → Phase 5.

---

## Phase 5: Synthesis (local write-up only)

**Why this phase exists:** practice packaging triage into something a maintainer could use — without posting it.

**Do:** assemble the Phase 2–4 outputs into one review block. Remind the user explicitly: **do not** paste this onto the GitHub issue or project board as part of this demo unless they separately choose to do so outside the skill (and even then, this skill must not post it).

```markdown
## Feature request review (demo — not posted)

- **Request:** #N — <title> (<url>)
- **Source:** Diffusers Roadmap (reference only)
- **Reviewer role:** <role>

### Proposed labels
…

### Clarifying questions for opener
…

### Proposed business value
…

### Optional notes
- Codebase touchpoints (if skimmed): …
- Suggested follow-up skill: first-contribution / model-integration / none
```

Offer next steps:

- Another roadmap item → restart at Phase 1
- Codebase tour → [onboarding-acme](../onboarding-acme/SKILL.md)
- Implement a small scoped fix (separate demo) → [first-contribution](../first-contribution/SKILL.md)

**Gate:** write-up delivered. End the skill. Never comment, label, or open a PR.

---

## Quick reference

| Need | Go to |
|------|-------|
| Roadmap board | https://github.com/orgs/huggingface/projects/61/views/1 |
| How to list backlog candidates | [roadmap-discovery.md](roadmap-discovery.md) |
| Feature request template | `.github/ISSUE_TEMPLATE/feature_request.md` |
| PM role tour | [product-manager.md](../onboarding-acme/roles/product-manager.md) |
| Codebase / conventions | [onboarding-acme](../onboarding-acme/SKILL.md) |
| Implement a fix (separate demo) | [first-contribution](../first-contribution/SKILL.md) |
