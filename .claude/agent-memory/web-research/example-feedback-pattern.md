---
name: One focused query usually beats three broad ones
description: Cost-efficient web research — narrow the query, stop when you have a recommendation
type: feedback
---

> **Example pattern.** This file shows the *shape* of a useful agent-memory note. Replace it with your own learnings as the agent runs.

When researching a local supplier or service, the cheapest reliable pattern is:

1. **One narrow search query** in the local language (e.g. region + trade + town), not a broad English query.
2. **Pull the top 3 candidates** from the first results page — name, phone, email, rating.
3. **Stop searching** once you have 3 candidates and one clear recommendation. A 4th option rarely changes the answer and doubles the cost.
4. **Cross-check the top pick** against one second source (map listing, review site, or the business's own site) before recommending.

**Why:** Repeated broad searches return the same top results re-ranked. They cost more and do not improve the recommendation.

**How to apply:** Default to one focused local-language query. Only widen the search if the first query returns fewer than 3 plausible candidates. When you find a known-good supplier, save them to memory so future runs hit the cache instead of re-searching.

**Anti-pattern to avoid:** Chaining 4–5 search queries before reading any of the results. By the time you've spent the budget on queries you've often left no room for the actual `WebFetch` reads that confirm contact details.
