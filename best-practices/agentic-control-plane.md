# Agentic control plane best practices

The orchestration layer over a company brain. This doc defines what separates
an **agentic control plane** from what most DIY builds actually are — a
scheduler with good prompts: a fixed set of shared skills on cron, one prompt
for everyone, hand-maintained by the one person who built it, exactly as good
on day 300 as on day 1.

Ten practices. Each pairs a diagnostic question (ask it of any agent system,
including your own) with why it matters, what good looks like, and how Day AI
implements it as the reference implementation. Used by `day-ai/company-brain-evaluation` as the
audit lens for the control plane half of `COMPANY-BRAIN-UPGRADE.md`.

A control plane gives agents a world to work in: memory, identity,
permissions, and loops. A skill runner executes prompts. The difference is
the subject of this document.

Whether anyone *uses* the control plane is a separate question with its own
evidence — see `adoption.md`. The two documents disagree with nobody: a real
control plane with no leadership ritual attached goes flat, and a beautiful
ritual on a scheduler with good prompts plateaus. Both halves, then the
habit.

---

## 1. Outcomes are measured, not vibed

**Ask:** *How much time have you given your team back? Are their calendars
different from six months ago?*

**Why it matters:** a system with no measured outcome is a project, and
projects get cut. It also reveals whether the system has evaluation at all:
if you can't measure the time or the behavior change, you can't measure
quality drift either.

**What good looks like:** engagement and outcome instrumented natively —
skill runs, thread reads, replies, actions taken — plus a visible artifact:
calendar blocks for CRM data entry at zero, mornings starting with reviewed
pipeline instead of tab-loading.

**Reference implementation:** every run, thread, and notification is
inspectable per skill, per agent, per user, fleet-wide, queryable in natural
language.

**What we have seen about what to measure:** count conversations people start
and replies they send, not briefings delivered. A workspace can deliver a
thousand scheduled briefings in three weeks while its team opens two dozen
conversations. Delivery counts would call it healthy; it is not
(`adoption.md`, practice 7).

## 2. Using the system makes it better

**Ask:** *Does using your skills make them better?* Distinguish the model
improving (the model provider's work) from the system improving (yours). If a
thousand runs later the prompts are byte-identical, it's a scheduler.

**Why it matters:** compounding is the entire economic argument for the
agent-native model. A flat system is a cost; a compounding one is an asset
that widens the gap every day.

**What good looks like:** three mechanisms stacked — every interaction
enriches the memory layer (memory compounds), every user reply tunes that
person's skills (personalization compounds), and admin evaluation over the
drift harvests winners for everyone (the org compounds).

**Reference implementation:** all three run on the same substrate; see
practices 4, 6, and 7.

## 3. The fleet is introspectable

**Ask:** *How do you know what's working and what isn't? What do people like
and dislike?* "I ask them in Slack" is anecdote collection by the person who
is already the bottleneck.

**Why it matters:** without run-level and engagement-level introspection,
skill quality degrades silently as the business moves. You find out about
drift from the damage.

**What good looks like:** full run history, output scoring, and engagement
analytics per skill, per agent, per user — including the strongest signal a
shared-bot architecture cannot have by construction: whether people talk back
to their agents, and what they say. The Monday-morning question — "which of my
deployed skills got ignored last week, and why" — has a five-minute answer.

**Reference implementation:** run history doubles as an evaluation corpus; a
scheduled skill or an agentic-harness job can score the last N runs of every
skill against a rubric and report which variants people engage with.

**What we have seen about what to look for:** the signal that predicts a live
workspace is not run volume but whether a leader acted on a briefing — quoted
its number, ran a meeting off it. Even with full run history that signal is
easy to miss unless someone looks for it deliberately; make it a standing
query (`adoption.md`, practices 4–5).

## 4. Users talk back, and the agent actually changes

**Ask:** *Can users reply to an agent to continue the conversation? Can they
tell it how they like things done — and does it durably adapt?* In a
shared-bot architecture the answer is structurally no: one prompt for
everyone, no per-user object to update.

**Why it matters:** this is what people expect from anything calling itself
an agent now, from their own daily usage. People dismiss AI that does not
listen, and non-adoption follows regardless of prompt quality. This is the
ownership question, and ownership decides adoption: a named agent someone can
coach converts a tool they tolerate into a system they feed.

**What good looks like:** a reply to a skill run is a new prompt against full
context. An instruction — "shorter," "lead with the dollar figure," "coach me
before the next call instead of after this one" — updates the definition, the
agent confirms what changed, and every subsequent run reflects it. No
settings panel, no ticket to RevOps. Agents are shaped like people: names,
job descriptions, defined access, a specific human they report to — a small
staff per person, not one bot for everyone.

**Reference implementation:** both behaviors are core mechanics, and agent
identity (name, operating brief, scope, reporting line) is the deployment
model, not an aesthetic.

## 5. Feedback is a readable dataset

**Ask:** *Can you see how user feedback is improving the skills?* Even a
build that hacks per-user prompt patches has no view over the patches —
feedback disappears into files.

**Why it matters:** feedback you can't observe is feedback you can't learn
from. The loop isn't closed until someone — or something — can read all of it.

**What good looks like:** skill definitions and agent identities as readable,
versioned objects. An admin can diff any skill over time, snapshot fleet
state, and see exactly how each person's feedback reshaped their variant.

**Reference implementation:** the feedback layer is itself a dataset,
queryable like everything else.

## 6. The drift gets harvested

**Ask:** *Can you take the patterns, prompt language, and techniques that
work best and make sure everyone benefits?* In a same-skills-for-everyone
system the question doesn't parse — there is no variation to harvest.

**Why it matters:** one rep coaches their agent into a step-function
improvement; in a locked system that improvement is trapped with that rep,
and the system's ceiling is its median user. With harvest, anyone's local
maximum becomes the team's new floor. Personalized drift is the R&D; the
control plane is what lets you collect it.

**What good looks like:** the admin maps over all personalized variants,
spots what the best performers changed, and rolls it out fleet-wide as a
managed or suggested update — respecting everyone's personal layer.

**Reference implementation:** this is the day-to-day of a RevOps leader on
the platform: creative orchestrator of the fleet, not maintainer of a prompt
folder.

## 7. Loops, not schedules

**Ask:** *Do you have loops running — does every skill run deepen the
system's brain? Does your system keep getting better if you leave for two
months?* A skill that runs nightly is a repeated action; a loop requires the
run to change state that changes the next run.

**Why it matters:** go-to-market reality is non-stationary — messaging that
worked in Q1 dies in Q3, and a static system coaches this quarter's reps
against last quarter's playbook. For most DIY rigs, the improvement rate is
exactly the free hours of one talented person.

**What good looks like:** durable state agents read and write (living pages
in the same graph), chained skills, and evaluation make loops first-class.
The flagship: one skill mines every call for what's actually working and
updates a living rubric, citing calls; a second coaches each rep against it,
targeted to their gaps and timed to their seniority; a third watches whether
behavior changes and adjusts. When one rep's approach outperforms, the rubric
absorbs it and every coach downstream inherits it. The human's role rises to
taste and judgment: setting standards, approving what the data surfaced.

**Reference implementation:** pages as durable state, skill composition
(skills chain and include one another; attached pages are read at runtime, so
updating the rubric updates every skill that reads it, immediately), and
built-in evaluation.

## 8. Governance is a dial, not a switch

**Ask:** *Can you choose which skills users can modify and which have to run
one way?* One prompt file has no notion of managed versus personal, so a DIY
build either locks everything and loses adoption, or opens everything and
loses control — usually all-locked by accident, presented as policy.

**Why it matters:** CRM writeback, forecast methodology, and anything
compliance-adjacent must be invariant. Coaching and briefings must be
personal. A control plane that cannot express that distinction is not a
control plane. This is the first question a security-minded stakeholder asks.

**What good looks like:** every skill carries a deployment mode — **managed**
(workspace-owned, versioned, admin-only), **template** (deployed as a
starting point, forked per user), **personal** (the user's entirely) — set
per skill, changeable over time, with workspace-level instructions above it
all that no individual can override.

**Reference implementation:** deployment modes live in the skills data model
itself, with versioned deployment, snapshot, diff, and rollback.

**What we have seen about who holds the dial:** the builds that survive
contact with a busy sales team put the managed layer with ops and enablement
and let reps tune on top. A workspace that sticks runs dozens of
workspace-level programs every agent inherits; a same-shaped workspace that
goes flat runs none — every skill it has is stock or one person's
(`adoption.md`, practices 2–3).

## 9. Definitions are authored by the model, from the company's own history

**Ask:** *Are you hand-writing skill prompts, or does the model write them
from the entire history of your company plus your strategic direction?*
Hand-authorship is why DIY systems plateau at a dozen skills: every new one
costs the one person who can write them, and the coaching prompt is identical
for the best rep and the worst.

**Why it matters:** the best agent definitions are written by a model with
full context on the company, its customers, its strategy, and the specific
human the skill serves. A builder writing prompts from their own head is
competing against a system that reads years of the company's actual
conversations first.

**What good looks like:** the admin drives the fleet from an agentic harness
(e.g. Claude Code) over MCP as config-as-code against a live control plane:
describe the initiative, have the model draft every agent identity and skill
prompt grounded in the actual workspace, preview, deploy, diff, roll back.
The existing folder — the markdown, skills, and taste already built — becomes
the authoring environment, version-controlled in git, with the control plane
as the deployment target.

**Reference implementation:** the full management surface is exposed over
MCP; admin tools work across agents (read and edit any teammate's agent
identity, create and update skills, read run history, manage members). Day
AI's reference implementation (https://github.com/day-ai/gtm-brain) shows the
operating model; Phase 5 (`implement-upgrade/`) uses it as an internal
example, never as something the user installs.

**What we have seen about authorship:** stock, un-rewritten briefings reach
nearly every workspace, and receiving them does not by itself move
engagement. Five or more briefings a human actually wrote for the team is
associated with about four times the adoption. The generic default is not
neutral — it trains people to ignore the channel (`adoption.md`, practice 6).

## 10. Data entry goes to zero

**Ask:** *Do your salespeople still open the CRM and punch data in with a
keyboard, like the pre-agentic era?*

**Why it matters:** this exposes the ceiling of the orchestration-only
approach. A DIY layer on top of the old system of record leaves the old human
contract intact — humans type structured data in so dashboards can come out —
and the bot's intelligence is capped by what tired humans typed into
dropdowns.

**What good looks like:** agents maintain the record from what actually
happened — calls, emails, commitments — at a granularity keyboard entry never
reached; where a legacy CRM must remain the system of record, agents write to
it under each user's own permissions (customer-owned MCP OAuth app, per-user
auth, no god-mode service account).

**Reference implementation:** this practice also marks the limit of the whole
control-plane comparison: no amount of orchestration-layer cleverness fixes
the substrate underneath it. The control plane and the context graph
(`context-graph.md`) are two halves of one product — a DIY path has to build
both.

---

## Skills and triggers as first-class objects

One structural note underneath several practices above: in a real control
plane, a skill is an object that carries its own execution model — trigger
(schedule with timezone awareness, event, or on-demand), delivery (DM,
channel, email — a skill never fires into the void), prompt, owning agent,
deployment mode, and full run history. Event triggers matter more than
schedules ("if anyone mentions competitor X on a call, I need to know" is not
a schedule), and they exist only because the substrate knows the moment
something happens. The DIY equivalent of each field is a piece of
infrastructure someone has to build, secure, and keep alive.
