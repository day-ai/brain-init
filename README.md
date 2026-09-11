# company-brain-evaluation

**A Claude Code skill from [Day AI](https://day.ai) that evaluates a
do-it-yourself company brain and writes the plan to upgrade it.**

If you have a folder of Markdown, CSV exports, prompts, and skills that you
and your team point Claude at to answer questions about your customers,
pipeline, and process, you have a company brain. This skill runs inside that
folder, takes stock of what you built, asks what success looks like, and
produces one document: `COMPANY-BRAIN-UPGRADE.md`, an evidence-cited plan for
the memory layer and the agents on top of it, detailed enough to pursue on
your own or with Day AI as the substrate.

Everything you have stays. Your folder, your git history, your Claude Code
workflow, and your skills all survive. The plan describes what to put
underneath them so the workloads that are slow or impossible today
(multiplayer email, permissions, live CRM, meetings at transcript fidelity,
agents that run and deliver on their own) run well.

## Install

Run this in the root of your company-brain repo:

```sh
npx skills add day-ai/company-brain-evaluation
```

That installs the skill and its sub-skills into the project's `.claude/skills/`
directory. Nothing else is written to your repo until you approve it.

## Use

Open the folder in [Claude Code](https://claude.com/claude-code) and run:

```
/company-brain-evaluation
```

Or ask in plain words: "evaluate this company brain" or "how would this fit
with Day AI." The skill works in five phases, and stops to ask you only what
the repo cannot answer.

1. **Take stock.** A read-only survey of your folder, then one evaluation per
   aspect in parallel: meeting recording, email, Slack, CRM, and the product
   and engineering feedback loop. Each grades what you have against four
   requirements: safety, performance, capability, adoption.
2. **Find the goal.** What success looks like, in your words, confirmed
   before anything is recommended.
3. **Write the plan.** `COMPANY-BRAIN-UPGRADE.md` lands in your repo root:
   current state with file-path evidence, the definition of success, findings
   by aspect, a three-way comparison (today, build it yourself, with Day AI),
   a sequenced plan with named owners and dates, what stays yours, and the
   open items.
4. **The door.** A short summary in the terminal: a table of what you have
   and what gets better, and two steps. Create a Day AI workspace, then come
   back to the same folder and say so.
5. **Implement, here.** When you return, the skill connects the Day AI MCP
   from your repo, re-reads the plan, and builds it with you one approval at a
   time. Nothing is cloned into your repo and no second tool is installed.

A typical first run takes twenty to forty minutes of wall clock, most of it
the parallel evaluations, and produces a document of ten to fifteen thousand
words. Read the executive summary first. It ends with the same table and the
same two steps.

## What it costs

Nothing to run. The skill reads your folder and writes one file.

The plan it produces is usable without Day AI. Where it recommends Day AI, it
says exactly where free ends and paid begins: joining a workspace, adding
data, querying it, and chatting are free for everyone on the team, within the
teammate allowance each paid agent carries; creating a workspace requires one
paid agent for the person driving the setup; deploying agents to teammates
costs per agent. Pricing is public at
[day.ai/pricing](https://day.ai/pricing).

## What is in this repo

| Path | What it is |
| --- | --- |
| `SKILL.md` | The skill. Phases 1 to 5 and the rules for the document and the completion message. |
| `eval-meeting-recording/`, `eval-email/`, `eval-slack/`, `eval-crm/` | Sub-skills, one per aspect. Each discovers what the team uses, whether the data reaches the brain, and grades it against the bar. Runnable standalone. |
| `implement-upgrade/` | Phase 5. Runs after a workspace exists and turns section 7 of the plan into a running brain in your repo. |
| `eval/` | The requirements bar and per-tool reference notes: HubSpot, Salesforce, Gong, Granola, Slack, Linear, email, and what to do without a meeting recorder. |
| `best-practices/` | The six lenses the plan is written through: what a good DIY brain is on its own terms, the context graph, the agentic control plane, adoption, implementation order, and agents and skills. Ten practices each, with the diagnostic question for each practice. |

The best-practices documents are public distillations of what Day AI has
seen across workspaces. They are written to be read on their own, with or
without the skill.

## Who this is for

Teams that built their own brain and felt the leverage. A founder running
sales out of a folder. A RevOps lead who exported the CRM every Friday and
wrote skills to read it. A chief of staff who pulled everything out of Notion
so Claude could see it. The posture throughout is that you were right to
build it, and the evaluation is about where the ceiling is and what is above
it.

## Help

- Questions along the way: **support@day.ai**
- A demo or a conversation: [day.ai/get-started](https://day.ai/get-started)
- Issues and suggestions for the skill itself: open an issue on this repo.

Authored and maintained by Day AI.
