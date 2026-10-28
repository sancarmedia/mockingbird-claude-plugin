---
name: test-batch
description: Plan and make this week's batch of Meta ads to test, one buyer or angle per ad, mixing library remakes and new ads.
---

# This week's test batch

Run the `weekly-test-batch` skill:

1. Check credits and brand with `get_account`.
2. Agree the buyers or angles, the number of ads and the sizes.
3. Show the plan as a table (buyer, format, message, source) with the total credits, and wait for a yes.
4. Make it with `remake_ad` for formats the library has and `make_ads` for the rest.
5. Deliver every ad with image links per size and a suggested Ads Manager name.
