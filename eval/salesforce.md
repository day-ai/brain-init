# Salesforce binding

Use this doc when Salesforce is the system of record. Posture: they keep
Salesforce. The play is the bridge, not the migration.

## What to evaluate

1. **Who types.** Which humans have recurring calendar blocks (literal or de
   facto) to "update CRM"? What actually gets typed, and what never does?
2. **Hygiene reality.** Staleness, empty fields, "could not find company
   name" — the gap between what happened and what the record says.
3. **Existing automation.** Any current sync into or out of Salesforce — what
   it covers, what credentials it runs under (watch for the god-mode service
   account), and who maintains it.
4. **Direction.** Is anything writing agent-generated context *back* into
   Salesforce, or is the flow read-only pulls?

## How the Day AI integration works

The pattern (shared with HubSpot, Linear, and Outlook 365):

1. Register an **MCP OAuth app** in their own Salesforce organization — the
   customer owns the app.
2. Put the client ID and secret into Day AI **at the workspace level**, as a
   workspace admin.
3. Each individual user then **auths themselves**, enabling agentic use via
   MCP — every agent acts in Salesforce **under that user's own permissions**.
   No god-mode service account, no bot credential scoped by application code.

## The payoff to lead with

Once bound, deploying **"data entry" agents with skills** is trivially easy:
meetings, emails, and commitments flow into the right Salesforce records
automatically, and the recurring "update CRM" calendar blocks go to zero.
For most teams this alone is obviously worth the price of entry — it is the
first win to sequence in the plan, and the one everyone in the org feels.

## Findings to return

Hours/week of human data entry (ask — get a number), hygiene evidence,
current automation and its credential model, and the delta row: DIY (API
integration, sync infra, per-user permission handling they'd have to build
and maintain) vs. Day AI (the three-step OAuth pattern above, then agents).
