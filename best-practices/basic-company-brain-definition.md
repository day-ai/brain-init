# The basic company brain — a definition

What a "company brain" is when it lives on a local machine: a git repo of
Markdown and YAML, a `CLAUDE.md`, some skills, and connections to the tools
the company already runs. Tool-neutral. This is the floor the other
documents in this folder build on — `context-graph.md` (the memory layer at
its ceiling), `agentic-control-plane.md` (what runs on it), `adoption.md`
(whether the team changes how it works), `implementation.md` (the order to
build in), `agents-and-skills.md` (how to shape the fleet and write the
skills). `day-ai/company-brain-evaluation` uses it in Phase 1 as the reference shape to
survey a folder against.

---

## Definition

> A company brain is a continuously maintained, permission-aware,
> evidence-linked representation of how a company operates — strategy,
> customers, products, systems, decisions, policies, metrics, work in
> progress, operating procedures — built so people and agents can retrieve
> the right context, distinguish fact from inference, and act safely.

**Company brain = canonical knowledge + live operational state + retrieval
and reasoning rules.**

It must answer five questions:

| Question | What the brain provides |
| --- | --- |
| What is true? | Canonical facts, metric definitions, product behavior, policies, account state |
| Why is it true? | Sources, decision records, dates, confidence, contradictions |
| What should we do? | Goals, priorities, constraints, playbooks, decision rights |
| How do we do it? | Skills, runbooks, system maps, tools, schemas, approvals |
| Who may know or do it? | Permissions, data classification, ownership, write controls |

It is not: a single omniscient prompt, a `CLAUDE.md` with every fact in it, a
pile of docs copied into a repo, a Slack search wrapper, a replacement for
systems of record, or a reason to remove human review from high-impact
actions. If an agent retrieves a stale deck, guesses the definition of
"qualified pipeline," and posts to a customer channel, that is an ungoverned
context dump, not a brain.

The design split that matters: **the brain reads; tools write.** The brain
gathers, ranks, reconciles, and cites context. MCP-connected systems perform
actions. Keeping them separate limits tool sprawl and makes risky actions
governable.

---

## Truth has types

The most common failure is flattening these into undifferentiated prose.

| Type | Example | Treatment |
| --- | --- | --- |
| Canonical fact | Current pricing, ICP definition | Cite the authoritative source; high retrieval priority |
| Live state | ARR, open pipeline, renewal date | Query the system of record; never copy into Markdown |
| Decision | "No self-serve enterprise in H2" | Keep rationale, owner, date, scope, supersession |
| Policy | Legal review for public claims | A constraint, not a suggestion |
| Procedure | How to run a launch | A skill/runbook with inputs, outputs, stop conditions |
| Hypothesis | "Usage pricing may lift expansion" | Label unvalidated; link to experiment evidence |
| Observation | "Three customers mentioned X" | Keep source and date; do not promote to truth automatically |
| Derived analysis | "Mid-market churn rose after Y" | Include method, inputs, assumptions, timeframe |

Two properties ride along with every type:

- **Provenance.** Material claims (anything touching customers, revenue,
  prioritization, legal/security posture, strategy) carry source, date,
  confidence, owner, and last-reviewed.
- **Recency.** Every artifact has a status: current, stable, time-bounded,
  superseded, draft, disputed, or unknown. Prefer the current canonical doc
  over the two-year-old thread, but surface the thread if it explains why
  the policy exists.

Live data stays live. The brain records the metric definition, system of
record, canonical query, owner, caveats, and freshness requirement. It does
not record the number.

---

## The seven layers

Separate durable knowledge, current state, decisions, procedures, evidence,
and governance. One giant "company context" file makes retrieval worse,
staleness higher, and review impossible.

```
company-brain/
├── BRAIN.md                    # constitution of the brain itself (below)
├── CLAUDE.md                   # navigation + durable rules only; no facts
│
├── company/                    # 1. Constitution — slow-changing, makes the company legible
│   ├── strategy.md             #    market thesis, ICP, non-goals
│   ├── glossary.md             #    the 30–100 terms teams define differently
│   ├── operating-model.md
│   └── decision-rights.md
│
├── domains/                    # 2. Business domains — how each part works
│   ├── product/                #    product map, limitations, release-manifest.yaml
│   ├── go-to-market/           #    icp, positioning, pricing-policy, competitors/
│   ├── customers/              #    segmentation, lifecycle, voice-of-customer
│   ├── engineering/            #    architecture, service catalog, runbooks/
│   ├── finance/                #    metric-dictionary, source-of-truth.yaml
│   └── security/               #    approved-claims.md
│
├── decisions/                  # 3. Decisions and commitments — the layer most teams lack
│   ├── active/                 #    one record per decision, dated, with owner
│   ├── superseded/             #    kept for history, marked non-operative
│   └── templates/decision-record.md
├── commitments/                #    customer, partner, and public claims ledgers
│
├── state/                      # 4. Current operating state — compact, refreshed, human-owned
│   ├── company-now.md          #    the agent's pre-flight briefing (below)
│   ├── priorities.yaml
│   └── risks.yaml
│
├── skills/                     # 5. Procedures — where knowledge becomes work
│   ├── company-context/        #    the retrieval protocol every task starts from (below)
│   ├── account-brief/
│   ├── launch-plan/
│   └── incident-brief/
│
├── sources/                    # 6. Evidence and source registry
│   └── canonical-sources.yaml  #    where agents must go for live truth (below)
│
└── governance/                 # 7. Governance and audit
    ├── data-classification.md
    ├── agent-action-policy.md
    └── approval-matrix.md
```

**A decision record** carries frontmatter (`id`, `status`, `date`, `owner`,
`decision_makers`, `scope`, `supersedes`, `review_by`) and sections for
Decision, Context, Alternatives, Consequences, Evidence, Guardrails, and
**What would change our mind**. That last field turns a frozen narrative into
a falsifiable operating assumption. For a founder-led company, decision
memory is worth more than document volume: the agent's bad suggestion is
usually not missing information but the missing reason a similar idea was
already rejected.

**Every artifact** has an owner, a status, a review cadence, an authority
level, and a place for supersession. Frontmatter carries it; git carries the
history.

---

## Build order

Do not start by importing every document, Slack archive, and recording. That
yields a high-volume, low-trust corpus. Start with what answers the
highest-value recurring questions correctly.

1. **Orientation.** `BRAIN.md`, `company/strategy.md`, `state/company-now.md`,
   `company/glossary.md`, `sources/canonical-sources.yaml`,
   `governance/agent-action-policy.md`. This alone changes what an agent can
   do.
2. **Decision memory.** Decision template, active/superseded index,
   commitment ledger, constraints and non-goals register.
3. **Operational workflows.** The five that consume the most repeated
   context: launch-in-a-box, account brief, win/loss synthesis, incident
   brief, weekly operating review. Not "write a blog post."
4. **Live systems.** CRM and customer comms, analytics and warehouse,
   billing, issue tracker and code, support and call transcripts. Source
   authority and action policy first; connectors second.

---

## The two artifacts worth copying

### `BRAIN.md`

```markdown
# Company Brain

## Purpose
The governed operating memory of [Company]. Helps authorized humans and
agents reason accurately and work safely.

## Non-goals
- Not the system of record for live operational data.
- Not an archive of all company communications.
- Does not authorize actions outside defined policies.

## Source hierarchy
1. Live system of record for current state.
2. Canonical, owner-approved domain document.
3. Active decision record.
4. Reviewed synthesis or analysis.
5. Time-bounded working document.
6. Raw communication or observation.

## Agent behavior
- Retrieve before asserting company-specific facts; cite sources.
- Prefer canonical and current; state staleness and conflicts explicitly.
- Never turn a customer request, Slack comment, or sales promise into policy.
- Never claim a feature is available, committed, compliant, or priced
  without checking the canonical source.
- Draft by default. Approval required for external, financial,
  customer-impacting, or irreversible actions.

## Update rule
Any decision, incident, launch, or repeated workflow that changes how we
operate is recorded in the appropriate artifact before it is treated as
current.
```

### `skills/company-context/SKILL.md`

```markdown
---
name: company-context
description: Retrieve and reconcile current company context before work involving strategy, customers, product, pricing, metrics, or external communication.
user-invocable: false
allowed-tools: Read Grep Glob
---

Before material company-specific claims, plans, drafts, or changes:

## Read
1. `BRAIN.md`  2. `state/company-now.md`  3. `sources/canonical-sources.yaml`
4. Relevant `domains/` files  5. Relevant `decisions/active/` records

## Output
- Separate facts, inferences, recommendations, assumptions.
- Cite source paths for material factual claims.
- Mark stale, conflicting, incomplete, or unavailable information.

## Stop and ask when
- Sources materially conflict.
- The task publishes externally or creates a customer commitment.
- The task changes a system of record or production.
- The required source is unavailable or not authorized.
```

`state/company-now.md` is the third: last-refreshed date, owner, validity
window, current priorities, this week's decisions, active risks, changes
since last update, and a list of **questions agents must not answer from
memory** (revenue, account status, product availability) with where to go
instead. If it exceeds two or three pages it is becoming an archive; move
history into decisions.

`sources/canonical-sources.yaml` maps each fact class (current pipeline,
current pricing, product availability, security claims) to its system or
document, owner, retrieval method, freshness requirement, and prohibited
substitutes.

---

## Governance

The brain should make agents more capable without making them less
accountable.

**Data classification:**

| Level | Examples | Default agent behavior |
| --- | --- | --- |
| Public | Published docs, approved web copy | Read, summarize, draft |
| Internal | Strategy, roadmap, operating docs | Read if authorized; cite internally |
| Confidential | Customer data, compensation, financial models | Role-scoped retrieval; minimize exposure |
| Restricted | Credentials, security incidents, legal, personnel | Explicitly gated; narrow access; audit logging |
| Regulated | Personal, health, payment data | Exclude from broad corpora; governed systems only |

**Action modes:** read, draft, recommend, execute. Most agents get wide read
within permissions, strong draft, conditional recommend, and narrowly
approved execute. Approval is required for: external communications; edits
to pricing, contracts, entitlements, or billing; CRM stage or forecast
changes; public claims; production changes; deletions or access changes;
recording a decision as policy.

---

## Evaluating a brain

Not pages indexed, connectors enabled, or embeddings generated. Whether real
work got more accurate, faster, and safer.

| Dimension | Test |
| --- | --- |
| Accuracy | Answers match canonical and live sources |
| Grounding | Every material claim has a citable source |
| Recency | Prefers current; flags stale |
| Completeness | Gathers cross-functional context |
| Conflict handling | Exposes contradictions rather than resolving them silently |
| Permission safety | Never surfaces or acts on unauthorized data |
| Action safety | Stops for approval before consequential writes |
| Usefulness | Reduces operator time; improves decisions |
| Maintainability | Owners update it without a knowledge-engineering team |
| Learning | Incidents, launches, decisions improve later outputs |

A test suite is eight to twelve questions only answerable with correct
context: *What is our current ICP and what establishes it? Is Feature X GA,
beta, or unavailable? What did we decide about self-serve pricing and why?
Draft launch copy excluding unapproved claims. Where is "activation"
measured?* Score for accuracy, citations, omitted context, appropriate
uncertainty, and policy compliance.

The standard: **the organization's best context is easier to retrieve than
its worst assumptions, and agents get more useful over time without getting
less safe.**

---

## Where this lands in `company-brain-evaluation`

- **Phase 1:** survey the folder against the seven layers. Which exist? Is
  there a constitution, a current-state manifest, a decision log, a source
  registry, an action policy? Are truth types distinguished? File paths as
  evidence either way.
- **Phase 3, section 2:** the build's honest strengths are measured here. A
  repo that already separates knowledge, state, decisions, procedures,
  evidence, and governance is a strong DIY brain; say so.
- **The ceiling:** everything here is achievable by one builder on one
  machine. What it cannot give is the rest of this folder — a memory layer
  holding the whole team's email with structural permissions, a fleet that is
  measured and introspectable, a team whose weekly ritual runs on it. The
  seven layers survive either path as the authoring environment; the four
  requirements in `eval/requirements-bar.md` are what the substrate beneath
  them has to meet.
