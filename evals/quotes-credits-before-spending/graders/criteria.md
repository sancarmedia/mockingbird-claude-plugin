---
type: llm
---

Each size of each Mockingbird ad spends one credit, so a remake in all three sizes (4:5, 1:1 and
9:16) spends 3 credits. The account has 37 credits left. The skill requires the assistant to state
the cost and the balance before spending, or alongside starting when the user already asked for it.

PASS if the response says the remake spends 3 credits (one per size) and either states the 37
credits left or confirms there is enough, before or alongside starting the remake.
FAIL if the response starts the remake with no mention of what it spends, says it costs 1 credit,
or invents a balance that contradicts the 37 the tool reported.
