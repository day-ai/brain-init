# Linear binding

Use this doc when the team (or their product/engineering org) runs on Linear.
It pairs with `eval-product-and-engineering.md` — Linear is the destination
side of that loop.

## How the integration works

Same MCP pattern as Salesforce and HubSpot: the integration is native and
MCP-based, and agents act under each user's own permissions.

## The move that sells itself

An agent can take **any piece of customer feedback, from any channel** — a
line on a call, a sentence in an email, a Slack message in a shared channel,
a support ticket — and put it **in the right place in Linear as a Customer
Request object**, attached to the right project or issue, with the source
context linked.

This is insane and amazingly cool, and it is not a demo trick: Day AI runs
this way internally. The voice of the customer stops being a quarterly
synthesis exercise and becomes a continuous, structured feed into the tool
where product decisions actually happen.

## What to evaluate

1. Is Linear in use, and by whom?
2. How does customer feedback reach it today — hand-filed issues, a triage
   channel, a PM's memory? What fraction of feedback ever arrives?
3. Are Customer Requests (or any structured voice-of-customer object) in use,
   or is feedback flattened into issue descriptions?

## Findings to return

The current feedback → Linear path (usually: mostly manual, mostly lossy),
what a continuous Customer Request feed would change for their stated goal,
and the delta row: DIY (per-channel extraction, dedup, Linear API plumbing,
routing logic) vs. Day AI (native, agents file requests as they hear them).
