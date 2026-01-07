# cache-aware-prompts

cache-aware-prompts helps you assemble prompts in a way that maximizes prefix-cache hits (stable ordering, deterministic serialization, strict separation of static vs dynamic sections). It can diff two prompt builds to show exactly what changed and where caching is likely to break. Built to pair with API-side prompt caching for lower latency and lower input costs on repeated prefixes (tools, policies, schemas).

## Prompt/prefix caching is valuable but fragile/confusing

**Goal:** design prompts so they *actually* hit cache.

- **Keep the prefix stable:** don’t inject timestamps, random IDs, or reorder sections/tools. Even a 1-token difference can invalidate cache from that point onward. ([Manus][7])
- **Deterministic serialization:** stable JSON key order, stable tool list ordering, stable formatting. (This matters a lot for agents.)
- **Split “cached” vs “dynamic” segments:** put policies/tool schemas/project rules into the static prefix; keep user task + retrieved snippets in a dynamic tail. OpenAI documents prompt caching as a way to cut latency/cost on repeated prefixes. ([OpenAI Cookbook][8])
- **Instrument it:** show a “cache hit likelihood” indicator (e.g., “prefix changed at token 312 because tool list changed”).

COMING SOON
