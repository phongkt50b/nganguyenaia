# Ready-made, commercial and low-code platforms in Vietnam for AI chatbots/agents on Zalo, Messenger and websites: the "buy vs build" landscape (as of 14 Oct 2026)

Research notes. Research date: 14 Oct 2026. USD figures use an assumed rate of about 26,000 VND/USD. I did not check this rate against a source, so read every USD figure as approximate. Many Vietnamese vendor pages could only be reached through a scraping service or through search-engine snippets. Each finding says where it came from. "Not verified" means I could not open a primary page or find a price.

---

## 1. What is the "Zalo Agent" ecosystem in the reference image? Who makes it, is there an open-source edition on GitHub, and what do the paid editions cost?

### Takeaway
"Zalo Agent" is a Vietnamese ecosystem marketed as **"Zalo Agent (Minh Số)"** by **Minh Đỗ**, Facebook handle @dotanminh, GitHub user `dotanminh`. He runs a Vietnamese AI-education community. He posted "Cập nhật Hệ Sinh Thái - Zalo Agent (Minh Số)" on 12 Oct 2026 and is starting a "CLB Gỡ rối AI 5H" club on 18 Oct 2026. His public sales page lists **no prices**: every package says "Liên hệ báo giá" (contact for a quote). I found no GitHub repo from him containing the open-source "Release" edition. The closest public open-source match is **vuhai2002/zalo-agent** (MIT license, "Agent AI thường trú trên Zalo", TypeScript + SQLite). Its stack and wording match the image closely, but I could not confirm that it is the "Release" edition of Minh Số's ecosystem.

### Cited Findings
- On 12 Oct 2026, Facebook user Minh Đỗ (@dotanminh) posted "Cập nhật Hệ Sinh Thái - Zalo Agent (Minh Số) - Ngày 12/10". The same post announces "CHÍNH THỨC THÀNH LẬP 'CLB GỠ RỐI AI 5H' - 18/10/2026" and says a slide deck was made by "Minh Số". I saw only search excerpts: the scraper does not support Facebook and the page could not be fetched. — [Facebook post, search excerpt](https://www.facebook.com/dotanminh/posts/c%E1%BA%ADp-nh%E1%BA%ADt-h%E1%BB%87-sinh-th%C3%A1i-zalo-agent-minh-s%E1%BB%91-ng%C3%A0y-1210/39685431997708532/); [Minh Đỗ profile, excerpt](https://www.facebook.com/dotanminh/)
- A user post describes "bào" (heavily using) "bé trợ lý AI nhắn trực tiếp bên Zalo của anh Minh Đỗ", i.e. the Zalo assistant is in community use. — [Facebook post by nnbaonam96, excerpt](https://www.facebook.com/nnbaonam96/posts/b%E1%BB%AFa-gi%E1%BB%9D-m%C3%ACnh-b%C3%A0o-b%C3%A9-tr%E1%BB%A3-l%C3%BD-ai-nh%E1%BA%AFn-tr%E1%BB%B1c-ti%E1%BA%BFp-b%C3%AAn-zalo-c%E1%BB%A7a-anh-minh-%C4%91%E1%BB%97-th%E1%BA%ADt-s%E1%BB%B1-th/29065277963064280/)
- GitHub user `dotanminh` owns the repo **`zalo-agent-premium-site`** ("Trang giới thiệu Zalo Agent", created 1 Oct 2026, updated 8 Oct 2026). He also owns `zaplo`, described as "Desktop app (Electron + React + TypeScript) quản lý Zalo & Facebook đa tài khoản: CRM, AI & Agent Hub tự động, Kho bài & rải lịch, Auto-comment, Workflow no-code, ERP nội bộ. Local-first". His other repos are dozens of Vietnamese AI "skills", for example thue-tncn-vietnam with 241 stars. — [GitHub search user:dotanminh zalo](https://github.com/dotanminh/zalo-agent-premium-site); [zaplo](https://github.com/dotanminh/zaplo); [thue-tncn-vietnam](https://github.com/dotanminh/thue-tncn-vietnam)
- **The Zalo Agent sales page** (index.html of `zalo-agent-premium-site`) contains:
  - Title: "Zalo Agent: trợ lý AI trên Zalo của riêng bạn". It says it is not affiliated with Zalo, Google, OpenAI or xAI.
  - Contact: Zalo 0918188448 and facebook.com/dotanminh.
  - Features: replies in private and group chats; remembers each person; reads images, voice, PDF, Word, Excel, PowerPoint and YouTube; web research; creates Word, Excel, PDF and PowerPoint files plus Nghị định 30 administrative documents; generates images, music and Vietnamese speech; downloads video/MP3; knowledge base that answers with sources; per-user and per-group permissions; reminders and "khách cần gấp" (urgent customer) alerts; connections to MCP, Notion and web scraping, with Apify via MCP.
  - Zalo connection: a secondary Zalo account logged in by QR, or the official Zalo Bot API, which has fewer features.
  - Mobile app: push alerts and reports over Tailscale. Included only in the "Cài hộ kèm VPS" package and the Business edition.
  - Stack: Windows/PowerShell. A Linux VPS is needed only for the video, premium PowerPoint and local VieNeu voice extensions. 9Router serves as model gateway; models come from OpenRouter, Google AI Studio, OpenAI, Gemini and xAI; search uses Tavily, Exa and Linkup.
  — [zalo-agent-premium-site index.html](https://raw.githubusercontent.com/dotanminh/zalo-agent-premium-site/main/index.html)
- **Packages and prices on that page:**

  | Package | What it includes | Price |
  |---|---|---|
  | Trọn gói tự cài (self-install) | Windows installer, updates, videos, docs | Liên hệ báo giá |
  | Cài hộ (installation service) | Self-install plus the team installs and configures it | Liên hệ báo giá |
  | Cài hộ kèm VPS | Cài hộ plus video and PowerPoint extensions, the mobile app and VPS setup; VPS rental billed separately | Liên hệ báo giá |

  The page mentions a "Business" edition but does not describe it. It does not list "Release", "Premium" or "Marketing" editions, the "75+ tools" count, or a license. — [same source](https://raw.githubusercontent.com/dotanminh/zalo-agent-premium-site/main/index.html)
- **Risk caveats the vendor states:** use only a secondary Zalo account, because the unofficial login library can get accounts banned; do not run one account in two places at once; Google family-shared accounts may break provider terms; **the bot has no payment or money-transfer tools, and the vendor advises against connecting any**; back up data regularly. — [same source](https://raw.githubusercontent.com/dotanminh/zalo-agent-premium-site/main/index.html)
- **Closest open-source match: vuhai2002/zalo-agent**, created 25 Jul 2026, MIT license, 8 stars as of Oct 2026.
  - Describes itself as "Self-hosted AI agent living inside Zalo (personal account). Multi-account, 15 tools + external MCP servers, web dashboard, scheduler, durable memory".
  - Runs personal accounts (via zca-js) and official Zalo Bot accounts (via the Zalo Bot API) in **one process**, "mỗi account một 'não' riêng".
  - Knowledge base imports docx/xlsx/pdf/txt/md and searches them with SQLite FTS5.
  - Dashboard has 70 settings; supports HTTP-only MCP servers.
  - Stack: TypeScript, Node 22, `node:sqlite`, Vercel AI SDK, Hono/React. Ships with Docker/VPS docs.
  - 2,657 tests.
  - A fork by andyluu98 adds a read-only chat-log mode and a Windows Task Scheduler autostart script.
  — [vuhai2002/zalo-agent README](https://raw.githubusercontent.com/vuhai2002/zalo-agent/main/README.md); [andyluu98/zalo-agent README](https://raw.githubusercontent.com/andyluu98/zalo-agent/main/README.md); [GitHub repo search](https://github.com/vuhai2002/zalo-agent)
- Neither README contains the exact taglines "một bộ não, nhiều kênh" or "75+ tools". vuhai2002's README says 15 tools, and the architecture gives each account its own brain rather than one shared brain. — [vuhai2002 README](https://raw.githubusercontent.com/vuhai2002/zalo-agent/main/README.md)

### Inferences
- The product in the image appears to be Minh Đỗ's "Zalo Agent (Minh Số)" ecosystem. Matching points: the "Hệ sinh thái Zalo Agent" name, the Business edition, the mobile app, the Notion/Apify MCP connections, Windows/VPS deployment and Zalo Bot API support. The image says "Node.js, TypeScript, SQLite" and "thường trú trên Zalo". This matches vuhai2002/zalo-agent exactly. So the "Release – open source" edition may be vuhai2002's MIT project, or something derived from it, with Minh Số's paid editions layered on top. **This link is unconfirmed.**
- The business model is "open-source core + paid installation/support packages + community/club". It is sold person to person (contact via Zalo or Facebook), not as a self-serve SaaS. This model is the one the user is thinking of replicating.
- If the open-source core is MIT, the user may legally fork it and white-label it. The main risks are (a) bans on personal Zalo accounts that use zca-js, and (b) support burden. Licensing is not the risk.

### Gaps
- Prices of the Premium, Business and Marketing editions and of the installation packages are not published anywhere I could reach. Facebook posts could not be fetched. The user should ask directly via Zalo 0918188448.
- I could not confirm the "75+ AI tools" count, the existence of a "Marketing" edition, or the GitHub URL of the "Release" edition.
- I could not check whether vuhai2002 is affiliated with Minh Đỗ. The GitHub API was blocked for non-session repositories.

---

## 2. Vietnamese commercial chatbot and omnichannel platforms with AI: channels, AI features, integrations, VND pricing and reseller programs

### Takeaway
The commercial market is mature for **customer-facing omnichannel inboxes plus sales chatbots**: Pancake/Botcake, Subiz, Fchat, Sapo, Harasocial and Bizfly. Entry prices are low, roughly 100k–700k VND/month. LLM-based "Chatbot AI" is increasingly sold as a higher tier or add-on. **Zalo personal-account support is spotty.** Subiz allows 3 personal Zalo accounts on its higher tier, and Fchat lists Zalo personal as a channel. On Zalo OA, any third-party AI needs a paid OA tier with API access, from 2.5M VND/year since the 1 Jun 2026 price list. Few vendors publish 2026 prices for their AI tiers. I found **no public white-label or reseller program** for any of them.

### Cited Findings

**Zalo itself (platform costs any solution must pay)**

New official OA price list, effective 1 Jun 2026, VAT included:

| OA tier | Price | Staff | Chatbot & automation | Open API | Chatbot scripts | Consult messages outside the 48h window |
|---|---|---|---|---|---|---|
| Tiêu chuẩn | 1,000,000đ/year (~83k/month, ~$38/yr) | 5 | No | No | – | – |
| Tăng trưởng | 1,400,000đ/6 months or 2,500,000đ/year (~$96/yr) | 15 | Yes | Yes, 100 req/min | 10 | 500/month |
| Toàn diện | 3,400,000đ/6 months or 6,000,000đ/year (~$231/yr) | 100 | Yes | Yes, 2,000 req/min | 50 | 2,000/month |

  - Extra consult messages outside the 48h window cost **55đ each**.
  - OA-owned group chats cost 25k–300k/month each. Messages from the OA to groups are free until 31 Dec 2026.
  - FAQ: "Để liên kết ứng dụng với OA thông qua API, bạn cần sử dụng Gói tăng trưởng hoặc toàn diện." Zalo is self-serve and does not negotiate contracts.
  — [Zalo Business Solutions – Bảng giá dịch vụ OA](https://zalo.solutions/oa/pricing)
- Zalo offers a free "Trợ lý Công Dân Số" (citizen legal and administrative Q&A, built on Zalo's own LLM) and the Kiki voice assistant. Neither is a business tool. — [Dân trí](https://dantri.com.vn/cong-nghe/huong-dan-cach-dung-tro-ly-ao-ai-tren-zalo-de-tra-cuu-thu-tuc-hanh-chinh-20250915161048255.htm); [Raccoon](https://raccoon.vn/chi-tiet-tin-tuc/zalo-them-tro-ly-ai-ten-la-kiki-289)
- Vietnamese press reports that OpenClaw supports Zalo and Zalo Personal as official channels. Báo Lâm Đồng calls Zalo "nền tảng nhắn tin đầu tiên của Việt Nam" on OpenClaw's support list. I did not verify the publication dates. — [Báo Nghệ An](https://baonghean.vn/openclaw-tich-hop-ai-agent-vao-zalo-nguoi-dung-viet-co-them-tro-ly-so-da-nang-10331792.html); [Báo Lâm Đồng](https://baolamdong.vn/openclaw-tich-hop-vao-zalo-ai-agent-thay-doi-cach-nguoi-dung-viet-tuong-tac-cong-nghe-434728.html)

**Pancake (pancake.vn / pages.fm) and Botcake**
- Pancake's official pricing page shows USD prices converted with Exchange Rate API, updated 13 Oct 2026. All plans include a 14-day free trial:

  | Plan | Connections | Users | Term | Price (list price) |
  |---|---|---|---|---|
  | Small | 1 | 3 | 6 months | $49.10 ($65) |
  | Standard | 3 | 3 | 6 months | $103.70 ($125) |
  | Business | 6 | 10 | 6 months | $365.80 ($415) |
  | Custom | – | – | – | Custom |

  - Standard features listed: "Connect all social networks and ecommerce platforms", instant order creation in the chat view, automated Facebook Ads optimization, phone and address detection, call-center integration, bundled access to all Pancake products.
  - The page lists no AI-agent feature or AI price.
  — [Pancake pricing](https://pancake.vn/pricing)
- Older third-party data (flag as stale): Mini (1 page, 3 staff), Standard (3 pages, 3 staff), Business (6 pages, 10 staff); 550,000đ (Mini, 6 months) to 4,210,000đ (Business, 12 months); extra page 40,000đ, extra staff 20,000đ. — [J&T Express article, via search snippet](https://www.jntexpress.vn/bai-viet/pancake)
- Pancake integrates Zalo OA into its chat interface. Orders can be created in chat, and auto-replies can be set for busy or off hours. — [didong3tot](https://didong3tot.com/blogs/blog-ky-thuat/tich-hop-zalo-oa-vao-ung-dung-pancake)
- **Botcake** (Pancake's chatbot product):
  - Starter: free, 500,000 automated messages/month.
  - Pro: from $30, 3M+ broadcast messages/month.
  - Premium: custom.
  - "BotcakeAI" is a separate add-on with no published price. "New AI features have just launched for our Pro and Elite plans."
  - Source is a search-engine snippet of botcake.io/pricing; a direct scrape was blocked by rate limits.
  — [Botcake pricing (snippet)](https://botcake.io/pricing)
- I found **no confirmation** that Pancake or Botcake offers an LLM agent on Zalo personal accounts, and no 2026 AI price. — search of Vietnamese sources returned none.
- There is open-source community tooling around Pancake: `aiecosvietnam/aiecos-social-crm`, "Pancake (Zalo/Facebook) → Supabase + Admin UI + MCP server for AI agents", 22 stars. — [GitHub](https://github.com/aiecosvietnam/aiecos-social-crm)

**Subiz** (official page, scraped 14 Oct 2026). Billed yearly only:

| Plan | Price | Channels | AI |
|---|---|---|---|
| Cơ bản | 3,994,000đ/year (~$154) | 100 fanpages, 1 TikTok, 1 WhatsApp, Zalo OA, Instagram, website livechat, virtual call center | Script bot only |
| Nâng cao | 7,834,000đ/year (~$301) | 1,000 fanpages, 20 TikTok, 20 WhatsApp, **3 Zalo cá nhân**, Zalo OA, Instagram, livechat | **"Chatbot AI"** |
| Doanh nghiệp | Contact sales | Everything in Nâng cao | Everything in Nâng cao, plus custom API integration |

  - No agency or reseller program is listed.
  — [Subiz pricing](https://subiz.com.vn/pricing.html); billing structure corroborated by [xpay snapshot, May 2026](https://www.xpay.sh/saas-pricing/subiz/)

**Fchat**
- The official homepage lists Messenger, **Zalo cá nhân**, Zalo OA, Website, WhatsApp, Telegram and Instagram, under the title "AI Chatbot Đa Nền Tảng Cho Bán Hàng Tự Động". The homepage shows no prices. — [fchat.vn](https://fchat.vn/)
- Third-party prices (unverified, sources 1–2+ years old): free Starter; Livechat 98,000đ/month; Chatbot 290,000đ/month; **Chatbot AI 980,000đ/month (~$38)**. Other sources say "from 199,000đ/month on annual plans". — [search summary citing phanmemmkt / toponseek](https://phanmemmkt.vn/fchat-la-gi); [toponseek](https://www.toponseek.com/blogs/phan-mem-chatbot-tot-nhat/)

**Sapo (Sapo Chatbot AI)**
- Sapo sells an "AI chatbot" that gives 24/7 consulting, captures leads, takes orders ("tự chốt đơn") and hands over to staff. It manages Facebook, Instagram and Zalo in one interface and offers a 7-day free trial. — [Sapo Chatbot AI page](https://www.sapo.vn/phan-mem-chat-bot.html)
- In an April 2025 update, Sapo added Zalo, Zalo OA and Instagram to Chatbot AI. — [Sapo feature update (2025, older)](https://www.sapo.vn/cap-nhat-tinh-nang/tich-hop-instagram-zalo-zalo-oa-vao-chatbot-ai)
- Price is unclear. A Sapo page shows "từ 170K/tháng" with a 7-day trial; other Sapo products show 250K–400K/month. I could not confirm the exact Chatbot AI price. — [search summary of sapo.vn pages](https://www.sapo.vn/blog/chatbot-shopee)
- Sapo itself is a POS/omnichannel platform with inventory, so the chatbot natively ties to Sapo orders and stock. — [Sapo Chatbot AI page](https://www.sapo.vn/phan-mem-chat-bot.html)

**Harasocial (Haravan)**
- Built by HARAVAN TECHNOLOGY CORPORATION: social-commerce conversation management (Facebook, Instagram) with a 24/7 chatbot. The app is free on the App Store. Sapo's comparison article lists it for Zalo OA management. **No 2026 price found.** — [App Store](https://apps.apple.com/app/id1555209490); [Sapo blog comparison](https://www.sapo.vn/blog/phan-mem-ban-hang-zalo)

**Bizfly (VCCorp)**
- Bizfly Chat gathers messages and comments from Facebook, Zalo and websites in one place. — [App Store](https://apps.apple.com/us/app/id1566526140)
- Bizfly launched **BizChatAI** industry demos (banking, education, retail, F&B) for 24/7 AI consulting and sales. **No pricing found.** — [BrandsVietnam](https://www.brandsvietnam.com/congdong/topic/doanh-nghiep-viet-co-the-trai-nghiem-loat-ai-chat-agent-theo-nganh-tu-vccorp)

**FPT.AI Conversation**
- NLP in Vietnamese, English and Japanese with intent and entity detection. Channels: Messenger, Zalo, website, Viber. API integration with enterprise systems. **No public price found.** — [FPT.AI docs](https://docs.fpt.ai/docs/vi/conversation/documentation/chatbot-introduction/starting-with-fptai)

**MISA AMIS / AVA**
- AVA is the AI assistant inside MISA AMIS. CEOs and managers can pull reports with one command, and it automates accounting tasks. In June 2026, MISA said it is moving to in-house models (MISA AI 1.0, 1.0 Plus, 2.0). **No price found.** — [MISA AMIS](https://amis.misa.vn/185121/nen-tang-quan-tri-doanh-nghiep-dau-tien-tich-hop-ai-tai-viet-nam/); [MISA AI factory, June 2026](https://amis.misa.vn/278249/misa-xay-dung-nha-may-ai-tren-nen-tang-nvidia/)

**Other commercial Zalo AI bots**
- **WorkGPT AI ZaloBot** (Zalo personal + OA customer-service bot): basic plan "5 Million VND / Year" (~$192), trial 20,000 VND/day. Source is a search-engine summary; the direct page could not be fetched. — [WorkGPT ZaloBot](https://workgpt.ai/en/workgpt-ai-zalobot/)
- **Chative.io** added a Zalo OA channel to its inbox and supports publishing Coze bots to Zalo OA. — [Chative release note](https://chative.io/blog/zalo-messenger-release/); [Coze → Zalo OA](https://chative.io/blog/publish-a-coze-bot-on-zalo-oa/)

### Inferences
- **What the buyer actually pays on the platform side:** a small shop on Zalo OA plus an AI bot needs at least the OA Tăng trưởng tier (2.5M/year) on top of the chatbot vendor's fee. A reseller should build this into its quotes.
- None of the incumbents covers both customer-facing sales and owner-facing assistance. Pancake, Subiz, Fchat, Sapo and Harasocial focus on customer-facing selling and customer service. MISA AVA is owner-facing but locked inside MISA AMIS.
- Prices cluster at about 1–8M VND/year for SaaS inboxes with AI tiers. A custom "AI employee" therefore has to justify its price with deeper integration, owner assistance or Zalo personal support, not basic auto-reply.
- No vendor publicly advertises white-label or reseller terms. A reseller would either have to negotiate privately or build its own stack. Pancake, Subiz and Fchat may have unpublished partner deals.

### Gaps
- **AhaChat:** no pricing or feature information surfaced in any search. Status and pricing are unknown.
- **Base.vn, GoHub, Vbot, Viettel AI, Zalo Cloud AI:** not researched in depth because of time and rate limits. Their pricing is unknown.
- I could not verify 2026 AI-tier prices for Pancake/Botcake, Harasocial, Bizfly, FPT.AI or Fchat.
- For most vendors I could not confirm KiotViet or bank-payment-check integrations (Sapo's own POS aside).
- I found no information on any vendor's reseller, agency or white-label program.

---

## 3. Low-code and self-hosted building blocks used in Vietnam (n8n, Dify, Flowise, Chatwoot, Typebot, Botpress, Voiceflow): Zalo connectors, costs and reselling licenses

### Takeaway
Vietnamese builders mostly assemble **n8n workflows** or **agent runtimes (OpenClaw, Hermes)** on top of **zca-js**, the unofficial MIT library for personal Zalo, or the **official Zalo OA / Bot API**. Community n8n nodes exist for both. **Licensing matters for a reseller.** n8n's Sustainable Use License bars charging others for access or hosting. Dify bars running a multi-tenant environment without written permission and bars removing its logo from the frontend. Typebot's FSL bars competing commercial use. Flowise is Apache-2.0 apart from its enterprise folder. Chatwoot is MIT apart from its enterprise folder. MIT projects (zca-js, vuhai2002/zalo-agent) are the freest to white-label.

### Cited Findings

**Zalo connectors and community projects**
- **zca-js:** an unofficial Zalo personal-account API that "simulates the browser" on Zalo Web. MIT license. Its maintainers warn accounts may be locked or banned. — [npm zca-js](https://npmjs.com/package/zca-js); [jsDelivr README](https://cdn.jsdelivr.net/npm/zca-js@2.2.0/README.md)
- **n8n community nodes:**
  - `n8n-nodes-zalo-tools`: personal account via zca-js. QR login node; Zalo Group and Zalo User nodes.
  - `n8n-nodes-zalo-user-v3` and `n8n-nodes-zl-sumo`: personal Zalo. Features not verified.
  - `n8n-nodes-zalo-platform`: official Zalo Bot Platform / OA.
  — [n8n-nodes-zalo-tools README](https://unpkg.com/n8n-nodes-zalo-tools@0.6.1/README.md); [zalo-user-v3](https://www.jsdelivr.com/package/npm/n8n-nodes-zalo-user-v3); [zl-sumo](https://www.jsdelivr.com/package/npm/n8n-nodes-zl-sumo); [zalo-platform](https://www.neura.market/ai-tools-directory/ai-agents/n8n-nodes-zalo-platform)
- **Agent-framework Zalo plugins on GitHub (all 2026):**

  | Repo | What it is | Stars |
  |---|---|---|
  | monas-team/zaloclaw | Full-featured OpenClaw Zalo channel plugin built on zca-js | 24 |
  | cuongdev/hermes-zalo-plugin | Hermes Agent ↔ personal Zalo via zca-js | 46 |
  | hoanghd218/hermes-zalo | Hermes Zalo plugin, "149 Zalo actions" | not noted |
  | luonghaianh1208/2anh-zalo-bot | 24/7 Zalo group bot for Hermes | 33 |
  | PhucMPham/zalo-agent-cli | "CLI tool for Zalo automation — multi-account, proxy support, bank transfers, QR payments" | 168 |
  | ardennguyen/zalo-agent-cli | Fork of zalo-agent-cli adding OA support | not noted |
  | ardennguyen/zalo-mcp | MCP server for zalo-agent-cli | not noted |
  | tanviet12/chat-quality-agent | Go; AI analysis of customer-service chat quality across Facebook Messenger and Zalo | 310 (289 forks) |
  | Genesis-ryan-84-0567536339/heo-agent-free | "Executive AI Assistant Suite on Zalo (Bé Heo)" | not noted |
  | DKMgaming/zalo-multi-cskh-ai | Pinecone + multi-account customer-service AI | not noted |

  — [GitHub repository search "zalo agent"](https://github.com/PhucMPham/zalo-agent-cli); [zaloclaw](https://github.com/monas-team/zaloclaw); [hermes-zalo-plugin](https://github.com/cuongdev/hermes-zalo-plugin); [hermes-zalo](https://github.com/hoanghd218/hermes-zalo); [chat-quality-agent](https://github.com/tanviet12/chat-quality-agent); [heo-agent-free](https://github.com/Genesis-ryan-84-0567536339/heo-agent-free)
- **OpenClaw + Zalo:** Vietnamese hosting companies publish guides. Vietnix shows free Zalo bots with OpenClaw + Ollama (Kimi 2.5). It contrasts the Zalo OA route (verification "vài ngày đến vài tuần") with Zalo Bot Creator, which is quick but suited to testing or personal use, not large-scale customer service. OpenClaw itself is free; users pay LLM API costs. — [Vietnix OpenClaw-Zalo](https://vietnix.vn/openclaw-zalo/); [LongVan guide](https://longvan.net/blogs/openclaw-ai/openclaw-ket-noi-zalo); [AZDIGI guide 2026](https://azdigi.com/blog/tri-tue-nhan-tao/huong-dan-openclaw)
- **Chatwoot:** I found **no native Zalo channel** in any search. Zalo OA inbox options do exist in Chative, Viindoo's Odoo module `viin_zalo` and a Zendesk Zalo app. — [Chative](https://chative.io/blog/zalo-messenger-release/); [Viindoo viin_zalo](https://viindoo.com/apps/app/18.0/viin_zalo); [Zendesk Zalo](https://supportapps.org/zendesk/zalo-integration)

**Licenses (primary LICENSE files)**
- **n8n (Sustainable Use License):** "You may use or modify the software only for your own internal business purposes or for non-commercial or personal use." "You may distribute the software or provide it to others only if you do so free of charge for non-commercial purposes." Files with ".ee." in the name need an n8n Enterprise License. — [n8n LICENSE.md](https://raw.githubusercontent.com/n8n-io/n8n/master/LICENSE.md)
  - Secondary interpretation: n8n support reportedly told a consultant that consulting and running workflows for clients is allowed. "What's not allowed: giving clients access to n8n, reselling it, or white-labeling it." Embedding n8n in a product requires the paid Embed license. — [n8n community forum](https://community.n8n.io/t/can-i-use-n8n-cloud-to-manage-workflows-for-multiple-consulting-clients/236130?tl=en); [nordflux guide](https://nordflux.de/en/guides/n8n-sustainable-use-license-erklaert); [horadecodar](https://horadecodar.com.br/licenca-n8n/)
- **Dify (Apache 2.0 with extra conditions):**
  - Current LICENSE: "Unless explicitly authorized by Dify in writing, you may not use the Dify source code to operate a multi-tenant environment." "one tenant corresponds to one workspace."
  - You "may not remove or modify the LOGO or copyright information in the Dify console or applications" (frontend = `web/`).
  - "Dify may be utilized commercially, including as a backend service for other applications."
  — [Dify LICENSE](https://raw.githubusercontent.com/langgenius/dify/main/LICENSE)
  - Dify's docs use older, narrower wording: "multi-tenant SaaS service that is similar to the Dify.AI service edition". This **conflicts** with the broader current LICENSE text. — [docs.dify.ai open-source policy](https://docs.dify.ai/policies/open-source)
  - An October 2025 OSI license-discuss thread disputes whether this counts as open source. — [OSI list](https://lists.opensource.org/pipermail/license-discuss_lists.opensource.org/2025-October/022423.html)
- **Flowise:** Apache 2.0, except `packages/server/src/enterprise` and certain files such as IdentityManager.ts, which are under a Commercial License. — [Flowise LICENSE.md](https://raw.githubusercontent.com/FlowiseAI/Flowise/main/LICENSE.md)
- **Typebot:** FSL-1.1-Apache-2.0. It prohibits "Competing Uses", meaning making the software available in a commercial product or service that substitutes for it or offers substantially similar functionality. Each version converts to Apache 2.0 two years after release. — [Typebot LICENSE](https://raw.githubusercontent.com/baptisteArno/typebot.io/main/LICENSE)
- **Chatwoot:** MIT (Expat), except the `enterprise/` directory, which has its own license. — [Chatwoot LICENSE](https://raw.githubusercontent.com/chatwoot/chatwoot/develop/LICENSE)

### Inferences
- **For a reseller serving many clients:**
  - (a) Running one shared n8n instance and charging clients for access breaches the SUL. Safer options are building client workflows on the reseller's own internal instance and selling the *service* (a grey area to confirm with n8n), installing a separate n8n in each client's environment for that client's internal use, or buying an Embed license.
  - (b) A single multi-workspace Dify serving many clients needs Dify's written permission. One single-workspace instance per client looks permissible, but the reseller must keep the Dify logo in the UI.
  - (c) Typebot cannot be offered as a competing chatbot-builder SaaS.
  - (d) MIT/Apache components (zca-js, vuhai2002/zalo-agent, Flowise core, Chatwoot core, OpenClaw, which press calls free) carry the least licensing risk for white-labeling.
- Self-hosting costs are dominated by VPS and LLM tokens, not licenses. Vietnamese OpenClaw VPS offers run about 250k–1M VND/month (see Q4).
- Personal-Zalo automation always relies on unofficial zca-js-type libraries, so account-ban risk sits with the client. The official alternatives are Zalo OA (paid tier for API access) and Zalo Bot API (fewer features, per Minh Số's page).

### Gaps
- I did not verify current cloud prices for n8n, Dify, Botpress or Voiceflow, or licenses for Botpress and Voiceflow, because of time and rate limits.
- I found no evidence of a Chatwoot Zalo connector. A custom one via Chatwoot's API inbox might be possible; this is unverified.
- I could not open Vietnamese builder discussions on Facebook groups, Viblo or Tinhte about which stack is most popular. The tool mix above comes from GitHub and hosting-company guides.

---

## 4. Vietnamese agencies and freelancers selling "AI agent cài đặt cho doanh nghiệp": setup and monthly price points

### Takeaway
Price points run **from a few million VND one-time to about 100M VND setup + 8M VND/month**:

| Tier | Typical offer | Setup | Monthly |
|---|---|---|---|
| Low | Ready-made n8n workflow or OpenClaw install | ~1.5M–20M VND | VPS ~250k–1M VND; LLM API and Zalo OA message fees usually extra |
| Mid | SaaS bot subscription | – | ~5M VND/year |
| High | Agency "AI employee" with CRM/inventory integration | 85–100M VND | 8M VND flat |

Courses that teach people to build Zalo AI staff themselves sell for under 3M VND.

### Cited Findings
- **MONA Media, "Chatbot AI" (page modified 26 Jul 2026):**
  - Gói chuẩn: **100,000,000đ one-time setup + 8,000,000đ/month flat**, not tied to message volume (~$3,850 + ~$308/month).
  - Includes industry knowledge loading, links to existing data, lead scoring, handoff of hard questions with a summary, and **"Nối thẳng CRM, kho và đơn hàng, trả lời bằng số thật"**. One Website bubble channel is included.
  - Each extra channel (Fanpage, Zalo OA, Messenger, call center) costs 5,000,000đ setup and the monthly fee is unchanged. **Zalo personal is not among the supported channels.**
  - The page also lists LLM token prices per million tokens.
  — [MONA chatbot AI](https://mona.media/chatbot-ai/)
  - **Conflict:** an earlier search snippet of the same site showed setup at 85M, discounted 30% to 59.5M for the "first 100 businesses", plus a "Webmaster AI agent từ 2tr/tháng". Pricing has evidently changed over 2026. — [MONA (search snippet)](https://mona.media/chatbot-ai/)
- **Web Media, n8n "Chatbot Zalo AI Agent" workflow:**
  - Workflow: 1,450,000đ excl. VAT (~$56).
  - "Trọn gói cài đặt và triển khai": +1,500,000đ.
  - 1-on-1 training: 3,000,000đ.
  - Excludes Zalo OA message fees and AI API costs (OpenAI, Gemini, Claude).
  - Source is a search snippet.
  — [webmedia.com.vn](https://webmedia.com.vn/shop/workflow-n8n-chatbot-zalo-ai-agent/)
- **WIFIM JSC, OpenClaw installation service (page modified 22 May 2026).** Token costs are passed through "giá theo provider":

  | Package | Setup | Server per month | Skills | Specs / support |
  |---|---|---|---|---|
  | Setup | 2,000,000đ | 250k | 1 | 2GB RAM, 1 day support |
  | Simple | 4,000,000đ | 300k | 4 | 1 month support |
  | Basic | 7,000,000đ | 450k | 10 | – |
  | Standard | 9,000,000đ | 600k | 15 | – |
  | Advance | 14,000,000đ | 700k | 22 | – |
  | Premium | 20,000,000đ | 1,000,000đ | 30 | 64GB RAM |
  | Custom | Contact | – | – | – |

  — [wifim.vn](https://wifim.vn/dich-vu-cai-dat-openclaw/)
- **TVT Agency, "VPS OpenClaw" sales and multichannel assistant:** 249,000đ–999,000đ/month infrastructure, "cài đặt sẵn 1-click", multichannel live in 24–48h. — [tvtagency.com (search snippet)](https://tvtagency.com/vps-openclaw-agent/)
- **IMGroup, "Nghề AI #01: Tự tạo nhân viên AI trực Zalo cá nhân":** an online coaching program priced at 3,000,000đ, 1,000,000đ or 666,000đ (tiers not mapped). It claims the AI introduces products on Zalo personal, collects order info, **generates payment QR codes, detects when money arrives, completes the order**, pushes orders to Google Sheets or sales software via API, and runs post-purchase care. No date shown. — [imgroup.vn](https://imgroup.vn/ngheai01.html)
- **Zalo Agent (Minh Số):** self-install, installation-service and installation-with-VPS packages, all "Liên hệ báo giá". — [zalo-agent-premium-site](https://raw.githubusercontent.com/dotanminh/zalo-agent-premium-site/main/index.html)
- **WorkGPT ZaloBot SaaS:** 5M VND/year basic. — [workgpt.ai (search snippet)](https://workgpt.ai/en/workgpt-ai-zalobot/)
- An older tutorial still cites the former OA tiers needed for chatbots ("OA Nâng cao 99,000/tháng" or "Premium 399,000/tháng"). These were superseded by the June 2026 price list. — [search snippet of interits/vihatsolutions](https://interits.com/chatbot-ai-cho-zalo-oa-huong-dan-tung-buoc/); current list: [zalo.solutions](https://zalo.solutions/oa/pricing)

### Inferences
- A plausible positioning for the user, "AI assistant cài hộ" for SMEs, sits between the WIFIM and Web Media tier (2–20M setup) and MONA (85–100M setup). For example: 5–20M VND setup + 0.5–3M VND/month covering VPS, maintenance and a token allowance, with Zalo OA fees and heavy token use passed through. This is an inference from the benchmarks, not a sourced market average.
- Vendors consistently separate out LLM token costs and Zalo OA fees. Transparent pass-through pricing is the market norm.
- Training and course products under 3M VND show a parallel "teach-to-build" market. It could compete with, or feed, a reseller.

### Gaps
- I found no independent survey of agency prices. All figures come from vendor pages, some only as search snippets.
- I could not see Vietnamese Facebook-group price discussions, the main venue for freelancers, because Facebook could not be fetched.
- Vihan's "Chatbot Zalo AI Agent" and Vihat Solutions' cost article surfaced in search but were not opened, so their prices are unknown. — [vihan.vn](https://vihan.vn/Chatbot-Zalo-AI-Agent-232-bl.html); [vihatsolutions](https://vihatsolutions.com/tin-cong-nghe/chi-phi-trien-khai-zalo-chatbot-doanh-nghiep/)

---

## 5. Gaps in existing Vietnamese platforms where a custom system adds value

### Takeaway
Existing platforms are built for **customer-facing sales and customer service on Facebook and Zalo OA**. They are weaker or silent on five things:
- (1) A **personal executive assistant for the business owner**: reminders, documents, reports across tools.
- (2) **Zalo personal accounts at scale**: only partial support, and always at ban risk.
- (3) **Bank and payment confirmation** and deep inventory/POS integration outside the vendor's own ecosystem.
- (4) **Durable per-customer memory and knowledge base** under the client's control.
- (5) **Reseller and white-label terms.**

The open-source projects and the Minh Số ecosystem target several of these gaps directly. That is evidence of demand and of a crowded DIY space.

### Cited Findings
- **Owner-facing assistant is rare in commercial tools:**
  - MISA AVA offers CEO reports by command, but only inside MISA AMIS. — [MISA AMIS](https://amis.misa.vn/185121/nen-tang-quan-tri-doanh-nghiep-dau-tien-tich-hop-ai-tai-viet-nam/)
  - Open-source projects explicitly target this: "heo-agent-free — Executive AI Assistant Suite on Zalo" — [GitHub](https://github.com/Genesis-ryan-84-0567536339/heo-agent-free); andyluu98's read-only mode logs daily Zalo chats "for AI task extraction" — [GitHub](https://raw.githubusercontent.com/andyluu98/zalo-agent/main/README.md)
  - The Minh Số Zalo Agent sells reminders, document creation, research and alerts. — [zalo-agent-premium-site](https://raw.githubusercontent.com/dotanminh/zalo-agent-premium-site/main/index.html)
- **Zalo personal support is limited:**
  - MONA's 7 channels exclude Zalo personal. — [MONA](https://mona.media/chatbot-ai/)
  - Subiz allows only 3 personal Zalo accounts, on the higher tier. — [Subiz](https://subiz.com.vn/pricing.html)
  - Vendors and libraries warn of bans: zca-js — [npm](https://npmjs.com/package/zca-js); Minh Số — [site](https://raw.githubusercontent.com/dotanminh/zalo-agent-premium-site/main/index.html)
  - The official path (Zalo OA with API) requires the Tăng trưởng or Toàn diện tier. — [zalo.solutions](https://zalo.solutions/oa/pricing)
- **Payment and bank integration:**
  - The Minh Số Zalo Agent deliberately has no payment tools and advises against adding them. — [site](https://raw.githubusercontent.com/dotanminh/zalo-agent-premium-site/main/index.html)
  - DIY builders do add them: zalo-agent-cli advertises "bank transfers, QR payments" — [GitHub](https://github.com/PhucMPham/zalo-agent-cli); IMGroup's course claims auto QR plus payment detection — [imgroup.vn](https://imgroup.vn/ngheai01.html)
  - I found no commercial SaaS that advertises bank-transfer confirmation in its 2026 pricing pages. — searches returned none
- **Inventory and order depth:**
  - Sapo's chatbot ties to Sapo's own POS. — [Sapo](https://www.sapo.vn/phan-mem-chat-bot.html)
  - Pancake creates orders in chat. — [Pancake](https://pancake.vn/pricing)
  - MONA charges 85–100M VND to "nối thẳng CRM, kho và đơn hàng". — [MONA](https://mona.media/chatbot-ai/)
  - So cross-system integration (e.g. KiotViet + bank + Zalo) is expensive or ecosystem-locked.
- **Script-based legacy bots:** a Ministry of Information and Communications (mic.gov.vn) article describes Vietnamese chatbots as basic AI running pre-set scripts, which sometimes reply off-topic. Undated in the search result; likely older. — [mic.gov.vn](https://mic.gov.vn/print/news-197137398.htm)
- **Memory per customer:** open-source zalo-agent keeps per-person memory (SQLite history, rolling summaries, `save_memory` facts) — [README](https://raw.githubusercontent.com/vuhai2002/zalo-agent/main/README.md). Minh Số's agent "remembers each person" — [site](https://raw.githubusercontent.com/dotanminh/zalo-agent-premium-site/main/index.html). I found no evidence in commercial pricing pages that Pancake, Subiz or Fchat offer LLM long-term memory per customer.
- **SME context:** a Sapo survey of 15,000 sellers (Jan 2026) found tax and e-invoice compliance pressures make sellers cautious. AI adoption is seen as important but not an immediate priority. — [Sapo press (search snippet)](https://www.sapo.vn/bao-chi-noi-ve-sapo/Ky-vong-cua-nha-ban-hang-dich-chuyen-manh-sang-kenh-online-a2792.html)

### Inferences
- **Where a custom or reseller system can differentiate:**
  - "One brain, many channels" for the owner: Zalo personal + OA + Messenger + web, plus a private executive assistant.
  - Integration with Vietnamese bank notifications and VietQR, KiotViet/Sapo/Haravan inventory and Google Sheets.
  - Per-customer memory the SME owns.
  - Pricing between DIY installs (2–20M) and agency builds (85–100M).
- **Build-on options:**
  - Fork MIT projects (vuhai2002/zalo-agent, zca-js) or OpenClaw/Hermes plugins. Free license; ban risk and maintenance remain.
  - Partner with Minh Số's ecosystem. Terms are unknown and require contact.
  - Layer on Pancake/Subiz for the inbox and add a custom agent for owner tasks. aiecos-social-crm shows a Pancake → Supabase + MCP pattern.
- **Risks for a reseller:**
  - Personal-Zalo bans: mitigate by using secondary accounts and the official OA or Bot API where possible.
  - n8n and Dify license limits on multi-client hosting.
  - Fast-moving competition: many 2026 GitHub projects and Vietnamese hosting companies bundling OpenClaw VPS.
  - Payment-automation liability. Even Minh Số avoids money-transfer tools.

### Gaps
- I found no systematic Vietnamese review comparing platforms on owner-assistant features, bank integration or memory. The gaps above are inferred from feature and pricing pages and GitHub.
- I could not verify whether Pancake AI, Botcake AI or Fchat Chatbot AI include long-term per-customer memory or KiotViet and bank integrations.
- I could not check customer satisfaction or churn data for any platform.
