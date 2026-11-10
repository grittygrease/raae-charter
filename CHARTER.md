# [NAME] working group: charter sketch

IETF 127 side-meeting material, 5 November 2026, revised 10 November
2026. A sketch for discussion, not an agreed charter.

Large objects are increasingly stored, sent and processed in segments
rather than as one unit: encrypted files and backups that are read in
ranges, archives and logs that grow by appending, objects edited in
place, deduplicated backups split at content-defined boundaries, media
delivered in small objects over relays, and HTTP resources fetched in
ranges or processed as they stream. Each of these needs to protect
segments individually while still knowing that the segments it holds
belong together, in order, and complete. Today each application defines
its own construction, with its own analysis and its own failure modes.
Common ones include truncation that goes undetected, segments that can
be reordered or swapped between objects, nonce reuse when data is
rewritten, and plaintext released before it can be verified.

The [NAME] working group will specify symmetric cryptography for
segmented data. Its work covers two related areas.

Encryption of segmented data. Building on the random-access
authenticated encryption (raAE) interface and the SEAL construction, the
group will specify how to encrypt, decrypt, append to and rewrite
individual segments of an object. This includes key commitment, position
and finality binding, nonce handling for each write model (write-once,
append-only and rewritable), cipher suites assembled from existing AEADs
and KDFs, usage limits, serialization layouts and test vectors. Suites
may use a shorter tag length where an existing AEAD already defines one.

Authentication of segmented data. The group will specify authenticators
that bind a set of segments, their order and their count to a single
value: keyed values for holders of a content key, and public digests
that can be signed or carried out of band for parties without one. The
work covers verifying a whole object, a subset or range of segments, and
a growing stream as it arrives, along with the trade-offs between these
structures (chains, lists, two-level structures and trees) for
streaming, random access, appending and overhead. These authenticators
are meant to be usable without encryption, for example for incremental
integrity of HTTP content.

Specifications will state the security properties they provide and the
assumptions they rest on, distinguish authentication of individual
segments from authentication of the whole set, and say what a reader
learns before an object is complete.

The group will produce profiles for the applications that motivate the
work, developed with the working groups that own those protocols.
Initial applications, drawn from the use-case document, are archives and
backups, append-only logs, real-time media such as Media over QUIC,
shared documents and MLS attachments, deduplicated storage with
content-defined chunking, block storage and filesystems, existing
segmented formats that lack a set authenticator, and integrity of HTTP
content without encryption.

Out of scope: new block ciphers, hash functions, AEAD algorithms or
other cryptographic primitives; changes to MLS, MoQ, HTTP or DNSSEC
protocols; general storage and transfer protocols; and key management
beyond what the profiles require. Protection against rollback to an
earlier complete object needs application support, and the group will
state what applications must provide for it rather than solve it in
general.

The group will coordinate with CFRG on security notions and analysis,
with MLS, MOQ and HTTPBIS on their profiles, with DNSOP participants
working on Merkle Tree Ladders on shared authentication requirements,
and with HPKE on identifier reuse. This charter does not presume CFRG
adoption of any document.

Deliverables:

1. Use cases and requirements for symmetric cryptography on segmented
   data (Informational).
2. raAE: the interface, security notions and requirements for
   random-access authenticated encryption. The venue is to be settled
   with CFRG.
3. SEAL: the construction, cipher suites, profiles, layouts, usage
   limits, registries and test vectors (Proposed Standard).
4. Authenticators for segmented data, keyed and public, with their
   verification modes and stated security properties (Proposed
   Standard).
5. Application profiles, starting with MLS attachments and Media over
   QUIC, each developed with the owning working group (Proposed
   Standard).

Milestones:

- [Month YYYY]: Use cases and requirements to the IESG (Informational).
- [Month YYYY]: SEAL specification to the IESG (Proposed Standard).
- [Month YYYY]: Authenticators specification to the IESG (Proposed
  Standard).
- [Month YYYY]: First application profiles to the IESG (Proposed
  Standard).

Candidate names: Segmented Data Cryptography, Segmented Object Security,
Symmetric Protection for Segmented Data.
