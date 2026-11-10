# raAE side meeting, IETF 127: agenda

Thursday 19 November 2026, 12:30 to 14:00 PT, Plaza A, Hilton Union
Square, San Francisco.
Remote: https://ietf.webex.com/meet/sidemeetings2
List: raae@ietf.org

Goal: agree on the shape of the work and a charter, and test both
against the applications that need it.

| PT | Min | Item |
|---|---|---|
| 12:30 | 5 | Welcome, Note Well, goals |
| 12:35 | 10 | What raAE provides today |
| 12:45 | 15 | Security notions and formal analysis (Andrés Fábrega, tbc) |
| 13:00 | 15 | Features new in -04/-01: unkeyed integrity and signing, streams and growing objects |
| 13:15 | 15 | Proposed features: tree authenticator, shorter or aggregated tags for short segments, a layout for content-defined chunking |
| 13:30 | 10 | Other applications |
| 13:40 | 10 | The shape of the group's outputs |
| 13:50 | 5 | Charter sketch |
| 13:55 | 5 | Next steps and owners, including an IETF 128 hackathon interop project |

Erasure of one segment was announced as a proposed feature. Deleting a
segment's ciphertext while keeping its leaf is already supported in
draft-sullivan-cfrg-raae-04 through subset verification (Sections 3.3.1
and 4.7.2), so it is covered briefly in the proposed-features slot as a
correction.

To read beforehand:

- draft-sullivan-cfrg-raae-04:
  https://datatracker.ietf.org/doc/draft-sullivan-cfrg-raae/
- draft-sullivan-seal-concrete-01:
  https://datatracker.ietf.org/doc/draft-sullivan-seal-concrete/
- [Use cases and requirements](../../use-cases.md)
- [Charter sketch](../../CHARTER.md)
