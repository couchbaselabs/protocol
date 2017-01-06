# Protocol Wishlist

Let's collaborate on a feature wish-list. Not sorted yet.

Consider a layered approach.  For example...
 - a lowest layer choice of transport.
   - For example, greenstack vs http/2 vs grpc vs thrift vs aeron vs whatever.
 - a choice of payload/message encoding.
   - For example, protobufs vs flatbuffers vs cbor vs bson vs json vs whatever.
 - a checklist/policy of general issues which higher-level messages and definitions should follow.
   - For example, naming conventions for commands (camelCase, under_scores?), fields, errors, namespaces and other cross-cutting aspects.
   - For example, do we call it "timeout" or "Timeout" or "ErrorAfterMSec"?  Pick one for across the board.
   - "reqId or requestId"?  "vbucketId" or "vbid" or "partition"?
   - Here, a checklist/policy can address inconsistencies of the past and help us to leave room for the future.
 - a set of protocol specs on a per-feature level
   - For example, FTS would have its own spec, with its own FTS-specific commands/messages/state-transitions.

    -------------------------------------------------------
    | FTS protocol    | Foo protocol    | Bar protocol    |
    -------------------------------------------------------
    | common shared tools / codegen                       | ex: state machine parser / doc generator / CI integration
    -------------------------------------------------------
    | common base concerns / policy / guidelines          | ex: "param names *should* be pascalCase"
    -------------------------------------------------------
    | choice of payload/message encoding                  | ex: protobuf vs flatbuffer
    -------------------------------------------------------
    | choice of transport                                 | ex: grpc over HTTP/2
    -------------------------------------------------------

## 1. Required
 - transport level concerns
   - reliable delivery
   - binary protocol
   - must scale to high throughput & low latency
   - encryption & auth
   - pipeline support
   - backpressure signalling
   - stream semantics
     - request/response (stream of 1)
     - request/stream (finite stream of many)
     - fire-and-forget (no response)
     - event subscription (infinite stream of many)
     - channel (bi-directional streams)
   - compression support
   - channels
 - payload/message encoding concerns
   - deterministic field access O(1), not O(n)
 
## 2. Good to Have
 - transport level concerns
   - wireshark dissector
 - payload/message encoding concerns
   - zero-copy access
 - cross-cutting policy concerns
   - machine parsable protocol
   - protocol state machine to grasp whats going on
   - codegen into languages
   - proxy'ability
     - ability to add new commands/messages and an intermediary proxy would need zero changes to support the new commands/messages
   - shared set of common error codes
   - namespaces for feature specific errors
   - request tracing identifiers
     - example: this activity on node Y was due originally to a user request that came into node X
   - common approach to timeouts
   - common approach to consistency (stale=very, at-plus, request-plus, etc)
   - consistency to not-my-vbucket / not-my-resource / redirect / CCCP handling
   - versioning rules
   - stats
     - common stats naming consistency ("cpu", "Cpu", "RAM", "memused", "rss")
     - self-describing stats metadata or naming to allow for automatic aggregation
       - For example, by using the right prefix or suffix ("totalRequests" vs "numConnections"), a stats collector can assume whether a stat is a ever-growing counter or a current-value gauge.  As new stats are added by independent teams, the stats collector should need zero changes.
     - a feature server might publish metadata of titles and descriptions for its stats (just a json thing?), which a UI can automatically display.
