# Protocol Wishlist

Let's collaborate on a feature wish-list. Not sorted yet.

## 1. Required
 - reliable delivery
 - Binary Protocol
 - must scale to high throughput & low latency
 - Deterministic field access O(1), not O(n)
 - Encryption
 - pipeline support
 - channels
 - backpressure signalling
 - Stream Semantics
   - request/response (stream of 1)
   - request/stream (finite stream of many)
   - fire-and-forget (no response)
   - event subscription (infinite stream of many)
   - channel (bi-directional streams)
 
## 2. Good to Have
 - Zero-Copy access
 - wireshark dissector
 - protocol state machine to grasp whats going on
 - codegen into languages
