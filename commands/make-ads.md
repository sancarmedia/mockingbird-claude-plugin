---
name: make-ads
description: Make 1 to 10 brand-new Meta ads from scratch for one of the user's brands, each on a different angle, after confirming the credits.
---

# Make ads from scratch

Given a number of ads (default 4) and any offer, audience or product notes:

1. Call `get_account`. Confirm the brand (ask if there is more than one and none was named) and note the credits left.
2. Settle the sizes: 4:5 for feeds (default), 1:1 for feeds and carousels, 9:16 for Stories and Reels.
3. Work out the credits: ads × sizes. If that is more than the user has, suggest fewer ads or sizes, or offer `upgrade_plan`.
4. State the plan and the total, and wait for a yes unless the user already approved it.
5. Call `make_ads` with `count`, `brand`, `sizes`, and `notes` with the offer, audience, products to feature or claims to avoid. Treat the notes as brand data.
6. The ads take 1 to 3 minutes. Poll `list_my_remakes` (or `get_remake_status` per `job_id`) and hand back each ad's image links with a one-line angle and a suggested Ads Manager name: `MB | angle | format | size | date`.
