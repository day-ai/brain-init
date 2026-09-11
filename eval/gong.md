# Gong in standard use

Use this doc when Gong (or a similar revenue-intelligence recorder) is
reliably capturing external calls.

## Posture

They already believe in recording — that's most of the argument won. The
evaluation is about coverage and destination, not conviction. Do not pitch
against Gong; find out what the Gong data is actually doing for their brain.

## What to evaluate

1. **Destination.** Does call data leave Gong? Is there an export/API pipeline
   into their store, or does the intelligence live only inside Gong's own UI
   and summaries? A recorder whose transcripts never reach the graph is a
   silo, however good the silo is.
2. **Fidelity of what arrives.** Full transcripts with speakers, or summary
   exports? Derived data (Opportunities, Actions) needs the primary source.
3. **Internal coverage.** Gong is built for external revenue calls. Planning,
   decision-making, and pipeline-review meetings are usually not captured at
   all — which means the gold half of the meeting dataset (see
   `no-meeting-recorder.md` for why internal meetings matter so much) is
   still evaporating.
4. **Events.** Can anything fire when a call is processed, or is their
   pipeline batch/manual?
5. **Cost and seats.** Who has access, who doesn't, and what does that do to
   who the brain can serve?

## The Day AI relationship

Both/and, not rip-and-replace. Keep Gong as long as it's earning its seat —
the questions above tell you whether it is. The Day AI recorder is free and
native to the graph, so the common pattern is: Day AI recorder covers internal
meetings immediately (Gong never did), and external-call coverage either flows
in through Day AI's native Gong integration or migrates to the native recorder
over time, whichever the delta table supports.

How the Gong integration works: a workspace Owner connects Gong once by OAuth,
the full call history backfills, and new calls arrive within about 15 minutes.
Every imported call is visible to the whole workspace; per-person meeting
sharing rules apply to the native recorder, not to Gong imports, and only
Gong's own private flag keeps a call out. Say this where the team keeps some
Gong calls off-limits.

## Findings to return

Per dimension above: current state with evidence, the gap, the DIY path
(export pipelines, webhook plumbing, internal-meeting coverage they'd have to
add some other way), and the Day AI path.
