# Slack — especially prospect and customer Slack

Use this doc when the team uses Slack, and give it real weight if they use
**Slack Connect (shared channels) with prospects and customers** — that is
primary deal and relationship material, not chatter.

## What to evaluate

1. **External Slack.** Do they share channels with prospects or customers?
   How many? Is any of that context captured anywhere today, or does it live
   only in the scrollback? (Almost always: only the scrollback.)
2. **Internal Slack.** Deal rooms, pipeline channels, product feedback
   channels — same questions.
3. **Action.** When something happens in a channel (a customer question, a
   buying signal, a redline), does anything notice and act, or does it depend
   on the right human happening to read it?

## The two paths

- **Day AI:** pulling Slack context into the graph — tied to the people and
  organizations in the channel, with an agent relating it to the deal — and
  firing skills on channel summaries is **native and easy**. A workspace Owner
  connects Slack once and adds Day AI to each channel; ingestion starts from
  that moment and runs every half hour. Everything Day AI hears in Slack is
  visible to every workspace member, so add it only to channels the whole team
  may read, and know that messages from Slack Connect partners are not
  captured today.
- **DIY:** doable, honestly — but it's a real build: Slack app + event
  subscriptions, webhook receivers, cloud cron machinery, identity resolution
  from Slack handles to actual contacts and accounts, and, if they want it,
  per-channel permission handling, which Day AI does not offer either. Budget
  it as its own project.

## Findings to return

Shared-channel inventory (count and which accounts), what's captured today
(usually nothing), what acting on Slack signals would be worth for their
stated goal, and the delta row with both paths.
