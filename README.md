# Engineering write-ups

*Benjamin Tweedy*

Two integration projects I designed and built in 2026, written up as short design documents. Clients are not named.

1. **[A monthly client-reporting pipeline for a marketing agency](reporting-pipeline.md).** Pulls analytics, search, and rank-tracking data into one record per client, computes every figure in code, and has a language model draft the owner's script, which is checked against the report before human review.
2. **[Migrating a live self-storage checkout between two vendor systems, one location at a time](vendor-migration.md).** Two vendor APIs behind one interface with a switch per location, card payments that are never sent twice against an API with no idempotency key, and lease signing confirmed by poll.
