# HubSpot binding

Use this doc when HubSpot is the system of record. Everything in
`salesforce.md` applies — same posture (they keep HubSpot; the play is the
bridge), same evaluation questions, same payoff. The integration pattern is
identical, with the portal in place of the org:

1. Register an **MCP OAuth app** in their HubSpot portal — the customer owns
   the app.
2. Put the client ID and secret into Day AI **at the workspace level**, as a
   workspace Owner (the built-in Admin role cannot).
3. Each individual user **auths themselves**, enabling agentic use via MCP —
   agents act in HubSpot **under that user's own permissions**. No god-mode
   service account. Scheduled and event-triggered skill runs use the agent
   owner's own connection.

## The payoff to lead with

Same as Salesforce: **data-entry agents**. Agents draft the HubSpot updates
from meetings, emails, and commitments and the rep approves each one in chat;
a field the team wants fully automated can be, by authorizing the skill for
the connector. The recurring "update CRM" calendar blocks go to zero.
Obviously worth the price of entry on its own, and the first win to sequence
in the plan.

What Day AI does not do: sync HubSpot into the graph or hear HubSpot events.
Reads and writes go through the connector live under each user's login,
history comes in once through CSV import, and a HubSpot stage change is not a
skill trigger.

## Findings to return

As in `salesforce.md`: hours/week of human data entry, hygiene evidence,
current automation and its credential model, and the DIY vs. Day AI delta row.
