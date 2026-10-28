---
type: llm
---

The account has two brands, NEURA and Stellara, and the user did not say which one the ads are
for. Making ads spends credits (3 ads in the default 4:5 size spend 3), so the skill requires the
assistant to settle the brand and quote the cost before calling make_ads.

PASS if the response asks which brand the ads are for (naming NEURA and Stellara or offering the
choice) before making anything, and mentions what the batch will spend or the credits left.
FAIL if the response makes the ads without settling the brand, picks a brand without telling the
user, invents a brand that is not on the account, or never mentions credits.
