# Infrastructure, replication, AI model costs and legal/security requirements for AI assistant agents for Vietnamese SMEs (as of 14 Oct 2026)

Conventions used throughout:
- **Exchange rate: 26,000 VND/USD.** Vietcombank quoted USD at 25,690 (transfer buy) / 26,070 (sell) on 10–11 Oct 2026 ([webgia.com – Vietcombank 11/10/2026](https://webgia.com/ty-gia/vietcombank/11-10-2026.html)). Paying foreign APIs by card is billed near the sell rate, so 26,000 is a slightly optimistic round number.
- "MTok" = 1 million tokens. Prices exclude VAT unless stated.
- **Verification status:** Anthropic, Google Gemini, DeepSeek, OpenAI, Hetzner, Zalo OA/ZBS, SePay, FPT AI Factory and Vietnix prices come from their **own pricing pages, fetched 14 Oct 2026**. Other prices come from aggregators and are flagged.
- Access note: during this session openai.com, ai.google.dev, deepseek.com and openrouter.ai were blocked by the network proxy. Official pages were read through a scraping service, and openrouter.ai could not be read directly.

---

## 1. LLM API pricing (Oct 2026), and which models handle Vietnamese well

### Takeaway
The cheapest capable tier costs about **$0.10 input / $0.50 output per MTok**: Claude Haiku 5.5 and OpenAI gpt-6-luna are priced identically, with cache reads at $0.01. The mid "strong" tier is about **$2 / $10**: Claude Sonnet 5.5 and gpt-6.1-sol. Prompt caching cuts repeated-prefix input by 90–95%. I found **no Vietnamese-native chat LLM sold by the token**. FPT AI Factory resells global/open models and adds a Vietnamese embedding model and Vietnamese TTS/STT.

### Cited Findings

**Anthropic Claude (official pricing page, cached 2026-10-06, fetched 14 Oct 2026)**
- Claude Haiku 5.5, prompts ≤100K tokens: input $0.10, 5-min cache write $0.125, 1-h cache write $0.20, cache read $0.01, output $0.50 per MTok. Prompts >100K tokens: $0.50 / $0.625 / $1 / $0.05 / $2.50 (5× higher) — [Anthropic pricing](https://platform.claude.com/docs/en/about-claude/pricing)
- Claude Sonnet 5.5: input $2, 5-min write $2.50, 1-h write $4, cache read $0.10 (0.05× input), output $10 per MTok — [Anthropic pricing](https://platform.claude.com/docs/en/about-claude/pricing). (Anthropic's SDK skill docs list the Sonnet 5.5 cache read at $0.20. The official page says $0.10. Use the official page.)
- Claude Opus 5.5: input $4, 5-min write $5, 1-h write $8, cache read $0.20, output $20 per MTok. Fast mode is $8 / $40 — [Anthropic pricing](https://platform.claude.com/docs/en/about-claude/pricing)
- The top tier (Claude Fable 5.1) costs $10 / $50 per MTok. It is not needed for SME chat — [Anthropic pricing](https://platform.claude.com/docs/en/about-claude/pricing)
- The Batch API gives 50% off both input and output (Haiku 5.5 $0.05/$0.25; Sonnet 5.5 $1/$5; Opus 5.5 $2/$10). Caching and batch discounts stack — [Anthropic pricing](https://platform.claude.com/docs/en/about-claude/pricing)
- Caching rules: a cache write costs 1.25× (5-min TTL) or 2× (1-h TTL) the input price. A cache hit costs 0.1× input in general, 0.05× on Opus/Sonnet 5.5. A 5-minute cache pays off after one read — [Anthropic pricing](https://platform.claude.com/docs/en/about-claude/pricing)
- Data residency: `inference_geo: "us"` costs 1.1× on all token categories. The default is "global". **There is no Vietnam or ASEAN residency option**, so data is always processed outside Vietnam — [Anthropic pricing](https://platform.claude.com/docs/en/about-claude/pricing)
- Tool use adds a hidden system prompt of about 286 tokens per request on Haiku/Sonnet/Opus 5.5 (406 tokens in a second Haiku 5.5 column) — [Anthropic pricing](https://platform.claude.com/docs/en/about-claude/pricing)
- Web search tool: $10 per 1,000 searches plus tokens — [Anthropic pricing](https://platform.claude.com/docs/en/about-claude/pricing)

**OpenAI (official developers pricing page, 14 Oct 2026)**
- gpt-6-luna (cheap tier), short context: input $0.10, cached input $0.01, output $0.50. Long context: $0.20 / $0.02 / $0.75 — [OpenAI API pricing](https://developers.openai.com/api/docs/pricing)
- gpt-6.1-sol (mid tier), short context: $2.00 / $0.10 / $10.00. Long context: $4.00 / $0.20 / $15.00 — [OpenAI API pricing](https://developers.openai.com/api/docs/pricing)
- gpt-6-astra (flagship): $10 / $1 / $50 (short context) — [OpenAI API pricing](https://developers.openai.com/api/docs/pricing)
- gpt-realtime-2.1-mini (voice, text modality) costs $0.60 / $0.06 / $2.40. Batch and Flex tiers are 50% of Standard for astra/sol/luna — [OpenAI API pricing](https://developers.openai.com/api/docs/pricing)
- Conflict: aggregators still quote the GPT-5.6 family (Sol $5/$30, Terra $2.50/$15, Luna $1/$6 at its July 2026 launch, with later cuts) — [BenchLM](https://benchlm.ai/openai/api-pricing); [morphllm](https://www.morphllm.com/openai-api-pricing). The official page now lists the GPT-6 family as flagship and keeps gpt-5.6-sol only under "Cyber models". FPT AI Factory still resells Azure gpt-5.6-luna at $0.20/$1.20 — [FPT AI Factory pricing](https://factory.fpt.ai/pricing)
- The page did not show embedding prices — [OpenAI API pricing](https://developers.openai.com/api/docs/pricing)

**Google Gemini (official Gemini Developer API pricing page, 14 Oct 2026)**
- Gemini 3.1 Flash-Lite: input $0.25, output $1.50, cache $0.025 per MTok, plus cache storage of $1.00/MTok/hour — [Gemini pricing](https://ai.google.dev/gemini-api/docs/pricing)
- Gemini 3.5 Flash-Lite: $0.30 / $2.50, cache $0.03 — [Gemini pricing](https://ai.google.dev/gemini-api/docs/pricing)
- Gemini 3.8 Flash and 3.6 Flash: **$0.75 / $3.75 until 31 Dec 2026, then $1.50 / $7.50 from 1 Jan 2027**. Cache $0.075, rising to $0.15 — [Gemini pricing](https://ai.google.dev/gemini-api/docs/pricing)
- Gemini 4 "Argon": $2 / $10 until 30 Nov 2026, then $4 / $20 from 1 Dec 2026. Implicit caching only — [Gemini pricing](https://ai.google.dev/gemini-api/docs/pricing)
- Gemini 3.1 Pro Preview: $2 / $12 for prompts ≤200K tokens, $4 / $18 above that — [Gemini pricing](https://ai.google.dev/gemini-api/docs/pricing)
- Older and cheapest: Gemini 2.5 Flash-Lite $0.10 / $0.40 (cache $0.01); Gemini 2.5 Flash $0.30 / $2.50 — [Gemini pricing](https://ai.google.dev/gemini-api/docs/pricing). One aggregator says Gemini 2.5 Flash is scheduled for deprecation on 16 Oct 2026 and 2.0 Flash shut down on 1 Jun 2026 — [CloudZero](https://www.cloudzero.com/blog/gemini-pricing/)
- Gemini Embedding 2: $0.20/MTok for text — [Gemini pricing](https://ai.google.dev/gemini-api/docs/pricing)
- Google Search grounding on Gemini 3+: 5,000 free requests/month, then $14 per 1,000 — [Gemini pricing](https://ai.google.dev/gemini-api/docs/pricing)

**DeepSeek (official pricing page, 14 Oct 2026)**
- `deepseek-flash` (DeepSeek-V4.1-Flash), 1M context, peak hours: cache hit $0.006, cache miss $0.30, output $1.20 per MTok. Off-peak: $0.003 / $0.15 / $0.60 — [DeepSeek pricing](https://api-docs.deepseek.com/quick_start/pricing)
- `deepseek-v4-pro` (V4-Pro-0813), peak: hit $0.044, miss $1.32, output $3.96. Off-peak is half: $0.022 / $0.66 / $1.98 — [DeepSeek pricing](https://api-docs.deepseek.com/quick_start/pricing)
- **Peak hours are 01:00–04:00 and 06:00–10:00 UTC, Mon–Fri**, which is 08:00–11:00 and 13:00–17:00 in Vietnam. The API supports OpenAI- and Anthropic-format endpoints and tool calls — [DeepSeek pricing](https://api-docs.deepseek.com/quick_start/pricing)
- Aggregators quoting the older "V4 Flash $0.14/$0.28" price are out of date. FPT AI Factory still lists DeepSeek-V4-Flash at $0.14 / $0.28 — [FPT AI Factory pricing](https://factory.fpt.ai/pricing); [CloudZero DeepSeek](https://cloudzero.com/blog/deepseek-pricing/)

**Qwen (Alibaba Model Studio, Singapore endpoint) — NOT verified on an official page**
- Snapshot of 1 Sep 2026: qwen3.8-max $2 / $6; qwen3.7-plus $0.40 / $1.60 ($1.20 / $4.80 above 256K); qwen3.8-flash $0.15 / $0.47; qwen3.7-flash $0.03 / $0.13 — [morphllm Qwen API](https://www.morphllm.com/qwen-api). FPT AI Factory's list partly cross-checks this: Alibaba:Qwen3.8-Max $2 / $6 and Qwen3.8-Omni-Flash $0.15 / $0.47 — [FPT AI Factory pricing](https://factory.fpt.ai/pricing)

**LLM routers**
- OpenRouter charges about 5.5% on credit purchases ($0.80 minimum by card) and adds no markup on inference — [TrueFoundry](https://www.truefoundry.com/blog/openrouter-pricing). The BYOK fee reportedly changed on 14 Jul 2026: the first $25,000/month of BYOK usage is free on Standard/Business plans, with 5% above that. Business-plan credits reportedly carry 8% — [TrueFoundry BYOK](https://www.truefoundry.com/blog/openrouter-byok-explained-cheaper-often-faster-and-changing). Not verified on openrouter.ai, which was blocked.

**Vietnamese / Vietnam-hosted options**
- FPT AI Factory serverless inference (pricing page dated 5 Oct 2026), per MTok:
  - **Vietnamese_Embedding $0.011**; multilingual-e5-large $0.022; bge-reranker-v2-m3 $0.022
  - gpt-oss-120b $0.143 / $0.605; Llama-3.3-70B $0.209 / $0.451
  - gemma-4-26B-A4B $0.14 / $0.40; GLM-5.3 Flash $0.15 / $0.50
  - DeepSeek-V4-Flash $0.14 / $0.28; Azure gpt-4.1-nano $0.10 / $0.40
  - FPT Vietnamese TTS (FPT.TTS-pro $16.50 per million characters) and FPT whisper STT
  - H100 GPU $2.99/hour (Southeast Asia)
  - Source: [FPT AI Factory pricing](https://factory.fpt.ai/pricing)
- Several FPT entries are labelled "Alibaba:" or "Azure:", meaning an upstream foreign provider serves them. A Vietnamese vendor contract does not guarantee in-country processing — [FPT AI Factory pricing](https://factory.fpt.ai/pricing)
- A product directory says FPT AI Factory gives 500,000 free inference tokens per LLM and 2M per vision model — [SourceForge](https://sourceforge.net/software/compare/FPT-AI-Factory-vs-ModelArk/)
- For Vietnamese quality, the main academic benchmark is VMLU (ACL 2025): general knowledge, reading comprehension, reasoning and conversation. The paper evaluated Llama-3, Qwen2.5 and GPT-4. No 2026 leaderboard covering Claude 5.5, GPT-6 or Gemini 3.x was found — [ACL Anthology VMLU](https://aclanthology.org/2025.acl-long.563)

### Inferences
- For routine Vietnamese customer service, **Claude Haiku 5.5 and gpt-6-luna are tied on price** ($0.10 / $0.50, cache $0.01). Gemini 2.5 Flash-Lite is the same price but on a deprecating generation. Gemini 3.1 Flash-Lite costs 2.5–3× more.
- DeepSeek's off-peak discount mostly misses Vietnamese business hours, so budget **peak rates** for daytime chat.
- Keep each request **under 100K tokens on Haiku 5.5**. Above that, every token costs 5× more, so trim memory and retrieval results.
- Gemini Flash prices double on 1 Jan 2027 and Gemini 4 prices double on 1 Dec 2026. Contracts priced off Oct 2026 Gemini rates will lose margin early in 2027.
- Recommended routing:
  - Routine replies: Haiku 5.5 / gpt-6-luna.
  - Hard cases, complaints and owner briefings: Sonnet 5.5 / gpt-6.1-sol.
  - Rare deep analysis: Opus 5.5.
  - Embeddings: FPT Vietnamese_Embedding ($0.011) or Gemini Embedding 2 ($0.20).
  - An OpenAI-compatible abstraction (LiteLLM-style, or OpenRouter at a 5.5% fee) lets you switch vendors per tenant.
- **Pilot test this before committing:** all major models handle Vietnamese, but no current, citable head-to-head benchmark exists. Run a 50–100-conversation Vietnamese evaluation using real shop FAQs.

### Gaps
- No public per-token API price was found for Viettel AI, VNPT AI/SmartBot, Zalo/VNG models, or VinAI.
- OpenAI embedding prices, and the "short" vs "long" context threshold for GPT-6 models, were not on the page read.
- The official Qwen international price page and the official OpenRouter fee page were not read; the figures above come from aggregators.
- No 2026 Vietnamese benchmark comparing Claude 5.5, GPT-6 and Gemini 3.x was found.

---

## 2. Token usage estimates and monthly LLM cost (small vs busy shop, with model routing)

### Takeaway
With caching and a cheap default model, a customer-service conversation costs about **130–360 VND** on a cheap model and **about 2,600–2,800 VND** on a Sonnet-class model. A 75-conversation/day shop pays roughly **0.3–0.9M VND/month** in LLM fees. A 500/day shop pays roughly **2.2–6.1M VND/month**, depending on how many conversations escalate to the strong model. The owner's personal assistant adds about **0.17–0.4M VND/month**.

### Cited Findings
- Per-MTok prices used here: Haiku 5.5 $0.10 / $0.125 write / $0.01 read / $0.50 out; Sonnet 5.5 $2 / $2.50 / $0.10 / $10; Opus 5.5 $4 / $5 / $0.20 / $20 — [Anthropic pricing](https://platform.claude.com/docs/en/about-claude/pricing)
- gpt-6-luna $0.10 / $0.01 / $0.50; gpt-6.1-sol $2 / $0.10 / $10 — [OpenAI API pricing](https://developers.openai.com/api/docs/pricing)
- Gemini 3.1 Flash-Lite $0.25 / $0.025 / $1.50; Gemini 3.8 Flash $0.75 / $0.075 / $3.75 until end-2026 — [Gemini pricing](https://ai.google.dev/gemini-api/docs/pricing)
- DeepSeek Flash peak $0.30 miss / $0.006 hit / $1.20 out — [DeepSeek pricing](https://api-docs.deepseek.com/quick_start/pricing)
- Haiku 5.5's newer tokenizer counts the same text as roughly 30% more tokens than Haiku 4.5. Thinking is on by default at "medium" effort, can be turned off at effort "high" or below, and thinking tokens are billed as output. (Source: Anthropic's claude-api SDK skill documentation, cached 2026-10-06, `shared/models.md`; no public URL.)

### Inferences (author's model — assumptions stated; not from a source)

**Customer-service conversation profile**
- 8 customer turns, about 12 LLM calls including tool calls (order lookup, product search, retrieval).
- Each call:
  - **3,000 tokens** of cached prefix: system prompt, persona, tool definitions and policy.
  - **2,500 tokens** uncached: conversation history plus about 1,500 tokens of retrieved knowledge on half the calls.
  - **300 output tokens**: short Vietnamese reply plus tool JSON, at low effort/thinking.
- Per conversation: 36K cache-read + 30K uncached input + 3.6K output + about 3K cache-write.

**Cost per conversation (VND at 26,000)**

| Model | USD | VND |
|---|---|---|
| Claude Haiku 5.5 | 0.0055 | ~144 |
| OpenAI gpt-6-luna | 0.0052 | ~134 |
| DeepSeek Flash (peak) | 0.0135 | ~352 |
| DeepSeek Flash (off-peak) | 0.0068 | ~176 |
| Gemini 3.1 Flash-Lite | 0.0138 | ~359 |
| Qwen3.8-flash (no cache discount assumed) | 0.0116 | ~302 |
| Gemini 3.8 Flash (2026 promo) | 0.0387 | ~1,006 (~2,000 from Jan 2027) |
| DeepSeek V4 Pro (peak) | 0.0554 | ~1,441 |
| OpenAI gpt-6.1-sol | 0.0996 | ~2,590 |
| Claude Sonnet 5.5 | 0.1071 | ~2,785 |
| Claude Opus 5.5 | 0.214 | ~5,570 |

**Routing mixes (Haiku default, Sonnet for escalations)**
- 95% Haiku / 5% Sonnet ≈ **$0.0106 ≈ 276 VND per conversation**.
- 90% Haiku / 10% Sonnet ≈ **$0.0157 ≈ 408 VND per conversation**.
- These are upper bounds, because escalated conversations usually hand only their remaining turns to Sonnet.

**Monthly customer-service LLM cost**

| Shop size | Conversations/month | Haiku only | 95/5 routing | 90/10 routing |
|---|---|---|---|---|
| Small (75/day) | 2,250 | $12.5 ≈ 324K VND | $24 ≈ 621K VND | $35 ≈ 918K VND |
| Medium (250/day) | 7,500 | $41.5 ≈ 1.08M | $80 ≈ 2.07M | $118 ≈ 3.06M |
| Busy (500/day) | 15,000 | $83 ≈ 2.16M | $159 ≈ 4.14M | $235 ≈ 6.12M |

**Escalation rate is the biggest cost lever.** At 10% escalation, Sonnet accounts for about two-thirds of the bill.

**Owner's personal assistant, per day**

| Task | Model | Tokens | Cost |
|---|---|---|---|
| Morning briefing (calendar, 30 email snippets, sales figures) | Sonnet 5.5 | ~15K in / 1K out | $0.04 |
| Email triage, 50 emails | Haiku 5.5 | ~1.5K in / 150 out each | $0.011 |
| Summarise 3 reports | Sonnet 5.5 | ~20K tokens each | $0.15 |
| 30 ad-hoc Zalo Q&A turns | Haiku 5.5 | — | $0.016 |
| 30 ad-hoc Zalo Q&A turns (alternative) | Sonnet 5.5 | — | $0.31 |

- Daily total: about $0.22 (mixed models) to $0.51 (Sonnet-heavy).
- Monthly: **$6.6–15.3 ≈ 170K–400K VND**.
- An occasional Opus 5.5 deep analysis (50K in / 3K out) costs about $0.26 (6,800 VND) each.

**Cost controls**
- Keep the system prompt and tools byte-stable so they cache.
- Use low effort / thinking off for Haiku chat routes.
- Cap history, for example summarise after 20 turns.
- Run nightly jobs (KB re-index, daily reports) through the Batch API at −50%.

**Embeddings are negligible:** a 2M-token knowledge base costs $0.40 with Gemini Embedding 2, or $0.02 with FPT Vietnamese_Embedding.

### Gaps
- No published, measured token profile for Vietnamese SME chat was found; the figures above are modelled.
- Real Vietnamese token counts per tokenizer were not measured. Validate with count_tokens on real transcripts during a pilot.

---

## 3. Hosting options and prices; specs per agent; data localization

### Takeaway
A Vietnamese VPS with 2 vCPU / 4 GB costs about **200–250K VND/month** (Vietnix list price, ~212K on annual billing). Hetzner's cheapest EU shared VMs are about **€5.49 (~165K VND)** but sit in Germany/Finland. **Hetzner Singapore costs 3–5× more** (CPX12 at €15.49 / $17.99). For a multi-tenant design, one 4 vCPU / 8 GB VPS (~350–580K VND/month) can host many small tenants. Using foreign LLM APIs means data leaves Vietnam regardless of where the VPS sits.

### Cited Findings
- **Vietnix "VPS giá rẻ"** (official page, 2026; Intel Xeon E5 v4; 100 Mbps domestic / 10 Mbps international; weekly backup):
  - 2 CPU / 2 GB: from 199K/month (3-month term) down to 178K (1-year).
  - **2 CPU / 4 GB: list 250K; 237K (3-month), 225K (6-month), 212K (1-year), 200K (2-year)**.
  - Higher tiers list at 370K, 410K, 530K, 680K and 870K/month.
  - VPS NVMe 2 (2 vCPU / 4 GB / 40 GB NVMe, 400 Mbps) from 391K/month.
  - New-customer code VIETNIX_WELCOME10 gives 10% off.
  - Sources: [Vietnix VPS giá rẻ](https://vietnix.vn/vps-gia-re/); [Vietnix VPS cấu hình khủng](https://vietnix.vn/vps-cau-hinh-khung/)
- **BizFly Cloud:** the official VPS page advertises "from 95,000 VND/month" (1 GB / 20 GB SSD entry plan). The Cloud Server page lists a 2 GB plan at 194,640 VND/month excluding VAT. Both figures come from search snippets of the official pages; the price page itself returned 404 when scraped — [BizFly Cloud VPS](https://bizflycloud.vn/en/cloud-vps)
- **AZDIGI comparison** (a competitor's article) puts Vietnix VPS CHEAP 2 (2 vCPU / 4 GB / 40 GB) at 209K/month — [AZDIGI vs Vietnix](https://azdigi.com/blog/kien-thuc-vps/azdigi-vs-vietnix-detailed-vps-comparison-from-actual-users)
- **Hetzner, price adjustment effective 15 Jun 2026**, excl. VAT:
  - Germany/Finland: CX23 €5.49 / $6.49; CX33 €8.49 / $9.99; CAX11 (Arm) €5.99 / $6.99; CAX21 €10.49 / $12.49; CPX22 €19.49 / $22.99; CPX32 €35.49 / $41.99.
  - **Singapore: CPX12 €15.49 / $17.99; CPX22 €26.49 / $30.99; CPX32 €48.99 / $57.99.** CX and CAX are not offered in Singapore.
  - Source: [Hetzner price adjustment](https://docs.hetzner.com/general/infrastructure-and-availability/price-adjustment/)
- Hetzner also raised prices 30–37% on 1 Apr 2026. CX23 is 2 vCPU / 4 GB / 40 GB / 20 TB traffic — [findstack](https://findstack.com/resources/hetzner-price-increase-2026); [igorslab](https://www.igorslab.de/en/hetzner-to-significantly-increase-prices-for-cloud-and-dedicated-servers-from-april-2026/). One source reports the Cost-Optimized CX/CAX lines showing "Currently not available" — [privatedevops](https://privatedevops.com/news/hetzner-june-2026-cloud-price-increase-what-to-do)
- **FPT AI Factory GPU:** H100 $2.99/hour (Southeast Asia); H200 $6.60/hour (Japan). This is only relevant if you self-host open models, which an SME setup does not need — [FPT AI Factory pricing](https://factory.fpt.ai/pricing)
- **Managed hosting for agent runtimes:** Hostinger 1-click OpenClaw from $6.99/month. Agent37 reports Hostinger promos of $5.99–25.99 that renew at about 2×, and its own plans from $0.99 (shared), $3.44 (metered) and $3.99 (managed). DIY VPS runs about $6–25. Agent37 is a competitor, so treat its numbers with caution — [Hostinger](https://www.hostinger.com/in/tutorials/best-openclaw-hosting/); [Agent37](https://www.agent37.com/blog/openclaw-costs-in-2026-what-you-actually-pay)
- **Data-localization law:** Decree 53/2022 localization duties apply to "domestic enterprises" (incorporated in Vietnam, including foreign-invested companies) for covered data types — [Tilleke & Gibbins](https://www.tilleke.com/insights/what-do-vietnams-new-data-localization-requirements-mean-for-domestic-enterprises/24/). The new Cybersecurity Law (116/2025/QH15, effective 1 Jul 2026) reportedly keeps localization for data such as account names, usage time, payment info and access IPs, with details left to a future decree. Tilleke notes official reports did not confirm this — [Duane Morris](https://blogs.duanemorris.com/vietnam/2026/01/08/vietnam-law-on-cybersecurity-2026-what-you-must-know/); [Tilleke](https://www.tilleke.com/insights/vietnam-issues-new-cybersecurity-law)
- **Cross-border transfer:** under Decree 356/2025, transferring to servers outside Vietnam, to foreign cloud providers, or processing on foreign platforms counts as cross-border transfer — [Vietnam Briefing](https://www.vietnam-briefing.com/news/vietnam-personal-data-protection-regulation-decree-356.html/)

### Inferences
**Sizing per agent instance** (engineering estimate, no source):

| Component | RAM |
|---|---|
| Node.js/Python agent process | 150–400 MB |
| Postgres + pgvector | 300–700 MB |
| Redis/queue (optional) | 50–100 MB |
| Headless Chromium (only for web-portal scraping) | 300 MB–1 GB per active browser |

- Per-client isolated stack without a browser: **1 vCPU / 2 GB**. With a browser: **2 vCPU / 4 GB**.
- Multi-tenant: one 4 vCPU / 8 GB host serves about **15–30 light tenants** (shared Postgres, no browsers). The LLM does the heavy work remotely, so CPU is mostly idle.

**Recommendation**
- Host the **database and logs in Vietnam** (Vietnix, BizFly, Viettel IDC, FPT Cloud). This keeps customer PII at rest in-country, gives low latency to Zalo/Facebook webhooks and bank-webhook providers, and supports Cybersecurity-law positioning.
- Send only the minimum context to foreign LLMs and file the cross-border dossier where required (see section 8).
- Vietnamese budget VPS international bandwidth (10 Mbps) is enough for LLM API calls, which are kilobytes per request.
- Hetzner EU is cheapest per GB of RAM, but it puts PII in the EU and adds about 200 ms latency. Hetzner Singapore is not cost-competitive with Vietnamese VPS.

### Gaps
- No official 2026 prices were obtained for Viettel IDC, VNPT, FPT Cloud (non-AI), CMC Cloud, AZDIGI, Long Vân, DigitalOcean, Vultr, Contabo, AWS Lightsail or GCP Singapore.
- BizFly 2 vCPU / 4 GB pricing was not captured; the price page returned 404.
- Whether Vietnamese VPS list prices include 10% VAT was not confirmed for every provider.

---

## 4. Replication architecture (isolated vs multi-tenant) and fast provisioning tools

### Takeaway
To onboard many SME clients quickly and cheaply, run a **multi-tenant core**: one codebase, a tenant_id on every row, per-tenant secrets and config. Offer an **"isolated" tier** for clients who pay for it: one Docker Compose stack per client, deployed from a golden template via Coolify/Dokploy or Ansible/cloud-init. Infrastructure provisioning takes minutes either way. **Real onboarding time is driven by channel approvals (Zalo OA verification and plan, Facebook app) and knowledge-base preparation, not servers.**

### Cited Findings
- **Coolify:** more mature, 55,000+ GitHub stars, large one-click template library, "advanced" multi-server management, suited to teams running many services — [INTROSERV](https://introserv.com/blog/dokploy-vs-coolify-complete-comparison-of-the-best-self-hosted-paas-platforms-for-vps-and-dedicated-servers-2026/); [toolradar](https://toolradar.com/compare/coolify-vs-dokploy)
- **Dokploy:** leaner, deploys Compose files as-is, native Docker Swarm clustering across nodes, idles at about 350 MB RAM and <1% CPU. Sources disagree on its license (Apache-2.0 vs a "Canary" license) — [INTROSERV](https://introserv.com/blog/dokploy-vs-coolify-complete-comparison-of-the-best-self-hosted-paas-platforms-for-vps-and-dedicated-servers-2026/); [Cherry Servers](https://www.cherryservers.com/blog/coolify-vs-dokploy)
- Both support Docker Compose, Let's Encrypt TLS, Git webhooks and database provisioning — [virtua.cloud](https://www.virtua.cloud/learn/en/concepts/coolify-vs-dokploy-self-hosted-paas)
- **Commercial "hosted agent" providers** sell per-instance managed hosting (one container/VM per user) from about $1–7/month, with 1-click install and bundled AI credits. Hostinger's plans renew at about 2× the promo price — [Hostinger](https://www.hostinger.com/in/tutorials/best-openclaw-hosting/); [Agent37](https://www.agent37.com/blog/hostinger-openclaw)
- **Zalo constraint:** a third-party app linking to an OA via API needs the **Tăng trưởng (Growth) or Toàn diện (Comprehensive) plan**. Each plan has an authorised-app limit: 3 for Tăng trưởng, unlimited for Toàn diện. API rate limit is 100 requests/minute (Tăng trưởng) vs 2,000/minute (Toàn diện) — [Zalo OA pricing](https://zalo.solutions/oa/pricing)
- To buy a plan, the OA must first be **verified** by Zalo and linked to a ZBS account that holds credit — [Zalo OA pricing](https://zalo.solutions/oa/pricing)

### Inferences
| Dimension | Per-client isolated (one Compose stack or VPS per client) | Multi-tenant SaaS |
|---|---|---|
| Security / blast radius | Strong: a breach or prompt-injection leak is confined to one client; easy to show "your data is separate" | Depends on discipline: row-level security, tenant-scoped tools and keys; one bug can leak across tenants |
| Infrastructure cost/client | ~170–250K VND (one VPS each) or ~50–100K (several stacks per larger VPS) | ~15–40K VND (shared 4 vCPU / 8 GB) |
| Updates | N stacks to roll out; needs automation (Ansible, Coolify/Dokploy redeploy, Watchtower-style image pinning) | One deploy updates everyone; needs feature flags per tenant |
| Customisation | Easy to fork per client, but forks become a maintenance trap | Config-driven only: prompts, tools and KB per tenant |
| Compliance | Easier to place in a client's chosen region; cleaner DPIA scope | One DPIA/processor agreement covers the platform; must document tenant isolation |
| Infra provisioning time | 5–15 min with a golden snapshot + cloud-init, or a Coolify template | ~1 min (create tenant row + secrets) |

**Fast-install toolchain**
- A Git repo with `docker-compose.yml` (agent + Postgres/pgvector + Caddy/Traefik + backup sidecar).
- A `.env.template` per client.
- **cloud-init** or an **Ansible** playbook. Hardening steps: SSH keys, UFW/firewall, fail2ban, unattended-upgrades, Docker install, compose up.
- Or Coolify/Dokploy managing many VPSs from one dashboard.
- A golden VPS snapshot where the provider supports it.
- One script, `./new-client.sh <slug>`, that creates DNS, secrets, the Postgres schema and webhook URLs.

**Realistic onboarding (labour hours, excluding Zalo/Facebook waiting days)**
- Infra: 0.5–1 h.
- Channel hookup (Zalo OA app + webhook + plan purchase; Facebook Page + Messenger app; web widget): 2–4 h.
- Knowledge base ingestion and cleanup: 2–8 h.
- Persona, policies, tool permissions and test scripts: 3–8 h.
- Owner/staff training and handover rules: 1–2 h.
- **Totals: small client 8–12 h; medium 16–24 h; large/integrated (inventory, bank, CRM) 32–60 h.**
- Template reuse per vertical (spa, F&B, fashion shop) can cut the KB and persona time by half.

**Suggested hybrid**
- A multi-tenant control plane: billing, monitoring, admin UI, shared LLM gateway with per-tenant budgets.
- Per-tenant agent containers, so clients get both isolation and cheap fleet updates.

### Gaps
- No public documentation was found on how specific hosted-agent vendors (hosted OpenClaw, "Hermes cloud") isolate tenants internally.
- Coolify Cloud and Dokploy Cloud subscription prices were not verified.
- Facebook/Meta app-review timelines for Messenger in 2026 were not researched here (channel researcher's scope).

---

## 5. Memory / knowledge-base technology

### Takeaway
For SME-scale knowledge bases (hundreds to a few thousand pages), **Postgres + pgvector** (or **SQLite + sqlite-vec** for single-tenant stacks) is enough. A dedicated vector DB (Qdrant) is not needed. Embedding cost is negligible: about $0.011–0.20 per MTok.

### Cited Findings
- Embedding prices:
  - FPT AI Factory **Vietnamese_Embedding $0.011/MTok**; multilingual-e5-large $0.022/MTok; bge-reranker-v2-m3 $0.022/MTok — [FPT AI Factory pricing](https://factory.fpt.ai/pricing)
  - Gemini Embedding 2: text $0.20/MTok; images $0.45/MTok — [Gemini pricing](https://ai.google.dev/gemini-api/docs/pricing)
- Claude accepts PDFs natively (up to 32 MB per request and 600 pages, or 100 pages for 200K-context models). It offers a Files API and citations (page-level for PDFs), which help with answers that cite sources. (Source: Anthropic's claude-api SDK skill documentation, cached 2026-10-06; no public URL fetched.)
- OWASP 2025 lists **LLM08 "Vector and Embedding Weaknesses"** as a top-10 risk, meaning a poorly secured RAG store is an attack surface — [Invicti OWASP LLM 2025](https://www.invicti.com/blog/web-security/owasp-top-10-risks-llm-security-2025)

### Inferences
**Recommended stack**
- Postgres 16 + pgvector, using HNSW indexes with a tenant_id filter. For a hybrid search over Vietnamese text, add a Postgres full-text/trigram index alongside the vectors, because exact product codes and SKUs match better lexically. A reranker ($0.022/MTok) improves precision.
- **sqlite-vec** fits isolated single-client containers: no extra service, and backup is one file.
- **Qdrant** is only worth it for many millions of vectors or heavy filtering.

**Ingestion**
- PDF/Word/Excel through a parser such as Docling/Unstructured/markitdown-style tools.
- Chunk at 500–1,000 tokens with headings preserved.
- **Excel price lists and inventory should become SQL tables queried by tools, not embeddings.** Prices and stock change daily, and RAG over stale chunks causes misinformation (OWASP LLM09).

**Memory for the personal assistant**
- Store short facts (preferences, contacts, recurring tasks) in a key-value/notes table, with a nightly summarisation job (Batch API, −50%).
- Keep per-request context under 100K tokens; this matters for Haiku 5.5 pricing.

**Cost:** ingesting a 2M-token KB costs about $0.02 (FPT) or $0.40 (Gemini). Re-embedding monthly is negligible.

### Gaps
- No 2026 benchmark comparing Vietnamese retrieval quality of FPT Vietnamese_Embedding vs Gemini Embedding 2 vs multilingual-e5 was found.
- The tool names in this section (pgvector, sqlite-vec, Qdrant, Docling) are engineering recommendations. They were not checked against 2026 sources this session.

---

## 6. Monitoring, reliability, backups and human handover

### Takeaway
SME reliability needs four things:
- Uptime checks on each tenant's webhook endpoint.
- Error/latency/token-spend dashboards per tenant.
- Daily off-site encrypted backups.
- **A clear human-handover path:** staff take over the Zalo/Facebook thread and the bot pauses.

Most of this can be done with free or open-source tools for under about 50–100K VND per client per month.

### Cited Findings
- SePay (bank-transfer notification service) retries a failed webhook up to **7 times on a Fibonacci schedule, over about 33 minutes**, and has a separate sandbox/test mode. **Webhook receivers must be idempotent and alerting must catch outages longer than about 30 minutes**, or payment confirmations are lost — [SePay webhooks](https://developer.sepay.vn/en/sepay-webhooks); [SePay changelog](https://developer.sepay.vn/en/changelog)
- Zalo OA plans include a staff chat console: 5 staff on Tiêu chuẩn, 15 on Tăng trưởng, 100 on Toàn diện. Advanced chat features (assign staff, labels, notes), activity history and the OA Manager mobile mini-app support human takeover inside Zalo — [Zalo OA pricing](https://zalo.solutions/oa/pricing)
- Zalo pricing distinguishes replies inside the 48-hour window from **"tin tư vấn ngoài khung 48h"** (consultation messages sent more than 48 hours after the customer's last message): 500 free/month on Tăng trưởng, 2,000 on Toàn diện, then **55 VND per message** (VAT included) — [Zalo OA pricing](https://zalo.solutions/oa/pricing)
- OWASP 2025 **LLM10 "Unbounded Consumption"** means runaway token use or abuse must be bounded by rate limits and budgets — [Invicti OWASP LLM 2025](https://www.invicti.com/blog/web-security/owasp-top-10-risks-llm-security-2025)

### Inferences
**Monitoring stack (no source; standard practice)**
- Uptime Kuma (self-hosted) or a hosted uptime checker pinging `/health` on each tenant.
- Structured JSON logs to Loki/Grafana or a hosted log service.
- Per-tenant token and cost counters, from each response's `usage` fields, with **daily budget caps** and alerts at 80%.
- Sentry-style error tracking.
- Alerts to the operator's Zalo/Telegram.

**Backups**
- Nightly `pg_dump`/restic, encrypted, to S3-compatible storage. Prefer a Vietnamese object-storage provider to keep PII in-country.
- Keep 7 daily + 4 weekly copies. Run a quarterly restore test.

**Human handover rules**
- Triggers:
  - The customer asks for a human.
  - Negative sentiment or a complaint.
  - Refund, return or payment disputes.
  - Low retrieval confidence.
  - Two failed answers in a row.
- Mechanics: the bot posts an internal note, tags staff, stops replying to that thread for N hours, and logs the handover reason.

**Reliability**
- An LLM provider failover chain, e.g. Claude → OpenAI → Gemini through one gateway.
- A queue so webhooks are acknowledged in under 2 s and processed asynchronously.
- Dead-letter retry.

### Gaps
- 2026 prices for hosted monitoring/log services (Better Stack, Grafana Cloud, Sentry) and for Vietnamese object storage were not verified this session.

---

## 7. Security for customer-facing agents (prompt injection, tool scoping, OWASP LLM Top 10 2025)

### Takeaway
Treat every customer message, retrieved document and tool result as untrusted (OWASP LLM01). Most of the damage potential comes from **what tools the agent can call** (LLM06 Excessive Agency). Rules:
- The public customer-service agent gets **read-only, customer-scoped tools** and **no money-movement tools**.
- Writes (order creation, refunds, discounts) go through templated, validated, human-approved paths.
- Run the owner's personal assistant as a **separate agent with separate credentials**.

### Cited Findings
- **OWASP Top 10 for LLM Applications 2025:**
  - LLM01 Prompt Injection
  - LLM02 Sensitive Information Disclosure
  - LLM03 Supply Chain
  - LLM04 Data and Model Poisoning
  - LLM05 Improper Output Handling
  - LLM06 Excessive Agency
  - LLM07 System Prompt Leakage
  - LLM08 Vector and Embedding Weaknesses
  - LLM09 Misinformation
  - LLM10 Unbounded Consumption
  - Sources: [Invicti](https://www.invicti.com/blog/web-security/owasp-top-10-risks-llm-security-2025); [HCL AppScan docs](https://help.hcl-software.com/appscan/Enterprise/10.10.0/topics/r_owasp_top_10_for_llm.html)
- Injected instructions can arrive directly or through retrieved documents, tool responses, web pages and attachments. Models see system prompts, user prompts and retrieved data in one context — [Indusface](https://www.indusface.com/blog/owasp-top-10-llm); [Outpost24](https://outpost24.com/blog/owasp-top-10-llm-risks-explained/)
- **LLM06 mitigation:** limit the tools an agent can call to those strictly needed, and limit each tool's functionality (e.g. read-only). Example threat: a support bot connected to CRM and billing is tricked into **issuing unauthorized refunds or exposing customer records** — [Invicti](https://www.invicti.com/blog/web-security/owasp-top-10-risks-llm-security-2025)
- Zalo requires ZBS template content to be transparent, not disguised advertising, and limits templates to customers with a prior transaction or interaction — [V9 Tech ZNS](https://v9.com.vn/bang-bao-gia-zalo-zns/)

### Inferences (design controls; standard practice, no single source)
1. **Two agents, two trust zones.**
   - Customer-service agent: tools such as `search_products`, `get_my_order(order_id, verified_customer_id)`, `create_draft_order`, `handover_to_human`.
   - Owner's personal assistant (only reachable from the owner's verified Zalo UID): broader tools such as email, calendar and reports.
   - Never share API keys or databases between the two without scoping.
2. **Bind permissions to the conversation, not the prompt.** The server injects the customer's identity (Zalo UID / Facebook PSID / verified phone) into tool calls. The model cannot choose whose order to read.
3. **No money transfer, refund or discount tools** on the customer-service agent.
   - Payment is **receive-only**: generate a VietQR/transfer instruction and confirm via the bank webhook (SePay).
   - Refunds and discounts above a threshold need human approval.
4. **Output handling (LLM05):** treat model output as data. Validate tool arguments against strict JSON schemas, never run model-written SQL or shell, and escape HTML in web widgets.
5. **System prompt leakage (LLM07):** keep no secrets in prompts and assume the prompt will leak.
6. **Retrieval poisoning (LLM04/08):** only the operator can add KB content, store a source label per chunk, and filter retrieval by tenant_id at the database layer.
7. **Misinformation (LLM09):** prices, stock and policies come from tools or SQL, not from model memory. Instruct the bot to say "Em sẽ chuyển nhân viên hỗ trợ" ("I'll transfer you to a staff member") when unsure.
8. **Unbounded consumption (LLM10):** per-tenant and per-customer rate limits, max turns per conversation, daily token budget, and `max_tokens` caps.
9. **Secrets:** per-tenant secrets encrypted at rest (SOPS/age, or the provider's secret manager), never in the Git repo. Rotate Zalo/Facebook tokens. Use least-privilege bank/webhook keys.
10. **Audit log:** append-only table of every tool call (who, tenant, arguments hash, result status), every handover and every admin change. Retain per the data-retention policy (see section 8).
11. **Operator instructions mid-conversation:** with Claude models, use the `role: "system"` mid-conversation message channel for operator updates rather than mixing them into user text. (Source: Anthropic claude-api skill docs; no public URL fetched.)
12. **Red-team before go-live:** run 30–50 Vietnamese injection prompts (e.g. "bỏ qua hướng dẫn trước…" = "ignore previous instructions…", fake "admin" messages, injected text inside uploaded images/PDFs).

### Gaps
- The OWASP list was confirmed via secondary sources (Invicti/HCL); genai.owasp.org was not opened directly.
- No Vietnam-specific security certification requirement for SME chatbots was identified (beyond the Cybersecurity Law and AI Law obligations in section 8).

---

## 8. Vietnam legal requirements (PDPL 2025, Decree 356/2025 replacing Decree 13/2023, AI Law, Cybersecurity Law, Decree 53/2022, Decree 147/2024)

### Takeaway
Since 1 Jan 2026, the **Personal Data Protection Law (91/2025/QH15)** and **Decree 356/2025/NĐ-CP**, which replaces Decree 13/2023, govern chatbots that store customer data:
- Consent and notice are required.
- A **processing impact assessment dossier** (hồ sơ đánh giá tác động xử lý dữ liệu cá nhân, i.e. a DPIA) must be prepared and filed within 60 days.
- A separate **cross-border transfer dossier** is needed when data goes to foreign LLM APIs or clouds.

**Small enterprises and startups may defer** the DPIA, transfer dossier and data-protection-officer duty for 5 years. Household businesses and micro-enterprises are exempt. **These exemptions do not apply to businesses that provide personal-data-processing services, process sensitive data, or process data of large numbers of people.** The installer running a multi-client AI platform likely falls into the "processing service" exception.

The **AI Law (134/2025/QH15)** has been in force since 1 Mar 2026 and adds chatbot transparency duties. Systems already operating before that date have a 12-month grace period, to about 1 Mar 2027.

### Cited Findings

**Personal Data Protection Law 91/2025/QH15**
- Passed 25 Jun 2025, effective 1 Jan 2026 — [VnEconomy](https://en.vneconomy.vn/data-breaches-could-cost-companies-5-of-annual-revenue.htm); [LuatVietnam](https://luatvietnam.vn/dan-su/luat-bao-ve-du-lieu-ca-nhan-2025-co-hieu-luc-khi-nao-568-103652-article.html); full text at [Thư viện Pháp luật](https://thuvienphapluat.vn/van-ban/Bo-may-hanh-chinh/Law-91-2025-QH15-Personal-Data-Protection-665440.aspx)
- **Fines:**
  - Up to **5% of prior-year revenue** for certain serious violations. Sources disagree whether the cap applies to cross-border-transfer violations specifically or more broadly.
  - Up to **VND 3 billion** for other violations.
  - **10× the proceeds** for buying or selling personal data.
  - Individuals pay half the organisational fine.
  - Sources: [VnEconomy](https://en.vneconomy.vn/data-breaches-could-cost-companies-5-of-annual-revenue.htm); [VietNamNet](https://vietnamnet.vn/en/data-security-breaches-could-cost-firms-5-of-annual-revenue-2539258.html); [Vietnam Law Magazine](https://vietnamlawmagazine.vn/fines-up-to-5-percent-of-revenue-for-violating-personal-data-protection-regulations-consequences-from-internal-threats-74308.html)
- **SME regime:**
  - Small enterprises and startups may choose whether to perform the duties in **Art. 21** (processing impact assessment), **Art. 22** (cross-border transfer assessment) and **Art. 33(2)** (designate a data-protection unit or officer) for **5 years from the effective date**, i.e. to about end-2030.
  - Household businesses and micro-enterprises are exempt from those articles.
  - **Exception:** neither carve-out applies to businesses providing personal-data-processing services, directly processing sensitive personal data, or processing data of a large number of data subjects.
  - Source: [LuatVietnam](https://luatvietnam.vn/dan-su/luat-bao-ve-du-lieu-ca-nhan-2025-co-hieu-luc-khi-nao-568-103652-article.html)
- **DPIA timing:** the controller (or controller-processor) must prepare and keep the processing impact assessment dossier and send one original to the specialised data-protection authority **within 60 days of starting processing** — [LuatVietnam](https://luatvietnam.vn/dan-su/luat-bao-ve-du-lieu-ca-nhan-2025-co-hieu-luc-khi-nao-568-103652-article.html). A separate LuatVietnam article covers whether dossiers filed before 1 Jan 2026 must be refiled — [LuatVietnam](https://luatvietnam.vn/linh-vuc-khac/ho-so-danh-gia-tac-dong-xu-ly-du-lieu-ca-nhan-nop-truoc-01-01-2026-co-phai-nop-lai-theo-luat-moi-883-106424-article.html)

**Decree 356/2025/NĐ-CP (dated 31 Dec 2025, effective 1 Jan 2026; replaces Decree 13/2023)**
- Source for the decree itself: [LuatVietnam English](https://english.luatvietnam.vn/decree-no-356-2025-nd-cp-dated-december-31-2025-of-the-government-detailing-a-number-of-articles-and-measures-for-the-implementation-of-the-law-on-p-422896-doc1.html); [Viet An Law](https://vietanlaw.com/decree-356-2025-vs-decree-13-2023-updates-on-personal-data-protection-in-vietnam/)
- **What counts as cross-border transfer:** sending data to servers outside Vietnam, to foreign cloud providers, or processing Vietnam-collected data on foreign platforms. **Calling Claude/OpenAI/Gemini APIs with customer PII is a cross-border transfer.**
- **Transfer dossier:**
  - The transfer impact assessment uses **Form No. 09**, accompanied by a copy of the transfer contract and the transferor's data protection policies.
  - Submit through the Ministry of Public Security portal **within 60 days** of starting the transfer.
  - The authority reviews within 15 days and may request completion within 30 days.
  - **This is not a prior approval.**
- **Update frequency:** sources conflict — "update when changes occur" vs "update every 6 months when changes occur".
- **Exemptions** include transfers needed to perform contracts such as payment and logistics, and cross-border HR management.
- Sources: [Vietnam Briefing](https://www.vietnam-briefing.com/news/vietnam-personal-data-protection-regulation-decree-356.html/); [Skylaw](https://skylaw.vn/en/cross-border-personal-data-transfer-procedures-and-key-notes/); [VILAF](https://www.vilaf.com.vn/blog/vietnams-new-personal-data-protection-decree-key-compliance-requirements-effective-immediately/)

**AI Law 134/2025/QH15**
- Passed 10 Dec 2025 (429–5 votes), effective **1 Mar 2026** — [Lexology](https://www.lexology.com/library/detail.aspx?g=1be8965b-2741-4f90-a0cc-348ac219fef2); [Allen & Gledhill](https://www.allenandgledhill.com/vn/publication/articles/32667/s-new-law-on-artificial-intelligence-risk-based-regulatory-framework-in-force-1-march-2026); [VILAF](https://www.vilaf.com.vn/blog/vietnam-enacts-its-first-law-on-artificial-intelligence-key-regulatory-obligations-from-1-march-2026/)
- Risk-based: high, medium and low risk. Providers and deployers must ensure transparency, and AI-generated audio, images and video must carry machine-readable labels — [CASRAI](https://casrai.org/guides/vietnam-ai-law-134-2025); [VILAF](https://www.vilaf.com.vn/blog/vietnam-enacts-its-first-law-on-artificial-intelligence-key-regulatory-obligations-from-1-march-2026/)
- **Chatbots are described as medium-risk**, with transparency, reporting and sample-testing duties. **A business that integrates a third-party AI chatbot into customer service can be a "deployer"** — [Securiti](https://securiti.ai/vietnam-ai-aw/); [Conventus Law](https://conventuslaw.com/featured-content/companies-developing-and-using-ai-in-vietnam-key-legal-obligations-under-the-new-law-on-artificial-intelligence/)
- **Grace periods** for systems operating before 1 Mar 2026: **12 months** in general, 18 months for healthcare, education and finance — [aipolicytracker](https://aipolicytracker.org/policies/vietnam-law-on-artificial-intelligence); [VILAF](https://www.vilaf.com.vn/blog/vietnam-enacts-its-first-law-on-artificial-intelligence-key-regulatory-obligations-from-1-march-2026/)
- Penalties for serious infractions reportedly reach **up to 2% of revenue** (single source, not confirmed) — [Securiti](https://securiti.ai/vietnam-ai-aw/)
- Much of the operational detail sits in implementing decrees — [Lexology](https://www.lexology.com/library/detail.aspx?g=1be8965b-2741-4f90-a0cc-348ac219fef2)

**Cybersecurity Law 116/2025/QH15**
- Passed 10 Dec 2025, effective **1 Jul 2026**. It replaces the 2018 Cybersecurity Law and the 2015 Law on Network Information Security.
- Illegal content must be taken down **within 24 h of a Ministry of Public Security request** (6 h if urgent).
- Data localization reportedly continues, with details left to a decree. Reports conflict on this.
- Sources: [Duane Morris](https://blogs.duanemorris.com/vietnam/2026/01/08/vietnam-law-on-cybersecurity-2026-what-you-must-know/); [Tilleke](https://www.tilleke.com/insights/vietnam-issues-new-cybersecurity-law)

**Decree 53/2022**
- Localization duties cover domestic enterprises, including foreign-invested companies. Foreign enterprises have 12 months after a Ministry of Public Security decision — [Tilleke](https://www.tilleke.com/insights/what-do-vietnams-new-data-localization-requirements-mean-for-domestic-enterprises/24/); [BSA comments](https://ww2.bsa.org/files/policy-filings/en09302022vtde53.pdf)

**Decree 147/2024/NĐ-CP**
- Effective 25 Dec 2024. Social-network accounts must be authenticated with a Vietnamese mobile number (or personal ID number). Only verified accounts may post, comment, livestream or share. Platforms must store registration data — [Vietnam News](https://vietnamnews.vn/society/1666838/social-network-users-must-authenticate-accounts-from-december-25.html); [LuatVietnam](https://luatvietnam.vn/legal-updates/social-network-user-accounts-to-be-verified-with-subscriber-phone-numbers-892-100003-article.html)

**Zalo platform rules**
- ZBS (Zalo Business Solutions) Template Messages — Zalo's paid notification messages, formerly ZNS — go only to customers with a prior transaction or interaction, and content must not be disguised advertising — [V9 Tech](https://v9.com.vn/bang-bao-gia-zalo-zns/)
- Zalo business services are available only to businesses established in Vietnam — [Infobip Zalo compliance](https://www.infobip.com/docs/zalo/compliance-guidelines)

### Inferences
**Roles**
- The SME client is the **data controller** (bên kiểm soát).
- The installer/platform is a **data processor** (bên xử lý) that provides processing services. That likely **removes the SME exemption for the installer**, even when each client is a micro-business.
- The foreign LLM vendor is a sub-processor abroad.

**Compliance pack per client (template once, reuse)**
1. Privacy notice and consent text in Vietnamese, shown on first contact in Zalo/Facebook/web. Example: "Bạn đang trò chuyện với trợ lý AI của [Shop]; dữ liệu được xử lý theo chính sách…" ("You are chatting with [Shop]'s AI assistant; your data is processed under the policy…"). This also satisfies the AI Law transparency duty.
2. Data processing agreement between installer and client.
3. A DPIA (Art. 21) for the platform, filed within 60 days of processing start, plus a per-client annex.
4. A cross-border transfer dossier (Form 09) naming the LLM vendors, filed within 60 days of first transfer.
5. A retention policy, e.g. chat logs 12 months, then anonymise.
6. A data-subject request process: access, deletion, withdrawal of consent.
7. Breach response.

**Minimise transfer**
- Mask phone numbers, addresses and account numbers before sending to foreign LLMs.
- Keep PII at rest on Vietnamese servers.
- Consider FPT AI Factory, or self-hosted open models, for clients in sensitive sectors (health, finance, education).

**AI Law timing:** chatbots deployed after 1 Mar 2026 have no grace period. Disclosure and labelling should be built in from day one.

**Agent restrictions:** do not let agents create or post content from social accounts that are not verified under Decree 147. Bots replying in a business's own OA/Page do not seem to be the target of Decree 147 (inference).

### Gaps
- The exact article text was not read in the original gazette: the 5% vs VND 3 billion fine scope, the transfer-dossier update frequency, and which body receives filings.
- The definitions of "large number of data subjects" and "sensitive data" thresholds in Decree 356 were not extracted.
- AI Law implementing decrees (chatbot disclosure wording, risk-classification lists, fines) were not confirmed.
- Status and effective date of the 2025 E-commerce Law, and its rules for chatbots on sales channels, were not researched successfully.
- Whether foreign API vendors charge Vietnamese VAT on API invoices was not verified.

---

## 9. Sample cost table per client (setup + monthly, VND)

### Takeaway
At 26,000 VND/USD, monthly running costs per client come out to roughly:
- **Small shop: ~1.2–1.6M VND** (~$46–62)
- **Medium shop: ~3.3–4M VND** (~$127–154)
- **Busy shop: ~7–8.5M VND** (~$270–330)

LLM tokens and Zalo fees dominate; hosting is minor. One-time setup is about **8–60 labour hours**. Zalo OA plans are paid **upfront per 6 or 12 months**.

### Cited Findings (price inputs)

**Zalo OA plans** (verified business OA; VAT 10% included; price list effective 1 Jun 2026)

| Plan | Price | Staff | Open API | Free out-of-48h messages/month |
|---|---|---|---|---|
| Tiêu chuẩn (Standard) | 1,000,000 VND/year (~83K/month) | 5 | No | — |
| **Tăng trưởng (Growth)** | 1,400,000 per 6 months (~233K/month) or **2,500,000/year (~208K/month)** | 15 | Yes, 100 req/min | 500 |
| **Toàn diện (Comprehensive)** | 3,400,000 per 6 months (~566K/month) or **6,000,000/year (~500K/month)** | 100 | Yes, 2,000 req/min | 2,000 |

- Overage on out-of-48h consultation messages: **55 VND per message**.
- Chat groups cost 25K–300K VND/month each. OA-to-group messages are free until 31 Dec 2026.
- Source: [Zalo OA pricing](https://zalo.solutions/oa/pricing)

**ZBS Template Messages** (formerly ZNS; VAT excluded; charged per successfully processed message)
- Base prices: authentication/OTP template 400 VND; payment-request template 300; voucher 400; **all other types 300**.
- Sending by Zalo UID gets a subsidised price, from about **210 VND** (custom template sent by UID vs 300 by phone number).
- Buttons: first button in each category is free; later ones cost 100–3,000 VND. An image adds 300 VND.
- Source: [ZBS pricing](https://zalo.solutions/business-message/pricing)

**SePay** (bank-transfer webhook service)

| Plan | Price | Transactions | API/webhook |
|---|---|---|---|
| FREE | 0 VND | 50/month | Yes |
| STARTUP | 96,000 VND/month | 180/month | Yes |
| Higher tiers | not captured | 350 / 600 / 1,000 / … up to 986,000/month | Yes |
| SHOP | 70,000/store/month billed annually (840K/year) | Unlimited | **No API** |

- Overage is allowed and billed.
- Source: [SePay pricing](https://sepay.vn/bang-gia.html)

**Hosting and LLM inputs**
- Vietnix 2 CPU / 4 GB: 212–250K VND/month — [Vietnix](https://vietnix.vn/vps-gia-re/)
- Hetzner CX33: €8.49/month — [Hetzner](https://docs.hetzner.com/general/infrastructure-and-availability/price-adjustment/)
- LLM prices as in sections 1–2 — [Anthropic](https://platform.claude.com/docs/en/about-claude/pricing); [OpenAI](https://developers.openai.com/api/docs/pricing); [Gemini](https://ai.google.dev/gemini-api/docs/pricing)

### Inferences (author's cost model; assumptions explicit)

**Assumptions**
- Customer-service model is Haiku 5.5 with 5% Sonnet 5.5 escalation (about 276 VND per conversation).
- Personal assistant on a mixed model (~170–400K VND/month).
- ZBS template messages at 300 VND each (order confirmations / payment requests).
- **Labour rate is an assumption: 300,000 VND/hour.** No market source was found; replace with your own rate.
- Facebook Messenger replies are assumed free inside Meta's messaging window. This was not verified this session.

**Monthly running cost per client (VND)**

| Item | Small (hộ kinh doanh / small shop) | Medium (shop chain / spa) | Busy (500+ conversations/day) |
|---|---|---|---|
| Customer-service volume | 50–100 conversations/day (~2,250/month) | ~250/day (~7,500/month) | ~500/day (~15,000/month) |
| Hosting | ~60K (shared multi-tenant) to 250K (dedicated 2 vCPU / 4 GB) | 120–250K | 350–580K (dedicated 4 vCPU / 8 GB, Vietnamese VPS) |
| LLM, customer service | ~620K (range 320K–920K) | ~2.07M (1.08–3.06M) | ~4.14M (2.16–6.12M) |
| LLM, personal assistant | 170K | 260K | 400K |
| Embeddings / reindex | <10K | <20K | <50K |
| Zalo OA plan (monthly equivalent) | 208K (Tăng trưởng, annual) | 208K | 500K (Toàn diện, annual) |
| Out-of-48h Zalo consultation messages | 0 (≤500 free) | ~30K | 0–100K |
| ZBS template messages | 300 msgs × 300 = 90K | 1,500 × 300 = 450K | 5,000 × 300 = 1.5M |
| Bank webhook (SePay) | 0–96K (FREE / STARTUP) | higher tier (price not captured; assume ~150–300K) | higher tier (assume ~300–600K) |
| Monitoring + backups + logs share | ~40K | ~60K | ~100K |
| **Total per month** | **~1.2–1.6M VND (~$46–62)** | **~3.3–4.0M VND (~$127–154)** | **~7.0–8.5M VND (~$270–330)** |

**One-time setup per client (assumed 300K VND/hour)**

| | Small | Medium | Busy / integrated |
|---|---|---|---|
| Labour hours | 8–12 h | 16–24 h | 32–60 h |
| Labour cost | 2.4–3.6M VND | 4.8–7.2M VND | 9.6–18M VND |

- Plus upfront cash: Zalo OA plan 2.5M (Tăng trưởng/year) or 6M (Toàn diện/year).
- Plus any VPS prepay for the annual discount (2.5–7M).
- Zalo OA verification typically takes days of calendar time, not labour hours.

**Pricing implications for the entrepreneur**
- A retail price of about **2–3M VND/month (small)**, 5–7M (medium) and 10–15M (busy), plus a setup fee of 5–20M, would give roughly a 40–60% gross margin over the costs above (inference).
- Budget an LLM spend cap per tenant and pass through overages above an included conversation quota.
- Re-price Gemini-based tenants before 1 Jan 2027, when Flash prices double.

### Gaps
- SePay mid- and high-tier prices were not captured.
- Facebook Messenger/Instagram API fees for 2026 were not verified.
- Market labour rates for Vietnamese AI/automation freelancers were not found.
- Monitoring/backup service prices are estimates, not verified quotes.
