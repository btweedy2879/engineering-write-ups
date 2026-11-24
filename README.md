# Engineering write-ups

*Benjamin Tweedy*

Two integration projects I designed and built in 2026, written up as short design documents: the constraints, the design, what failed or could have, the trade-offs, and the outcome. Clients and vendors are not named.

1. **[A monthly client-reporting pipeline for a marketing agency](reporting-pipeline.md).** Analytics, search, and rank-tracking APIs into one record per client; figures and verdicts computed in code; the spoken narration drafted by a language model and checked against the report; review points before anything is sent; a dashboard the agency runs.
2. **[Migrating a live self-storage checkout between two facility-management systems](vendor-migration.md).** Two vendor APIs behind one interface with a switch per location; card payments that are never sent twice against an API with no idempotency key; lease signing confirmed by poll.
