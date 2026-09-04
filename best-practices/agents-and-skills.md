# Agent and skill design best practices

How to shape the fleet and write the skills. `agentic-control-plane.md`
describes what the orchestration layer must be able to do — identity,
governance modes, loops, introspection. This document is the craft that runs
on it: how many agents, with what jobs, delivering what, in what words, with
what autonomy. It is drawn from the skill-authoring systems, role playbooks,
and fleet reviews that have been tested against real teams, and from what
those teams told us when the output missed.

Ten practices. Each pairs a diagnostic question with why it matters, what good
looks like, and what we have seen. Used by `day-ai/brain-init` in Phase 1 to
grade the skills a folder already contains, and in Phase 3 as the standard
the agent and skill portion of `COMPANY-BRAIN-UPGRADE.md` is written to.
The detailed build work belongs to `gtm-brain`
(https://github.com/day-ai/gtm-brain); this is what it builds toward.

---

## 1. An agent is a job description: one coherent standing job, one human owner

**Ask:** *Read your agent's description aloud. Is it a job a person could
hold, reporting to one manager — or is it a list of tools?*

**Why it matters:** a chatbot responds when asked. An agent has a standing
role and produces work without being asked, in the background, brought to a
person for review. That only works when the role is coherent enough to
reason from. A do-everything agent performs worse than two focused ones and
is illegible to the people it serves — nobody knows what to expect from it.

**What good looks like:** agent = who (name, title, standing role, the
person it works for), skill = what (a repeatable task), trigger = when. Three
instruction layers that never contradict each other: workspace-level
definitions everyone inherits, agent-level identity and standing role,
skill-level task prompt. The description is written the way you would brief a
new hire — specific company, specific person, specific lane — because that
specificity changes how the agent reasons about everything downstream.

**What we have seen:** the who / what / when distinction resolved most of the
confusion in early implementation calls by itself. Agents inherit exactly the
permission boundary of the person they belong to; whatever that person can
see and do is what their agent can see and do.

## 2. Start with one workhorse and one background skill; split when the job needs "and" twice

**Ask:** *How many of your agents received a real conversation or ran a
tailored skill last week? Which ones are shells or duplicates?*

**Why it matters:** provisioning several agents is common; running a genuine
division of labor is rare, and every fleet that did it well was deliberately
designed. The realistic target for most people is one trusted chat agent
plus at least one tailored scheduled skill firing. Duplicate agents are worse
than idle — two agents holding the same event skill fire twice on the same
email.

**What good looks like:** two to four agents per team, mapped to real roles,
not eight. Split when the job description needs "and" more than twice; when
two jobs have different cadence (daily triage versus weekly sweep), different
voice or audience (coaching a rep versus nagging about hygiene), or different
data domains; or when one person's agent produces work many people consume.
A proven role library: a seller gets a deal agent and a CRM-health agent; a
player-coach gets a "my deals" and a "team" agent; a founder gets a chief of
staff, a ghostwriter, and a research agent, each of whose prompts names the
other two and states what it does not do. Fleet grammar: each agent owns a
lane and routes cross-lane work rather than grabbing it; specialists publish
to shared pages and one rollup agent synthesizes for the human — never every
agent DMing the owner; nothing-to-report is one line.

**What we have seen:** the tell that a second agent is overdue is a builder
merging unrelated skills onto one agent because they ran out of room. Treat a
merged mega-skill as a signal, not a pattern to copy. Measure standing roles
in use, not agent count.

## 3. A skill's value is what the agent can see that the person can't

**Ask:** *Does this skill tell the person anything they would not already
know by lunchtime?*

**Why it matters:** the agent has the person's email, meetings, calendar, and
the whole team's memory at once; the person experiences those one at a time.
The value is in the cross-references, the patterns over time, the things that
slipped, the commitments coming due. A skill that summarizes what the person
already knows is low value, and a daily one teaches them to ignore the
channel.

**What good looks like:** four questions answered before a word of prompt is
written. What is their job, really — not the title, the daily reality (a
sales VP at a twelve-person company is selling; at five hundred, coaching and
forecasting)? What would be embarrassing for them to miss — the single best
design question there is? What data does their agent actually have, and what
is missing — gaps are design inputs and often the most valuable thing to
surface? What does their day look like — a phone-and-Slack founder needs
five tight bullets; a desk-bound pipeline reviewer needs a table? Skills
also match workspace maturity: a sparse new workspace gets workspace-health
coaching framed as "here is what I wish I could have told you," never a
setup checklist; a pattern-over-time skill is not deployed on three weeks of
data.

**What we have seen:** the skills people quote back — "that is killer" — are
the ones that caught something: a competitor evaluation surfaced from a
transcript, a timeline the rep had been optimistic about, an executive
attending tomorrow's call with nothing prepared.

## 4. Structure around what the person needs, never around data sources

**Ask:** *Are the section headings in your prompt named after systems or
after situations?*

**Why it matters:** a prompt organized as "check email, check calendar, check
CRM" produces a data dump. A prompt organized as "promises coming due,
relationships going quiet, unfinished business, connections they might not
see" produces judgment. The structure of the prompt is the structure of the
output.

**What good looks like:** open with identity and context. Name the patterns
to look for explicitly, with one to three inline examples each — the agent
reasons far better about ten named patterns than about "anything worth
flagging." Put skip filters at the top (skip internal-only calls, skip closed
deals, skip if a follow-up already went today), because bad output on a wrong
input costs more than an extra check. Specify anti-patterns: what not to do,
what not to say, what not to restate.

**What we have seen:** "surface insights" is the worst phrase in skill
design. Every prompt that leaned on it produced filler; every prompt that
replaced it with named patterns and examples produced something a person
acted on.

## 5. Set the bar with a number, lock the format, and give permission to say nothing

**Ask:** *What does your skill send when there is genuinely nothing to
report?*

**Why it matters:** without an explicit quality bar the agent pads. Without an
explicit empty case it manufactures. A scheduled skill that fires and says
"no recent meetings found" is worse than no skill at all — it trains the
person to stop reading. And output that arrives as a two-thousand-word essay
every morning is ignored within a week regardless of its quality.

**What good looks like:** a numeric bar — "two to five items, not fifteen;
each should make the reader think 'I'm glad someone caught that'; if nothing
clears that bar, say so in one line." The empty case stated before the full
case. A literal output template with branching conditionals, which
outperforms "format this nicely." Hard caps with the overflow named ("fifteen
deals reviewed; the count not reviewed is stated"). Delivery format as the
closing section: channel, shape, length. Every scheduled output states the
query logic it ran so a reader can check the result.

**What we have seen:** the best-performing morning briefs carry hard word
caps and a fixed shape — one lead line, a few evidence-specific bullets, a
handful of suggested follow-up prompts — with banned phrasing listed
explicitly (no greetings, no praise inflation, no restating what the reader
already knows). "Silence is praise: don't list healthy deals."

## 6. Acts, not flags — with a human in the loop by default and never an unsupervised send

**Ask:** *Does this skill hand the person a finished draft to approve, or a
list of things to go do?*

**Why it matters:** the bar is not pulling information together so a person
can act on it; the bar is acting on it — drafting the re-engagement, not
flagging that the deal went quiet. But a bad autonomous send costs more
credibility than the labor saved. The two ideas reconcile through the review
step: the agent does the work, the person approves it, and every approval or
correction sharpens the agent.

**What good looks like:** reader skills separated from actor skills — they
carry different risk profiles and fail differently. An autonomy convention
enforced by how the skill is written and how often it is reviewed, not by a
switch: report only; prepare the exact change for a human to apply; present
for approval, where approval executes; act autonomously with an audit trail,
once earned on a clean record. Anything not explicitly listed is report-only.
One bad write demotes the action. Customer-facing sends always start at the
bottom two levels: first touch drafted per contact, later touches
pre-drafted for batch review, a human triggers every send.

**Reference implementation:** deployment modes and per-skill governance live
in the control plane (`agentic-control-plane.md`, practice 8); CRM writes
follow the phased discipline in `implementation.md`, practice 9.

## 7. Bookend the day; cron for consistency, events for immediacy

**Ask:** *What arrives at the start of the person's day, what arrives at the
end of it, and what fires the moment something happens in between?*

**Why it matters:** scheduled delivery is the retention engine — it arrives
without the person remembering to ask. Chat wins satisfaction; skills win
retention; neither substitutes for the other. But not everything belongs on a
schedule. "If anyone mentions this competitor on a call, I need to know" is
not a cron job; it is an event, and it only exists if the substrate knows the
moment something happened.

**What good looks like:** the standard starter architecture — a morning
briefing (today's calls, prep, follow-ups due) and an end-of-day or weekly
review (call review, drafted follow-ups, coaching notes) — with the second
one built in week one, not month three. At least two scheduled or triggered
skills per seat producing specific, data-grounded output. Event triggers on
recording ready, email landed, stage moved. Skills that chain: one skill
reads another's instructions or a shared page, so updating the source once
updates everything downstream.

**What we have seen:** "the agents post and I react" is how the healthiest
users describe their day. A workspace with scheduled deliveries and zero
conversation is fragile — configuration without conversation does not
compound — so async output should always open a path back into chat.

## 8. Deliver where people live, sized for the device and shaped for the reader

**Ask:** *To read this, does the person have to open anything they were not
already in?*

**Why it matters:** if people must log into a web app every day, adoption
dies. Buried next-actions in a record do not get actioned; proactive
surfacing beats query-on-demand. And the same information is a different
skill for a different reader: a manager's briefing that also goes to the reps
stops being a manager's tool and becomes a noise channel.

**What good looks like:** Slack DM or channel as the primary delivery
surface; email where there is no Slack. The ping written to earn the click —
a colleague's message, short — with depth living in the follow-up thread
where headers and tables are allowed. The IC-shaped briefing and the
manager-shaped briefing as separate skills, not one skill with different
access; the manager's agent reads the whole team's pipeline but writes to
the manager. Coaching and operational intelligence kept in separate outputs
— when they are mixed, both degrade. A founder gets their own operating
cycle (morning, midday pulse, evening sweep) and separate per-rep coaching
agents delivering to each rep's own channel.

**What we have seen:** the delivery requirement usually arrives as a scar
from a previous tool: the team had already dropped something that made reps
log in. Verify the first run is specific before leaving — generic output is a
prompt problem, and it is caught on run one or not at all.

## 9. Names are tasks, boundaries are explicit, and some things are never done

**Ask:** *Does the skill's name answer "what am I doing when I read this"?
Does its prompt name the sibling skills it must not overlap?*

**Why it matters:** "Daily Schedule," "Morning Briefing," and "Daily Summary"
are the names of stock defaults, and stock defaults are what people learn to
ignore (`adoption.md`, practice 6). The most common bug in a workspace with
several skills is duplicate or contradictory output because two skills
quietly cover the same ground. And a handful of behaviors cost trust every
time they appear.

**What good looks like:** every skill named as a task the person would put
on their own list — Pipeline Review, Deal Follow-Up Drafter, Account Risk
Scanner, Investor Update Prep; two executives with different focus get
different names. An explicit coordination section drawing the boundary with
each sibling skill by name, mandatory when adding to a workspace that already
has skills. One skill, one role, one audience. And the hard rules: never
reference unsent drafts (drafting is normal work, not something to police);
never infer who manages whom from titles — say "works alongside"; never
rewrite a prompt the person authored themselves, regardless of its quality;
never open a scheduled skill with an introduction or a pitch — cron output
starts at section one; never use real customer data in a sample output.

**What we have seen:** each of these rules exists because a real deployment
broke it and a real person noticed. The reporting-relationship rule and the
unsent-drafts rule in particular came directly from user complaints.

## 10. Feedback is memory; the definition is versioned and owned

**Ask:** *When a person tells the skill "shorter" or "skip that section," where
does that instruction go, and who can see the prompt change over time?*

**Why it matters:** people expect anything called an agent to listen. A
correction that evaporates teaches them not to bother. But per-person tuning
without a versioned, owned definition underneath it becomes folklore — nobody
knows what the skill does anymore, and when the methodology changes there is
nothing to update. Both halves are needed: the person coaches conversationally
and never sees a prompt; ops owns the prompt and ships it like software.

**What good looks like:** every skill closes with a rotating one-line
invitation to react, and carries a handling section: format feedback is
acknowledged in a sentence, written to a preference store scoped to that
person, appended rather than overwritten so preferences accumulate, and read
back at the start of every subsequent run, overriding the prompt's defaults
where they conflict. Underneath, the definition lives in version control with
the ops owner, pushed centrally, with changelogs; improvements are dated
releases fed by a wishlist, and "skill tuning" is a standing agenda item in
the weekly check-in. Three skill types kept distinct: personal (one person,
tailored); shared (one workspace's audience, grounded in the workspace but
un-personalized — "the Discovery stage" is fine, "Sarah's deals in Discovery"
is not); template (a persona across workspaces, fully generic, tier-safe
schedules only). A shared skill works on the first click or it does not
belong in the library. The first skill that works becomes a template pushed
to the rest of the cohort during activation — versioning, sharing, and bulk
push are a kickoff topic, not a month-three retrofit.

**Reference implementation:** replies continue the thread with full context;
instructions durably change behavior from the next run; the feedback layer is
itself a readable dataset; the drift gets harvested (`agentic-control-plane.md`,
practices 4–6).

---

## Grading the skills a folder already has

For Phase 1 of `brain-init`, three grades, scored on prompt quality *and* on
actual run output where any exists:

| Grade | Looks like |
| --- | --- |
| Template-grade | Short, no business context, generic name, organized by data source, never updated since it was created |
| Borderline | Some personalization; mostly template; no quality bar or empty case |
| Good | Names the person and role; sections organized around situations; named patterns with examples; a numeric bar and an empty case; explicit anti-patterns; a delivery format |

Say which grade each skill earns, with the file path. A folder full of
template-grade skills is not a criticism of the builder — it is the natural
plateau of hand-authoring for one person, and the reason practice 9 of the
control plane exists.
