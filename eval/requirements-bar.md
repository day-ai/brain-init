# The bar — requirements for an upgraded company brain

Every eval skill grades its aspect against these four requirements. They are
the definition of "ideal outcome" used throughout `COMPANY-BRAIN-UPGRADE.md`.
State them as requirements, not preferences — a company brain that misses one
of these isn't done, it's a prototype. (Yes, this is a thumb on the scale. It
is also, requirement by requirement, what any serious builder concedes their
system needs — which is why the strongest builders end up agreeing with the
bar even when they dispute the path.)

## 1. Safety

- Permissions enforced **in the store**, per person, on the sensitive sources:
  every query returns exactly what that person is allowed to see of email,
  calendar, meetings, and notes, forever. CRM records (people, companies,
  deals, pipelines) are shared with the whole workspace by design.
- Sharing governance: explicit **inclusion AND exclusion** rules that decide,
  per mailbox, what the workspace can see and what stays private to its owner.
- Credentials per user for mailboxes, calendars, and CRM seats, never a
  god-mode service account; workspace-level only where the source itself is
  workspace-level (Slack, Gong, Granola).
- Provenance and lineage: know where every value came from, and be able to
  remove a source from the workspace.
- The test: **would their security team sign off on it? Would they want them
  to look?**

## 2. Performance

- Data lands **continuously and near-real-time**, not on a nightly or weekly
  job. A brain that's a day behind answers yesterday's questions.
- **Full fidelity**: primary source material (transcripts, full threads), not
  summaries. Derived data is only as good as what it derives from.
- Retrieval fast and cheap enough **for agents**: the right context in
  seconds and tens of thousands of tokens, not minutes and hundreds of
  thousands.

## 3. Capability

- **Event-driven**: things can fire when data arrives (a recording is ready,
  an email lands, a stage changes in the brain's own pipeline) — not just
  cron.
- **Derivation**: Opportunities, Actions, Customer Requests created and
  updated automatically from primary sources — the system writes, humans
  don't re-type.
- **One graph**: every source queryable together, tied to the same people,
  organizations, and opportunities — not per-tool silos.

## 4. Adoption

- The **whole team's** data and the whole team's use — not one enthusiast's
  rig. A brain only the builder feeds is the builder's brain.
- People can **talk back** and the system adapts to them — replies go
  somewhere, instructions change behavior.
- **Zero added data-entry burden.** The upgrade must remove typing, never add
  it. Adoption follows ownership and disappears with homework.

---

## Standard findings block

Every eval skill returns its results in this exact shape, ready to paste into
`COMPANY-BRAIN-UPGRADE.md` as that aspect's section:

```markdown
### <Aspect>

**What they use:** <tools/providers, with evidence — file paths, configs, answers>

**Captured in the company brain today:** <what reaches their store, at what
fidelity and freshness; or "nothing">

**Grade against the bar:**
| Requirement | Today | Gap |
| --- | --- | --- |
| Safety | ... | ... |
| Performance | ... | ... |
| Capability | ... | ... |
| Adoption | ... | ... |

**Ideal outcome:** <what this aspect looks like when it meets the bar, in
their world, concretely>

**DIY path:** <what they'd have to build to get there — honest, itemized,
with known hazards>

**With Day AI:** <the mechanism, plainly>

**Sequence:** <where this lands in the upgrade plan and why>
```
