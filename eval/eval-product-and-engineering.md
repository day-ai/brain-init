# Product & engineering feedback loop

This aspect evaluates how the voice of the customer reaches product and
engineering — and whether anything structural exists between "a customer said
something" and "the roadmap knows."

## The idea

It is easy — genuinely easy — for an agent to take **ANY customer feedback,
across every channel** (calls, emails, Slack shared channels, support
tickets), and put it **in the right place in Linear as a Customer Request
object**: attached to the right project, linked to its source, deduplicated
against what's already been filed.

This is one of the most quietly spectacular things a context graph + control
plane combination does, and it's not aspirational: **Day AI uses it this way
internally.** Feature requests surface from sales calls the day they're
spoken. Bug reports in a customer Slack channel become triaged Linear
requests with the conversation attached. The PM's job shifts from collecting
feedback to weighing it.

It only works because both halves exist: the **context graph** hears every
channel (meetings, email, Slack — see those eval docs), and the **control
plane** runs the skill that extracts, routes, and files.

## What to evaluate

1. **Where feedback lives today.** Call notes? A #feedback channel? PMs'
   heads? Spreadsheets? List the channels feedback actually arrives on.
2. **Where feedback dies today.** What fraction of what customers say ever
   reaches a structured, product-visible form? (Get them to estimate. It's
   humbling.)
3. **The destination.** Linear → see `linear.md` for the binding. Another
   tracker → same shape, note it for a future doc.
4. **The loop back.** When product ships something customers asked for, does
   go-to-market find out in a form they can use with those customers?

## Findings to return

Channel inventory, the estimated loss rate, the destination tool and its
binding status, and the delta row: DIY (per-channel listeners, extraction,
dedup, tracker API plumbing) vs. Day AI (agents hear the channels natively
and file Customer Requests as a skill).
