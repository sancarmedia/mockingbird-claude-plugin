---
name: remake-winning-ads
description: >
  Finds proven Meta ads in Mockingbird's hand-picked library that fit the user's product, explains
  in one line why each works, and remakes the chosen ones in the user's brand and sizes. Use when
  the user asks for winning ads, ad ideas, a swipe file, competitor ads, "ads like this one", or
  to remake a specific ad.
---

# Remake winning ads

> **Spends credits only after a yes.** Searching and shortlisting are free. Each size of each
> remake spends one Mockingbird credit, so state the total and the balance before calling
> `remake_ad`, unless the user already approved it. Tool calls follow the companion `mockingbird`
> skill.

The user wants ads built on structures that already work on Meta, rebuilt with their own product,
photos and words. Explicit instructions from the user override anything below.

## Steps

1. **Check the account and brand.** Call `get_account`. If there is more than one brand and the
   user did not name one, ask which. Set it as `brand` on every `remake_ad` call.
2. **Search.** Call `search_library` with the `industry` that matches the product and, when it
   helps, a short `query`: a benefit, a format ("comparison", "review", "listicle") or an
   advertiser. On hosts that support MCP Apps the results show as a grid the user can heart and
   remake from.
3. **Shortlist three.** Pick ads with different `hook_type` values, so the user gets three
   different angles rather than three versions of one idea. For each, say in one line what the
   format is and why it stops the scroll, based on what you can see in the ad and its copy.
4. **Settle the sizes.** Ask where the ads will run if the user has not said: 4:5 for feeds (the
   default), 1:1 for feeds and carousels, 9:16 for Stories and Reels.
5. **Confirm the spend.** "Remaking 2 ads in 4:5 and 9:16 spends 4 credits. You have 37 left. Go
   ahead?" Wait for a yes unless the user already asked you to go ahead.
6. **Remake.** Call `remake_ad` for each chosen ad with `brand` and `sizes`. Add `notes` only for
   direction the user gave: an offer, an audience, a claim to avoid.
7. **Deliver.** When each job finishes (the widget updates itself, or poll `get_remake_status`),
   give the image link for every size and one line on what changed from the original. If a job or
   a size failed, say so and that it was not charged.

## Rules

- Do not claim spend, reach or results for a library ad. "Winning" means Mockingbird picked it by
  hand from ads that brands keep running.
- Never ask for another brand's logo, name, faces, reviews or claims to be copied. Mockingbird keeps
  the structure and replaces the rest.
- To remake an ad that is not in the library, call `upload_ad` and let the user drop the image into
  the upload widget, then remake the new `library_ad_id`.
- If the user runs out of credits, call `upgrade_plan` and give them the link. You cannot complete
  the payment.
