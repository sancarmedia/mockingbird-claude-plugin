---
name: mockingbird
description: >
  Makes static Meta ads (Facebook and Instagram) through the Mockingbird connector: searches a
  hand-picked library of winning ads, remakes the ones the user picks in their brand's voice,
  colours and real product photos, makes new ads from scratch, and delivers each ad in 4:5, 1:1
  or 9:16. Use when the user asks for Meta, Facebook or Instagram ads, ad creative, ad ideas,
  winning or competitor ads, a swipe file, a remake of a specific ad, more ads to test, a Stories
  or Reels version of an ad, or asks about their Mockingbird credits, brands or plan. Also covers
  what each call spends, choosing sizes, polling a remake until it finishes, and upgrades.
---

# Mockingbird: Meta ads from proven winners

> **What this skill does to your account.** Read before installing.
>
> - **Spends Mockingbird credits.** Every size of every ad spends one credit from the user's
>   monthly plan: a remake in 4:5 and 9:16 spends 2, and 3 ads from scratch in all three sizes
>   spend 9. Call `get_account` and tell the user what the request will spend, and how many credits
>   they have left, before any `remake_ad` or `make_ads` call, unless they already approved it.
>   Failed ads and failed sizes spend nothing.
> - **Talks to one endpoint only:** Mockingbird's hosted MCP server at
>   `https://www.mockingbirdhouse.com/api/mcp`, under the user's own Mockingbird account, which
>   they sign in to with OAuth when they connect.
> - **Makes ad images only.** It does not publish ads, touch the user's Meta ad account, or change
>   budgets or campaigns. It does not run local commands and does not write files. Payments happen
>   on mockingbirdhouse.com, never inside Claude.

Mockingbird starts from a hand-picked library of winning Facebook and Instagram ads, the ones real
brands keep paying to run, and rebuilds the ones the user chooses with their own product, colours
and words. It can also plan and make brand-new ads with no source ad. This skill tells you which
tool to call, what it spends, and how to hand the finished ads back.

## Keeping this skill updated

**Source:** [github.com/sancarmedia/mockingbird-claude-plugin](https://github.com/sancarmedia/mockingbird-claude-plugin)
**Tool reference:** [mockingbirdhouse.com/docs/mcp](https://www.mockingbirdhouse.com/docs/mcp)

| Installation | How to update |
|---|---|
| Added from the Claude plugin directory | Updates arrive automatically once a new version is published |
| Claude Code, added as a marketplace | `/plugin marketplace update mockingbird` |

## Setup

Call `get_account` first. If the tool does not exist, the connector is not connected. Tell the
user to open the plugin's **Connectors** tab, connect Mockingbird and sign in when it prompts.
Sign-in is OAuth, so there is nothing to copy or paste.

Then read the result:

- **No brand** (`brand` is null): every ad is built from the brand's website, colours, voice and
  product photos, which the user sets up once at
  [mockingbirdhouse.com/onboarding](https://www.mockingbirdhouse.com/onboarding). Stop there; the
  tools cannot create a brand.
- **No plan, or status `blocked`**: making ads needs a Mockingbird plan. Call `upgrade_plan` and
  give the user the returned link. Do not quote prices the tool did not return.
- **Otherwise**: say the plan, the credits left this period and the brand names in two or three
  short lines, then help with what they asked.

## Route the request to the right tool

Every tool comes from the `mockingbird` connector this plugin bundles. If a bare tool name does not
resolve, qualify it with the server name, as in `mockingbird:remake_ad`.

| The user asks | Call | Credits |
|---|---|---|
| "Show me winning ads for my product" / "ad ideas" / "swipe file" | `search_library` (set `industry`, and a short `query` such as a benefit, a format or an advertiser) | free |
| "Save this one" | `heart_ad` | free |
| "Remake this ad for my brand" | `remake_ad` with `library_ad_id`, `brand`, `sizes`, optional `notes` | 1 per size |
| "Make me 5 ads" / "new ads, no source ad" | `make_ads` with `count`, `brand`, `sizes`, optional `notes` | count × sizes |
| "Remake my own or a competitor's ad" (not in the library) | `upload_ad`, then `remake_ad` with the new id | free to upload |
| "Is it done?" | `get_remake_status` with the `job_id` | free |
| "Show my recent ads" | `list_my_remakes` | free |
| "Which brands do I have?" | `list_brands` | free |
| "How many credits do I have?" | `get_account` | free |
| "I'm out of credits" / "I need more" | `upgrade_plan` | free; returns a link |

Call `search_library` once without filters when you need the list of valid `industry` values.

## Credits and sizes

- **Sizes:** `4:5` portrait (1080x1350) for Facebook and Instagram feeds, Meta's recommended feed
  size and the default; `1:1` square (1080x1080) for feeds and carousels; `9:16` full screen
  (1080x1920) for Stories and Reels, with text kept inside Meta's safe zone. Set any one, two or
  all three in `sizes`.
- **Match the placement.** If the user names placements, pick the sizes that fit ("Reels only"
  means `["9:16"]`). If they do not, use the default `["4:5"]` and mention the other sizes once.
  Do not add sizes the user did not ask for: each one spends a credit.
- **Quote before spending.** State the total ("2 ads in 4:5 and 9:16 spends 4 credits; you have
  37 left") and wait for a yes, unless the user already told you to go ahead.
- Extra sizes are made from the first finished size, so a set keeps the same words, product and
  colours.
- If the request needs more credits than the user has, say so, suggest fewer ads or sizes, and
  offer `upgrade_plan`.

## Pick the brand

An account can hold several brands. If the user named one, set it as `brand` (a name or
`brand_id`) on every `remake_ad` and `make_ads` call. If they did not and `get_account` or
`list_brands` shows more than one, ask which. Every response says which brand it used; say it back.

## Waiting for ads

- A remake takes under a minute for the first size and about a minute more for extra sizes.
  `make_ads` takes 1 to 3 minutes.
- On hosts that support MCP Apps, the remake widget updates by itself. Otherwise call
  `get_remake_status` about every 20 to 30 seconds, and stop when `status` is `done` or `failed`.
- `finished_ads` holds one image URL per size. `sizes_failed` lists sizes that did not render;
  they were not charged.
- A `failed` job spent nothing. Say so and offer to try again.

## Hand back ads the user can use

- **Lead with the ads.** For each finished ad give the image link for each size and one line on
  the angle or what changed from the source ad.
- **Show, don't describe.** Where the host renders markdown images, show the image.
- **Say where each size goes:** 4:5 and 1:1 in feeds, 9:16 in Stories and Reels.
- **Suggest a name for Ads Manager** in the form `MB | buyer or angle | format | size | date`, so
  results can be compared later.
- **End with one next step:** another angle to test, the same ad in another size, or a change
  request.

## Honesty rules

- "Winning" means Mockingbird picked the ad by hand from ads brands keep running. Never claim spend,
  reach, ROAS or results for a library ad, and never promise results for a new one.
- Only use facts about the product that the user gave or that their brand setup holds. If an offer
  or claim is unclear, ask.
- Mockingbird keeps a source ad's layout, hierarchy and hook pattern and replaces the product,
  photos, colours and words. Never ask it to copy another brand's logo, name, faces, reviews or
  claims.
- `upgrade_plan` only prepares a link. The user confirms the upgrade on mockingbirdhouse.com; you
  cannot complete a payment.

## Example prompts

- "Find me winning ads for my skincare brand and remake the best two."
- "Remake this ad for NEURA in 4:5 and 9:16."
- "Make me 5 new ads for our Black Friday offer, square and portrait."
- "I only run Reels. Turn my last ad into a 9:16."
- "How many credits do I have left this month?"

## Troubleshooting

| Symptom | Fix |
|---|---|
| Mockingbird tools missing | The connector is not connected. See Setup. |
| 401 or 403 | The sign-in expired or was revoked. Reconnect Mockingbird from the plugin's Connectors tab. |
| "No brand set up" | The user sets up their brand once at mockingbirdhouse.com/onboarding. |
| `make_ads` says the brand has no product photos or logo | The user adds a product photo at mockingbirdhouse.com. `remake_ad` still works. |
| Out of credits | Tell the user the balance, suggest fewer ads or sizes, and offer `upgrade_plan`. |
| A size is in `sizes_failed` | It was not charged. Offer a remake in that size. |
