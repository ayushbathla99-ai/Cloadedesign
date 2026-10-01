# GTM Strategist Study → Ayush's LinkedIn Article System

A full study of [knowledge.gtmstrategist.com](https://knowledge.gtmstrategist.com/), Maja Voje's *GTM Strategist* Substack (36K+ subscribers, weekly since Nov 2023), turned into a replicable content system for Ayush Bathla's LinkedIn articles.

## What's inside

| File | What it gives you |
|---|---|
| [`01-archive-index.md`](01-archive-index.md) | All **146 posts**: date, title, subtitle, topic cluster, word count, reactions, access, link |
| [`02-learning-pattern.md`](02-learning-pattern.md) | How the publication evolved (3 eras), topic clusters ranked by engagement, the top 10 posts and why they won, the teaching loop, cadence and business model |
| [`03-content-style-guide.md`](03-content-style-guide.md) | Voice DNA (8 traits), 12 hook patterns, master article skeleton, 10 title formulas, formatting signatures, rhetorical moves, do/don't, pre-publish checklist |
| [`04-topic-map-and-calendar.md`](04-topic-map-and-calendar.md) | Her topic architecture mapped to your 6 pillars + a **52-week calendar** of ready titles + distribution loop |
| [`05-article-templates.md`](05-article-templates.md) | 8 fill-in-the-blank blueprints (Framework, Hands-on Build, Teardown, Research, Contrarian, Diagnostic, Interview, Retro) |
| [`06-voice-prompt.md`](06-voice-prompt.md) | System prompt + brief + editing prompt to generate new articles in this style with Claude |
| [`linkedin-articles/`](linkedin-articles/) | **3 full, original articles** in the replicated style, each with a companion LinkedIn post |

## How to use it

1. Read `03-content-style-guide.md` once. It's the core of the system.
2. Pick next week's title from `04-topic-map-and-calendar.md`.
3. Fill the brief in `06-voice-prompt.md` and draft with Claude using the matching template from `05`.
4. Replace every `[bracketed placeholder]` with your **real** numbers and stories, then run the §8 checklist.
5. Publish, then run the 4-post distribution loop.

The three sample articles in `linkedin-articles/` are ready to publish once you fill in their placeholders.

## Coverage and sourcing notes

- **Collected:** metadata for all 146 posts (via the Substack archive API) and every publicly visible word of every post page (via a crawl), about **30K words** in total.
- **Full text was available for 4 free posts.** The other **142 are paywalled**: only the public intro (~150–250 words each) is accessible without a paid subscription. That's ~9% of the ~325K total words. The paywall was not bypassed. The intros carry the hooks, voice, and promise blocks, which is where the style lives. The deep body structure was learned from the 4 full posts plus the outlines visible in the previews.
- **To go deeper:** with a paid subscription, export the posts and drop them in, and the same analysis can be re-run on full text.
- **Copyright:** this folder contains analysis, metadata (titles and subtitles), and brief quoted fragments for study. It does **not** republish the articles. The sample articles are original writing. They use her *structural patterns*, not her text, stories, or coined frameworks.
