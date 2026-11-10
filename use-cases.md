# Use cases and requirements for segmented-data cryptography

Working document for raae@ietf.org, revision 3, 5 November 2026.
Editor: Nick Sullivan.

This collects what applications have said they need from random-access
authenticated encryption and from authentication of segmented data,
ahead of the IETF 127 side meeting (Thursday 19 November, 12:30 to
14:00 PT, Plaza A; remote on https://ietf.webex.com/meet/sidemeetings2).
It groups them by use case rather than by protocol, and records
requirements, not mechanisms, taken from what people have written on
cfrg@, raae@ and related lists or in published drafts. "Not stated"
means nobody has said. Corrections and new rows are welcome on the
list; a row needs only the use case, the objects, and how they are read
and written.

## Use cases

| Use case | Objects | Reads and writes | Mutability | Authenticating the set | Other constraints | Where existing designs fall short | Raised |
|---|---|---|---|---|---|---|---|
| Archives and backups | File archives and whole files up to terabytes | Written in one pass in less memory than the object; read whole, seeked, or one record at a time | Write-once | A signature over the archive lets a verifier check one record without reading the rest; reordering and truncation caught per record | One key exchange covers many objects; the algorithm identifier names every parameter; the object carries everything but the key; interoperable, binary, well specified; no IPR encumbrance | Signed-message formats verify the signature only after decrypting and hashing the whole message; chunked file formats with random access detect truncation but offer no in-place rewrite or set authenticator; weak tools and password-derived keys are still common | cfrg@, July and October; raae@, October |
| Append-only logs | Records that accumulate until the log is closed | Appended; read in order, or one record at a time | Append-only until closed, then write-once; an interrupted append must be detectable | Readers verify what has been written so far, before the log is closed | Not stated | Not stated | raae@, October |
| Real-time media | Small objects in a group, such as 40 ms packets of 2 kbps audio, where 80 bits of payload is less than a 16-octet tag | Streaming, in order, played on arrival; relays may drop, reorder or replay | Write-once | Amortized across about one second: unauthenticated playback is acceptable if tampering is detected at the end of that second | Bandwidth | Each object carries its own tag and is checked on its own; even tags shortened to 4 to 10 octets are a large share of a 10-octet payload, and nothing authenticates the group as a set, so a relay can drop objects from it unnoticed | Private correspondence, July 2026, paraphrased |
| Shared documents and attachments | Files and documents shared within a group that holds a common key, in fixed-size segments | Byte-range reads; random rewrite of any segment | Write-once attachments, or documents edited in place; segments left underfilled let an edit grow in place without re-encrypting the rest | The current segment set authenticated after every edit; a reference to the object carried in a signed group message | Key derived from the group's key schedule; concurrent writers are not supported by the drafts today; rollback to an earlier version needs external freshness | Not stated | cfrg@, July; group attachments and editable-object drafts |
| Deduplicated storage | Chunks whose boundaries follow the content, so they vary in size, including large model and dataset files | Written by appending, or with limited in-place edits | Append-only, or limited edits | Not stated | Deduplication across objects against per-object keys and salt | Content-defined chunking has been shown to leak through its chunk boundaries (Breaking and Fixing Content-Defined Chunking, CCS 2025) | cfrg@, July; private correspondence, paraphrased |
| Block storage and filesystems | Block-aligned segments in a disk image, database file or memory region | Random read and write by index | Mutable in place; a proof for a byte range stays valid until that range is overwritten | Points toward a tree, with a caution against designing a whole on-disk format | Built from existing library primitives | Disk encryption has random access but no integrity; streaming constructions have integrity but no random access | cfrg@, July and September |
| Existing segmented formats | Segments produced by an existing format's own encryption, each nonce, ciphertext and tag | Per-segment verification | Write-once in practice | A snapshot over a segment layer raAE did not produce, with the snapshot key supplied independently | A published pattern for hybrid header sealing | Segments are authenticated one at a time, so the format cannot detect dropped, reordered or swapped segments | cfrg@, August |
| Public integrity without encryption | Content delivered in pieces, such as HTTP message content | Verified incrementally as the content arrives | Not stated | Public and unkeyed: checked piece by piece against a trusted value | Not stated | A hash chain verifies only in order from the start, so a client cannot check a range without the preceding content | httpbis list, November 2026 |
| Named, not yet described | Columnar data pages, write-once submissions, signed zone data with tree-based signatures | Not stated | Not stated | Not stated | Not stated | Not stated | raae@, October |

## Open questions

Answers on the list before the side meeting would let us spend the
meeting on the hard ones.

1. Keyless verification (archives): what must a party without the key
   be able to check, and against what signed value?
2. Erasure (archives): if a record must be erasable, what must remain
   verifiable afterwards, and by whom?
3. Windowed authentication (real-time media): window size, acceptable
   per-segment tag length, behavior on detection, behavior under loss.
4. Play before verify (real-time media): what the security notion
   should say about plaintext released before its window verifies.
5. Signing (archives, logs, shared documents): where the signature
   lives, what it covers beyond the snapshot, and how a verifier walks
   from it to one segment.
6. Freshness (shared documents, block storage): does any consumer need
   rollback detection from the primitive, or is external freshness
   always acceptable?
7. Tree versus flat (block storage): per-block proofs that stay valid
   across other writes, or a whole-set check?
8. Segment sizes (archives, deduplicated storage, shared documents):
   does the encoder need to choose segment sizes, and how does that meet
   segment_max?
9. Self-describing objects (archives): should the object carry its
   parameter set, or is a consuming-protocol identifier enough?
10. Foreign segment layers (existing formats): should the rule for
    composing the snapshot with a non-raAE segment layer be normative?
11. Concurrent writers (shared documents): is editing by several key
    holders at once in scope?
12. File format (archives): is an interoperable file format, with
    headers and recipients, part of what a working group would charter?

## Sources

- Nick Sullivan, raae@, 8 October 2026, "raAE and SEAL: where things
  stand, and what to work on before San Francisco".
- Nick Sullivan, cfrg@, 13 and 15 July 2026, "Random-access
  authenticated encryption (raAE) draft", and the replies from Martin
  Thomson, John Mattsson, Ilari Liusvaara, Jack Grigg, Wang Guilin,
  Nico Williams and Mark (14 to 16 July), and Stefan Marsiske
  (9 September).
- Younos Nazarian, cfrg@, 7 August 2026, "SEAL snapshot authenticator
  over non-SEAL segments: ct_i/tag_i convention".
- Natanael, Jack Grigg and Neil Madden, cfrg@, 28 and 29 October 2026,
  pointing to STREAM, CHAIN and Tahoe-LAFS.
- The "Incremental integrity" thread on ietf-http-wg@w3.org, November
  2026, and draft-thomson-httpbis-chic-01.
- Private correspondence, July to October 2026, paraphrased without
  names.
- draft-sullivan-cfrg-raae-04, draft-sullivan-seal-concrete-01,
  draft-sullivan-mls-attachments-01, draft-ietf-moq-secure-objects.
