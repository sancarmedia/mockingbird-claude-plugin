# Mockingbird for Claude

Static Meta ads from proven winners, made in chat. Ask Claude for Facebook and Instagram ads and
Mockingbird searches its hand-picked library of winning ads, the ones real brands keep paying to
run, then remakes the ones you choose with your own product photos, colours and words. It can also
plan and make brand-new ads from scratch, each on a different angle. Every ad comes back as a
ready-to-upload image in 4:5 for feeds, 1:1 square, or 9:16 for Stories and Reels.

Built for ecommerce founders, marketers and agencies who run Meta ads and need a steady supply of
genuinely different creative to test, without briefing a designer for every round.

## What you get

**Skills**

- `mockingbird`: routes a request to the right Mockingbird tool, quotes the credits before
  anything is spent, picks sizes that fit the user's placements, and polls each ad until it is
  ready.
- `remake-winning-ads`: finds library ads that fit your product, shortlists three different
  angles with a one-line reason each, and remakes the ones you pick in your brand.
- `weekly-test-batch`: plans a batch where each ad is a different buyer or message rather than a
  colour swap, then makes it from library remakes and new ads.

**Commands**

- `/mockingbird:find-winning-ads`: shortlist winning ads for a product or industry. Spends no
  credits.
- `/mockingbird:remake-ad`: remake one ad in your brand, in the sizes you need.
- `/mockingbird:make-ads`: make 1 to 10 new ads from scratch.
- `/mockingbird:test-batch`: plan and make this week's test batch.

## Setup

You need a Mockingbird account with a plan, and your brand set up once at
[mockingbirdhouse.com](https://www.mockingbirdhouse.com): your website, colours and a few product
photos. Every ad is built from that brand profile.

In Claude Code, install the plugin from this repository:

```
/plugin install mockingbird --marketplace sancarmedia/mockingbird-claude-plugin
```

In the Claude apps, add Mockingbird from the plugin directory. Then connect the Mockingbird
connector from the plugin's **Connectors** tab and sign in when Mockingbird prompts you. Sign-in
is OAuth, so there is no API key to create, paste, or store.

## What this plugin does, exactly

Disclosed in full so you can judge it before installing:

- **It connects to one endpoint and nothing else:** Mockingbird's hosted MCP server at
  `https://www.mockingbirdhouse.com/api/mcp`, over HTTPS. There is no other network destination,
  no telemetry, and no analytics in the plugin.
- **It runs no local code.** The plugin ships skills, commands, and one remote MCP server
  declaration. It contains no hooks, no scripts, no executables, and no `bin/` directory. It
  does not read or write files on your machine.
- **It authenticates as you, through OAuth.** The plugin never reads an environment variable,
  never asks for a key, and never handles a credential itself. Your Mockingbird session is held
  by the client, and Mockingbird's authorization server issues it.
- **It makes ad images only.** It does not publish ads, connect to your Meta ad account, or
  change campaigns, budgets or audiences. You download the finished images and upload them to
  Ads Manager yourself.
- **It sends what you ask for.** Tool calls carry your search terms, the ad you chose, the sizes,
  and any notes you add (an offer, an audience, a claim to avoid). If you upload an ad to remake,
  the upload widget sends that image to `https://www.mockingbirdhouse.com/api/mcp/upload`, and it
  stays private to your Mockingbird account. Finished ads come back as time-limited links to
  Mockingbird's image storage, which you open or download.
- **It spends Mockingbird credits.** Each size of each ad spends one credit from your monthly
  plan: a remake in 4:5 and 9:16 spends 2. The `mockingbird` skill tells Claude to check your
  balance and quote the total before making anything. Failed ads and failed sizes spend nothing.
  Upgrades are confirmed on mockingbirdhouse.com, never inside Claude.

## Credits and billing

Mockingbird plans come with a monthly allowance of credits, shared between the web app and
Claude. Searching the library, saving ads and checking on ads are free. Ask Claude "how many
credits do I have left?" at any time. Plans and prices are on
[mockingbirdhouse.com](https://www.mockingbirdhouse.com).

## Links

- Product: [mockingbirdhouse.com](https://www.mockingbirdhouse.com)
- MCP docs and tool reference: [mockingbirdhouse.com/docs/mcp](https://www.mockingbirdhouse.com/docs/mcp)
- Support: [hello@mockingbirdhouse.com](mailto:hello@mockingbirdhouse.com)
- Privacy: [mockingbirdhouse.com/privacy](https://www.mockingbirdhouse.com/privacy)
- Terms: [mockingbirdhouse.com/terms](https://www.mockingbirdhouse.com/terms)

## License

MIT. See [LICENSE](./LICENSE).
