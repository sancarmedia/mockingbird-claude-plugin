---
name: remake-ad
description: Remake a winning Meta ad (from the library or uploaded) in the user's brand, in 4:5, 1:1 and/or 9:16, after confirming the credits.
---

# Remake an ad

Given a `library_ad_id`, an ad the user described, or an image they want to remake:

1. Call `get_account`. Confirm the brand (ask if there is more than one and none was named) and note the credits left.
2. If the ad is not in the library, call `upload_ad` and let the user drop the image into the widget, then use the new `library_ad_id`. If the user only described an ad, find it with `search_library`.
3. Settle the sizes: 4:5 for feeds (default), 1:1 for feeds and carousels, 9:16 for Stories and Reels.
4. Tell the user what it spends (one credit per size) and what they have left, and wait for a yes unless they already approved it.
5. Call `remake_ad` with `library_ad_id`, `brand`, `sizes`, and `notes` only for direction the user gave.
6. Poll `get_remake_status` about every 20 to 30 seconds (the widget does this itself where it shows) and hand back the image link for each size, with where each size runs. Note any size in `sizes_failed`; it was not charged.
