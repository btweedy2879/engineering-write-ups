# Moving a live checkout from one vendor system to another, one location at a time

*Benjamin Tweedy, November 2026*

## Context

A client with 25 locations takes reservations, card payments, and signed leases through an online checkout I built and run for them. The checkout was tied to one back-office vendor's SOAP API. In 2026 the client decided to move to a second vendor, whose API is REST with signed requests, and to move one location at a time over several months. Online rentals had to keep working at every location the whole way through.

## My role

I was the only engineer. I led discovery with the client's operations lead and the new vendor's team, read and probed the new API, wrote the phased plan, built and tested the integration, wrote the documentation, and supported the first cutovers. I work AI-native: I wrote the specification for each phase, had AI tools build and test against it, and reviewed each phase myself before the next one started.

## Goals

- A location is moved by changing one setting, and can be moved back.
- A customer is never charged twice and never charged a price different from the one shown.
- The existing integration keeps running unchanged for locations that have not moved.

Not a goal: replacing the checkout itself, or moving every location on one date.

## Design

- One interface for the rental steps (price, reserve, pay and move in, sign) with an implementation per vendor. Each location's record names its system, and every request resolves the implementation from that record.
- The switch is a screen in the admin portal for corporate users. It is refused while the location has online rentals turned on or rentals in progress, it refreshes the location's units from the new system at once, and rentals cannot be turned back on until a sync has succeeded. Every change writes an activity log entry.
- Before a card is charged, the price is requested again and the payment is refused if the total has moved.
- A payment request is marked in flight before it is sent. If the vendor never answers, the code does not retry. It reads the unit from the vendor: if the customer is on it, the rental is recorded from the vendor's ledger; if not, the session is held for staff to check and release.
- The new vendor has no webhook for a signed lease, so a scheduled job checks each paid rental every ten minutes and completes it once, however many times the signature is reported.

## Trade-offs

- An adapter per vendor instead of a rewrite. The old code path stayed as it was, which kept risk off the locations that had not moved. The cost is two implementations to maintain until the last location moves.
- Polling instead of a browser message for the signed lease. A browser message is immediate but is lost if the customer closes the tab. The poll can be up to ten minutes late, so the customer also has a button that checks right away.
- Hold and reconcile instead of retrying a payment that timed out. A held session needs a person to release it. A blind retry can charge a card twice.

## Outcome

The integration shipped on October 31, 2026. The first two locations moved to the new system on November 2, and the remaining locations are following on the client's schedule while rentals continue on both systems. A full review of the code before release found a case where a released hold could never be paid, and it was fixed before any location moved.

Built with PHP, a REST API, MariaDB, and Vue.js. Client and vendor names are left out.
