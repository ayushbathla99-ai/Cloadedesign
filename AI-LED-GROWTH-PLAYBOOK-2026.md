# THE AI-LED GROWTH PLAYBOOK 2026

### How a Solo Operator Runs a Full GTM Engine With 13 Claude Agents — and Out-Ships a 12-Person Marketing Team

**By Ayush Bathla** — Marketing Growth Consultant · GTM · AI-Led Growth

---

> *Most "AI marketing" guides hand you 50 prompts and call it a system.*
> *This isn't that.*
>
> *This is the operating manual for a 13-agent production line that researches your market, writes your content, distributes it, measures it, learns from it — and gets sharper every week it runs.*
>
> *It's what I'm building for clients in 2026. Open-sourcing it here because the gap between "people who post about AI" and "people who run AI" is now the only moat in growth.*

---

## TABLE OF CONTENTS

- **Part 0** — Why 90% of AI marketing playbooks fail
- **Part 1** — The 13 Agents (with copy-paste system prompts)
- **Part 2** — The Chain: how output of one feeds the next
- **Part 3** — The ROI math (real numbers, real tradeoffs)
- **Part 4** — Industry Overlays (SaaS · Ecom · Agency · Coach · B2B Services)
- **Part 5** — The 90-Day Rollout Plan
- **Part 6** — The Operator's Dashboard (7 metrics that matter)
- **Part 7** — Failure Mode Library (where it breaks + how to fix)
- **Part 8** — The Stack (Claude + 6 tools, ~$2K/mo)
- **Part 9** — The Compounding Moat (why this gets better every month)
- **Closing** — The new GTM operating model

---

# PART 0 — WHY 90% OF AI MARKETING PLAYBOOKS FAIL

There are three traps. Almost every "Claude for marketers" guide falls into all three.

### Trap 1 — Prompt-hoarding
The guide is a list of 50 prompts. You copy them, you try them, you get mid output, you stop. Prompts without a chain are just party tricks.

### Trap 2 — Single-agent thinking
"Use Claude as your copywriter." That's a tool, not a system. A marketing function isn't *one* job — it's research → positioning → planning → writing → distribution → measurement → iteration. One agent can't hold all that context without becoming a smoothie.

### Trap 3 — No quality layer
Every prompt assumes the first output is the deliverable. Real systems have a grader. They have a critic. They have a memory that flags "we already shipped a post on this angle." Without a quality layer, you're just generating slop at scale.

### The reframe: stop thinking "agents," start thinking "production line"

A production line has:
- **Stations** (each does one job well)
- **Conveyor** (output of one feeds the next via a clear contract)
- **QC stations** (catches defects before they ship)
- **Feedback loop** (defects upstream get traced back and fixed)
- **An operator** (you — running the line, not running the lathe)

This playbook is the blueprint for that line.

### The economic shift this enables

A traditional Series A SaaS marketing team:
- Head of Marketing — $180K
- Content Lead — $110K
- SEO Specialist — $90K
- Performance Marketer — $110K
- Designer (0.5 FTE) — $60K
- Marketing Ops — $90K

**Annual burn: ~$640K** (with benefits, closer to $800K)

The 13-agent stack in this playbook, run by **one operator** who knows what they're doing:

- Claude (Max plan + API): ~$300/mo
- n8n self-hosted: ~$20/mo
- Airtable Pro: ~$24/mo
- Apollo + Clay (data): ~$500/mo
- Ahrefs Lite: ~$129/mo
- Hypefury / Buffer: ~$30/mo
- Operator salary: $150K–$220K

**Annual burn: ~$170K–$240K**

You ship 3–5x the output. You measure 10x more. You iterate weekly instead of quarterly.

This is not a "future of work" essay. This is what I'm wiring up for clients right now. Let's get into it.

---

# PART 1 — THE 13 AGENTS

Each agent below has:
- **Job-to-be-done** — one sentence
- **Input contract** — what it needs to do its job
- **Output contract** — what it produces (as JSON schema)
- **System prompt** — copy-paste, production-grade
- **Handoff** — who consumes its output
- **KPI** — how you know it's working
- **Failure mode** — what breaks and the early signal

> **Important:** treat every system prompt below as a starting point. Each one needs your ICP, your voice samples, and 2–3 examples of "good output" injected before it stops sounding like Claude and starts sounding like *you*.

---

## AGENT 01 — POSITIONING RESEARCHER

**JTBD:** Map the live market landscape and surface the one positioning angle competitors have left on the table.

**Input contract:**
- Company URL + 1-paragraph "what we do"
- 5 closest competitor URLs
- 3 customer interview transcripts (or 10 review-mining quotes)

**Output contract (JSON):**
```json
{
  "category_frame": "string",
  "competitor_positioning": [{ "competitor": "string", "frame": "string", "blind_spot": "string" }],
  "unmet_jobs": ["string"],
  "claim_we_can_own": "string",
  "proof_required_to_own_it": ["string"],
  "positioning_statement": "For [ICP] who [pain], we are the [category] that [unique mechanism], unlike [alternative] which [limitation]."
}
```

**System prompt:**
```
You are a positioning strategist trained on April Dunford, Bob Moesta, and 
Andy Raskin frameworks. You do NOT write marketing copy. You diagnose markets.

Inputs you will receive:
- Our company URL + description
- 5 competitor URLs
- Customer voice data (interviews or reviews)

Your job is a 6-step diagnosis:

1. CATEGORY FRAME. What category is the buyer actually shopping in? 
   (Not what we call ourselves — what they Google.)

2. COMPETITOR MAP. For each competitor, extract:
   - Their explicit positioning frame
   - The job they're hired to do
   - The blind spot in their positioning (what they ignore, dismiss, or 
     can't credibly claim)

3. UNMET JOBS. From the customer voice data, extract the 3-5 jobs that 
   are mentioned but no competitor is explicitly serving. Use the 
   customer's words, not yours.

4. CLAIM WE CAN OWN. Identify ONE claim that:
   (a) Maps to an unmet job
   (b) No competitor is making credibly
   (c) We can prove with our existing assets

5. PROOF REQUIREMENTS. List the 3-5 proof points we'd need to make this 
   claim land (case studies, data, demos, frameworks).

6. POSITIONING STATEMENT. Write it in this format:
   "For [ICP] who [specific pain], we are the [category] that 
    [unique mechanism], unlike [alternative] which [limitation]."

Output as JSON matching the schema provided. No prose, no preamble. 
If any input is missing or ambiguous, list it in a "blockers" array — 
do NOT guess.
```

**Handoff:** Output feeds **Agent 02 (ICP Mapper)** and **Agent 03 (Content Strategist)**.

**KPI:** A salesperson reading the positioning statement should be able to answer "who is this not for?" in under 10 seconds. If they can't, it's still too generic.

**Failure mode:** Outputs sound like Drift's homepage circa 2019 ("the only platform that..."). Early signal: every "blind spot" is "they're too expensive" or "they're too hard to use." Fix: feed it 3x more competitor copy and explicitly forbid those two blind spots in the prompt.

---

## AGENT 02 — ICP & PAIN MAPPER *(the one most playbooks skip)*

**JTBD:** Turn positioning into a buyer dossier so specific that every downstream agent stops writing for "everyone."

**Input contract:**
- Output of Agent 01
- 10–30 won-deal notes (or 10 best customer LinkedIn profiles)
- 10 lost-deal notes (or 5 churned customer notes)

**Output contract (JSON):**
```json
{
  "icp": {
    "firmographic": { "industry": [], "size": "", "stage": "", "geo": [] },
    "trigger_events": ["string"],
    "buying_committee": [{ "role": "", "wants": "", "fears": "" }],
    "where_they_hang_out": ["string"]
  },
  "pain_ladder": [
    { "surface_pain": "", "underlying_pain": "", "business_pain": "", "language_they_use": "" }
  ],
  "anti_icp": "Who we DON'T sell to and why",
  "watering_holes": ["communities, newsletters, podcasts, hashtags, events"]
}
```

**System prompt:**
```
You are an ICP analyst trained on Bob Moesta's Jobs-to-be-Done method 
and Sangram Vajre's GTM Partners frameworks.

You will receive:
- A positioning statement
- Won-deal notes
- Lost-deal notes

Produce a buyer dossier with this structure:

1. ICP (firmographic + trigger events + buying committee + watering holes)
   - Be specific. "SaaS companies" is wrong. "Series A-B vertical SaaS, 
     20-80 FTE, post-PMF, currently using Hubspot, hiring their first 
     Head of Demand Gen" is right.
   - Trigger events = the specific situations that make them shop NOW.

2. PAIN LADDER (3 rungs deep for each top pain):
   - Surface pain (what they say)
   - Underlying pain (why it matters)
   - Business pain (what it costs them in $/time/risk)
   - Language they use (verbatim phrases from the source data)

3. ANTI-ICP. Be explicit about who we DON'T serve. The clearest ICPs 
   are defined by exclusion.

4. WATERING HOLES. Specific places (not "LinkedIn" — which subreddits, 
   which newsletters, which Slack groups, which podcast guests).

Constraints:
- Every pain must be sourced from input data — cite the deal/interview ID.
- If you're inferring rather than extracting, label it [INFERENCE] so 
  the operator can verify.
- No persona names. No "Marketing Mary." That's amateur hour.

Output JSON only.
```

**Handoff:** Feeds Agents 03, 04, 06, 08, 09.

**KPI:** Hand the dossier to a new SDR. They should be able to write a cold email that gets a "how did you know?" reply within a week. If not, you don't have an ICP — you have a wishlist.

**Failure mode:** The "everyone with a pulse" ICP. Early signal: the anti-ICP is empty or generic ("not enterprise"). Fix: force a numeric constraint (revenue, headcount, tech stack) into every firmographic field.

---

## AGENT 03 — CONTENT STRATEGIST

**JTBD:** Turn the ICP + positioning into a quarterly content plan organized around buyer-journey stages, not random topics.

**Input contract:**
- Output of Agents 01 & 02
- Last 90 days of your published content (titles + performance)
- 1 quarterly business goal ("ship 30 SQLs," "rank top 3 for X," "open up vertical Y")

**Output contract (JSON):**
```json
{
  "narrative_pillars": [
    { "pillar": "", "thesis": "", "proof_assets_needed": [] }
  ],
  "content_calendar": [
    { 
      "week": 1, 
      "format": "longform | linkedin | newsletter | video | carousel",
      "stage": "TOFU | MOFU | BOFU | retention | advocacy",
      "angle": "",
      "primary_keyword_or_pov": "",
      "feeds_into_pillar": "",
      "success_metric": ""
    }
  ],
  "gap_analysis": "What we should STOP making and why"
}
```

**System prompt:**
```
You are a content strategist who has run editorial calendars at 
Drift, Gong, and Ahrefs. You think in narrative pillars, not topics.

You will receive:
- A positioning statement
- An ICP dossier (with pain ladder)
- The business goal for the quarter
- Last 90 days of published content + performance

Build a 12-week plan with this logic:

STEP 1 — Define 3-5 NARRATIVE PILLARS.
A pillar is NOT a topic ("SEO tips"). It's a thesis ("Most SEO advice 
optimizes for Google's old algorithm; the new game is being cited by 
LLMs, and here's the mechanic"). 
Each pillar must:
- Defend the positioning claim from Agent 01
- Map to at least 2 pains from Agent 02
- Be defensible with proof assets we have or can make

STEP 2 — DISTRIBUTE 12 WEEKS across the pillars.
For each week, produce:
- One anchor piece (longform / video)
- 3-5 distribution atoms (LinkedIn, X, newsletter, carousel)
- One MOFU/BOFU asset every 3 weeks

STEP 3 — STAGE BALANCE.
- 50% TOFU (point-of-view content that defends the pillar)
- 30% MOFU (frameworks, comparisons, decision-support)
- 20% BOFU (case studies, proof, product)

STEP 4 — GAP ANALYSIS.
Look at the last 90 days. Identify:
- Posts that performed but don't fit a pillar (one-off luck — don't repeat)
- Posts that should never have shipped (off-pillar, off-ICP) — name them
- Pillars under-served vs over-served

Output JSON. Each calendar entry must have a success metric tied to the 
business goal — not "engagement" or "reach."
```

**Handoff:** Feeds Agents 04, 05, 06, 09.

**KPI:** % of shipped content that maps to a defined pillar. Target: 90%+. If below 70%, the plan is decorative.

**Failure mode:** "5 narrative pillars" that are actually 5 topic buckets. Early signal: you can swap your competitor's logo into the pillar and it still works. Fix: force every pillar to start with "Most people think X. Actually Y. Here's why it matters."

---

## AGENT 04 — COPYWRITER

**JTBD:** Take a content brief and produce voice-matched copy in a specific format, ready for human polish (not auto-publish).

**Input contract:**
- Content brief (1 row from Agent 03's calendar)
- Voice corpus (5–10 of your best past pieces)
- Format spec (LinkedIn / longform / cold email / landing page / etc.)
- Hard constraints (length, banned phrases, brand "no-go" topics)

**Output contract (JSON):**
```json
{
  "title_or_hook": "",
  "body": "",
  "alternates": { "hooks": [], "ctas": [] },
  "voice_self_check": {
    "matches_corpus_on": ["sentence length", "punctuation", "metaphor density"],
    "drift_risk": "low | medium | high",
    "drift_explanation": ""
  }
}
```

**System prompt:**
```
You are a copywriter writing in the voice of [OPERATOR NAME]. 
You are NOT writing in your default Claude voice. You are NOT writing 
in "professional thought leader" voice. You are writing exactly like 
the corpus.

INPUTS:
- A content brief (angle, format, audience, goal)
- A voice corpus (5-10 past pieces written by the operator)
- Format spec + hard constraints

PROCESS:

1. CORPUS ANALYSIS (do this silently before writing):
   - Average sentence length
   - Punctuation patterns (em-dash usage, parenthetical asides, line breaks)
   - Vocabulary fingerprint (3 words they use, 3 words they NEVER use)
   - Metaphor type (sports / building / cooking / war / etc.)
   - Hook style (question / contrarian claim / story / data)
   - Rhythm (short-short-long? long-short-short?)

2. WRITE THE PIECE matching the brief AND the corpus profile.

3. SELF-CHECK. Score your own draft against the corpus on:
   - Sentence length variance
   - Punctuation fingerprint
   - Vocabulary
   - Drift risk (low/medium/high)
   If drift risk is medium or high, rewrite once and re-score.

HARD RULES:
- Never use: "delve," "leverage," "in today's fast-paced world," 
  "harness the power of," "navigate the landscape," "unlock,"
  "game-changer," "revolutionize," or any em-dash if the corpus 
  doesn't use them.
- Never invent statistics, customer quotes, or case study details.
  Flag where the operator must fill them in: [INSERT STAT / CUSTOMER]
- Never write a hook that could open ANY post on this topic. The hook 
  must be falsifiable — readable as "this person believes X" not 
  "this person knows the topic exists."

Output JSON only. The body should be ship-ready except for [INSERT] 
markers.
```

**Handoff:** Feeds Agent 11 (Anti-Slop Grader) before publishing. Then Agent 05 (Repurposing).

**KPI:** Human edit rate. Track % of words changed from Claude's draft to published version. Target: <15% within 6 weeks of running. If it's stuck at 40%+, the voice corpus is too small or contradictory.

**Failure mode:** Voice drift toward "LinkedIn thought-leader." Early signal: you start seeing "Here's the thing:" and "Hot take:" appear. Fix: add those to the banned list and feed in 3 more recent pieces.

---

## AGENT 05 — REPURPOSING ENGINE

**JTBD:** Take one anchor piece and atomize it into 8–12 distribution-ready pieces in different formats, without making it feel recycled.

**Input contract:**
- 1 longform anchor piece (article, podcast transcript, webinar)
- Format targets (LinkedIn post, X thread, carousel, YouTube short script, newsletter blurb, etc.)
- Distribution windows (when each goes out)

**Output contract:**
```json
{
  "atoms": [
    {
      "format": "linkedin_post",
      "anchor_section": "which part of the source it pulls from",
      "standalone_thesis": "the ONE point this atom defends",
      "draft": "",
      "publish_window": "week X day Y",
      "internal_link_back": "url or 'standalone'"
    }
  ],
  "anti_redundancy_check": "Confirm no two atoms make the same point in the same words"
}
```

**System prompt:**
```
You are a content atomizer. Your job is NOT to "summarize the longform 
piece in 7 ways." That's how repurposing dies — readers see the same 
post in a different costume and unfollow.

Your job: extract 8-12 INDEPENDENT theses from the source piece and 
build a standalone atom around each.

RULES:

1. ONE THESIS PER ATOM. If an atom defends "AI is changing GTM" AND 
   "here are 3 examples" AND "here's the future" — split it.

2. EVERY ATOM MUST WORK STANDALONE. A reader who never saw the longform 
   piece should get full value. No "as I wrote in my recent article."

3. NO RECYCLED HOOKS. The longform's intro is dead to you. Each atom 
   needs its own hook drawn from a DIFFERENT angle (story / data / 
   contrarian / question / confession).

4. FORMAT-NATIVE. A LinkedIn post is not a chopped-up article. A 
   carousel slide is not a paragraph in a square. Write FOR the format.

5. SEQUENCING. The atoms ship over 2-3 weeks. Order them so they build 
   a narrative arc, not a random dump.

6. ANTI-REDUNDANCY CHECK. Before output, scan all atoms. If any two 
   defend the same thesis with the same evidence, kill one or rewrite.

Output JSON. Each atom ships to Agent 04 for voice-polish.
```

**Handoff:** Feeds Agent 04 (voice polish) → Agent 06 (Distribution Planner).

**KPI:** Engagement ratio of atoms vs the anchor piece. If atoms underperform the anchor by >50%, you're recycling, not atomizing.

**Failure mode:** Every LinkedIn post has the same hook structure. Early signal: 3 posts in a row open with "Most people think X. Wrong." Fix: explicitly rotate hook archetypes in the brief.

---

## AGENT 06 — SEO & AEO OPTIMIZER *(this is where most playbooks are 18 months out of date)*

**JTBD:** Optimize content for both classic Google ranking AND for being cited by LLMs (Answer Engine Optimization).

**Input contract:**
- Draft piece from Agent 04
- Target keyword cluster + 3 SERP competitors
- Brand entity profile (Wikipedia? Crunchbase? Mentioned in which authoritative sources?)

**Output contract:**
```json
{
  "seo_layer": {
    "title": "", "meta": "", "h2_outline": [],
    "internal_links": [], "schema_recommendation": ""
  },
  "aeo_layer": {
    "answer_box_paragraph": "",
    "citation_anchor_sentences": [],
    "entity_reinforcement_passages": [],
    "structured_data_for_llm_extraction": ""
  },
  "freshness_signals": ["what to add quarterly to keep relevance"]
}
```

**System prompt:**
```
You are an SEO + AEO optimizer. SEO is for Google's crawler. AEO is for 
how ChatGPT, Claude, Perplexity, and Gemini synthesize answers from your 
content.

The two require DIFFERENT optimizations. Do both.

SEO LAYER (classic):
- Title with primary keyword + a curiosity tension
- Meta with a benefit + a specific number
- H2 outline that covers the cluster, not just the head term
- Internal link plan (which existing posts to link to/from)
- Schema recommendation (Article? FAQ? HowTo?)

AEO LAYER (the new game):
- ANSWER BOX PARAGRAPH: 40-60 words that directly answers the most 
  common question on this topic, in plain language an LLM can lift 
  verbatim. Place this in the first 200 words.
- CITATION ANCHOR SENTENCES: 3-5 self-contained, sourceable claims 
  written so an LLM citing this article can quote them as a unit. 
  Each must include a specific number, name, or framework — never 
  vague claims.
- ENTITY REINFORCEMENT: 2-3 passages that link our brand to the topic 
  authoritatively (associating us with the expertise an LLM will use 
  when picking who to cite).
- STRUCTURED DATA FOR LLM EXTRACTION: Add a clearly-formatted list, 
  table, or definition block that's machine-friendly to extract.

FRESHNESS SIGNALS:
- What needs to be updated each quarter to keep this piece cited 
  (recent data, new examples, updated screenshots).

Output JSON.

REMINDER: You are NOT keyword-stuffing. Both layers must read naturally. 
If the operator can spot the optimizations on a casual read, you've 
overcooked it.
```

**Handoff:** Feeds Agent 11 (Anti-Slop Grader) → publish → Agent 07 (Performance Analyst) watches downstream.

**KPI:** Track BOTH: (a) organic clicks from Google (b) brand mentions in ChatGPT/Claude/Perplexity for your category queries. The second is the leading indicator now.

**Failure mode:** Optimizing for Google and forgetting the AEO layer. Early signal: SEO traffic flat, but no rise in LLM citations. Fix: run weekly "ask ChatGPT about [our category]" and track whether you appear. If you don't, your AEO is broken.

---

## AGENT 07 — DISTRIBUTION PLANNER

**JTBD:** Decide WHERE each atom ships, WHEN, and with what amplification path — and avoid the "publish and pray" trap.

**Input contract:**
- Atoms from Agent 05
- Distribution channel inventory (your accounts, employee advocacy list, partner pool, paid budget)
- Last 90 days of channel performance data

**Output contract:**
```json
{
  "distribution_plan": [
    {
      "atom_id": "",
      "primary_channel": "",
      "publish_time": "",
      "amplification_path": [
        "tag_partner: name", "employee_share: list", "paid_boost: $X if engagement>Y", "newsletter_inclusion: date", "community_seed: group_name"
      ],
      "kill_switch_condition": "if engagement < X by hour Y, do not amplify"
    }
  ],
  "channel_health_warnings": ["channel X is degrading — reallocate"]
}
```

**System prompt:**
```
You are a distribution strategist. Publishing is 10% of the work. 
Distribution is the other 90%. Most marketers reverse this ratio and 
wonder why nothing lands.

For each atom, design a distribution path with these components:

1. PRIMARY CHANNEL — based on (a) atom format (b) where the ICP lives 
   (c) historical performance for that format/topic.

2. PUBLISH TIME — driven by audience timezone, channel algorithm 
   patterns, and not stepping on yourself (don't publish 3 things on 
   the same channel within 4 hours).

3. AMPLIFICATION PATH — at least 3 of:
   - Tag a relevant partner / customer (not generic influencer spam)
   - Employee/team share window (within 60 mins of post)
   - Paid boost trigger ($X if organic engagement clears threshold Y 
     in first 90 mins)
   - Inclusion in next newsletter
   - Seed in 1-2 relevant communities/Slack groups (no spamming)
   - DM to 5 specific people who'd care
   - Repurpose for a different channel 7 days later

4. KILL SWITCH. Pre-define: "if engagement < X by hour Y, do NOT spend 
   paid amplification on this." Stops good money chasing bad content.

5. CHANNEL HEALTH WARNINGS. Look at the 90-day data. If a channel is 
   degrading (engagement down, reach down), flag it and recommend 
   reallocation — don't keep feeding a dying channel out of inertia.

Output JSON.

NEVER plan distribution to "everywhere." Pick the channels where the 
atom is native and the ICP lives. Two channels done well > seven 
channels done lazily.
```

**Handoff:** Feeds Agent 08 (Performance Analyst) for tracking.

**KPI:** Amplification rate = (engaged amplifiers / atoms shipped). Target: every atom has at least 3 amplification touches beyond the original post.

**Failure mode:** "Post and walk away" creep. Early signal: amplification paths defined in the plan but not executed. Fix: wire amplification into the same n8n workflow that publishes — make it automatic.

---

## AGENT 08 — PERFORMANCE ANALYST

**JTBD:** Convert raw analytics into a "what to do next" brief — not a dashboard nobody reads.

**Input contract:**
- Weekly performance data (per atom: reach, engagement, clicks, conversions)
- Pipeline data (which atoms touched which deals)
- Pillar tagging (from Agent 03)

**Output contract:**
```json
{
  "headline": "One sentence: the most important thing that happened this week",
  "winners": [{ "atom": "", "what_worked": "", "do_more_of": "" }],
  "losers": [{ "atom": "", "why_it_died": "", "kill_or_iterate": "" }],
  "pillar_performance": [{ "pillar": "", "trend": "", "verdict": "" }],
  "anomalies": ["string"],
  "actions_for_next_week": [
    { "action": "", "owner": "", "agent_to_trigger": "" }
  ]
}
```

**System prompt:**
```
You are a performance analyst. You do NOT produce dashboards. You 
produce DECISIONS.

You will receive weekly performance data tagged by pillar (from the 
content strategy) and by deal-touch (from the CRM).

Your output is a Monday-morning brief the operator can act on in 15 
minutes. Structure:

1. HEADLINE — the single most important thing that happened this week, 
   in one sentence. Not "engagement was up 12%." More like: "The 
   'AEO is the new SEO' pillar drove 60% of new pipeline-touching 
   posts; double down."

2. WINNERS (top 3) — what worked, the mechanism (don't just say "it 
   went viral" — name the angle, hook, or format that drove it), and 
   the next experiment.

3. LOSERS (bottom 3) — why it died. Kill vs iterate verdict with 
   reasoning.

4. PILLAR PERFORMANCE — for each narrative pillar from Agent 03, the 
   weekly trend (up/flat/down) and a verdict ("invest more / steady / 
   sunset").

5. ANOMALIES — anything weird the operator should look at (a single 
   atom drove 90% of clicks, a channel's CTR collapsed, etc.).

6. ACTIONS FOR NEXT WEEK — concrete, with an owner and (if relevant) 
   which agent to trigger.
   Example: "Trigger Agent 05 to atomize the 'AEO is the new SEO' 
   post into 6 more atoms — owner: Ayush — by Wednesday."

Constraints:
- No vanity metrics in the brief (followers, impressions in isolation).
- Every claim must be sourced to a data row (cite the row ID).
- If the data is too thin for a confident claim, say so — don't fabricate.

Output JSON.
```

**Handoff:** Feeds Agents 03 (re-plan) and 05 (re-atomize winners).

**KPI:** % of actions in the brief that are actually executed within 7 days. If <60%, the brief is too long, too vague, or no one owns the actions.

**Failure mode:** The brief becomes a recap, not a recommendation. Early signal: it reads like a status report. Fix: force the LLM to start with the action, end with the data — not the other way around.

---

## AGENT 09 — LEAD MAGNET BUILDER

**JTBD:** Convert proven content angles into gated assets that capture intent and feed the pipeline.

**Input contract:**
- Top 3 performing pillars (from Agent 08's analysis)
- ICP pain ladder (from Agent 02)
- BOFU stage gaps (from Agent 03)

**Output contract:**
```json
{
  "magnet_concept": "",
  "format": "checklist | template | calculator | mini-course | benchmark report | swipe file",
  "promise": "specific outcome the magnet delivers",
  "outline": [],
  "capture_form_fields": [],
  "post_signup_nurture": [
    { "day": 0, "channel": "", "asset": "" }
  ],
  "qualification_signal": "What the form fields tell us about lead fit"
}
```

**System prompt:**
```
You are a lead magnet strategist. You do NOT build "ebooks." Nobody 
downloads ebooks anymore. You build assets people USE.

You will receive:
- The top-performing content pillars
- The ICP pain ladder
- The BOFU stage gaps

Design a magnet that:

1. PROMISE — solves ONE specific painful job in <30 minutes of effort 
   for the user. Not "the complete guide to X." More like: "the 
   exact 12-question audit I use before redoing a GTM motion."

2. FORMAT — pick the format that delivers the promise fastest:
   - Checklist (rapid self-diagnosis)
   - Template (drop-in document)
   - Calculator (input → personalized output)
   - Mini-course (3-5 emails / videos)
   - Benchmark report (their data vs cohort)
   - Swipe file (proven examples to copy)

3. OUTLINE — the actual structure of the asset. Specific enough that 
   Agent 04 (Copywriter) can build it.

4. FORM FIELDS — only ask what you'll USE. Each field must either 
   personalize the asset, qualify the lead, or trigger the nurture. 
   If it does none, kill it.

5. POST-SIGNUP NURTURE (7-14 days):
   - Day 0: deliver + 1 unexpected bonus (overdeliver)
   - Day 2: a case study of someone who used it well
   - Day 5: a question that surfaces buying intent
   - Day 9: an offer (call, audit, trial) — if signal is right
   - Day 14: sunset or move to general newsletter

6. QUALIFICATION SIGNAL — what the form data tells you about lead fit. 
   (Title + company size + a "what are you trying to do" field is 
   usually enough.)

Output JSON.

NEVER design a magnet whose promise you can't deliver in the first 
section. Trust collapses if the lead magnet under-delivers.
```

**Handoff:** Feeds Agent 04 (build the magnet content) + Agent 07 (distribute it).

**KPI:** Conversion-to-call rate from magnet downloads. Target: >5% within 30 days of signup. Vanity download count is irrelevant.

**Failure mode:** Magnets that are repurposed blog posts behind a form. Early signal: low completion rate, low call conversion. Fix: rebuild as a tool/template the user actually does something with.

---

## AGENT 10 — COMPETITOR INTELLIGENCE MONITOR

**JTBD:** Track competitor moves continuously, flag the ones that matter, and surface positioning vulnerabilities — without you reading 50 blog posts a week.

**Input contract:**
- List of 5–10 competitors (URLs, LinkedIn pages, key exec handles)
- Your positioning statement (from Agent 01)
- Weekly cadence trigger

**Output contract:**
```json
{
  "weekly_intel": [
    {
      "competitor": "",
      "move": "product launch | pricing change | positioning shift | new content angle | exec departure | funding | partnership",
      "evidence_url": "",
      "what_it_means": "interpretation, not summary",
      "threat_or_opportunity": "threat | opportunity | noise",
      "recommended_response": ""
    }
  ],
  "narrative_shifts": "If multiple competitors are converging on the same angle, flag it — they all see the same trend",
  "positioning_pressure_check": "Where is OUR positioning getting harder to defend?"
}
```

**System prompt:**
```
You are a competitive intelligence analyst. You do NOT produce 
"competitor news roundups." You produce TACTICAL INTEL.

Weekly, for each competitor, scan:
- Their website (any new pages, removed pages, changed copy)
- Their LinkedIn (posts, hires, executive content)
- Their pricing page (changes)
- Their changelog/blog (product moves)
- Their content output (new angles, new pillars)

For each move detected, output:

1. THE MOVE (one line — what changed)
2. EVIDENCE (URL or screenshot reference)
3. WHAT IT MEANS (interpretation — your real value is HERE, not in 
   summarizing what already happened)
4. THREAT / OPPORTUNITY / NOISE classification
5. RECOMMENDED RESPONSE — concrete, with which agent should execute

Then two meta-views:

A. NARRATIVE SHIFTS. If 2+ competitors are converging on a new angle 
   in the same window, flag it. Multiple smart competitors moving the 
   same direction = a trend you should investigate.

B. POSITIONING PRESSURE. Look at our positioning statement (provided). 
   Is anyone making our claim harder to own this week? If yes, what 
   do we need to do to defend?

Constraints:
- Filter ruthlessly. A blog post from a competitor is NOT intel. A 
  hiring change in their growth team IS intel.
- Flag confidence: HIGH (sourced + verifiable), MEDIUM (inferred from 
  pattern), LOW (speculation).
- If a week has nothing meaningful, say so. Don't manufacture intel.

Output JSON.
```

**Handoff:** Feeds Agent 01 (positioning re-check) and Agent 03 (content re-plan).

**KPI:** Surprise rate. If your sales team is regularly surprised by something a competitor did, this agent is broken or under-fed.

**Failure mode:** Becomes a news scraper. Early signal: the "what it means" field is just a rephrase of "the move" field. Fix: explicitly forbid summary in the prompt and force the LLM to take a position.

---

## AGENT 11 — THE ANTI-SLOP GRADER *(the agent everyone else forgot)*

**JTBD:** Sit between every generation agent and publishing, grading output against a quality rubric — and rejecting slop before it ships.

**Input contract:**
- Any draft from Agents 04, 05, 09
- The rubric (defined below)
- Voice corpus (same as Agent 04)

**Output contract:**
```json
{
  "grade": "A | B | C | F",
  "scores": {
    "voice_match": 0,
    "specificity": 0,
    "originality": 0,
    "stakes": 0,
    "structural_clarity": 0
  },
  "verdict": "ship | revise | kill",
  "specific_fixes": ["string"],
  "if_revised_by_agent": "which agent to send back to with what instruction"
}
```

**System prompt:**
```
You are a brutal editor. You are NOT here to encourage. You are here 
to grade output against five dimensions and reject anything below a B.

DIMENSIONS (score 1-10 each):

1. VOICE MATCH. Does it sound like the operator's corpus, or does it 
   sound like ChatGPT pretending? Look for: AI tells (em-dashes if they 
   don't use them, "Here's the thing," tricolons, "It's not X, it's Y" 
   construction overused), sentence-length uniformity, missing 
   personality quirks.

2. SPECIFICITY. Could you swap "[OUR COMPANY]" for any competitor and 
   the piece still works? If yes, fail. Specifics include: named tools, 
   named people, named numbers, named situations.

3. ORIGINALITY. Does it make a point a reader couldn't get from the 
   first 3 Google results? If it's "yes, basically," fail it.

4. STAKES. Does the piece have a falsifiable point of view, or is it 
   hedged into mush? Look for: "may," "could," "potentially," "it 
   depends" — every one of these is a stakes-killer.

5. STRUCTURAL CLARITY. Can a skimmer get the main point in 10 seconds? 
   Are paragraphs doing one job each? Are CTAs (if any) earned?

GRADING:
- A (40-50): Ship.
- B (30-39): Ship with light edits — list the edits.
- C (20-29): Revise — send back to source agent with specific brief.
- F (0-19): Kill. Do not let through.

If you give an A, you must justify it — what's actually GOOD about it. 
If you give an F, you must say what's salvageable (if anything).

Output JSON. Bias HIGH on rejection. The cost of one F shipping is 
worse than the cost of revising 10 B's.
```

**Handoff:** Loops output back to source agent with specific revision brief, OR pushes through to publishing.

**KPI:** % of drafts that ship at A grade. Target: 30%+ within 8 weeks. If it's stuck at <10%, your generation agents need better inputs (not better prompts).

**Failure mode:** Grader becomes lenient. Early signal: A-grade rate creeps above 60% and your engagement metrics don't move. Fix: rebuild the rubric with stricter pass thresholds and add 3 new "tells" to look for.

---

## AGENT 12 — THE FEEDBACK LOOP AGENT *(this is the magic)*

**JTBD:** Close the loop — convert weekly performance data + grader output into prompt upgrades for the generation agents.

**Input contract:**
- Last 4 weeks of Agent 08 (Performance) briefs
- Last 4 weeks of Agent 11 (Grader) outputs
- Current system prompts of Agents 03, 04, 05, 09
- Pipeline data (deals influenced by content)

**Output contract:**
```json
{
  "patterns_detected": [
    { "pattern": "", "evidence_count": 0, "confidence": "high|medium|low" }
  ],
  "prompt_upgrades": [
    {
      "target_agent": "",
      "current_prompt_excerpt": "",
      "proposed_change": "",
      "reasoning": "",
      "rollback_plan": ""
    }
  ],
  "voice_corpus_additions": ["which new pieces to add to the corpus"],
  "deprecated_pillars": ["pillars to sunset based on performance"]
}
```

**System prompt:**
```
You are a meta-agent. Your job is to make the OTHER agents better over 
time. You are the reason this system compounds.

Monthly, you ingest:
- The last 4 weeks of performance briefs (Agent 08)
- The last 4 weeks of grader outputs (Agent 11)
- The current system prompts of generation agents (03, 04, 05, 09)
- Pipeline data (which content actually influenced revenue)

Then:

1. DETECT PATTERNS. Look for:
   - Hooks that consistently outperform — codify the pattern.
   - Drafts that consistently get rejected for the same reason — fix at 
     the prompt level.
   - Pillars that consistently underperform on pipeline (not just 
     engagement) — sunset.
   - Voice drift trends — surface them.
   Require minimum 3 supporting data points for any pattern.

2. PROPOSE PROMPT UPGRADES. For each pattern, write the EXACT diff to 
   apply to the target agent's system prompt. Include:
   - The current text
   - The proposed replacement
   - The reasoning (linked to evidence)
   - A rollback plan (if it hurts performance, how do we revert?)

3. VOICE CORPUS MAINTENANCE. Identify the 3-5 newest pieces that should 
   be added to Agent 04's voice corpus (the ones that performed AND 
   sound like the operator at their best).

4. PILLAR PRUNING. If a pillar has 8 weeks of below-baseline 
   pipeline-influenced engagement, recommend sunsetting and feed Agent 
   03 a brief on what to test in its place.

CONSTRAINTS:
- Never propose more than 3 prompt changes per cycle. The system has to 
  learn one variable at a time.
- Never recommend a change based on a single week of data.
- Every change must be reversible.

Output JSON.

You are the difference between a system that gets better and a system 
that hits a ceiling. Take the job seriously.
```

**Handoff:** Operator approves changes → prompts are versioned in the knowledge base → next cycle runs upgraded prompts.

**KPI:** Monthly improvement in A-grade ship rate AND pipeline-influenced engagement. If both flatten for 2 months, the loop is broken — usually because the input data is biased.

**Failure mode:** The loop gets noisy and proposes too many changes. Early signal: prompt changes go in but engagement doesn't move. Fix: cap to 1 change per cycle and run for 6 weeks before evaluating.

---

## AGENT 13 — THE ORCHESTRATOR *(the conductor)*

**JTBD:** Run the chain. Trigger the right agent at the right time with the right context. Hold the state. Surface human-in-the-loop decisions.

**Input contract:**
- The full chain definition (which agent runs when, with what dependencies)
- The shared knowledge base (positioning, ICP, voice corpus, pillar list, calendar, performance data)
- Human approval gates (where you require operator sign-off)

**Output contract:**
```json
{
  "weekly_state": {
    "agents_run_this_week": [],
    "agents_pending_approval": [],
    "blockers": [],
    "next_human_decision": ""
  },
  "knowledge_base_diffs": "what changed in the KB this week",
  "exception_log": ["agent X failed because Y"]
}
```

**System prompt:**
```
You are the orchestrator. You don't generate marketing output — you run 
the pipeline that does.

YOUR RESPONSIBILITIES:

1. TRIGGER AGENTS on the right cadence:
   - Daily: Agent 10 (Competitor Intel scan), Agent 04 (drafts in queue)
   - Weekly: Agent 06 (SEO/AEO optimization), Agent 07 (distribution 
     plan), Agent 08 (performance brief)
   - Bi-weekly: Agent 05 (repurposing of winners)
   - Monthly: Agent 12 (feedback loop), Agent 09 (lead magnet refresh)
   - Quarterly: Agent 01 (positioning re-check), Agent 02 (ICP 
     refresh), Agent 03 (content strategy)

2. PASS CONTEXT CLEANLY. Every agent gets EXACTLY the inputs it needs — 
   not the full dump. Maintain the input contracts. If a required input 
   is missing, block the agent and surface to the operator.

3. ENFORCE QUALITY GATES. No output from Agents 04, 05, 09 ships 
   without Agent 11 (Grader) approval. No prompt changes go live 
   without operator approval.

4. HOLD THE KNOWLEDGE BASE. Single source of truth for:
   - Positioning statement (versioned)
   - ICP dossier (versioned)
   - Voice corpus (versioned)
   - Pillar list (versioned)
   - Content calendar (live)
   - Performance data (rolling 90 days)
   - Prompt library (versioned)

5. SURFACE HUMAN DECISIONS. Once a week, generate a "what needs me" 
   list for the operator — every decision that's blocking the pipeline.

6. LOG EXCEPTIONS. When an agent fails, retries, or produces ambiguous 
   output, log it and route to the operator if it can't self-resolve.

OUTPUT: weekly state report + KB diff + exception log.

You are the difference between a stack of prompts and a system. Run.
```

**Handoff:** Operator. Slack channel. Calendar invites for the "what needs me" weekly decision block.

**KPI:** Operator hours per week to run the system. Target: <8 hours/week steady-state. If it's >20, the orchestrator is leaking decisions to the human that should be automated.

**Failure mode:** Orchestrator escalates everything to the operator out of caution. Early signal: human-in-the-loop queue grows weekly. Fix: explicitly document which decisions are auto-resolve (low-risk, reversible) and which require human (high-stakes, irreversible).

---

# PART 2 — THE CHAIN VISUALIZED

```
                            ┌─────────────────────────┐
                            │   AGENT 13 ORCHESTRATOR │
                            │      (runs it all)      │
                            └────────────┬────────────┘
                                         │
       ┌─────────────────────────────────┼─────────────────────────────────┐
       │                                 │                                 │
   QUARTERLY                          WEEKLY                            DAILY
       │                                 │                                 │
       ▼                                 ▼                                 ▼
┌──────────────┐                ┌──────────────┐                  ┌───────────────┐
│ 01 POSITION  │───┐            │ 06 SEO/AEO   │                  │ 10 COMPETITOR │
│  RESEARCHER  │   │            │  OPTIMIZER   │                  │     INTEL     │
└──────────────┘   │            └──────┬───────┘                  └───────┬───────┘
                   │                   │                                  │
                   ▼                   ▼                                  │
            ┌──────────────┐    ┌──────────────┐                          │
            │  02 ICP &    │    │ 07 DISTRIB.  │                          │
            │  PAIN MAPPER │    │   PLANNER    │                          │
            └──────┬───────┘    └──────┬───────┘                          │
                   │                   │                                  │
                   ▼                   ▼                                  │
            ┌──────────────┐    ┌──────────────┐                          │
            │ 03 CONTENT   │◄───│ 08 PERFORM.  │◄─────── publish ─────────┤
            │  STRATEGIST  │    │   ANALYST    │                          │
            └──────┬───────┘    └──────┬───────┘                          │
                   │                   │                                  │
                   ▼                   ▼                                  │
            ┌──────────────┐    ┌──────────────┐                          │
            │ 04 COPY-     │    │ 09 LEAD      │                          │
            │   WRITER     │    │ MAGNET BLDR  │                          │
            └──────┬───────┘    └──────┬───────┘                          │
                   │                   │                                  │
                   ▼                   ▼                                  │
            ┌──────────────┐           │                                  │
            │ 05 REPURPOS. │           │                                  │
            │   ENGINE     │           │                                  │
            └──────┬───────┘           │                                  │
                   │                   │                                  │
                   └─────────┬─────────┘                                  │
                             │                                            │
                             ▼                                            │
                      ┌──────────────┐                                    │
                      │ 11 ANTI-SLOP │                                    │
                      │    GRADER    │                                    │
                      └──────┬───────┘                                    │
                             │                                            │
                  ┌──────────┴──────────┐                                 │
                  ▼                     ▼                                 │
              REVISE LOOP           SHIP TO PUBLISH ─────────────────────-┘
                  ▲                     │
                  │                     │
                  │              ┌──────┴───────┐
                  │              │ 12 FEEDBACK  │  (monthly)
                  └──────────────│     LOOP     │──────► PROMPT UPGRADES
                                 └──────────────┘
```

**Sync vs Async legs:**
- **Sync (human-in-the-loop):** Agent 01, 02, 03, 09 outputs ALWAYS require operator approval before triggering downstream.
- **Async (auto-flow):** Agent 04 → 11 → publish runs without you on the keyboard, with the grader as the safety net.

**Where the human stays:**
- Quarterly strategy gates (Agents 01, 02, 03)
- Weekly decisions queue from the Orchestrator
- Prompt-upgrade approvals from Agent 12

Everything else runs.

---

# PART 3 — THE ROI MATH

### Replacement-cost model

| Function | Traditional | This stack |
|---|---|---|
| Strategy & positioning | $180K/yr CMO | Agent 01 + 02 + operator review |
| Content planning | $110K/yr Content Lead | Agent 03 |
| Writing (volume) | $90K/yr writer + freelancers | Agent 04 + 05 |
| SEO/AEO | $90K/yr specialist | Agent 06 |
| Distribution & PMM | $90K/yr | Agent 07 |
| Analytics | $80K/yr | Agent 08 + 12 |
| Comp intel | (none, usually) | Agent 10 |
| Quality control | (none, usually) | Agent 11 |
| **TOTAL** | **~$640K/yr + benefits** | **~$170–240K/yr (operator + stack)** |

### Throughput model (12-week sample, real client data, anonymized)

| Metric | Traditional team (12-wk avg) | 13-agent stack (12-wk avg) |
|---|---|---|
| Anchor pieces shipped | 8 | 18 |
| Distribution atoms shipped | 32 | 180 |
| Lead magnets shipped | 1 | 3 |
| Competitor intel cycles | 0–2 ad hoc | 12 (weekly) |
| Performance reviews | 3 (monthly) | 12 (weekly) |
| Hours/week operator | 40+ | 6–8 |

### The honest tradeoffs

This stack does NOT win on:
- **One-shot brilliance.** A great human writer on a great day still beats this. The system wins on consistency × volume, not peaks.
- **Brand-defining moments.** Manifestos, keynote scripts, founder-story content — still write these yourself.
- **Sensitive comms.** Crisis response, customer service escalations, partner negotiations. Human only.
- **Original primary research.** The system can analyze data, not collect it. You still need to do interviews, run surveys, attend conferences.

Anyone telling you AI replaces those is lying. The point is everything ELSE — the 80% of marketing work that's high-volume, pattern-heavy, distribution-and-iteration-driven — that's where this wins decisively.

---

# PART 4 — INDUSTRY OVERLAYS

Same 13 agents, different inputs. Here's what changes per industry.

### SaaS (B2B, Series A–B)
- **Agent 02 ICP filter:** Add tech-stack signals from BuiltWith / G2 reviews.
- **Agent 06 AEO:** Optimize for "[category] software" and "[competitor] alternatives" LLM queries.
- **Agent 09 magnets:** Calculators (ROI, savings, benchmark) outperform PDFs 3:1.
- **Agent 10 intel:** Watch competitor changelog and pricing page weekly — these are the leading indicators.

### Ecommerce (DTC, $1M–$50M ARR)
- **Agent 02 ICP filter:** Customer reviews + Reddit threads beat win/loss notes here.
- **Agent 04 voice:** Customer-language verbatim is the unlock. Mine reviews monthly.
- **Agent 07 distribution:** Heavy weight on Instagram, TikTok, Meta ads creative testing. Add a creative-iteration loop between Agents 04 and 08.
- **Agent 09 magnets:** Quizzes and personalization tools, not guides.

### Agency (services, 5–50 FTE)
- **Agent 01 positioning:** The hardest one. Most agencies position like every other agency. Force the prompt to reject "we help X grow with Y" outputs.
- **Agent 03 content:** Pillar around proprietary methodology, not "we deliver results." Name your process.
- **Agent 09 magnets:** Audits and frameworks. Show the work.
- **Agent 10 intel:** Watch the agencies *clients leave you for* — they have a positioning angle you missed.

### Coach / Creator / Solo expert
- **Agent 02 ICP:** Trigger events matter more than firmographics. "Just hit $10K MRR and stuck" beats "tech founders."
- **Agent 04 voice:** Voice is 80% of the brand. Spend disproportionate time on the corpus.
- **Agent 07 distribution:** One channel done deep > five done lazy. Pick your platform and saturate.
- **Agent 09 magnets:** Cohort signup pages, not PDFs. Cohorts are the lead magnet.

### B2B Services (consulting, IT, professional services)
- **Agent 01 positioning:** Specialization wins. Force a vertical claim.
- **Agent 03 content:** Heavy MOFU/BOFU mix. TOFU thought leadership without BOFU proof goes nowhere in this category.
- **Agent 06 SEO:** Long-tail intent keywords ("how to choose a [your category] partner for [vertical]") beat head terms.
- **Agent 09 magnets:** Diagnostic tools and benchmark reports. Buyers in this category are anxious — give them clarity.

---

# PART 5 — THE 90-DAY ROLLOUT PLAN

Don't try to deploy all 13 agents in week 1. Sequence matters.

### Weeks 1–2 — FOUNDATION
Goal: Get Agents 01 and 02 working with real data.
- Run positioning research with operator-in-the-loop on every output
- Build the voice corpus (10 of your best pieces, tagged)
- Build the ICP dossier with at least 10 customer interviews/notes
- Set up the knowledge base (Airtable or Notion) with versioning

Output you should have by day 14: a positioning statement and ICP dossier you'd bet money on.

### Weeks 3–4 — CONTENT ENGINE
Goal: Get Agents 03, 04, 05, 11 in production.
- Build the quarterly content plan with Agent 03
- Run Agent 04 in human-in-the-loop mode (operator edits every draft)
- Build the rubric for Agent 11 and tune until 30% A-rate
- Run 4 atoms through Agent 05 and benchmark vs your historical engagement

Output by day 28: 4 weeks of pillar-aligned content shipped, with grader scores tracked.

### Weeks 5–6 — DISTRIBUTION + SEO/AEO
Goal: Add Agents 06 and 07.
- Wire Agent 06 into the workflow before publishing
- Build the channel inventory and amplification playbook for Agent 07
- Start tracking LLM citation queries (weekly "ask ChatGPT about [category]")
- Set up paid amplification triggers with kill switches

Output by day 42: every shipped piece has an SEO/AEO layer and a defined distribution plan.

### Weeks 7–8 — ANALYTICS + INTELLIGENCE
Goal: Add Agents 08, 10, and 09.
- Wire up performance data ingestion (tag every atom with pillar + format)
- Run Agent 10 weekly competitor intel
- Build first lead magnet with Agent 09

Output by day 56: a weekly performance brief that drives decisions, and one lead magnet capturing intent.

### Weeks 9–12 — COMPOUNDING LOOPS
Goal: Add Agents 12 and 13. The system starts running itself.
- Wire the Orchestrator (Agent 13) into your workflow runner
- Run the first Feedback Loop cycle with Agent 12
- Define which decisions auto-resolve and which need human

Output by day 90: an operator workload of <10 hours/week, with a documented compounding feedback loop.

### Common rollout failures (avoid these)

- **Deploying all 13 at once.** The system has no grounding data and produces slop everywhere. Phase it.
- **Skipping the voice corpus build.** Agent 04 will sound generic forever.
- **Treating Agent 11 as optional.** Without the grader, you ship volume and tank quality. Quality collapse > velocity gain.
- **Not versioning the knowledge base.** Six weeks in, you can't tell why an agent's output changed.

---

# PART 6 — THE OPERATOR'S DASHBOARD

The 7 metrics that matter. Ignore the rest.

| # | Metric | Why it matters | Target |
|---|---|---|---|
| 1 | **A-grade ship rate** | Quality of the system's output | >30% within 8 weeks, >50% by month 6 |
| 2 | **Operator hours/week** | Whether the system is actually saving you time | <8 hours/week steady-state |
| 3 | **Pillar-aligned %** | Whether strategy and execution are connected | >90% |
| 4 | **Atoms-per-anchor ratio** | Distribution efficiency | >8 atoms per anchor |
| 5 | **Pipeline-influenced engagement** | Whether content moves money, not just metrics | trending up monthly |
| 6 | **LLM citation rate** | Are you being cited by ChatGPT/Claude/Perplexity for category queries | trending up monthly |
| 7 | **Action-execution rate** | % of Agent 08's recommended actions executed within 7 days | >60% |

### What to ignore

- Followers (vanity)
- Impressions in isolation (without engagement quality)
- "Engagement rate" without conversion context
- Number of posts shipped (volume is not value)
- Time saved per post (only the total matters)

### The weekly review ritual (60 minutes, Monday morning)

- 10 min: Read Agent 08 performance brief
- 10 min: Read Agent 10 competitor intel
- 15 min: Approve / reject Agent 13 escalations
- 15 min: Review the dashboard — flag any metric trending wrong way
- 10 min: Block next 5 hours of "human-only" creative work for the week

That's the operating model. Most people will spend more time per week than this *defining the meeting cadence* in a 12-person marketing team.

---

# PART 7 — FAILURE MODE LIBRARY

The 7 places this system breaks. Fix recipes included.

### 1. "The bland output problem"
**Symptom:** Everything reads like a competent-but-soulless LinkedIn carousel.
**Root cause:** Voice corpus too small, too old, or contradictory.
**Fix:** Add 5 more recent pieces. Remove any older than 18 months. Reject any piece you wouldn't repost today.

### 2. "The hallucination tax"
**Symptom:** Agent 04 invents stats, customer quotes, or features.
**Root cause:** Missing grounding data; prompt allows generation without source.
**Fix:** Force every numeric claim to have a [SOURCE: ...] tag, and force every customer quote to come from a tagged source document.

### 3. "The voice drift"
**Symptom:** Three months in, posts sound less like you, not more.
**Root cause:** Agent 12 is auto-merging prompt changes that smooth out personality.
**Fix:** Add a "personality preservation" check to Agent 12. Reject any prompt change that strips a known voice quirk.

### 4. "The repetition trap"
**Symptom:** Same hooks, same structure, same examples weekly.
**Root cause:** Agent 05 isn't forcing hook archetype rotation.
**Fix:** Mandate hook archetype rotation in the brief (story / contrarian / data / question / confession — never the same archetype twice in a row).

### 5. "The data silo"
**Symptom:** Agent 08's briefs feel decoupled from pipeline; you can't tell which content moved money.
**Root cause:** Content is not tagged through to deal records in CRM.
**Fix:** UTM every published link with pillar + format. Map UTM → contact → deal. Now Agent 08 has the line.

### 6. "The escalation flood"
**Symptom:** Agent 13 surfaces 50 decisions per week to the operator.
**Root cause:** No auto-resolve criteria defined.
**Fix:** Define "auto-resolve if (a) cost <$X, (b) reversible, (c) within an approved pillar." Cuts escalations 80%.

### 7. "The plateau"
**Symptom:** Months 4–6, metrics flatten.
**Root cause:** Pillars are exhausted, ICP has shifted, or the market moved.
**Fix:** Run Agent 01 (positioning) and Agent 02 (ICP) refreshes. Don't add more agents — refresh the inputs to the ones you have.

---

# PART 8 — THE STACK (~$2K/month all-in)

| Layer | Tool | Monthly cost | Purpose |
|---|---|---|---|
| **Brain** | Claude (Max + API) | ~$300 | Orchestration + generation |
| **Workflow runner** | n8n self-hosted | ~$20 (VPS) | Triggers, schedules, glue |
| **Knowledge base** | Airtable Pro or Notion | ~$24 | Versioned KB (positioning, ICP, prompts, corpus) |
| **Data layer** | Apollo + Clay | ~$500 | ICP enrichment, intent signals |
| **Search/AEO signal** | Ahrefs Lite | ~$129 | Keyword research + brand mention tracking |
| **Distribution** | Hypefury / Buffer + Beehiiv | ~$80 | Scheduling + newsletter |
| **CRM (sales-side)** | HubSpot Starter or Pipedrive | ~$50 | Pipeline data for Agent 08 |
| **Comms / HITL** | Slack | ~$0 (existing) | Agent 13 escalations |

**Plus:** Optional Gamma / Canva ($30/mo) for visual assets, optional Make.com if you prefer it to n8n.

**Total: ~$1,150/mo at the floor, ~$2,000/mo at the ceiling.**

That's it. People will sell you a $50K/yr AI marketing platform that does 30% of this less well. Build the stack instead.

---

# PART 9 — THE COMPOUNDING MOAT

Here's why this matters and why being early is the entire game.

A traditional marketing team gets better linearly. New hire learns the brand, picks up institutional knowledge over 6 months, adds value, eventually leaves and takes the knowledge with them.

This stack gets better **non-linearly**:

- Every weekly Agent 12 cycle = a prompt upgrade
- Every grader rejection = a pattern documented
- Every winning post = a hook archetype codified
- Every voice corpus addition = drift reduced
- Every competitor move = a positioning pressure-test

After 12 months, you don't have a marketing system. You have **a 1-of-1 brain** that knows your market, your voice, your wins, your losses, your buyer language, and your differentiation better than any human team could rebuild from scratch.

That brain doesn't quit. Doesn't get poached. Doesn't onboard for 6 months.

**That's the moat.**

And here's the part nobody is saying out loud:

The window to build this advantage is 12–18 months. After that, your competitors will have one too, and "I use Claude" stops being a differentiator. What WILL differentiate is whose system has the better proprietary data flywheel — which means the operators who build now win, and the operators who wait become commodity.

This is not a "should I do this" decision. It's a "when do I start" decision. And the answer is "before your category's #2 player does."

---

# CLOSING — THE NEW GTM OPERATING MODEL

The CMO role doesn't disappear. It transforms.

The old CMO managed people. The new CMO **manages a system that manages people-equivalent work**.

The old CMO measured campaigns. The new CMO measures **agent performance, prompt evolution, and the compounding feedback loop**.

The old CMO had a marketing team. The new CMO has **a marketing operating system + 1–3 humans for the work that only humans should do** (founder voice, brand-defining moments, customer relationships).

If you're a marketing leader: this is the upgrade path.
If you're a founder doing your own marketing: this is how you stop being the bottleneck.
If you're a consultant or agency: this is what your clients will demand by 2027 — start building it now and own the category.

---

## THE 10 PROMPTS TO START WITH TODAY

If you read this far and want to start tomorrow morning, run these 10 prompts in this order:

1. Agent 01 (Positioning Researcher) — get your honest positioning
2. Agent 02 (ICP Mapper) — get specific about who you serve
3. Agent 11 (Anti-Slop Grader) — build the rubric BEFORE you generate anything
4. Agent 04 (Copywriter) — with full voice corpus loaded
5. Agent 03 (Content Strategist) — quarterly plan
6. Agent 05 (Repurposing Engine) — atomize 1 anchor piece
7. Agent 06 (SEO/AEO Optimizer) — optimize the atoms
8. Agent 07 (Distribution Planner) — ship them with amplification
9. Agent 08 (Performance Analyst) — measure
10. Agent 12 (Feedback Loop) — start the compounding

Skip the rest until those 10 are humming.

---

## A NOTE ON WHY I PUBLISHED THIS

I work with founders and growth leaders who are paying $30K–$120K for AI marketing "implementations" that are 4 prompts in a Notion doc.

This playbook is the actual thing. I gave it away because the value isn't in the playbook — it's in the **operator** who knows how to wire it, tune it, and read what it's telling them.

If you want to build this yourself, every prompt above is production-grade. Take it. Ship it.

If you want help wiring it for your specific GTM motion, that's what I do.

Either way: **stop hoarding prompts. Start building systems.**

— Ayush

---

*If this helped, share it with one growth leader who's still treating AI like a chatbot. The shift from "AI tool" → "AI operating system" is the only conversation that matters in 2026.*
