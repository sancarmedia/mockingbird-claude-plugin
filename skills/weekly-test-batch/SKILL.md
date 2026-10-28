---
name: weekly-test-batch
description: >
  Plans and makes a batch of Meta ads to test this week, built so each ad is a genuinely different
  message for a different buyer or angle rather than a colour swap, in the sizes the user's
  placements need. Use when the user wants ads to test, a creative batch, "ads for this week",
  more ads to scale, or asks how many ads to make.
---

# Weekly test batch

> **Spends credits only after a yes.** Planning is free. Every size of every ad spends one
> Mockingbird credit, so show the plan with its total and the balance, and wait for approval
> before any `remake_ad` or `make_ads` call. Tool calls follow the companion `mockingbird` skill.

Goal: a set of ads Meta can tell apart, made quickly, ready to upload and label. Meta groups ads
that look alike, so ten colour swaps of one idea test like one ad. Explicit instructions from the
user override anything below.

## Steps

1. **Check the budget.** Call `get_account`: note the brand and the credits left. If the user gave
   a number of ads, use it; otherwise suggest a batch that fits their credits, usually 4 to 10.
2. **Find the angles.** Take from what the user said, or ask: what they sell, who buys it
   (situations, not demographics, for example "parents at bedtime" or "students in small rooms"),
   and any current offer. If they give only the product, propose three to five buyers and let them
   edit the list.
3. **Choose sizes once for the batch.** Ask where the ads will run: 4:5 for feeds (the default),
   1:1 for feeds and carousels, 9:16 for Stories and Reels. Every size multiplies the credits.
4. **Plan the grid.** Pair buyers with different formats, for example a comparison, a big headline,
   a note or letter, a review, an offer. Show it as a short table: buyer, format, one-line message,
   source (library ad or from scratch). Keep every row a different message. Put the total credits
   under the table.
5. **Confirm.** Wait for a yes, unless the user already approved the spend.
6. **Make it.**
   - For formats the library has, call `search_library` with the right industry, then `remake_ad`
     on the best match with `brand`, `sizes`, and the buyer and message as `notes`.
   - For the rest, call `make_ads` with `count`, `brand`, `sizes`, and `notes` that list the buyers
     and angles, so each concept targets a different one.
7. **Deliver.** As ads finish (the widget updates itself, or poll `list_my_remakes`), list each ad
   with its buyer, format and an image link per size. Suggest an Ads Manager name for each in the
   form `MB | buyer | format | size | date`.

## Rules

- Do not promise results, win rates or ROAS. You can say that testing more genuinely different ads
  gives more chances to find a winner.
- Use only product facts the user gave or their brand setup holds. If an offer or claim is unclear,
  ask.
- Failed ads and failed sizes spend nothing. Say which ones failed and offer to redo them.
