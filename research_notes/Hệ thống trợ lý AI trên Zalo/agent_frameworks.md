# Agent frameworks for a Zalo/Telegram personal assistant and a CS agent: Hermes Agent vs Javis OS ("Jarvis OS") vs OpenClaw (as of 14 Oct 2026)

Research date: 14 Oct 2026. Sources are 2026 unless marked otherwise. GitHub star and fork counts were read off the GitHub pages on 12–14 Oct 2026. Several primary sites (docs.openclaw.ai, tienphong.vn, znews.vn) could only be read through a scraping service. One scrape of the Javis OS Zalo doc page returned an HTTP 503 error page, so no content from it is used below.

---

## 1. What "Hermes" is: Nous Research's Hermes Agent

### Takeaway
"Hermes" in the 2026 agent hype almost certainly means **Hermes Agent** by Nous Research (github.com/NousResearch/hermes-agent). It is an MIT-licensed, self-hosted, "self-improving" personal agent. It launched in **February 2026, not 2025**. It has persistent memory, auto-created skills, cron jobs, MCP and a multi-platform messaging gateway. It is now one of the two dominant open-source agent harnesses, alongside OpenClaw. The latest release is v0.22.0 (14 Oct 2026), and Nous raised $90M on 7 Oct 2026 to build a business edition. **Zalo is not an official Hermes channel.** Only community plugins exist.

### Cited Findings
- Nous Research describes itself as having "created Hermes Agent, the most widely used open source agent harness in the world." — [Nous Research](https://nousresearch.com/)
- Release timing: "Hermes Agent is an open-source autonomous AI agent built by Nous Research and released in February 2026" (unofficial fan/mirror site). — [hermes-agent.org](https://www.hermes-agent.org/). The Batch independently says it was "launched in February" 2026. — [The Batch / DeepLearning.AI, 22 May 2026](https://charonhub.deeplearning.ai/hermes-agent-challenges-openclaw/)
- GitHub (read 14 Oct 2026): **253.6k stars, 55.0k forks, 978 watchers, 52,346 commits, 39 releases. Latest is "Hermes Agent v0.22.0 — The Threshold Release", 14 Oct 2026.** License MIT. Code is about 70% Python and 25.9% TypeScript. — [GitHub NousResearch/hermes-agent](https://github.com/nousresearch/hermes-agent)
- Pricing: "Hermes Agent is free and open source under the MIT license." — [hermes-agent.nousresearch.com](https://hermes-agent.nousresearch.com/)
- Core pitch: "the only agent with a built-in learning loop — it creates skills from experience, improves them during use, nudges itself to persist knowledge, searches its own past conversations, and builds a deepening model of who you are across sessions. Run it on a $5 VPS, a GPU cluster, or serverless infrastructure..." — [GitHub README](https://github.com/nousresearch/hermes-agent)
- Feature list from the README:
  - agent-curated memory and autonomous skill creation
  - FTS5 session search
  - Honcho user modelling
  - agentskills.io-compatible skills
  - built-in cron scheduler that delivers to any platform ("Daily reports, nightly backups, weekly audits — all in natural language")
  - subagents
  - 40+ tools
  - MCP integration
  - seven terminal backends: local, Docker, SSH, Singularity, Modal, Daytona, Vercel Sandbox

  — [GitHub README](https://github.com/nousresearch/hermes-agent)
- Model providers: "Use any model you want — Nous Portal, OpenRouter, OpenAI, your own endpoint, and many others. Switch with `hermes model`." Nous Portal is an optional single subscription covering "300+ models" plus a tool gateway (web search, image generation, TTS, cloud browser). — [GitHub README](https://github.com/nousresearch/hermes-agent)
- Install:
  - one-liner `curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash` on Linux/macOS/WSL2
  - PowerShell one-liner for native Windows
  - signed APT repo for Android/Termux
  - Dockerfile, docker-compose.yml and Nix flake in the repo
  - `hermes setup` wizard and `hermes gateway setup` for messaging

  — [GitHub README](https://github.com/nousresearch/hermes-agent)
- OpenClaw migration is built in: `hermes claw migrate` imports SOUL.md, memories, skills, allowlists, messaging settings and API keys from `~/.openclaw`. — [GitHub README](https://github.com/nousresearch/hermes-agent)
- Messaging gateway platforms listed in the official docs:
  - Telegram, Discord, Slack, WhatsApp, Signal, SMS, Email
  - Home Assistant, Mattermost, Matrix, DingTalk, Feishu/Lark
  - WeCom, Weixin, BlueBubbles (iMessage), QQ, Yuanbao
  - Microsoft Teams, LINE, ntfy, browser
  - also Google Chat, WhatsApp Cloud API, an OpenAI-compatible API server, Webhooks and A2A

  **Zalo is not listed.** — [Hermes docs: Messaging Gateway](https://hermes-agent.nousresearch.com/docs/user-guide/messaging/)
- Adoption:
  - "cloned over 24 million times" and drives "approximately 2.5% of all global token usage (best internal estimates)". — [Nous: A Note on Our Fundraise, 7 Oct 2026](https://nousresearch.com/a-note-on-our-fundraise)
  - WSJ: about 22.7 million downloads since February. — [WSJ Pro, Oct 2026](https://www.wsj.com/pro/venture-capital/nous-research-scores-90-million-to-bring-open-source-ai-assistant-to-enterprises-516f9208)
  - Hermes "recently moved ahead of OpenClaw on a leaderboard that tracks the number of tokens agents consume daily, as tallied by ... OpenRouter." — [The Batch, 22 May 2026](https://charonhub.deeplearning.ai/hermes-agent-challenges-openclaw/)
- Funding and roadmap: Nous raised **$90M**. Investors are NVIDIA, Microsoft M12, Samsung, Robot Ventures, USV, Y Combinator and Menlo Ventures. Nous says it "will build and deliver Hermes for Businesses". — [Nous, 7 Oct 2026](https://nousresearch.com/a-note-on-our-fundraise)
- Origin: Hermes started as Nous's internal productivity tool, rebuilt in Python. It is designed around open weights, local execution, private data, long-running sessions and tiered memory. — [Arize Observe 2026 talk (YouTube)](https://www.youtube.com/watch?v=Y3NDtqk6ags)
- Community Zalo plugins (none from Zalo/VNG):
  - **hermes-zalo-plugin** by cuongdev connects a *personal* Zalo account through zca-js (an unofficial Zalo API) and is marked **[beta]**. It installs with `npm i -g hermes-zalo-plugin`, and npm has v1.0.9. — [awesome-hermes-skills](https://github.com/ZeroPointRepo/awesome-hermes-skills); [UNPKG hermes-zalo-plugin@1.0.9](https://app.unpkg.com/hermes-zalo-plugin@1.0.9); [FB group vnhermesagent](https://www.facebook.com/groups/vnhermesagent/posts/1370697711590149/)
  - **hermes-plugin-zalo** by GitHub user moken627-hub targets Zalo Bot (Marketplace/OA) and supports polling or webhook. It was announced in the "Hermes Agent VN" Facebook group as a "plugin Zalo chính thức" (official Zalo plugin), but the poster is a community member, and I found no evidence that it is endorsed by Zalo or Nous. — [GitHub moken627-hub/hermes-plugin-zalo](https://github.com/moken627-hub/hermes-plugin-zalo/blob/main/.github/ISSUE_TEMPLATE/bug_report.md); [FB Hermes Agent VN post](https://www.facebook.com/groups/2600004717066101/posts/2764222207311017/)
- Security:
  - The gateway "denies all users who are not in an allowlist or paired via DM. This is the safe default for a bot with terminal access."
  - `GATEWAY_ALLOW_ALL_USERS=true` is "NOT recommended for bots with terminal access".
  - Messages are routed "through a per-chat session store".

  — [Hermes docs: Messaging](https://hermes-agent.nousresearch.com/docs/user-guide/messaging/)
- Known CVE: **CVE-2026-9353** is an injection flaw in the Skills Guard component (agent/skills_guard.py) that affects versions up to 2026.4.23. Severity is disputed: NVD lowered it from HIGH 7.3 to MEDIUM 5.5. VulDB says the vendor did not respond, but a fix PR is referenced. — [GitLab Advisory](https://advisories.gitlab.com/pypi/hermes-agent/CVE-2026-9353/); [CIRCL](https://vulnerability.circl.lu/vuln/CVE-2026-9353)

### Inferences
- Hermes is the most mature and best-funded option on paper: an institutional backer, very frequent releases (39 releases in about 8 months) and a migration path from OpenClaw. The high release cadence also means frequent breaking changes for anyone maintaining many client installs.
- For Vietnamese users, the gap is Zalo. Using Hermes on Zalo means depending on a beta community plugin, either an unofficial personal-account plugin (ban risk) or a small community Bot API plugin. Telegram works out of the box.
- Hermes's distinctive features (self-learning skills, user modelling) are designed for one owner's assistant, not for serving many anonymous customers.

### Gaps
- No official minimum RAM/CPU figure was found. The only guidance is the claim that it runs on a "$5 VPS", and real load depends on whether the LLM is remote (API) or local.
- I could not confirm whether "Hermes for Businesses" (multi-user, admin controls) has shipped or has pricing as of 14 Oct 2026.
- I could not verify whether any other Vietnamese product named "Hermes" is circulating. None appeared in searches, and all Vietnamese "Hermes" mentions found point to Nous's Hermes Agent.

---

## 2. What "Jarvis OS" / "Javis OS" is

### Takeaway
With high confidence, the "Javis OS" hyped in Vietnam is **Javis OS (javisos.com, github.com/blogminhquy/javis-os)**. It is an independent, MIT-licensed, self-hosted Python/FastAPI "AI OS" built by Vietnamese online-business marketer **Nguyễn Minh Quý**. It is **not a fork of OpenClaw or Hermes**. It wraps subscription CLIs (Claude Code, Codex, Grok, Antigravity) or API models with a markdown "Second Brain", an MCP connection hub preloaded with Vietnamese commerce tools (Pancake POS, Botcake, Webcake, Zalo), Telegram, Vietnamese voice and scheduled background jobs. It is small (244 stars), essentially a single-maintainer project, and its marketing claim of "0đ API" conflicts with its own README warning that using a Claude subscription this way can get accounts suspended.

### Cited Findings
- Official site title: "Javis OS - Trợ Lý AI Cá Nhân Cho Doanh Nhân" (a personal AI assistant for entrepreneurs). The site advertises: "Hơn 15 kết nối cài sẵn · 100% tiếng Việt · Tự host" (15+ built-in connectors, fully in Vietnamese, self-hosted). — [javisos.com](https://www.javisos.com/)
- The site's main promise: it turns your existing Claude/ChatGPT subscription into a full-control assistant on your own machine, "Không cần API key. Không phí phần mềm" (no API key, no software fee). It claims costs of "0đ phí phần mềm", "0đ phí API" and "~0k/tháng một VPS nhỏ". — [javisos.com](https://www.javisos.com/)
- Maker: "Nguyễn Minh Quý - một người làm kinh doanh và marketing online tại Việt Nam" (an online business and marketing practitioner in Vietnam), who built it for his own store tracking and content work. — [javisos.com](https://www.javisos.com/). The README says: "Made with ☕ in Vietnam by Minh Quý" and "it is still one person writing the code and paying for the test servers". Donations go to PayPal, MB Bank and MoMo. — [GitHub blogminhquy/javis-os](https://github.com/blogminhquy/javis-os)
- GitHub (read 12 Oct 2026): **244 stars, 153 forks, 4 watchers, 1,174 commits, 11 contributors (one is "claude", i.e. AI-authored commits), MIT license.** The last commit is "0.103.0" on 12 Oct 2026, and the latest tagged release is 0.64.37 (24 Sep 2026). — [GitHub](https://github.com/blogminhquy/javis-os)
- Architecture:
  - Python FastAPI backend and a plain JS dashboard with no build step
  - an "Engine router" to "Subscription CLIs (Claude Code, Codex, Grok, Antigravity)" or "API engines (OpenRouter, OpenAI, Anthropic, Gemini, Groq, DeepSeek, Ollama)", 13 providers in all
  - an "MCP Hub" shared toolbox
  - a "Second Brain" markdown vault (memory, wiki, sources) that can sync to a private GitHub repo
  - background tasks, loops and reminders
  - channels: web dashboard (voice and 3D graph), Telegram, CLI, "Zalo / bot channels"

  — [GitHub README](https://github.com/blogminhquy/javis-os)
- Connectors advertised: Pancake POS, Meta Ads, Gmail, Google Calendar, Google Sheets, Slack, Lark, Zalo, Botcake, Webcake and Systeme.io, plus Google Ads, TikTok Ads, Google Workspace, Search Console and custom MCP. The site says one service can have multiple accounts ("3 shop POS, 2 số Zalo", i.e. 3 POS shops and 2 Zalo numbers) and that Zalo connects by QR scan. Permissions have three levels (read-only, draft-write, full) enforced at the hub, and "Loop chạy nền không bao giờ tự tiêu tiền hay gửi tin" (background loops never spend money or send messages on their own). — [javisos.com](https://www.javisos.com/)
- Install options:
  - Hostinger Docker Manager one-click compose URL (`docker-compose.hostinger.yml`)
  - Docker on any VPS
  - `install.sh` / `install.ps1` scripts
  - asking Claude Code or Codex to "install Javis OS for me"

  The dashboard runs at port 7777. — [GitHub README](https://github.com/blogminhquy/javis-os)
- Replication: the README documents "**Several Javis instances on one VPS** — Brains, settings and accounts stay fully separate per instance. Only three values differ between them: `JAVIS_NAME`, `JAVIS_HOST_PORT`, `DOMAIN_NAME`." This is supported through a shared proxy compose file or by redeploying the Hostinger stack. — [GitHub README](https://github.com/blogminhquy/javis-os)
- Security model:
  - "Javis runs an AI brain with **full rights** on the machine."
  - It forces login when public, and offers 2FA (TOTP), rate limiting, CSRF/DNS-rebinding protection and encrypted secrets.
  - User plugins are blocked by default.
  - Caveat: "While no admin exists, whoever opens the link first can create it."

  — [GitHub README](https://github.com/blogminhquy/javis-os)
- **Red flag in its own README:** "Anthropic scopes Claude Pro/Max to **ordinary personal use** of Claude Code. Continuous background execution (loops, reminders, Kanban jobs, chatbots), running on a VPS, or several people sharing one account all fall outside that scope, and accounts **have been suspended** over it... To be safe, set Claude Code to run on an **API key**." — [GitHub README](https://github.com/blogminhquy/javis-os). This contradicts the landing page's "0đ tiền API... Không cần API key". — [javisos.com](https://www.javisos.com/)
- Vietnamese social spread:
  - The author posted "Ae lấy link Repo github của Javis OS vác về sài làm trợ lý AI free" (grab the repo and use it as a free AI assistant), adding that truly free use means OpenRouter free models. — [FB Minh Quý](https://www.facebook.com/minhquy1711/posts/ae-l%E1%BA%A5y-link-repo-github-c%E1%BB%A7a-javis-os-v%C3%A1c-v%E1%BB%81-s%C3%A0i-l%C3%A0m-tr%E1%BB%A3-l%C3%BD-ai-free-t%E1%BA%A1i-javisosco/3620187278120366/)
  - A third-party FB user offered a setup session: "Thấy nhiều ae inbox hỏi về con trợ lý AI Javis OS mình đang dùng, nên mình quyết định mở một buổi hướng dẫn setup" (many people asked about the Javis OS assistant I use, so I'm running a setup tutorial). — [FB treyypham](https://www.facebook.com/treyypham/posts/th%E1%BA%A5y-nhi%E1%BB%81u-ae-inbox-h%E1%BB%8Fi-v%E1%BB%81-con-tr%E1%BB%A3-l%C3%BD-ai-javis-os-m%C3%ACnh-%C4%91ang-d%C3%B9ng-n%C3%AAn-m%C3%ACnh-quy%E1%BA%BFt-/122123454945123888/)
  - Other coverage: a beginner install guide on blogdaytinhoc.com ([link](https://blogdaytinhoc.com/huong-dan-cai-dat-va-su-dung-javis-os-don-gian-cho-nguoi-moi-1454)), a Substack post by the author ([link](https://blogminhquy.substack.com/p/cach-minh-tu-dung-mot-con-ai-os-rieng)), a TiniX trending listing ([link](https://repo.tinix.ai/vi/project/blogminhquy-javis-os-11c76b78-c6f9-400c-8bf4-82cb707d7399)) and a Threads post that OCRs it as "blogminhquy/jarvis-os ... Jarvis OS Trợ lý AI cá nhân + Second Brain - chạy bằng Claude Code" ([Threads](https://www.threads.com/@felix_c888/post/DaSAW0BDujM/javis-is-now-public-on-git-hub-and-the-most-interesting-part-is-not-just-the/)). That Threads post shows the Jarvis/Javis spellings are used interchangeably.
- Other unrelated products named "Jarvis OS", none tied to Vietnam or Zalo:
  - an Arch-Linux-based LLM security research OS ([jarvisoslinux.org](https://jarvisoslinux.org/))
  - an enterprise "AI authority & automation platform" ([getjarvisos.com](https://getjarvisos.com/))
  - a personal macOS voice-agent project ([brandensoo.com](https://brandensoo.com/project-jarvis-os))
  - a hobby GitHub repo ([MAL19INDUSTRIES/JARVIS-OS-V.2](https://github.com/MAL19INDUSTRIES/JARVIS-OS-V.2))

### Inferences
- Javis OS is the most Vietnam-native of the three: Vietnamese UI and voice, plus Pancake/Botcake/Webcake/Zalo connectors used by Vietnamese online shops. It is attractive for an owner's back-office assistant (revenue reports, ad checks, reminders on Telegram).
- It is a one-person project with a small community and no security track record or audits. Compared with Hermes and OpenClaw, this is a much higher bus-factor and maintenance risk if you deploy it for many clients.
- The "run on your Claude subscription" economics are its main marketing hook. For a business that resells or installs it for clients, that model likely breaches Anthropic's consumer terms: VPS, 24/7 background work and shared accounts are exactly the cases the README flags. Budget for API keys instead.
- "Quét QR (Zalo)" and "2 số Zalo" strongly suggest that the Zalo connector automates *personal* Zalo accounts through an unofficial web-session API, not the official OA/Bot API. That carries account-ban risk. This is an inference from the marketing page and was not verified in the docs.

### Gaps
- I could not read the Javis OS Zalo guide (docs/en/12-zalo-agent-mcp.md) because the scrape returned HTTP 503. The exact Zalo mechanism (zca-js personal vs Bot API/OA) and whether it supports customer auto-replies are unverified.
- I found no paid tier, course pricing or commercial entity behind Javis OS. The landing page has a lead form ("we will contact you soon"), so paid setup or consulting may exist, but this is unconfirmed.
- I found no independent security review of Javis OS.

---

## 3. OpenClaw (formerly Clawdbot / Moltbot): status, channels, security record

### Takeaway
OpenClaw is the largest open-source personal agent: **391.9k GitHub stars and 82.4k forks**, MIT licensed, TypeScript/Node. It has the broadest channel coverage, including an official bundled **Zalo Bot** channel and, since June 2026, a **Zalo-built "Zalo ClawBot" plugin**. Its creator joined OpenAI in February 2026 and the project moved to a foundation. It also has the worst security record of 2026:
- a one-click RCE CVE
- 21k–42k internet-exposed instances
- 341 → 824+ malicious skills on ClawHub (the ClawHavoc campaign)
- prompt injection found in about 36% of audited skills

Its own docs define a **single-trusted-operator** model that offers no isolation between hostile users.

### Cited Findings
- GitHub (read 14 Oct 2026): **391.9k stars, 82.4k forks, MIT.** Tagline: "The AI that really does things. Any OS. Any Platform. The lobster way." Runtime is Node 24.16+ or 26.1+. Install is `curl -fsSL https://openclaw.ai/install.sh | bash`, a PowerShell one-liner on Windows, or `npm install -g openclaw@latest`. — [GitHub openclaw/openclaw](https://github.com/openclaw/openclaw)
- The README now says: "One Gateway runs it as a personal assistant on a laptop or as a shared team deployment; configuration is the only difference." It adds: "Treat inbound messages as untrusted input... Tools run on the host for the main session unless you configure sandboxing. Read the security guide... before connecting other users or exposing the Gateway remotely." — [GitHub openclaw/openclaw](https://github.com/openclaw/openclaw)
- An earlier README, mirrored on a third-party directory and possibly outdated, said: "If you want a personal, single-user assistant that feels local, fast, and always-on, this is it." It recommended "Anthropic Pro/Max + Opus 4.6 for long‑context strength and better prompt‑injection resistance" and listed channels: WhatsApp (Baileys), Telegram, Slack, Discord, Google Chat, Signal, BlueBubbles/iMessage, Teams, Matrix, **Zalo, Zalo Personal**, WebChat, plus macOS/iOS/Android apps. Docker install docs exist. — [openclawdir.com mirror of README](https://openclawdir.com/plugins/zalo-official-account-60bzdf)
- Name history: Clawdbot was renamed after Anthropic raised trademark concerns, then became Moltbot, then OpenClaw. — [secondary summary via search; tenten.co history](https://tenten.co/openclaw/en/blog/openclaw-history)
- Governance: Sam Altman announced that Peter Steinberger is "joining OpenAI to drive the next generation of personal agents" (mid-Feb 2026) and that "OpenClaw will live in a foundation as an open source project that OpenAI will continue to support." — [Techloy](https://www.techloy.com/openai-hires-openclaw-creator-to-lead-personal-ai-agent-development/); [OpenSourceForU](https://www.opensourceforu.com/?p=96842)
- Trust model (official security docs, as summarised in search results):
  - one trusted operator per gateway
  - no isolation between adversarial users sharing one agent or gateway; for split trust boundaries use a "separate gateway + credentials, ideally separate OS users/hosts"
  - untrusted people messaging a tool-enabled agent "share the same delegated tool authority for that agent"
  - "Exec approvals (allowlist + ask) are guardrails for operator intent, not hostile multi-tenant isolation"

  — [docs.openclaw.ai/security](https://docs.openclaw.ai/security); [docs.openclaw.ai/gateway/security](https://docs.openclaw.ai/gateway/security)
- **CVE-2026-25253** (CVSS 8.8) is a one-click RCE: the Control UI accepted a gateway URL from the query string and leaked the auth token. It is exploitable even on loopback-only instances and was fixed in v2026.1.29 (30 Jan 2026). — [The Hacker News, Feb 2026](https://thehackernews.com/2026/02/openclaw-bug-enables-one-click-remote.html); [ProArch](https://www.proarch.com/blog/threats-vulnerabilities/openclaw-rce-vulnerability-cve-2026-25253)
- Exposed instances:
  - Censys counted 21,639 public instances on 31 Jan 2026, with at least 30% on Alibaba Cloud.
  - Later scans counted 40,214 (SecurityScorecard) and 42,665, using different methods.

  — [CyberInsider](https://cyberinsider.com/critical-1-click-rce-bug-in-openclaw-enables-full-system-takeover-and-data-theft/); [Techzine](https://www.techzine.eu/news/security/138633/over-40000-openclaw-agents-vulnerable/)

  Conscia says "30,000+ internet-exposed instances... many running without authentication." — [Conscia](https://conscia.com/blog/the-openclaw-security-crisis/)
- Leaked credentials: researcher Jamieson O'Reilly found hundreds of unauthenticated instances "leaking plaintext API keys, bot tokens, OAuth credentials, and conversation histories". — [AuthMind](https://www.authmind.com/blogs/openclaw-malicious-skills-agentic-ai-supply-chain)
- ClawHavoc supply-chain campaign: Koi Security audited 2,857 ClawHub skills and found **341 malicious** (12%), mostly delivering the Atomic macOS Stealer. By 16 Feb 2026 the count was "over 824 across an expanded registry of 10,700+ skills". — [Conscia](https://conscia.com/blog/the-openclaw-security-crisis/); [Unit 42 / Palo Alto](https://unit42.paloaltonetworks.com/openclaw-ai-supply-chain-risk/)
- Snyk "ToxicSkills": prompt injection in 36% of skills, 1,467 malicious payloads, and hardcoded secrets in 10.9% of all ClawHub skills. — [Snyk](https://snyk.io/blog/toxicskills-malicious-ai-agent-skills-clawhub/)
- Other advisories:
  - HKCERT (17 Mar 2026) warned of skills supply-chain and fake-installer risks. — [HKCERT](https://www.hkcert.org/blog/openclaw-s-rapid-adoption-exposes-skills-supply-chain-and-fake-installer-risks-in-a-high-privilege-ai-agent-platform)
  - Bitdefender described abuse of OpenClaw via ClawHub for initial access into enterprise networks. — [Bitdefender](https://businessinsights.bitdefender.com/technical-advisory-openclaw-exploitation-enterprise-networks)
  - China's industry ministry warned about misconfigured open-source agents. — [search summary; jang.com.pk](https://jang.com.pk/en/59982-openai-nabs-openclaws-petersteinberger-to-lead-nextgen-personal-agents-news)
- OpenClaw's own Zalo channel docs say DM default is `dmPolicy: "pairing"`, so unknown senders get a code and are ignored until approved. Public access needs `dmPolicy: "open"` plus `"*"`. — [OpenClaw docs: Zalo](https://docs.openclaw.ai/channels/zalo)

### Inferences
- OpenClaw has the biggest ecosystem and the best Zalo coverage, but its skills marketplace is a proven malware channel and default host-level tool execution is a real risk. Any deployment should:
  - never install ClawHub skills unvetted
  - run in a sandbox or VPS rather than on the owner's laptop
  - keep the Control UI off the public internet
  - stay on current releases
- "Shared team deployment is just configuration" (README) and "no hostile multi-tenant isolation" (security docs) can both be true: teammates are trusted, customers are not.

### Gaps
- The latest OpenClaw release tag and date as of Oct 2026 could not be read; the GitHub page scrape did not show releases. Versioning is date-based (vYYYY.M.D) per the older README.
- I could not read primary text of the security docs page; the trust-model quotes come from search-result summaries of docs.openclaw.ai.
- I found no data on the number of OpenClaw users in Vietnam.

---

## 4. Zalo support across the three, and which Zalo path suits which use

### Takeaway
OpenClaw has by far the strongest Zalo story. It offers three paths:
- **(a)** a bundled official **Zalo Bot API** channel, marked experimental, that supports DMs and mention-gated groups
- **(b)** **Zalo ClawBot**, a plugin built and maintained by Zalo itself (June 2026). It logs in by QR and is **owner-bound**: it only talks to its owner, which makes it ideal for a personal assistant and unusable for customer service.
- **(c)** an unofficial **Zalo Personal** channel via zca-js (personal account automation), plus community forks

Hermes has only community plugins. Javis OS advertises Zalo via QR, with the mechanism unverified.

### Cited Findings
- OpenClaw bundled Zalo channel:
  - "Status: experimental. Direct messages and group chats are both implemented."
  - It covers "Zalo Bot Creator / Marketplace bots" created at bot.zaloplatforms.com. "Zalo Official Account (OA) bots are a different product surface ... This page does not cover them."
  - Long-polling by default, webhook optional (HTTPS, secret)
  - text chunked to 2,000 chars (Zalo API limit), media default 5 MB, groups require an @mention
  - reactions, threads, polls and native commands are not supported
  - multi-account is supported via `channels.zalo.accounts.<id>`

  — [OpenClaw docs: Zalo](https://docs.openclaw.ai/channels/zalo)
- Zalo ClawBot (official, by Zalo):
  - plugin `@zalo-platforms/openclaw-zaloclawbot` v0.1.4, "Active / Beta", requires OpenClaw ≥ 2026.4.10
  - login: "Scan the terminal-rendered QR code with the Zalo mobile app, accept the Terms of Use inside the Zalo Mini App"
  - "Zalo ClawBot is an **owner-bound personal assistant** on shared official infrastructure... binds a newly provisioned, private bot under a shared official OA directly to your Zalo user ID... **the bot only communicates with its owner. Messages from other users are dropped at the platform level**... uses Zalo Bot Platform APIs, not browser or web-session automation."

  — [OpenClaw docs: Zalo ClawBot](https://docs.openclaw.ai/channels/zaloclawbot)
- Zalo's announcement (30 Jun 2026): Zalo "phát triển plugin chính thức cho OpenClaw" (developed an official plugin for OpenClaw), which replaces bot creation, authentication and API-key entry with a QR scan. Lê Thị Kim Xuyến, Zalo's Director of Digital Transformation, said: "Những thao tác như tạo bot, xác thực, nhập API key vẫn còn rất khó với phần lớn người dùng" (steps like creating a bot, authenticating and entering an API key are still very hard for most users). Zalo itself says the experience currently "phù hợp hơn với nhóm người dùng sẵn sàng thử nghiệm công nghệ và chủ động thiết lập công cụ cho nhu cầu cá nhân" (better suited to users willing to experiment and set up tools for personal needs). — [Tiền Phong, 30 Jun 2026](https://tienphong.vn/zalo-cap-nhat-phuong-thuc-ket-noi-den-openclaw-don-gian-hoa-trai-nghiem-voi-agentic-ai-post1855677.tpo); [Zalo blog](https://zalo.me/vi/blog/zalo-cap-nhat-phuong-thuc-ket-noi-den-openclaw-don-gian-hoa-trai-nghiem-voi-agentic-ai)
- ZNews reports that Zalo is one of OpenClaw's official channels alongside WhatsApp and Telegram. — [ZNews (via search summary)](https://znews.vn/zalo-la-nen-tang-ket-noi-chinh-thuc-voi-openclaw-post1640255.html)
- OpenClaw Zalo Personal (`zalouser`): automates "a personal Zalo user account" via zca-js. It is unofficial and **experimental**, intended for cases "where the Zalo Bot API is not available". — [OpenClaw docs: Zalo personal](https://docs.openclaw.ai/channels/zalouser)
- Community OpenClaw Zalo plugins:
  - **openzalo** (`@tuyenhx/openzalo`, via the `openzca` CLI) handles personal accounts and multi-account. It has per-group `tools.allow/deny`, `dmPolicy` and `groupPolicy`, and per-group system prompts. — [GitHub darkamenosa/openzalo](https://github.com/darkamenosa/openzalo)
  - **caochitam/zalo-personal** offers QR login, keep-alive and "141 Tool Actions". — [GitHub caochitam/zalo-personal](https://github.com/caochitam/zalo-personal)
- Hermes: only the community plugins described in section 1 (zca-js personal beta; Bot/OA community plugin). — [awesome-hermes-skills](https://github.com/ZeroPointRepo/awesome-hermes-skills)
- Javis OS: its connector catalogue lists Zalo and says "quét QR (Zalo)" and "2 số Zalo". — [javisos.com](https://www.javisos.com/)

### Inferences
- For use case (a), a personal assistant on the owner's phone, **OpenClaw + Zalo ClawBot is the lowest-risk Zalo path available in 2026**. It is official, built by Zalo, uses no web-session hacks, and is owner-only by design. Telegram is a mature fallback for all three frameworks.
- For use case (b), customer service on Zalo, ClawBot is ruled out by design. The bundled Zalo Bot channel can technically accept public DMs (`dmPolicy: "open"`), but it does not cover Zalo OA, the main business surface for CS in Vietnam, and it is experimental. Personal-account automation (zca-js) for CS risks account bans and violates Zalo's intent. CS on Zalo should go through the official **Zalo OA** API, which another researcher is covering (channels).

### Gaps
- No primary statement from Zalo on whether automating personal accounts via zca-js leads to bans. The OpenClaw docs page summary did not include ban warnings.
- I could not confirm whether any of the three frameworks has a maintained **Zalo OA** (Official Account) channel suitable for business CS.

---

## 5. Brief alternatives (Claude Agent SDK, n8n, Dify, LangGraph, Letta, CrewAI, Agno)

### Takeaway
None of the general frameworks gives you a "personal agent with memory + tools + Zalo" out of the box, but they fit **customer service** better because of workspaces/RBAC (Dify), deterministic workflows (n8n) or full code control (LangGraph). Evidence here is thin and mostly from vendor-adjacent blogs.

### Cited Findings
- Dify is described as "a full production platform with multi-tenant workspaces and role-based access control". Caveats:
  - its license reportedly restricts running a hosted multi-tenant SaaS without negotiation
  - SSO and granular RBAC are paid-tier features

  — [baeseokjae comparison](https://baeseokjae.github.io/posts/langflow-vs-n8n-vs-dify-2026/); [FutureAGI](https://futureagi.com/blog/best-no-code-llm-builders-2026/); [Sim.ai](https://www.sim.ai/library/dify-alternatives)
- n8n is a general automation engine with AI nodes, suited to agents acting across business systems (CRM, tickets, databases). It has no tenant model for a support product. — [lowcode.agency](https://www.lowcode.agency/blog/n8n-vs-dify); [baeseokjae](https://baeseokjae.github.io/posts/langflow-vs-n8n-vs-dify-2026/)
- LangGraph is the code-first option for fine-grained agent control. — [Sim.ai](https://www.sim.ai/library/dify-alternatives)
- Javis OS itself is built on Claude Code and the Claude Agent SDK as one of its "brains". — [GitHub javis-os credits](https://github.com/blogminhquy/javis-os)

### Inferences
- A sensible split: use a personal agent (OpenClaw/Hermes/Javis) only as the owner's private "brain", and build the customer-facing agent on a constrained stack such as Dify, n8n or custom code. That stack should have no shell or file tools, a fixed knowledge base, human handoff and per-tenant isolation, connected to Zalo OA, Facebook and web.

### Gaps
- No 2026 sources were found for Letta (MemGPT), CrewAI, Agno or the Claude Agent SDK regarding messaging channels, multi-tenancy or Vietnamese use. Their feature claims are not included here.

---

## 6. Suitability: personal executive assistant vs customer-facing CS agent

### Takeaway
All three named products are **personal agents designed for one trusted owner** with powerful host tools: shell, files, browser, skills. They fit use case (a), an executive assistant for calendar, email, reports, reminders and anomaly alerts. They are **not safe to expose directly to anonymous customers**. OpenClaw's docs explicitly say there is no isolation between hostile users sharing an agent, and Hermes defaults to deny-all and warns against allow-all for bots with terminal access.

### Cited Findings
- OpenClaw: one trusted operator per gateway. Untrusted users messaging a tool-enabled agent "share the same delegated tool authority". Use separate gateways and credentials per trust boundary. — [docs.openclaw.ai/security](https://docs.openclaw.ai/security)
- Hermes: default deny for non-allowlisted users. `GATEWAY_ALLOW_ALL_USERS=true` is "NOT recommended for bots with terminal access". — [Hermes docs](https://hermes-agent.nousresearch.com/docs/user-guide/messaging/)
- Javis OS "runs an AI brain with full rights on the machine". — [GitHub README](https://github.com/blogminhquy/javis-os)
- Prompt injection is pervasive in the skills ecosystem (36% of skills, per Snyk). — [Snyk](https://snyk.io/blog/toxicskills-malicious-ai-agent-skills-clawhub/)
- Personal-assistant fit:
  - Hermes has a built-in cron with delivery to any platform ("Daily reports... weekly audits"). — [Hermes README](https://github.com/nousresearch/hermes-agent)
  - Javis OS targets shop owners: morning revenue, slow-moving stock and draft captions, delivered via Telegram. — [javisos.com](https://www.javisos.com/)
  - OpenClaw has cron, webhooks and the browser as first-class tools. — [openclawdir README mirror](https://openclawdir.com/plugins/zalo-official-account-60bzdf)
- Mitigations exist if a personal agent must face others:
  - openzalo supports per-group `tools.deny: ["group:fs","group:runtime"]` and per-group system prompts. — [openzalo](https://github.com/darkamenosa/openzalo)
  - OpenClaw supports sandboxing configuration ("Tools run on the host for the main session unless you configure sandboxing"). — [GitHub README](https://github.com/openclaw/openclaw)

### Inferences
- Recommended architecture for a Vietnamese SME owner:
  - **(1) Personal "brain"**: OpenClaw (Zalo ClawBot or Telegram), Hermes (Telegram) or Javis OS (Telegram plus Vietnamese commerce connectors), on a dedicated VPS with read-only or draft permissions to business systems.
  - **(2) Separate CS agent** on official business channels (Zalo OA, Facebook Messenger, web widget), built on a constrained platform with no shell/file tools, a curated knowledge base, escalation to humans and logging.
  - The CS agent can hand off summaries or alerts to the personal brain, but they should not share tool authority.
- Choosing among the three for (a):
  - **OpenClaw** if official Zalo is essential, but with strict security hygiene.
  - **Hermes** for the most robust and best-funded engine with self-learning, if Telegram is acceptable.
  - **Javis OS** for the most turnkey Vietnamese business integrations, accepting single-maintainer risk and paying for API keys rather than relying on a Claude subscription.

### Gaps
- No published benchmark of prompt-injection resistance for these three frameworks in customer-facing settings.
- No documented production case study of OpenClaw, Hermes or Javis used as a public CS bot in Vietnam was found.

---

## 7. Multi-tenant and replication: one install for many clients, or one per client?

### Takeaway
For all three, the safe pattern is **one isolated instance per client**: a separate container or VPS, separate credentials, separate memory. OpenClaw's docs explicitly prescribe separate gateways per trust boundary. Javis OS documents running several isolated instances on one VPS by changing three environment variables. Hermes ships Docker and serverless backends, and Nous has announced but apparently not yet shipped a business edition. Replication is technically quick (one-line installers, Docker compose, one-click Hostinger for Javis), but each instance still needs per-client LLM keys, channel logins (Zalo QR or bot token) and security hardening.

### Cited Findings
- OpenClaw: split trust boundaries need a "separate gateway + credentials, ideally separate OS users/hosts". — [docs.openclaw.ai/security](https://docs.openclaw.ai/security)
- OpenClaw: "Multi-agent routing — route inbound channels/accounts/peers to isolated agents (workspaces + per-agent sessions)". This is isolation of context, not hostile isolation. — [README mirror](https://openclawdir.com/plugins/zalo-official-account-60bzdf)
- OpenClaw one-click options: OpenClaw appears in the 1Panel app store ([1panel.pro](https://1panel.pro/apps/openclaw)), and a third-party hosted service claims "Deploy in 30 seconds" including a Zalo bot guide ([openclawlaunch.com](https://openclawlaunch.com/guides/openclaw-zalo)). Fake installers are a documented threat. — [HKCERT](https://www.hkcert.org/blog/openclaw-s-rapid-adoption-exposes-skills-supply-chain-and-fake-installer-risks-in-a-high-privilege-ai-agent-platform)
- Hermes:
  - Dockerfile, docker-compose and Nix in the repo
  - serverless backends ("Daytona and Modal offer serverless persistence — your agent's environment hibernates when idle")
  - per-chat session store
  - a third-party "How to run Hermes Agent on Zo" tutorial

  — [Hermes README](https://github.com/nousresearch/hermes-agent); [Hermes docs](https://hermes-agent.nousresearch.com/docs/user-guide/messaging/); [Zo Computer](https://www.zo.computer/tutorials/how-to-run-hermes-agent-on-zo)

  Business edition: "we will build and deliver Hermes for Businesses". — [Nous, 7 Oct 2026](https://nousresearch.com/a-note-on-our-fundraise)
- Javis OS:
  - "Several Javis instances on one VPS — Brains, settings and accounts stay fully separate per instance", using a shared proxy plus `docker-compose.multi.yml`, or a redeployed Hostinger stack
  - auto-update option (`JAVIS_AUTO_UPDATE=true`)
  - branding and custom domains ("app mang tên bạn", i.e. the app carries your name)

  — [GitHub README](https://github.com/blogminhquy/javis-os); [javisos.com](https://www.javisos.com/)

### Inferences
- For a "clone for many clients" business model, Javis OS has the most explicit white-label and multi-instance tooling, but the weakest maintenance backing. OpenClaw and Hermes need you to build your own provisioning (Docker template, secrets management, update pipeline), but rest on much larger communities.
- Per-client cost drivers are the LLM API (not consumer subscriptions, per the Javis README warning), the VPS, and your time handling updates and security patches. All three ship very frequent releases (Hermes 39 releases in about 8 months; Javis at v0.103), so a managed update process is essential.
- Auto-update is convenient but can push breaking changes to many clients at once. It is a trade-off.

### Gaps
- No official managed or hosted multi-tenant offering with pricing was confirmed for OpenClaw, Hermes or Javis OS as of 14 Oct 2026.
- No measured per-instance RAM/CPU figures were found for any of the three.

---

## 8. What Vietnamese communities are saying (2026)

### Takeaway
Vietnamese coverage is mostly OpenClaw + Zalo news, driven by Zalo's official plugin (June–July 2026), plus Facebook-group and blog tutorials. Hermes has dedicated Vietnamese Facebook groups. Javis OS spreads through its author's and influencers' Facebook posts as a free, Vietnamese "AI OS for business owners". I found no VnExpress coverage.

### Cited Findings
- Tiền Phong (30 Jun 2026) and the Zalo blog covered Zalo's official OpenClaw plugin. — [Tiền Phong](https://tienphong.vn/zalo-cap-nhat-phuong-thuc-ket-noi-den-openclaw-don-gian-hoa-trai-nghiem-voi-agentic-ai-post1855677.tpo); [Zalo blog](https://zalo.me/vi/blog/zalo-cap-nhat-phuong-thuc-ket-noi-den-openclaw-don-gian-hoa-trai-nghiem-voi-agentic-ai)
- ZNews covered Zalo as an official OpenClaw channel. — [ZNews](https://znews.vn/zalo-la-nen-tang-ket-noi-chinh-thuc-voi-openclaw-post1640255.html)
- CellphoneS Sforum ran a step-by-step "Cách kết nối Zalo với OpenClaw AI cho người mới" (how to connect Zalo to OpenClaw AI for beginners), dated 6 Jul 2026 per search summary. — [Sforum](https://cellphones.com.vn/sforum/cach-ket-noi-zalo-voi-openclaw-ai)
- A vnreview forum thread is titled about people queuing at a Chinese conglomerate to install an agent "tích hợp cho cả Zalo của người Việt" (integrated even with Vietnamese users' Zalo). I only saw the title. — [vnreview](https://vnreview.vn/threads/nguoi-gia-xep-hang-dong-nghit-duoi-chan-1-tap-doan-o-trung-quoc-de-cai-dat-1-agent-tich-hop-cho-ca-zalo-cua-nguoi-viet.80278/latest)
- Facebook groups:
  - "Hermes Agent VN" (Zalo plugin announcements) — [FB group](https://www.facebook.com/groups/2600004717066101/)
  - "vnhermesagent" (guide to turning a personal Zalo account into a Hermes agent) — [FB post](https://www.facebook.com/groups/vnhermesagent/posts/1370697711590149/)
  - an OpenClaw group post "hướng dẫn cài openzalo - biến zalo thành kênh chat cho openclaw" (installing openzalo to turn Zalo into a chat channel for OpenClaw) — [FB post](https://www.facebook.com/groups/852586990732832/posts/880728651251999/)
- A Vietnamese-language comparison on kimi.com (updated 22 Jul 2026) frames Hermes as "agent software with a built-in learning loop" and OpenClaw as built around a Gateway control plane. — [kimi.com/vi](https://www.kimi.com/vi/resources/hermes-vs-openclaw)
- English comparisons commonly conclude: choose OpenClaw for ecosystem and channel breadth, Hermes for self-improvement. Both run real shell and browser, so use a sandboxed VPS. — [Composio](https://composio.dev/content/openclaw-vs-hermes-agent); [igeeksblog](https://www.igeeksblog.com/hermes-vs-openclaw/)
- Javis OS spread: author posts ("vác về sài làm trợ lý AI free", i.e. take it and use it as a free AI assistant), influencer setup sessions, and blog guides. — [FB Minh Quý](https://www.facebook.com/minhquy1711/posts/ae-l%E1%BA%A5y-link-repo-github-c%E1%BB%A7a-javis-os-v%C3%A1c-v%E1%BB%81-s%C3%A0i-l%C3%A0m-tr%E1%BB%A3-l%C3%BD-ai-free-t%E1%BA%A1i-javisosco/3620187278120366/); [FB treyypham](https://www.facebook.com/treyypham/posts/th%E1%BA%A5y-nhi%E1%BB%81u-ae-inbox-h%E1%BB%8Fi-v%E1%BB%81-con-tr%E1%BB%A3-l%C3%BD-ai-javis-os-m%C3%ACnh-%C4%91ang-d%C3%B9ng-n%C3%AAn-m%C3%ACnh-quy%E1%BA%BFt-/122123454945123888/); [blogdaytinhoc](https://blogdaytinhoc.com/huong-dan-cai-dat-va-su-dung-javis-os-don-gian-cho-nguoi-moi-1454)

### Inferences
- The Vietnamese conversation is tutorial and hype driven ("free AI assistant", "scan QR to connect Zalo"). Security caveats (ClawHub malware, exposed instances, ToS risk of subscription use) are largely absent from Vietnamese consumer coverage.
- Zalo's own investment in an OpenClaw plugin signals that OpenClaw is the ecosystem Zalo is betting on for agent access, at least for personal assistants.

### Gaps
- No VnExpress, Genk, Tinhte or Viblo articles on Hermes, Javis OS or OpenClaw surfaced in my searches. They may exist but were not found.
- Facebook group content (sentiment, member counts, complaints) could not be read beyond search snippets.
- No YouTube Vietnamese tutorial content was verified.
