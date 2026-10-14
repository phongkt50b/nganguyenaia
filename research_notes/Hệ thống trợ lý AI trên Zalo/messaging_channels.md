# Messaging channels for an AI assistant in Vietnam (Zalo, Facebook/Instagram, Web, TikTok Shop) — status as of 14 Oct 2026

Scope notes for the report writer:
- Two use cases are evaluated throughout: **(UC1)** a personal assistant for the business owner on their phone (Zalo preferred); **(UC2)** a customer-service (CSKH) agent answering customers on Zalo / Facebook / web.
- USD conversions use a rounded working rate of **~26,000 VND = 1 USD**. That rate is an assumption for readability, not a sourced figure.
- Zalo's official OA prices **include 10% VAT**. Zalo's official ZBS Template Message (ZNS) prices **exclude VAT**. This difference is easy to miss.
- Tool constraint: WebFetch and curl were blocked in this environment. Pages were read through Firecrawl scrapes and search snippets. Every official page cited below was scraped in full unless marked "snippet".

---

## 1. Zalo personal accounts via unofficial libraries (zca-js, zlapi, "Zalo Agent" with nick phụ)

### Takeaway
Libraries like zca-js and zlapi reverse-engineer Zalo Web: they log in with a QR code or with copied cookies and pretend to be a browser. They still work in 2026, but they break often whenever Zalo changes its login. Zalo's terms forbid unauthorised third-party software, and users in 2026 report security warnings, wiped friend lists and locked accounts. That makes them unsuitable for a business's main account. Use them only on a disposable secondary account ("nick phụ"), and only when the official Zalo Bot or OA cannot do the job, for example reading the owner's existing personal chats.

### Cited Findings
**How it works**
- zca-js describes itself as an "Unofficial Zalo API for JavaScript" that works "by simulating the browser to interact with Zalo Web" and supports building bots on personal accounts. Its README warns: "Using this API could get your account locked or banned. We are not responsible…" — [RFS-ADRENO/zca-js on GitHub](https://github.com/RFS-ADRENO/zca-js) (search snippet); [zca-js docs (gitbook)](https://tdung.gitbook.io/zca-js); [author's Threads post: "Sử dụng thư viện này có thể vi phạm chính sách của Zalo và khiến cho tài khoản của bạn bị khóa"](https://www.threads.com/@hoanghung_1508/post/C-7NTl4TFJS)
- The Python equivalent is zlapi ("Zalo API for Python (Unofficial)"). Go ports exist (zca-go) and so do many forks. Forks are maintained independently and are not checked against upstream — [zlapi](https://github.com/Its-VrxxDev/zlapi); [zca-go](https://github.com/vuongnq9x/zca-go)
- Login is by **QR code** (loginQR) or **cookie** (loginCookie). Issues filed in 2026 show both methods in use, at zca-js v2.1.2 — [Issue #299 (Mar 2026, "Login QR… Can't login")](https://github.com/RFS-ADRENO/zca-js/issues/299); [Issue #321 (Apr 2026, cookie login fails: "có vẻ zalo đã thay đổi api login hoặc header")](https://github.com/RFS-ADRENO/zca-js/issues/321)
- **Zalo allows only one Zalo Web (zca-js) session per account.** Running two clients causes a reconnect/eviction loop — [zalo-agent-cli README](https://github.com/PhucMPham/zalo-agent-cli) (search snippet)
- OpenClaw's "Zalo Personal" channel (`zalouser`) runs zca-js in-process with QR login. Its docs carry the warning: "This is an unofficial integration and may result in account suspension or ban. Use at your own risk." — [OpenClaw zalouser docs (mirror)](https://www.neura.market/ai-agents/integrations/open-claw/docs/plugins/zalouser); [OpenClaw docs (vi)](https://docs.openclaw.ai/vi/channels/zalouser)
- Vietnamese hosting providers now publish step-by-step guides for connecting a personal Zalo account to Hermes Agent and OpenClaw. The practice has gone mainstream even though it is unofficial — [Tino: connect personal Zalo with Hermes Agent / OpenClaw (related-articles list)](https://tino.vn/blog/tao-bot-zalo-ca-nhan/)

**2026 evidence of instability and enforcement (from the zca-js issue tracker)**
- Mar 30, 2026: after a QR login, the user received a warning from **"Zalo Bảo Mật" that the account was "đang sử dụng phần mềm của bên thứ ba trái phép"** (using unauthorised third-party software). Login also failed with error -1005 "Đăng nhập không hợp lệ" — [Issue #317](https://github.com/RFS-ADRENO/zca-js/issues/317)
- Mar 9, 2026: a shop owner used zca-js to auto-reply to customers, auto-accept friend requests and send friend requests to customer phone numbers. **The entire friend list (customers) was then deleted** by Zalo, although chat history remained — [Issue #291](https://github.com/RFS-ADRENO/zca-js/issues/291)
- Jun 22, 2026: "trước quét được giờ bị zalo lock rồi" — an account was locked after scraping members of a private group (44 comments) — [Issue #359](https://github.com/RFS-ADRENO/zca-js/issues/359)
- May 22, 2026: the web session is kicked (error 3003) whenever the owner uses the Zalo app. Oct 9, 2026: a user reports being logged out after logging into Zalo PC, which worked before. The user asks whether Zalo changed its login rules — [Issue #344](https://github.com/RFS-ADRENO/zca-js/issues/344); [Issue #386](https://github.com/RFS-ADRENO/zca-js/issues/386)
- Mar 2026: OpenClaw users could not complete the QR login ("Zalo login failed: Can't login") — [Issue #301](https://github.com/RFS-ADRENO/zca-js/issues/301)
- Messages to strangers are blocked by the recipient's privacy settings (code 122). The maintainers do not support bypassing this — [Issue #307](https://github.com/RFS-ADRENO/zca-js/issues/307)

**Zalo Terms of Service**
- Zalo's terms allow it to restrict features, temporarily lock or permanently lock accounts that violate the terms. A locked user also loses access to third-party products that use Zalo login — [Zalo Điều khoản sử dụng](https://zalo.vn/dieukhoan/) (via search summary)
- Zalo does not allow third-party software or systems to access the service unless Zalo developed, authorised or approved them. A support-page summary names mass-messaging tools and auto-friending tools as grounds for locking — [code.pro.vn (Oct 2026), citing Zalo support](https://www.code.pro.vn/2026/10/tai-khoan-zalo-bi-khoa.html); [miniai.vn](https://miniai.vn/zalo-bi-khoa/) (secondary sources)
- One plugin README states plainly: "Zalo không cung cấp API cho tài khoản cá nhân và không cho phép tự động hóa (ToS)… có thể vi phạm ToS, dẫn đến khóa tài khoản" — [zaloclaw plugin (ClawHub)](https://clawhub.ai/monasprox/plugins/zaloclaw)

**The "Zalo Agent" / nick phụ pattern**
- "Zalo Agent" is not an official Zalo product. It is the name of open-source projects. andyluu98/zalo-agent, a fork of vuhai2002/zalo-agent under the MIT licence, runs a resident AI agent on Zalo. It supports **both** a personal account (through zca-js) **and** an official Zalo Bot account. Its comparison table marks the personal-account mode as having a lock risk, "chỉ dùng nick phụ" (use secondary accounts only). The Zalo Bot mode has no lock risk, but users can only reach the bot through a link, and it exposes fewer tools (7/15). The fork also adds a **read-only mode** that only logs daily chats so the AI can extract tasks — [andyluu98/zalo-agent](https://github.com/andyluu98/zalo-agent) (search summary)
- A self-hosted Zalo↔Chatwoot bridge based on zca-js warns of restrictions, locks or permanent bans and advises "Nên dùng tài khoản phụ". It notes that the Zalo OA channel uses the official API and so is outside the zca-js risk group — [diendh/zca-bridge](https://github.com/diendh/zca-bridge) (search snippet)

### Inferences
- Ban risk is **medium-to-high** and grows with automation intensity. Bulk sends, auto-friending, group scraping and messaging strangers are the behaviours most associated with locks. Low-volume replies in existing conversations appear less risky, but nobody guarantees that. Zalo now detects at least some third-party logins and sends a security warning (Issue #317).
- Even without bans, **reliability is poor**: there are frequent login breakages after Zalo changes its login (Mar–Apr 2026) and session conflicts with the owner's own phone and PC app (May–Oct 2026). A business process should not depend on it.
- The **nick phụ** approach limits the damage of a ban (a secondary SIM and account is cheap) but creates new problems. Customers must add or message a non-official personal account. Contacts and history are lost if it is locked. It is still a ToS violation, only a cheaper one to lose.
- For UC1 (owner's personal assistant), a personal-account library is only justified when the agent must **read or act on the owner's real personal Zalo chats**, for example summarising customer messages that arrive on the owner's personal Zalo. Even then, the read-only mode on a secondary account that customers also message is the lower-risk variant. For simply talking to an AI assistant, the official Zalo Bot (section 2) is strictly better.
- For UC2 (customer service), it is **not recommended** for a legitimate business. Legally it is a contract (ToS) issue rather than a criminal one as far as found, but it risks losing the customer list. There is also a data-protection angle: Vietnam's AI Law requires users to be able to tell they are talking to AI (see section 4).

### Gaps
- No official Zalo statement quantifying enforcement (ban rates, thresholds) was found. The evidence is anecdotal from GitHub issues.
- The exact full text of the current (2026) Zalo ToS clause on third-party software was not retrieved directly. The summaries come from secondary sites.
- No primary source was found for a commercial product literally named "Zalo Agent" that sells nick-phụ setups. Only open-source projects were found.
- zlapi's 2026 maintenance status was not checked.

---

## 2. Zalo Bot Platform (Zalo Bot Creator / Zalo Bot API)

### Takeaway
Zalo now runs an **official, Telegram-style Bot API**. You create a bot for free from a personal Zalo account through the "Zalo Bot Manager" OA and the "Zalo Bot Creator" mini app, get a token, and use long-polling or a webhook. Zalo's own docs include a guide to building a **personal AI assistant ("My ClawBot") with OpenClaw**. That makes it the best fit for UC1. It is still **beta** and limited: text up to 2,000 characters, images by URL, stickers, voice, typing indicator, no file or document sending, group chats still in internal testing, 3 bots per account, and no OA-style brand verification. That makes it weaker for UC2.

### Cited Findings
**What it is and how to create a bot**
- Official docs live at docs.zaloplatforms.com under "Zalo Bot – Bot hội thoại Zalo". To create a bot: open Zalo, search for the OA **"Zalo Bot Manager"**, choose **"Tạo bot"** to open the **Zalo Bot Creator** mini app, and enter a name that **must start with "Bot"** (e.g. "Bot MyShop"). The system then sends the bot info and the **Bot Token** to your Zalo account by message — [Zalo Platforms docs: Tạo Bot](https://docs.zaloplatforms.com/docs/BOT/create_bot)
- Two update mechanisms exist: **long polling (getUpdates)** and **webhook (setWebhook)** — [Tạo Bot](https://docs.zaloplatforms.com/docs/BOT/create_bot). The two are mutually exclusive. Zalo signs webhook calls with an `X-Bot-Api-Secret-Token` header. The webhook must be HTTPS and the secret must be 8–256 characters — [OpenClaw Zalo channel docs](https://docs.openclaw.ai/channels/zalo)
- API format: `https://bot-api.zaloplatforms.com/bot<BOT_TOKEN>/<functionName>`. GET and POST are both accepted, with query strings, form, JSON or multipart/form-data (multipart for uploads). Responses are JSON with `ok/result/description/error_code` — [Zalo docs: Sử dụng API](https://docs.zaloplatforms.com/docs/BOT/call_api)
- **The complete API method list** (sidebar): getMe, getUpdates, setWebhook, testWebhook, deleteWebhook, getWebhookInfo, **sendMessage, sendPhoto, sendSticker, sendChatAction, sendVoice** — [Zalo docs: sendPhoto page](https://docs.zaloplatforms.com/docs/BOT/apis/sendPhoto)
- sendMessage text is 1–2,000 characters — [Zalo docs: Giới thiệu](https://docs.zaloplatforms.com/docs/BOT). sendPhoto takes `chat_id`, `photo` (an **image URL**) and an optional caption of 1–2,000 characters — [sendPhoto](https://docs.zaloplatforms.com/docs/BOT/apis/sendPhoto)
- Error codes include **429 "Quota exceeded – Vượt quá giới hạn sử dụng API cho phép"**. No numeric quota is published — [Zalo docs: Bảng mã lỗi](https://docs.zaloplatforms.com/docs/BOT/error_code)
- The intro page markets NLP, "carousel", buttons, quick replies and forms. **None of these appear in the API reference** (11 methods only) — [Zalo docs: Giới thiệu](https://docs.zaloplatforms.com/docs/BOT) vs [API list](https://docs.zaloplatforms.com/docs/BOT/apis/sendPhoto)
- A community Rust client notes: no file uploads (media by URL), no keyboards, no message edit or delete, no reply threads, no group management — [docs.rs zalo_bot](https://docs.rs/zalo-bot/latest/zalo_bot/) (search snippet; may predate later API additions)
- Domain change: older community SDKs use `bot-api.zapps.me` / bot.zapps.me, while current official docs use **bot-api.zaloplatforms.com**. Integrators should use the zaloplatforms.com host — [go-zalo-bot-api](https://pkg.go.dev/github.com/nduyhai/go-zalo-bot-api); [nhanchaukp/zalo-bot-sdk](https://github.com/nhanchaukp/zalo-bot-sdk); [Zalo docs: Sử dụng API](https://docs.zaloplatforms.com/docs/BOT/call_api)

**Personal-assistant use is officially supported**
- Zalo's "Best Practices" section starts with **"Xây dựng trợ lý cá nhân với OpenClaw"**. The plugin `@zalo-platforms/openclaw-zaloclawbot` (installed with `npx -y @zalo-platforms/openclaw-zaloclawbot-cli install`) shows a QR code. You scan it with Zalo and tap "Tạo My ClawBot", and **My ClawBot** appears in your Zalo chat list as an AI assistant connected to your OpenClaw. You configure your own AI provider (OpenAI, Claude, Gemini…) — [Zalo docs: Build personal assistant with OpenClaw](https://docs.zaloplatforms.com/docs/BOT/best-practices/build-personal-assistant-with-open-claw)
- OpenClaw ships Zalo Bot as a bundled plugin. It supports DMs (direct messages), pairing/allowlist access control, long-polling or webhook, a 2,000-character chunk size and a 5 MB default media cap (an OpenClaw setting). Status: "experimental" — [OpenClaw Zalo docs](https://docs.openclaw.ai/channels/zalo)
- The Hermes Agent plugin catalog lists a Zalo platform plugin with the same Bot API endpoints — [Hermes zalo-platform plugin](https://hermes-agent.nousresearch.com/docs/plugins/zalo-platform) (search snippet)

**Groups**
- Official doc: **"Tính năng đang trong giai đoạn thử nghiệm nội bộ, sẽ được ra mắt trong thời gian tới"** (still in internal testing). Once released: the group admin taps the bot's invite link and confirms "Thêm Bot vào Nhóm". The bot receives only messages that **reply to (quote) the bot** or **@mention** it, and the group ID comes from `chat.id` — [Zalo docs: Bot in group chat](https://docs.zaloplatforms.com/docs/BOT/best-practices/build-bot-interaction-with-group)
- Sources conflict. OpenClaw lists groups as "Supported (mention-gated)" but notes "on some Marketplace-bot setups the bot could not be added to a group at all" — [OpenClaw Zalo docs](https://docs.openclaw.ai/channels/zalo). Tino (Jul 2026) says the bot management screen shows a group-invite link but that group conversations are Beta and may be unstable — [Tino](https://tino.vn/blog/tao-bot-zalo-ca-nhan/)

**Limits and beta status (secondary source, Jul 2026)**
- The Zalo Bot Creator mini app is labelled **Beta**. Each Zalo account can own a **maximum of 3 bots**. Bots **cannot be renamed or deleted** yet. Each bot gets a deeplink (usable as a QR code on a website), a group-invite link, a webhook URL, a secret token and a token-reset function. The creator must agree to Zalo's terms and follow the Zalo Bot Manager OA. No OA or business registration is needed — [Tino: Hướng dẫn tạo bot Zalo cá nhân (23/07/2026)](https://tino.vn/blog/tao-bot-zalo-ca-nhan/)
- The andyluu98/zalo-agent comparison says the official Zalo Bot mode carries **no lock risk**, but users can only message the bot through a link, and fewer tools are available — [andyluu98/zalo-agent](https://github.com/andyluu98/zalo-agent) (search summary)

**Pricing**
- No pricing page for Zalo Bot was found in the official docs. A third-party comparison describes the Creator as needing only a personal Zalo account, giving an immediate bot, and being completely free — [askany.com](https://askany.com/blog/zalo-chatbot) (third-party, via search summary)

### Inferences
- **UC1 (owner's assistant): strong fit.** It is official, free (no published fee), and set up in minutes from a phone. Zalo itself documents the OpenClaw personal-assistant pattern. The owner chats 1:1 with "Bot <Tên>" in Zalo. Gaps to design around: files can't be sent (share links to PDFs or Drive instead), replies cap at 2,000 characters (chunk them), there is no rich UI (buttons and carousels are absent from the API), and group use is not production-ready.
- **UC2 (customer service): possible but second-tier.** Customers must find the bot through a deeplink or QR, the bot name must start with "Bot", it lacks OA branding and verification, and it lacks OA features such as CRM, staff inbox, ZNS and broadcast. The 3-bot cap and beta status add platform risk. It is reasonable for a pilot or an internal staff bot, not as the main customer channel.
- Undocumented 429 quotas mean high-volume use needs back-off logic and testing.
- The Bot API looks deliberately Telegram-like (method names, `bot<TOKEN>` URL scheme, getUpdates/webhook exclusivity). Telegram bot frameworks port easily.

### Gaps
- No official launch date, SLA, quota numbers or pricing/ToS page specific to Zalo Bot was found.
- Whether a bot can **initiate** a message to a user who never messaged it was not confirmed. The Telegram-like model suggests not, but this is unverified.
- Whether the bot can **receive** files or documents from users (inbound media types) was not confirmed from official webhook docs.
- Max image size and allowed formats for sendPhoto and sendVoice were not documented on the pages read.

---

## 3. Zalo Official Account (OA): verification, 2026 packages, messaging rules and costs, ZBS Template Message (ZNS), API/webhook/tokens

### Takeaway
**Zalo replaced its OA packages on 1 June 2026.** The new tiers are Cơ bản (free), Tiêu chuẩn (1.0M VND/year), **Tăng trưởng (2.5M VND/year)** and Toàn diện (6.0M VND/year). Prices include VAT and require a *verified* OA. **API integration (needed for any AI agent or third-party inbox) requires at least Tăng trưởng**, so 2.5M VND/year (≈ $96) is the realistic minimum. Messaging rules: after the customer's last interaction, the OA gets **8 free "tin tư vấn" (consultation messages) within 48 hours**. After that, and up to **7 days**, each consultation message costs **55 VND** or comes out of the package quota (500 per month on Tăng trưởng, 2,000 on Toàn diện). After 7 days the OA can only reach the customer with paid template messages (ZBS Template Message, formerly ZNS, from about 300 VND before VAT).

### Cited Findings
**2026 package prices (official, effective 01/06/2026, VAT included)**
- **Tiêu chuẩn: 1,000,000 VND/year** (~83,000 VND/month): advanced chat UI, customer info management, mobile chat tool, max **5 staff** — [zalo.solutions OA pricing](https://zalo.solutions/oa/pricing)
- **Tăng trưởng: 1,400,000 VND/6 months or 2,500,000 VND/year** (~208,000 VND/month): everything in Tiêu chuẩn plus **Chatbot & automation** and **API integration with internal or third-party systems**, max **15 staff** — [zalo.solutions OA pricing](https://zalo.solutions/oa/pricing)
- **Toàn diện: 3,400,000 VND/6 months or 6,000,000 VND/year** (~500,000 VND/month): all advanced features, highest API limits, branding and service-quality features, max **100 staff**, priority support with a response within 2 hours — [zalo.solutions OA pricing](https://zalo.solutions/oa/pricing)
- **Cơ bản**: free (3 staff accounts) — comparison table at [zalo.solutions OA pricing](https://zalo.solutions/oa/pricing)
- The page heading says the prices are "Dành cho OA đã xác thực" (for verified OAs). Purchase steps: (1) verify the OA, (2) link a Zalo Business Solutions (ZBS) account and top it up, (3) buy the package. Fees are non-refundable. If not renewed, the OA drops back to Cơ bản — [zalo.solutions OA pricing FAQ](https://zalo.solutions/oa/pricing)
- FAQ, verbatim intent: "Để liên kết ứng dụng với OA thông qua API, bạn cần sử dụng **Gói tăng trưởng hoặc toàn diện**." (To link an app to the OA through the API, you need the Tăng trưởng or Toàn diện package.) Sending ZBS Template Messages through the ZBS Account (OpenAPI) is also limited to Tăng trưởng/Toàn diện, or the old Trial/Nâng cao/Premium packages — [zalo.solutions OA pricing FAQ](https://zalo.solutions/oa/pricing)
- Comparison table, Cơ bản / Tiêu chuẩn / Tăng trưởng / Toàn diện — [zalo.solutions OA pricing](https://zalo.solutions/oa/pricing):
  - Free consultation messages outside the 48h window per month: –/–/**500**/**2,000**
  - Zalo Open API integration: –/–/✓/✓. **API rate limit**: –/–/**100 req/min**/**2,000 req/min** (excluding ZBS Template Message send APIs)
  - Authorised apps per OA: 1/1/3/unlimited
  - Chatbot scenarios: –/–/10/50
  - OA-owned group chats included (GMF-100): –/–/1/3
  - Free broadcast messages received per follower per month: –/–/4/4
  - Posts per month: –/15/unlimited/unlimited
  - Staff accounts: 3/5/15/100
- **Out-of-package costs**: consultation message outside 48h **55 VND/message**. Group chat maintenance per month: GMF-10 25,000, GMF-50 55,000, GMF-100 75,000, GMF-1000 300,000 VND. **Messages from the OA to a group are free until 31/12/2026** — [zalo.solutions OA pricing](https://zalo.solutions/oa/pricing)
- **Older packages are now superseded.** Before 1/6/2026, third-party pages list Nâng cao at 99,000 VND/month and Premium at 399,000 VND/month, valid only until existing subscriptions expire — [miniai.vn comparison](https://miniai.vn/so-sanh-goi-zalo-oa-cu-va-moi/); [v9.com.vn](https://v9.com.vn/bang-gia-zalo-oa-2026/) (search summaries). A 2022 article lists Trial at 10,000 VND/month, Nâng cao at 59,000 VND and Premium at 399,000 VND/month — [doanhnghiephoinhap.vn](https://doanhnghiephoinhap.vn/zalo-thu-phi-nguoi-dung-tu-ngay-1-8.html). **Treat any blog quoting these as outdated.**
- Conflict: one blog (Mini AI) cites ~72 VND per message for over-quota consultation messages. The official page says **55 VND/message (VAT included)** — [search summary of miniai.vn](https://miniai.vn/bang-gia-zalo-oa-moi-nhat-2026/) vs [zalo.solutions](https://zalo.solutions/oa/pricing)

**Messaging rules (message types)**
- Official policy page "Chính sách gửi các loại tin từ Zalo Official Account" (effective 20/06/2023, article dated 14/01/2024) — [oa.zalo.me policy](https://oa.zalo.me/home/resources/news/thong-bao-chinh-sach-gui-tin-va-quy-dinh-phi-gui-tin_1433049880779375099):
  - An OA can message a user **only after the user interacts** with it or allows interaction. "Interaction" means following the OA, messaging it, calling it or accepting its call, or tapping a menu or chatbot CTA.
  - **Tin Tư vấn (consultation)**: used to answer customer questions. Recipients can be followers or non-followers. **The user's last interaction must be within 7 days.** Free-form content: text, image or GIF, sticker, info-request form, **file**. Sent through OA Manager (web and mini app) or OA OpenAPI. Pricing: **the first 8 messages within 48 hours of the user's last interaction are free**, and the counter resets each time the user interacts again. After 8 messages, or outside 48h, each message costs **55 VND**. Package quotas cover messages outside the 8-in-48h allowance. Auto-greetings and OA chatbot replies count as consultation messages.
  - **Tin Giao dịch (transaction)**: order, appointment and invoice updates, in a fixed transaction format, through OpenAPI. **165 VND/message** (2024 figure; this item no longer appears on the 2026 pricing page, which points to ZBS Template Message instead).
  - **Tin Truyền thông (promotional)**: followers only. Free, but capped by the package's broadcast quota (4 per follower per month on Tăng trưởng/Toàn diện under the 2026 table).
  - Zalo monitors user **report ("báo xấu") rates** and takes action against OAs that misuse message types.
- The 2026 pricing page still defines the consultation quota as "tin tư vấn ngoài khung 48h" and links to the same policy page. That indicates the 48h / 8-free / 7-day framework remains current — [zalo.solutions OA pricing](https://zalo.solutions/oa/pricing)

**OA verification (xác thực)**
- An OA must submit its verification dossier **within 14 days of creation**. There are three ways to verify: (1) by the registered business or household-business name, or the store/location name; (2) by a trademark registered with Cục Sở hữu trí tuệ; (3) by another name, or for conditional business lines, adding "yếu tố kinh doanh" documents. Required: a scan or photo of the original **Giấy đăng ký kinh doanh** (business registration, all pages, with seal and signature), the legal representative's name, Zalo phone number and CCCD/passport. For dossiers 1A, 2A and 3A, the legal representative must confirm on Zalo **within 48 hours** or use the signed "Công văn xác thực". Review takes **2–3 working days** and can run longer — [oa.zalo.me: Hướng dẫn xác thực](https://oa.zalo.me/home/documents/guides/huong-dan-xac-thuc_70)
- Household businesses (hộ kinh doanh) qualify with a household business registration certificate and the owner's CCCD. Third-party guides estimate approval in 24–48 hours — [esms.vn help center](https://helpcenter.esms.vn/thong-tin-dich-vu/tra-cuu-thong-tin-ho-so/zalo/zalo-oa/quy-trinh-tao-zalo-oa); [misaeshop.vn](https://www.misaeshop.vn/38111/cach-dang-ky-zalo-oa/) (search summaries)

**ZBS Template Message (formerly ZNS — Zalo Notification Service)**
- Official prices for successfully delivered messages, **excluding VAT** — [zalo.solutions Business Message pricing](https://zalo.solutions/zns/pricing):
  - Authentication (OTP) template **400**; payment-request template **300**; voucher template **400**; bus-ticket (Beta) **300**; journey templates for delivery/transport industries **from 200**; **all other types 300 VND**
  - **Sending by UID (to users who already interacted with the OA) is subsidised.** Example: a custom template costs 300 VND by phone number but **210 VND by UID**. Cards show "Template tuỳ chỉnh từ 210đ", "xác thực từ 280đ"
  - Buttons: default (copy / transfer / view details) 0. Links to OA page, Mini App or OA post, and the business website: the first button is free and each further one 100 VND. Phone call 200, business app 600, other website or Mini App 500, other app 1,000, app-download landing page 3,000
  - Add-ons: image **300**, reply button (Beta) 100. Advanced: call-permission request 400
- **Conflict and outdated info:** many third-party 2025–2026 pages still quote older ZNS prices (≈300 for OTP/payment/voucher, 200 for others, 120 for public administration, image +200, other-app button 600–700) — [smsthuonghieu.com](https://www.smsthuonghieu.com/gia-zns/); [zns.nhanh.vn](https://zns.nhanh.vn/bang-gia-dich-vu-zbs-va-cach-tinh-phi-zbs-template-message-cuc-de-n97625.html) (search summaries). **The official page above supersedes them.**

**API, webhook, tokens**
- OA API auth uses **OAuth v4** (`https://oauth.zaloapp.com/v4/oa/access_token`). Each access token comes with a refresh token. **A refresh token is single-use**: it returns a new access and refresh token pair and invalidates the old access token. **Refresh token lifetime is 3 months** from issue — [developers.zalo.me: Xác thực và ủy quyền cho Ứng dụng](https://developers.zalo.me/docs/api/official-account-api/phu-luc/official-account-access-token-post-4307) (search excerpt)
- The access token lifetime is commonly reported as **~25 hours**, so it must be refreshed automatically — [Facebook developer group post](https://www.facebook.com/groups/852586990732832/posts/929097966415067/) (community, unverified against the official doc)
- Rate limits are 100 req/min (Tăng trưởng) and 2,000 req/min (Toàn diện), as listed above — [zalo.solutions](https://zalo.solutions/oa/pricing)
- Off-the-shelf connectors exist. For example, the Viindoo/Odoo `viin_zalo` module links an OA with App ID and Secret Key, **auto-refreshes tokens**, and creates one Discuss channel per Zalo customer. There is also a Zendesk Zalo app, and Chative supports Zalo OA — [Viindoo viin_zalo](https://viindoo.com/apps/app/18.0/viin_zalo); [Zendesk Zalo integration](https://www.zendesk.com/se/marketplace/apps/support/1022071/zalo-integration/); [Chative](https://chative.io/blog/zalo-messenger-release/) (search summaries)

### Inferences
- **Minimum realistic cost for an AI customer-service agent on Zalo OA:** Tăng trưởng at **2.5M VND/year (≈ $96/year, ≈ 208k VND or $8 per month)**, plus 55 VND (≈ $0.002) per consultation message outside 48h once the 500-per-month quota runs out, plus optional ZBS template messages at about 210–400 VND + VAT (≈ $0.009–0.017) each for proactive notifications. A small shop with normal reply patterns rarely pays much beyond the package. Most AI replies happen within 48h and are free up to 8 per user interaction.
- **Rule of thumb for the AI agent design:** answer within 48 hours and stay under 8 messages per user turn. Long multi-message AI answers burn the free quota faster, so merge them into fewer messages. Never message after 7 days of silence except with an approved template. Use templates (ZNS) for order or appointment notifications.
- **Setup time:** about 1 week end to end (create OA, verify in 2–3 working days, link ZBS, buy package, create app on developers.zalo.me, authorise, set up webhook and token refresh). **Documents:** business or household-business registration plus the legal representative's CCCD.
- **Ban risk: low**, provided the OA follows message-type rules and keeps user report rates low.
- **For UC1 (owner's personal assistant)** an OA is overkill: it needs business verification and a paid package, and is designed for business-to-customer use. The Zalo Bot is the right tool.
- The old 2023–2025 pricing (Nâng cao/Premium monthly packages) appears all over Vietnamese blogs. The report should explicitly warn readers that it changed on 1/6/2026.

### Gaps
- Whether OA verification itself carries a fee in 2026: no official statement was found. The official guide mentions no fee, and the pricing page implies verification is a prerequisite rather than a paid item.
- The official access-token lifetime (≈25h) was not confirmed from the developers.zalo.me page text retrieved.
- The current (2026) status and price of "Tin Giao dịch" (165 VND in 2024) is unclear. It may have been folded into ZBS Template Message.
- No official OA webhook event-list page was scraped. Webhooks are standard (user_send_text, user_send_image, follow, etc.) per general knowledge, but this was not verified in this session.

---

## 4. Facebook Messenger & Instagram Messaging (Meta)

### Takeaway
Messenger and Instagram DMs are **free to use for replies inside the 24-hour standard window**. The window opens each time the customer messages, and lasts up to 7 days after a Click-to-Messenger ad. A **human** (not the bot) can reply up to 7 days later with the HUMAN_AGENT tag. Outside those windows, businesses must use paid **Marketing Messages** (available in Vietnam) or approved **Utility templates**. Three old message tags were deprecated in early 2026. A self-built app needs **Advanced Access for `pages_messaging` (App Review) plus Business Verification** to talk to the public. Many SMEs avoid that by connecting through an already-approved partner tool (Pancake, Chatwoot Cloud, etc.). Meta's policy requires automated bots to respond within 30 seconds, and requires bot disclosure where law requires it. Vietnam's AI Law (effective 1/3/2026) effectively does.

### Cited Findings
**Policy (official page updated Sep 28, 2026)** — [Messenger Platform and IG Messaging API policy](https://developers.facebook.com/documentation/business-messaging/messenger-platform/policy)
- **Standard messaging window: 24 hours** for most user actions. When the first message comes after a **Click-to-Messenger / Click-to-Direct ad, the window may last up to 7 days**. Messages inside the window **may contain promotional content**. Window-opening actions include the user messaging the Page, tapping Get Started, opening m.me or ig.me links with a ref, using plugins, and **reacting to a message**.
- **Message tags** allow important, personally relevant 1:1 updates outside the window for approved use cases only. Misuse "may result in restrictions". The **Human Agent tag** lets businesses **manually** respond within **a separate 7-day period**.
- **One-time Notification** (Messenger only, not IG): a user opts in and the Page gets a single-use token, valid up to 1 year.
- **Private replies**: when a user comments on a Page post or writes a visitor post, the Page may send **one** private message through Messenger **within 7 days**. IG has its own private-replies feature.
- **Responsiveness policy for "automated" bots**: respond to **any and all** user input (free text, quick replies, buttons, menu), **within 30 seconds**. Bot types are Automated, Manual Reply and **Hybrid** (automated plus human handover). Violations lead to notices in the Page Support Inbox, and messaging is limited after 7 days without a fix.
- **AI/automation disclosure**: "When required by applicable law, automated chat experiences must disclose that a person is interacting with an automated service" — at the start of a conversation, after a long lapse, and when a chat moves from human to automated. Suggested wording: "I'm the [Page Name] bot". Meta recommends disclosure as best practice even where it is not legally required.

**Deprecations and new paid options (official FAQ, updated Sep 28, 2026)** — [FAQs for the Marketing Message API for Messenger](https://developers.facebook.com/documentation/business-messaging/messenger-platform/marketing-messages-on-messenger/faq)
- Tags **`account_update`, `confirmed_event_update`, `post_purchase_update` were deprecated on January 12, 2026**. **Utility templates** (Meta-approved, transactional, no marketing content) replace them.
- **Recurring Notifications were discontinued globally on February 10, 2026** (except AU, EU, JP, KR, UK). Sponsored Messages were deprecated on July 31, 2024 (the policy page still mentions them, an inconsistency in Meta's docs).
- **Marketing Messages**: paid, sent to opted-in subscribers outside the window. **Available for businesses in Vietnam.** Each Page can send **at most 1 marketing message per user per day**. **Pay per delivered message**, billed through the Meta ads account and requiring a payment method. A user reply opens a new 24h window, and **"Businesses are not charged for their replies within that 24-hour window."** Subscribers come from Click-to-Messenger ads, CRM list uploads or opt-in templates.
- Conflicting dates for the tag deprecation from third parties: Manychat cites early February 2026, and another guide says API calls with those tags return error 100 from April 27, 2026 — [Manychat community](https://community.manychat.com/product-updates/meta-s-deprecation-of-the-message-tags-feature-on-messenger-9010); [singhamandeep.com](https://singhamandeep.com/?p=1586) (search summaries). Meta's FAQ says Jan 12, 2026.
- HUMAN_AGENT is **not** among the deprecated tags — [Manychat community](https://community.manychat.com/product-updates/meta-s-deprecation-of-the-message-tags-feature-on-messenger-9010) (search summary); [Meta policy](https://developers.facebook.com/documentation/business-messaging/messenger-platform/policy)
- Utility messages on Messenger were announced in July 2025, and Vietnam was among the initial countries (with the Philippines, Thailand and the USA) — [Meta developer blog (Jul 29, 2025)](https://developers.facebook.com/blog/post/2025/07/29/simplify-customer-updates-with-utility-messages-on-messenger/) (search summary)

**App review, access levels, business verification**
- Access levels: **Standard Access** (default) is limited to people with a role on the app or Page. **Advanced Access** reaches all users and requires App Review. **Business Verification is required to get Advanced Access** — [Meta Access Levels](https://developers.facebook.com/docs/graph-api/overview/access-levels/) (search summary)
- Until `pages_messaging` is approved, the API returns "Cannot message users who are not admins, developers or testers of the app until pages_messaging permission is reviewed and the app is live." To submit: App Review > Permissions and Features > request advanced access for **pages_messaging, Human Agent, Business Asset User Profile Access** — [Meta Messenger sample submission](https://developers.secure.facebook.com/docs/app-review/resources/sample-submissions/messenger-platform); [Meta developer community threads](https://developers.secure.facebook.com/community/threads/716843210362688/) (search summaries)
- Developers report reviews stuck for more than 13 days ("dashboard says 10 days max") — [Meta community forum](https://communityforums.atmeta.com/discussions/Questions_Discussions/app-review-stuck-13-days-%E2%80%94-pages-messaging-with-zero-response-from-meta-anyone-s/1374451) (anecdotal)

**Instagram Messaging**
- Available to any **Instagram Professional account** (Business or Creator), not personal accounts. Permissions: `instagram_manage_messages` (Facebook Login route, requires a linked Facebook Page) or `instagram_business_manage_messages` (Instagram Login route). Advanced Access is needed to serve accounts you don't own. With Standard Access an app can only message people with a role on the app. Window: 24h, plus **human_agent tag for 7 days** — [Meta Instagram Messaging overview](https://developers.facebook.com/documentation/business-messaging/instagram-messaging/overview); [IG send-message docs (updated Jul 2, 2026)](https://developers.facebook.com/documentation/business-messaging/instagram-messaging/features/send-message) (search summaries)

**Meta AI-chatbot policy context**
- **WhatsApp Business API banned "general-purpose" AI chatbots** (assistants whose AI is the core product, such as ChatGPT or Perplexity). New users were covered from Oct 15, 2025 and all users from **Jan 15, 2026**. **Business AI used for customer support, bookings, orders and so on remains allowed** — [respond.io](https://respond.io/blog/whatsapp-general-purpose-chatbots-ban); [Business Standard](https://www.business-standard.com/technology/tech-news/meta-bans-ai-chatbots-from-whatsapp-business-api-chatgpt-to-shut-down-jan-2026-125102200335_1.html) (secondary sources). No equivalent Messenger/IG ban was found. The Sep 2026 Messenger policy has only the disclosure and responsiveness rules above.

**Vietnamese legal overlay**
- **Luật Trí tuệ nhân tạo (Law No. 134/2025/QH15)** was passed on 10/12/2025 and took **effect 01/03/2026**. Article 11 (transparency): providers must ensure AI systems that interact directly with people are designed so **users know they are interacting with an AI system**, unless other law provides otherwise. AI-generated audio, images and video must carry machine-readable labels — [daibieunhandan.vn](https://daibieunhandan.vn/print/10412799.html); [lsvn.vn](https://lsvn.vn/nhung-luat-moi-chinh-thuc-co-hieu-luc-tu-thang-3-a169804.html); [vietnamnet.vn](https://vietnamnet.vn/nguoi-viet-phai-duoc-thong-bao-truoc-khi-tuong-tac-voi-he-thong-tri-tue-nhan-tao-2446420.html) (search summaries)

### Inferences
- **Cost: effectively free** for an AI agent answering inbound messages inside 24h. No per-message fee was found for standard-window messages. The only paid Messenger product is Marketing Messages, plus ads. This is a big contrast with Zalo OA's package fee.
- **Combining Meta's "disclose where required by law" rule with Vietnam's AI Law Article 11** means a Vietnamese business's Messenger/IG/Zalo AI agent should **announce itself as an AI assistant** at the start of each conversation. This applies on all channels, not just Meta.
- **Practical setup paths:** (a) a self-built Meta app needs a Business Portfolio, Business Verification (business documents) and App Review for pages_messaging (expect 1–3+ weeks, uncertain); or (b) **connect the Page to an approved partner inbox** (Pancake, Chatwoot Cloud, etc.) and let the AI work through that tool's API. That takes minutes to days, at the cost of a monthly subscription. For a non-technical owner, (b) is usually preferable.
- **Design rules for the agent:** reply within 30 seconds (or classify the bot as "hybrid"). Hand over to a human for anything after 24h, since only a human may use HUMAN_AGENT up to 7 days. Use comment private replies (one message, within 7 days) to turn comments into DMs. Do not use tags for promotions.
- **Ban risk: low to medium.** It is low with policy compliance. Page-level integrity strikes affect all messaging, including marketing messages. The main risks are spammy behaviour, tag misuse, slow bot responses, and user reports.

### Gaps
- The current official list of *remaining* Messenger message tags (e.g. whether CUSTOMER_FEEDBACK and others persist) could not be scraped; the Send API page rendered empty.
- The per-message price of Messenger Marketing Messages in Vietnam was not found (Meta uses bidding/estimates; no rate card located).
- No official Meta statement on App Review turnaround times.
- Messenger "Handover Protocol" (multi-app handover to human inbox) details were not re-verified for 2026 in this session.

---

## 5. Website chat widget

### Takeaway
A website chat widget is the **lowest-risk channel**. No third-party platform can ban the business, and there are no per-message fees. Options are a self-hosted **Chatwoot** (free Community Edition, needs a ~2 vCPU/4 GB server), Chatwoot Cloud (free tier: 2 agents, 500 conversations/month, website chat only), or a custom widget wired directly to the AI agent. In Vietnam, though, customers mostly start on Zalo or Facebook, so web chat is a complement, not a replacement.

### Cited Findings
- **Chatwoot Community Edition (self-hosted): $0 licence**. The only costs are infrastructure and time. Chatwoot recommends **≥4 GB RAM and 2 CPU cores** for production; one measurement found ~1.8 GB RAM in use. Server cost is estimated at $10–40/month. Captain AI (Chatwoot's built-in AI), Voice and SSO are **not** in Community Edition. Paid self-hosted "Premium Support" costs $19/agent/month. Self-hosted keeps all history — [eesel.ai Chatwoot pricing 2026](https://www.eesel.ai/blog/chatwoot-pricing); [achiya-automation.com](https://achiya-automation.com/en/blog/chatwoot-pricing/) (search summaries, third-party)
- **Chatwoot Cloud** (billed annually, per agent/month, June 2026 snapshot): **Hacker $0** (up to 2 agents, 500 conversations/month, 30-day retention, **website live chat only**), Startups $19, Business $39, Enterprise $99. One source lists Startups at $49 instead, so verify on chatwoot.com/pricing — [eesel.ai](https://www.eesel.ai/blog/chatwoot-pricing); [featurebase.app](https://www.featurebase.app/blog/chatwoot-pricing) (third-party)
- The zca-js-based **zca-bridge** connects personal Zalo to Chatwoot. It carries the same ban caveats as section 1 — [diendh/zca-bridge](https://github.com/diendh/zca-bridge)

### Inferences
- **Setup:** 1–2 days for a developer (Chatwoot plus a widget snippet on the site, the AI agent connected through Chatwoot's API or webhooks), or under a day with a hosted widget. **Documents:** none. **Monthly cost:** ~$0 (Hacker tier) to ~$10–40 for a VPS if self-hosted. **Ban risk:** none (self-controlled).
- Chatwoot is also a candidate **unified human-handover inbox** for web plus Messenger/IG/others. Zalo OA would need a custom API channel or a third-party connector (not native; see section 7).
- Deeplink and QR to the official Zalo Bot or OA can be placed on the website as an alternative "chat with us on Zalo" button.

### Gaps
- Chatwoot's official pricing page was not scraped directly; the figures come from third-party snapshots.
- No Vietnamese-specific web-chat vendors (Subiz, etc.) were priced in this session.

---

## 6. TikTok Shop / TikTok messaging API in Vietnam (brief)

### Takeaway
TikTok Shop buyer chat is reachable through the **TikTok Shop Partner Center** API: register as an app developer (ISV), get Customer Service/Message scopes, and have the merchant authorise via OAuth. Several helpdesks (Zendesk, Freshchat, Zaapi) already connect to it, and Zaapi lists Vietnam as supported. Expect text and images only, a 1,000-character cap, and no video. An AI agent should come through one of these connectors or an approved partner rather than a self-built app, unless the business is ready for TikTok's developer onboarding.

### Cited Findings
- TikTok Shop uses **OAuth 2.0**. Developers register as an **App ISV in TikTok Shop Partner Center**, enable Customer Service / Message permissions, and merchants authorise scopes. Helpdesks pull buyer inquiries into an agent inbox and push custom replies back — [truto.one TikTok Shop Partner connector](https://truto.one/integrations/detail/tiktokshoppartner/); [docs.klink.cloud](https://docs.klink.cloud/getting-started/setup-channels/tiktok) (search summaries)
- **Zaapi supports TikTok Shop integration in Vietnam** (US shops are not supported). Limits: **1,000 characters per TikTok message**, and video cannot be sent "because of TikTok Shop API limitations" — [Zaapi help: TikTok Shop limitations](https://help.zaapi.com/integrations/tiktok-shop/limitations) (search summary)
- Connector capabilities differ. One syncs text and images both ways, but not product or order cards or system notifications. Freshchat supports text, image, video and file, but not audio. Zendesk has an official "Customer Service Connector by TikTok Shop" — [Qiscus docs](https://documentation.qiscus.com/app-center/tiktok-shop-partner-center--ttspc-); [Freshworks](https://crmsupport.freshworks.com/support/solutions/articles/50000009564-tiktok-shop-in-freshchat); [Zendesk marketplace](https://www.zendesk.co.jp/marketplace/apps/support/932171/customer-service-connector-by-tiktok-shop) (search summaries)
- TikTok's **Message Assistant** (an ads-side automated messaging feature) is available to Verified Business Accounts registered in Southeast Asia, **including Vietnam**, with Vietnamese language support — [TikTok Ads help: About Message Assistant](https://ads.tiktok.com/help/article/about-tiktok-lead-genie) (search summary)
- TikTok named **Pancake** a Vietnam-based **messaging partner** (TikTok Marketing Partners, messaging specialty) for advertisers. Pancake's own docs describe TikTok **ads** messaging routed to a Facebook Page or WhatsApp, which is separate from Shop buyer chat — [TikTok Ads blog](https://ads.tiktok.com/business/en/blog/introducing-messaging-partner-specialty); [Pancake docs: TikTok Messaging Ads](https://docs.pancake.vn/english/advance-features-by-channels/tiktok/tiktok-messaging-ads) (search summaries)
- Sapo notes that TikTok Shop and TikTok For Business currently operate independently and are not integrated with each other — [Sapo](https://www.sapo.vn/cap-nhat-tinh-nang/ket-noi-tiktok-for-business) (search summary)

### Inferences
- Ban risk is low when going through official Partner Center apps. Setup takes days to weeks for a custom app, or hours with an existing connector. The cost is the connector subscription; no TikTok per-message fee was found.
- This is a lower priority than Zalo and Facebook for most Vietnamese SMEs unless TikTok Shop is a main sales channel.

### Gaps
- Official TikTok Shop Partner API endpoint documentation was not retrieved. Vietnam-specific CS API rules (response-time SLAs, AI restrictions) were not found.
- Whether Pancake supports TikTok Shop buyer chat specifically (as opposed to TikTok ads messaging) is unconfirmed.

---

## 7. Omnichannel inboxes for human handover (Chatwoot, Pancake, others) — brief

### Takeaway
For UC2, an AI agent should sit behind a **shared inbox** so staff can see and take over conversations. **Pancake** is the dominant Vietnamese tool: Facebook, Instagram, Zalo, TikTok and Shopee in one inbox with order creation; priced per "connection"; TikTok messaging partner. **Chatwoot** is the open-source option: excellent for web, Messenger, IG, WhatsApp and Telegram, but **no native Zalo OA channel** (it needs custom integration or a bridge). Other options with Zalo OA connectors include Viindoo/Odoo, Zendesk and Chative.

### Cited Findings
- **Pancake pricing** (shown in USD to this crawler; Vietnamese visitors may see VND): **Small** 1 connection, 3 users, 6 months **$49.10** (list $65); **Standard** 3 connections, 3 users, 6 months **$103.70** (list $125); **Business** 6 connections, 10 users, 6 months **$365.80** (list $415); custom plans available. Features include connecting all social networks and e-commerce platforms, instant order creation in chat view, lifetime data retention, automatic phone and address detection, call-centre integration, and a **14-day free trial** — [pancake.vn/pricing](https://pancake.vn/pricing)
- A third-party Pancake API description: the Platform API manages pages, conversations, messages, customers and tags. User tokens are valid for 90 days; page tokens don't expire unless deleted or renewed — [aiindigo.com](https://aiindigo.com/blog/managing-social-commerce-in-2026-a-deep-dive-into-pancake) (search summary, third-party)
- **Chatwoot:** no native Zalo channel was found in search. A community zca-js bridge exists for personal Zalo (with ban risk). The Zalo OA route would be a custom "API channel" — [diendh/zca-bridge](https://github.com/diendh/zca-bridge); search found no Chatwoot–Zalo OA integration
- **Other Zalo OA-capable inboxes:** Viindoo `viin_zalo` for Odoo 17/18 (auto token refresh, one Discuss channel per customer), Zendesk Zalo integration (Quesera), Chative — [Viindoo](https://viindoo.com/apps/app/18.0/viin_zalo); [Zendesk](https://www.zendesk.co.uk/apps/support/1022071/zalo-integration/); [Chative](https://chative.io/blog/zalo-messenger-release/) (search summaries)
- Botcake, a Messenger chatbot tool in the Pancake ecosystem, lists auto-replies to comments and messages within 24h as free up to 500K/month — [botcake.io/pricing](https://botcake.io/pricing) (search snippet)

### Inferences
- **Recommended pattern for UC2:** Zalo OA (Tăng trưởng) plus the Facebook Page, connected to one inbox (Pancake if the business already sells on Facebook/TikTok; Chatwoot if self-hosting and web-first). The AI agent answers first, and staff take over in the inbox when needed. This also satisfies Meta's "hybrid bot" model and keeps HUMAN_AGENT-tag replies human.
- An inbox subscription (~$8–60/month equivalent for Pancake's small and medium tiers, computed from the 6-month prices above) is often cheaper and faster than building and maintaining direct Meta and Zalo integrations plus going through Meta App Review yourself.

### Gaps
- Pancake's VND price list, and whether Pancake connects to **Zalo OA via the official API** or via personal Zalo, were not confirmed in this session.
- Whether Pancake or Chatwoot let an *external* AI agent reply through their API (vs only their built-in bots) was not verified in detail.

---

## 8. Per-channel summary and recommendation (setup time, documents, monthly cost, ban risk)

### Takeaway
**UC1 (owner's assistant on phone): use the official Zalo Bot** (free, minutes to set up, officially documented for OpenClaw personal assistants). Add a personal-account library on a nick phụ *only* if the agent must read the owner's real Zalo conversations, and accept the lock risk. **UC2 (customer service): use Zalo OA on the Tăng trưởng package (2.5M VND/year), the Facebook Page/Instagram through Messenger API or a partner inbox (free messaging), and a website widget**, all feeding one human-handover inbox. Avoid personal-account automation for customer-facing work.

### Cited Findings
- Zalo Bot: free creation via the Zalo Bot Manager OA, token by message, official OpenClaw personal-assistant guide; beta; groups in internal testing — [docs.zaloplatforms.com Tạo Bot](https://docs.zaloplatforms.com/docs/BOT/create_bot); [OpenClaw best practice](https://docs.zaloplatforms.com/docs/BOT/best-practices/build-personal-assistant-with-open-claw); [group doc](https://docs.zaloplatforms.com/docs/BOT/best-practices/build-bot-interaction-with-group); [Tino (3-bot cap, beta)](https://tino.vn/blog/tao-bot-zalo-ca-nhan/)
- Zalo OA: package prices, API only on Tăng trưởng/Toàn diện, 55 VND per consultation message outside 48h — [zalo.solutions/oa/pricing](https://zalo.solutions/oa/pricing). Verification documents and 2–3 working days — [oa.zalo.me guide](https://oa.zalo.me/home/documents/guides/huong-dan-xac-thuc_70). 8 free in 48h, 7-day limit — [OA messaging policy](https://oa.zalo.me/home/resources/news/thong-bao-chinh-sach-gui-tin-va-quy-dinh-phi-gui-tin_1433049880779375099). ZNS prices — [zalo.solutions/zns/pricing](https://zalo.solutions/zns/pricing)
- Personal Zalo via zca-js: unofficial, 2026 locks, friend-list wipes, security warnings, login breakages — [zca-js issues #291, #317, #359, #386](https://github.com/RFS-ADRENO/zca-js/issues/317)
- Messenger/IG: 24h window, 7-day human agent, 30-second responsiveness, disclosure, tag deprecations, Marketing Messages available in Vietnam — [Meta policy](https://developers.facebook.com/documentation/business-messaging/messenger-platform/policy); [Meta FAQ](https://developers.facebook.com/documentation/business-messaging/messenger-platform/marketing-messages-on-messenger/faq)
- Vietnam AI Law Article 11 AI-interaction disclosure, effective 1/3/2026 — [lsvn.vn](https://lsvn.vn/nhung-luat-moi-chinh-thuc-co-hieu-luc-tu-thang-3-a169804.html)

### Inferences
Summary table (costs are approximate; VND→USD at ~26,000; "setup time" assumes a technical helper is available):

| Channel | How the AI connects | Setup time | Documents needed | Typical cost | Ban / restriction risk | Use-case fit |
|---|---|---|---|---|---|---|
| **Zalo Bot (official Bot API)** | Bot token; webhook or long-polling; Telegram-like API; official OpenClaw plugin "My ClawBot" | Minutes to 1 day | None (personal Zalo account) | **Free** (no published fee; beta) + server/LLM costs | **Low** (official). Platform risk: beta, 3 bots/account, undocumented quotas, groups not live | **UC1: Best.** UC2: pilot or secondary only (deeplink/QR, "Bot…" name, no OA branding) |
| **Zalo OA (Official Account) API** | OAuth v4 app on developers.zalo.me; webhook; token refresh (refresh token 3 months, single-use) | ~1 week (verify in 2–3 working days + package + app) | Business or household-business registration; legal rep CCCD; Zalo confirmation by legal rep | **Tăng trưởng 2.5M VND/yr (≈$96/yr ≈ $8/mo)** or 1.4M/6 months; Toàn diện 6M/yr (≈$231). +55 VND per consultation message beyond quota; ZNS ~210–400 VND + VAT per template | **Low** if message rules are followed (Zalo watches report rates) | UC1: overkill. **UC2: primary Zalo channel** |
| **Personal Zalo via zca-js/zlapi (nick phụ)** | Reverse-engineered Zalo Web session (QR/cookie) | Hours | None | Free software; cost of SIM or secondary account | **High**: ToS violation; 2026 reports of locks, friend-list wipes, "unauthorised third-party software" warnings, frequent login breakage, conflicts with the owner's own sessions | UC1: only if it must read real personal chats, on a nick phụ, ideally read-only. **UC2: not recommended** |
| **Facebook Messenger (+ Instagram DM)** | Meta Graph API webhook (own app needs pages_messaging Advanced Access + Business Verification) **or** an approved partner inbox | Own app: 1–3+ weeks (App Review). Via partner: minutes to days | Business verification documents for own app; IG must be a Professional account linked to the Page | **Free** in the 24h window; paid only for Marketing Messages and ads; partner inbox subscription if used | **Low–medium**: 30-second response rule, tag misuse, user reports → Page-level messaging restrictions | **UC2: primary for FB sellers.** Comment-to-DM via private replies (1 message, 7 days) |
| **Website chat widget** | Chatwoot widget (self-hosted or cloud) or custom widget → AI | 1–2 days | None | $0 (Chatwoot Cloud Hacker: 2 agents, 500 conversations/month) to ~$10–40/mo VPS self-hosted | **None** (self-controlled) | **UC2: complement** (lower traffic in VN) |
| **TikTok Shop chat** | Partner Center app (ISV, OAuth, CS scopes) or connectors (Zendesk, Freshchat, Zaapi) | Days–weeks (custom) / hours (connector) | TikTok Shop seller account; developer registration for custom app | Connector subscription; no TikTok per-message fee found | **Low** via official API | UC2: only if TikTok Shop is a key channel |
| **Omnichannel inbox (Pancake / Chatwoot)** | Hosts the channels; AI plugs in through its API; staff take over | Hours–days | Channel-specific | Pancake from ~$49/6 months (1 connection) to ~$104/6 months (3 connections); Chatwoot CE free | n/a | **UC2: needed for human handover** |

Additional cross-channel recommendations:
- **AI disclosure:** every channel's AI should state it is an AI assistant (Vietnam AI Law Art. 11; Meta policy).
- **Human handover:** required in practice on Meta (HUMAN_AGENT tag must be human; the hybrid bot model) and good practice on Zalo OA (assign staff in OA chat).
- **Warn the business owner about outdated sources:** Zalo OA prices changed on 1/6/2026, ZNS prices changed (now "ZBS Template Message", 300–400 VND before VAT), and three Messenger tags plus Recurring Notifications ended in early 2026.

### Gaps
- No single official source states that Messenger standard-window messaging is "free". This is inferred from the absence of any fee and Meta's statement that replies after marketing messages are not charged.
- Real-world Zalo Bot quota/throughput numbers and SLA are unknown.
- Pancake's official VND pricing and Zalo OA (official API) support were not verified.
