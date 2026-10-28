---
name: find-winning-ads
description: Find winning Meta ads in Mockingbird's library for a product or industry, shortlisted by angle. Spends no credits.
---

# Find winning ads

Given a product, brand or industry (ask if none was given):

1. Call `get_account` to see the user's brands and credits. If there is more than one brand and none was named, ask which.
2. Call `search_library` once without filters if you need the valid industries, then with the matching `industry` and, if useful, a short `query` (a benefit, a format such as "comparison" or "review", or an advertiser).
3. Shortlist three ads with different `hook_type` values. For each, give the `library_ad_id`, what the format is and, in one line, why it stops the scroll. Do not claim spend or results for library ads.
4. Offer to remake any of them with `/mockingbird:remake-ad`, and say what that would spend: one credit per size.

This command does not call `remake_ad` or `make_ads`, so it spends no credits.
