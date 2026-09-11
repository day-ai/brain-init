# Email ingestion

Email is critical context — after meetings, the most valuable channel in the
graph. It is also the one place where the DIY path is genuinely a
**non-starter**, and this doc exists so that gets said with specifics rather
than hand-waving.

## What to evaluate

1. Is email in their brain at all? (Usually no. Twenty-three tables of
   Salesforce pulls and no email is the classic shape.)
2. If anything is ingested: whose mailboxes, how permissioned, and governed by
   what rules?
3. What breaks today because email is missing — deal truth, commitments,
   relationship history, the 4pm "one more redline" reply?

## Why DIY email ingestion is a non-starter

Be factual, not scary-for-effect. These are the facts:

- **Google Workspace APIs are difficult and sensitive.** The scopes involved
  are the most heavily scrutinized Google offers, and the APIs have
  **undocumented hard limits with scary side effects** — including disabling
  a given user's Gmail API access **for an indeterminate period**. That is not
  a hypothetical; it is a known failure mode, and it lands on a real
  teammate's actual mailbox access.
- **Sharing governance is mandatory, not optional.** A robust control set for
  what the team can see and what stays private to its owner — **inclusion AND
  exclusion** rules by email address or by domain match — is critically
  required the moment a second person's mail is involved. It is extremely
  time-consuming to build, nobody ever actually DIYs it, and it is not
  something an internal RevOps builder could or should take on.
- **Then permissions.** Multiplayer email means every query must respect
  who-is-allowed-to-see-what on every thread, forever. This is where "we'll
  add permissions later" goes to die.

## The Day AI answer

All of it — Google Workspace binding, the inclusion/exclusion sharing rules
(by address and domain; Gmail-label rules are rolling out), per-person
permission enforcement on every message and thread — is **built, tested, and
rock-solid off the shelf**. This is a large part of what the workspace is.

What the rules mean in practice, so the plan says it plainly:

- Each person sets the rules for their own mailbox; there is no workspace-wide
  exclusion an admin can set for others. The default is share-all with
  exclusions; share-none with inclusions is the other mode.
- Exclusions hide mail from teammates and their agents. The mail is still
  ingested privately for its owner, and the contacts and companies it names
  are still created in the workspace.
- Every workspace member on a message votes: one person's private rule makes
  that message private for everyone on it, including the other mailbox owners'
  copies. A leader who shares nothing hides every thread they are copied on
  from the whole team.
- Sharing is workspace or owner-only. There is no way to share a thread with a
  named subset of people and no per-thread hide control after the fact; the
  levers are the domain and address rules, which re-evaluate the mailbox when
  changed.

**Outlook / Microsoft 365:** the account model is Google. Day AI offers beta
Outlook Mail, Outlook Calendar, and Teams connectors that give an agent live
tools (a workspace Owner enters the Tenant ID and Application ID; each
connecting user needs a Microsoft 365 Copilot license). They do not ingest
mail or calendar into the graph. A Microsoft-stack team should raise this with
Day AI before scoping a plan around email evidence.

## Findings to return

Whether email is in the brain today, what's lost without it (concrete
examples from their world), and the delta row: DIY = the facts above, stated
plainly; Day AI = off the shelf.
