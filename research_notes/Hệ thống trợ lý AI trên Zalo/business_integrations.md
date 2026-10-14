# Business-System Integrations for a Vietnamese AI Assistant (payments, POS/inventory/orders, shipping, booking, owner email/calendar/reports) — as of 14 Oct 2026

Notes for the report writer:
- USD figures use an indicative rate of about 26,000 VND/USD. I did not verify the October 2026 rate, so treat every USD number as approximate.
- Research method: direct HTTP fetches of .vn and google.com sites were blocked by the sandbox proxy. Official pages were read through Firecrawl scrapes/search and other web searches instead. Firecrawl cache dates were 12–14 Oct 2026, so the prices below are current as of those dates unless marked otherwise.
- Every claim has an inline source. Anything I could not verify from an official page is flagged as such.

---

## 1. Bank payment confirmation in Vietnam ("Have you received my payment?")

### Takeaway
The standard pattern among Vietnamese SMEs in 2026 is a "bank-feed middleman" that watches the shop's own bank account and pushes a webhook for each incoming transfer. The shop's server matches the order code in the transfer content (nội dung chuyển khoản) or in a dynamic VietQR. The main providers are SePay, Casso Flow and payOS, which is Casso's free gateway. SePay is the most flexible and cheapest to start: it has a free tier of 50 incoming transactions per month, monthly plans from 96,000đ, 12 banks connected directly by API, and an OAuth2 partner API for multi-client platforms. payOS is effectively free (bank-sponsored packs) when the merchant uses one of its partner banks. Card and e-wallet gateways (VNPay, MoMo, ZaloPay) charge percentage fees. I found no published 2026 fee schedule for them.

### Cited Findings
**How it works (SePay as the reference model)**
- SePay POSTs a JSON webhook for each bank transaction. Example payload fields: `id`, `gateway` (bank), `transactionDate`, `accountNumber`, `code` (payment code), `content` (transfer description, e.g. "SEVN63DC8E5C chuyen tien"), `transferType` ("in"), `transferAmount`, `referenceCode` — [SePay Developer home](https://developer.sepay.vn/vi)
- `code` is "a payment code, automatically detected by SePay based on Company → General Settings. Can be null if not detected." In other words, the shop sets an order-code pattern and SePay pulls it out of the transfer content — [SePay "Create Webhooks" docs, via search excerpt](https://developer.sepay.vn/en/sepay-webhooks/tich-hop-webhook)
- Retries: the webhook is retried when the endpoint is unreachable or returns a non-2xx status. There are 8 attempts in total (1 + 7 retries) at Fibonacci spacing, covering about 33 minutes, after which the delivery is marked Failed and an alert is sent. The BankHub IPN flow only retries on network errors — [SePay error-handling docs](https://developer.sepay.vn/en/sepay-webhooks/xu-ly-loi); [SePay BankHub IPN](https://developer.sepay.vn/en/bankhub/thong-bao-bien-dong-so-du). The two pages disagree on retry behaviour, so check which flow is in use.
- SePay also documents reconciliation (đối soát) through its API and dashboard, so a missed or duplicate webhook can be caught — [SePay Đối soát giao dịch](https://developer.sepay.vn/vi/sepay-webhooks/doi-soat-giao-dich)
- SePay has a hosted checkout/payment-gateway SDK. Example: `SePayClient(MERCHANT_ID, SECRET, 'sandbox')`, `orderInvoiceNumber`, `orderAmount`, success/error/cancel URLs — [SePay cổng thanh toán quick start](https://developer.sepay.vn/vi/cong-thanh-toan/bat-dau)
- Dynamic VietQR without any middleman: the VietQR.io "Quick Link" image URL is `https://img.vietqr.io/image/<BANK_ID>-<ACCOUNT_NO>-<TEMPLATE>.png?amount=<AMOUNT>&addInfo=<DESCRIPTION>&accountName=<NAME>`. The amount and order code go into the QR, and the customer's banking app pre-fills them — [Viblo tutorial](https://viblo.asia/p/tao-ma-qrcode-chuyen-tien-ngan-hang-7ymJXnd5Vkq); [vietqr-io/vietqr-api GitHub](https://github.com/vietqr-io/vietqr-api). **I could not confirm from an official source whether Quick Link is free or what its rate limits are.**

**SePay pricing (official pricing page, scraped Oct 2026)** — [SePay Bảng giá](https://sepay.vn/bang-gia.html)
- FREE: 0đ/month, 50 transactions/month. Overage is allowed and charged. Includes webhook, API and payment gateway.
- STARTUP: 96,000đ/month (about $3.7) on annual billing ("Tiết kiệm thêm 15%"), 180 transactions/month (about 533đ/transaction). Larger tiers lower the unit price: 1,000 tx ≈ 471đ/tx, 5,100 ≈ 389đ, 12,000 ≈ 332đ, 58,000 ≈ 271đ, up to 986,000 tx/month ≈ 68đ/tx. Every plan has every feature.
- Only **incoming** transactions count ("tổng số lượng giao dịch tiền vào trong thời gian một tháng"). Bank accounts are unlimited. Both personal and business accounts are supported. Data-retention time is configurable.
- SHOP plan: 70,000đ/store/month (annual), unlimited transactions, balance-change sharing per store. **"Không hỗ trợ API" and no payment-gateway integration**, so an AI agent cannot use this plan.
- Banks: "SePay đã kết nối API trực tiếp đến ngân hàng Vietcombank, BIDV, VietinBank, MB, ACB, VPBank, TPBank, Sacombank, MSB, OCB, KienLongBank và Shinhan Bank. Các ngân hàng còn lại kết nối qua SMS Banking." API-connected banks are described as syncing instantly ("Giao dịch được đồng bộ tức thì"). Bank promotions include free transaction packs for opening VietinBank, Vietcombank or BIDV business accounts through SePay.
- SePay eShop (its own POS): Cơ bản 50,000đ/month (in-store sales, e-invoice, bank connection plus payment confirmation, payment speaker). Chuyên nghiệp 99,000đ (online store, basic inventory, multiple stores, shipping carriers) and Toàn diện 199,000đ are marked "Dự kiến ra mắt" (coming soon).

**Casso Flow pricing (official, annual-only)** — [Casso Flow Bảng giá](https://casso.vn/flow/bang-gia/); [Casso Bảng giá](https://casso.vn/bang-gia/)
- SPONSOR: free, 1,000 transactions per pack, renewable ("nhận nhiều lần"), for virtual accounts (VA) at OCB, LPBank and KienlongBank. 1 bank account/VA, 2 app integrations.
- PIONEER: 790,000đ/year (about $30) at the 1,000-transaction tier, scaling to 1.2M transactions/year. Covers "official API" partner banks: open-banking services of VPBank, BIDV, MB, ACB, Vietcombank, plus VAs at BIDV, ACB, OCB, LPBank, KienlongBank, PVComBank, Shinhan. 10 bank accounts.
- STANDARD: 1,190,000đ/year (about $46) at 1,000 transactions. Adds RPA (screen-scraping-style sync) for Vietcombank, Techcombank, BIDV, VPBank, OCB, Eximbank, MSB, TPBank, Timo, plus VietinBank open banking. Maximum 2 RPA-connected accounts.
- Counting rule: "Giao dịch = tiền vào + tiền ra trên mọi tài khoản liên kết. VA chỉ tính tiền vào." Debits count too, unlike SePay. No monthly plan and no refunds after payment. 14-day trial with 100 real transactions.
- Latency: "Official API cập nhật realtime; RPA đồng bộ theo lịch." Security: "Dữ liệu chỉ đọc, không có quyền chuyển tiền"; ISO/IEC 27001:2022.
- Outputs: Telegram, Slack, Google Chat, Discord, custom webhooks, WordPress/WooCommerce plugin for automatic order confirmation.

**payOS (by Casso) — free A2A gateway**
- Model: A2A. Money goes straight into the merchant's own bank account, the dynamic QR contains the order info, and payment links work for social selling. Sign-up takes "05 phút" with only a CCCD (citizen ID) and no business licence. Runs on GCP and advertises ISO/IEC and SOC 2/SOC 3 — [payOS home](https://payos.vn/)
- Free packs: every account gets FREE-100 and PIONEER-500 once each. Bank-sponsored 1,000-transaction packs (OCB-1000, MB Bank-1000) are renewable once 50% is used; payOS states it offers 1,000,000 OCB and 100,000 MB transactions free in total — [payOS hướng dẫn nâng cấp gói](https://payos.vn/huong-dan-nang-cap-goi-payos/)
- From 23/01/2026 there is a free pack "KLB-1000" (KienlongBank), renewable without limit, for individuals and household businesses (HKD) — [payOS announcement 28/01/2026](https://payos.vn/cong-thanh-toan-mien-phi-payos-2026/); [payOS Kênh thu](https://payos.vn/thu-ho/)
- For business customers with BIDV: "Miễn phí gói BIDV-1K (1.000 giao dịch/năm), tặng chữ ký số và tới 06 Soundbox", valid until 31/12/2026 — [payOS home](https://payos.vn/)
- Paid: "Gói Flex" deducts x% of each successful order from prepaid credit. Enterprise (>100,000 tx/year) is negotiated. **The Flex percentage was not visible in the sources (it is in an image)** — [payOS 3 phút hiểu rõ](https://payos.vn/3-phut-hieu-ro-ve-payos/)
- Plugins for Sapo, Haravan, WordPress — [payOS là gì](https://payos.vn/payos-la-gi/). payOS also launched a **payout** ("Kênh chi – chi hộ tự động") product — [payOS home](https://payos.vn/). An AI agent should never hold that capability (see section 8).

**Bank Open APIs and legal status**
- Circular 64/2024/TT-NHNN (Open API in banking) was issued 31/12/2024 and took effect 01/03/2025. The basic Open API list includes exchange rates, interest rates and customer information such as accounts and transaction history. Banks must sign a contract with each third party and publish Open API information before connecting — [thuvienphapluat.vn](https://thuvienphapluat.vn/hoi-dap-phap-luat/open-api-la-gi-nguyen-tac-trien-khai-open-api-tu-ngay-132025-138042814.html); [luatvietan.vn](https://luatvietan.vn/chia-se-du-lieu-qua-giao-dien-lap-trinh-ung-dung-mo-open-api-la-gi.html); [SBV article](https://sbv.gov.vn/en/w/open-api-tr%E1%BB%A5c-k%E1%BA%BFt-n%E1%BB%91i-m%E1%BB%9Bi-c%E1%BB%A7a-h%E1%BB%87-sinh-th%C3%A1i-t%C3%A0i-ch%C3%ADnh-s%E1%BB%91-vi%E1%BB%87t-nam); [thuvienphapluat – obligations](https://thuvienphapluat.vn/hoi-dap-phap-luat/ngan-hang-co-bat-buoc-phai-cong-khai-thong-tin-open-api-tren-trang-thong-tin-dien-tu-khong-138043899.html)
- Banks with formal open-banking or VA integrations through middlemen (Casso list): VPBank, BIDV, MB, ACB, Vietcombank, VietinBank (open banking); BIDV, ACB, OCB, LPBank, KienlongBank, PVComBank, Shinhan, VietinBank (VA). Techcombank appears only as RPA in Casso's list — [Casso Flow](https://casso.vn/flow/bang-gia/)
- MB Bank: third parties such as GPM Pay advertise "MB Open API" real-time balance webhooks (vendor claim, unverified). Press reports describe MB offering 600+ API sets for BaaS and real-time balance push to enterprise systems — [Viblo GPM Pay](https://viblo.asia/p/api-mb-bank-giai-phap-mb-open-api-chinh-thuc-tu-gpm-pay-G24B8m6GLz3); [thitruongtaichinhtiente.vn](https://thitruongtaichinhtiente.vn/print/44819.html)
- Unofficial libraries such as `mbbank-lib` log in with the account holder's credentials. This is not an official integration and carries security and compliance risk — [PyPI mbbank-lib](https://pypi.org/project/mbbank-lib/)

**Card/e-wallet gateways (VNPay, MoMo, ZaloPay)**
- I found no published 2026 merchant fee schedule. A bank tariff dated 15/06/2026 shows POS card MDRs such as 0.85% for domestic cards in retail fuel and says VNPAY may charge customers for international cards — [Nam A Bank biểu phí 15/06/2026 (PDF)](https://www.namabank.com.vn/Data/Sites/1/media/bieu-phi/doanh-nghiep/2026/website_bi%E1%BB%81u-ph%C3%AD-d%E1%BB%8Bch-v%E1%BB%A5-%C3%A1p-d%E1%BB%A5ng-cho-%C4%91%C6%A1n-v%E1%BB%8B-ch%E1%BA%A5p-nh%E1%BA%ADn-thanh-to%C3%A1n---15.06.2026.pdf)
- Competitor Pay2S: Free plan 50 transactions/month and 1 bank account; paid plans have unlimited transactions and no per-transaction fee — [Pay2S bảng giá](https://pay2s.vn/bang-gia)

**Replication (many clients)**
- SePay offers **OAuth 2.0 for partners**, so a platform can act on behalf of many merchants' SePay accounts, plus API Webhooks over OAuth2 — [SePay OAuth2 overview](https://developer.sepay.vn/vi/sepay-oauth2/tong-quan); [SePay OAuth2 API Webhooks](https://developer.sepay.vn/vi/sepay-oauth2/api-webhook)
- SePay Bank Hub has an idempotent `PUT /webhooks/config` that creates or updates a webhook (URL, secret, events) — [SePay BankHub API](https://developer.sepay.vn/vi/bankhub/api/api-webhook/cap-nhat-webhook)

### Inferences
- **Best for a single SME:** SePay FREE (≤50 incoming tx/month) or STARTUP (96k/month) if the shop already banks with one of SePay's 12 API banks. payOS is cheapest (0đ) if the owner will open or use an OCB, MB, KienlongBank or BIDV (business) account and is happy with gateway-style payment links. Casso suits shops that need Techcombank (RPA only) or a richer bookkeeping/reporting suite, but annual billing and counting debits make it pricier for high-volume shops.
- **Best for replicating to many clients:** SePay, because of the OAuth2 partner model, one consistent webhook schema across 12 banks, per-client plans the client pays for, and an API sandbox. With payOS, each client creates their own account (5 minutes with a CCCD) and pastes the client ID, API key and checksum key into your platform. That is also easy, and free for the client.
- For a chat assistant, the reliable answer to "đã nhận tiền chưa?" is: generate a per-order code (e.g. `DH12345`), show a dynamic VietQR with amount plus code, store webhooks in a DB, and let the agent query `payments WHERE code = 'DH12345'`. Typing an exact amount and free-text name without a code leads to ambiguous matches.
- RPA-based connections (Casso Standard) sync on a schedule and are slower than official-API banks. For instant answers, push clients toward API-connected banks.

### Gaps
- VNPay, MoMo and ZaloPay merchant fees for 2026: not found in official sources.
- payOS Flex percentage: not readable (image only).
- Exact webhook latency in seconds for SePay or Casso: only qualitative claims ("tức thì", "realtime").
- Whether Circular 64 has a transition deadline (e.g. 2027) for banks' existing APIs: not confirmed.
- VietQR.io Quick Link and API key pricing and limits: not found.

---

## 2. Inventory / POS / order management (check stock, look up and create orders)

### Takeaway
KiotViet, Haravan, Sapo, Nhanh.vn and Pancake POS all expose REST APIs, and most support webhooks. KiotViet's Public API is locked to its most expensive "Gói Cao cấp" (490,000đ/branch/month). Haravan and Sapo use per-store "private app" tokens. By October 2026 there are several **community** MCP servers for KiotViet, Haravan, Sapo and Pancake POS, but **none are official**. No official n8n nodes exist for these Vietnamese platforms. MISA eShop/AMIS has no public API documentation that I could find.

### Cited Findings
**KiotViet** (largest SME POS; "Hơn 300.000 cửa hàng")
- Public API (retail) uses OAuth 2.0 client credentials (ClientId + "Mã bảo mật"/client_secret), token endpoint `https://id.kiotviet.vn/connect/token`, `expires_in: 86400` (24h). Rate limit: "Với các hàm GET sẽ giới hạn 5000 request/1h" — [KiotViet Public API doc, updated 06/10/2026](https://www.kiotviet.vn/huong-dan-su-dung-kiotviet/retail-ket-noi-api/public-api/)
- Endpoints (base `https://public.kiotapi.com`): products (GET/POST), stock on hand `GET /productOnHands`, orders `GET/POST /orders` (create order), invoices `GET/POST /invoices`, customers, pricebooks, branches, payment QR (section 2.29) — [same](https://www.kiotviet.vn/huong-dan-su-dung-kiotviet/retail-ket-noi-api/public-api/)
- Webhooks: `customer.update/delete`, `product.update/delete`, **`stock.update`**, **`order.update`**, **`invoice.update`**, pricebook/category/branch events. Registered via `POST /webhooks` with an optional `Secret` — [same](https://www.kiotviet.vn/huong-dan-su-dung-kiotviet/retail-ket-noi-api/public-api/)
- **Plan requirement and price:** retail tariff is Gói hỗ trợ 270,000đ, Gói chuyên nghiệp 330,000đ, Gói cao cấp 490,000đ per branch per month. "Hỗ trợ kết nối API" appears only under Cao cấp. Hotel and beauty tables also say "Public API (Gói Cao cấp)". All plans include KiotViet's shipping gateway and free e-invoices — [KiotViet Phí dịch vụ](https://www.kiotviet.vn/phi-dich-vu). The salon and hotel API docs state "Phạm vi áp dụng: gian hàng sử dụng gói Cao cấp" — [KiotViet salon API](https://www.kiotviet.vn/huong-dan-su-dung-kiotviet/salon-ket-noi-api/ket-noi-api-salon/); [KiotViet hotel API](https://www.kiotviet.vn/huong-dan-su-dung-kiotviet/hotel-ket-noi-api/ket-noi-api/). An older notice lists Chuyên nghiệp at 310,000đ, so prices changed over time — [KiotViet price notice](https://www.kiotviet.vn/kiotviet-thong-bao-dieu-chinh-gia-ban-san-pham-phan-mem-quan-ly-ban-hang/)
- Community MCP servers on GitHub, all unofficial with few stars: `haudnn/kiotviet-mcp` (TypeScript, 36 tools covering products, customers, orders, invoices; updated Jul 2026), `dehuy69/kiotviet-mcp` (FastMCP, "securely handles multi-tenant authentication"), `blackmouse572/kiotviet-mcp` ("Multi tenant support with BYO, key rotation, envelope encrypt"), `minhpttk/mcp-kiotviet` ("KiotViet SDK"), and others — [GitHub search results](https://github.com/haudnn/kiotviet-mcp); [dehuy69](https://github.com/dehuy69/kiotviet-mcp); [blackmouse572](https://github.com/blackmouse572/kiotviet-mcp); [PulseMCP listing](https://www.pulsemcp.com/servers/haudnn-kiotviet); [mcpservers.org](https://mcpservers.org/servers/minhpttk/mcp-kiotviet)
- n8n: a third-party npm package "n8n integration for KiotViet" v0.4.0 (publisher doivjpxx); not official — [npm](https://www.npmjs.com/~doivjpxx). A Membrane "KiotViet" agent skill also exists — [skills.sh](https://www.skills.sh/membranedev/application-skills/kiotviet). An Odoo app module integrates KiotViet — [Odoo Apps](https://apps.odoo.com/apps/modules/18.0/ta_kiotviet_integration)

**Haravan**
- Private app: "Only the Store owner can create Private app". It installs only on that single store. The token is sent as Bearer to `apis.haravan.com` — [Haravan private app docs](https://docs.haravan.com/docs/tutorials/authentication/private-app/); [overview](https://docs.haravan.com/docs/get-started/overview)
- Scopes: `com.read_orders`/`com.write_orders`, `com.read_inventories`/`com.write_inventories`. Webhook scope `wh_api`. Webhooks are configured in admin under Settings → Notifications → Webhooks — [Haravan access scopes](https://docs.haravan.com/docs/omni-apis/access-scopes/)
- Community MCP: `pluginmd/Haravan-MCP` ("70 tools: orders, products, customers, inventory, RFM segmentation, smart analytics"; supports OAuth 2.0 and Docker HTTP mode; 19 stars, updated 03 Oct 2026) — [GitHub](https://github.com/pluginmd/Haravan-MCP)
- n8n community node `n8n-nodes-nqdev` (eSMS, Haravan, Zalo); minimal adoption — [ecosyste.ms](https://science.ecosyste.ms/projects/38399)

**Sapo**
- Webhook events: Order create/updated/paid/cancelled/fulfilled/partially_fulfilled, Fulfillment, Product, Refund, Customer, Collection, Store, App. No dedicated inventory event is listed. The page looks templated from Shopify docs — [Sapo support – webhook](https://support.sapo.vn/sapo-webhook)
- Auth (from a community MCP README): store subdomain plus **Private App API Key + Secret** (`SAPO_STORE`, `SAPO_API_KEY`, `SAPO_API_SECRET`). The MCP gates destructive operations behind `SAPO_ALLOW_OPS` and requires a bearer token when exposed over HTTP — [nguyennguyenit/sapo-mcp](https://github.com/nguyennguyenit/sapo-mcp)
- Price examples (product line unclear; the page does not mention API access): StartUp 170,000đ, PRO 249,000đ, OMNI 449,000đ per month, 7-day trial — [Sapo bảng giá](https://www.sapo.vn/bang-gia-sapo-go.html)

**Nhanh.vn**
- Open API v3 docs at apidocs.nhanh.vn with an `llms.txt` index. Inventory webhook `inventoryChange` gives remain/shipping/holding/damage/available totals and a per-warehouse (`depots`) breakdown, and may batch several products. The app needs the "Product list" permission — [Nhanh v3 inventory webhook](https://apidocs.nhanh.vn/v3/webhooks/inventory); [v2](https://apidocs.nhanh.vn/v2/webhooks/inventory)

**Pancake POS**
- Community MCP wraps `https://pos.pages.fm/api/v1` (orders, products, inventory, warehouses, webhooks) and needs an API key plus shop ID. It warns that the Pancake inbox API key is a different product. Another listing uses `https://api.pancake-pos.com`, so the base URLs conflict — [Glama Pancake POS MCP](https://glama.ai/mcp/servers/dhprk2nj9u); [The Daily Workflow](https://thedailyworkflow.com/mcp/server/pancake-pos-mcp)
- Order webhooks are set under Settings → Advanced → Third-party connection → Webhook — [woku.app docs](https://woku.app/docs/en/integrations/pancake.md)

**MISA eShop / AMIS**
- Marketing pages describe real-time inventory sync with FB, Shopee, Lazada, TikTok Shop, Web, Zalo and integration with MISA accounting and meInvoice. **No public API docs found** — [MISA eShop](https://www.misaeshop.vn/166/phan-mem-quan-ly-ban-hang-da-kenh/)

**Google Sheets as a lightweight "POS/DB"**
- Sheets API default quota: 300 read requests/minute/project and 60/minute/user/project, no daily cap while within per-minute quotas. HTTP 429 when exceeded; use exponential backoff — [Google Sheets API usage limits](https://developers.google.com/sheets/api/limits)
- Scopes: `spreadsheets` and `spreadsheets.readonly` are **sensitive** (need verification but not CASA). `drive.file` is non-sensitive. Full `drive`/`drive.readonly` are **restricted** — [Choose Google Sheets API scopes](https://developers.google.com/workspace/sheets/api/scopes)

### Inferences
- **The KiotViet cost trap:** a KiotViet shop on the 270k or 330k plan must upgrade to Cao cấp (+160–220k/branch/month, about $6–8.5) before any AI agent can read stock or orders through the official API. This should be in the client's cost estimate.
- Replication is easiest where the client can self-generate a credential and paste it into an onboarding form: KiotViet (ClientId/secret), Haravan or Sapo (private app token), GHN/GHTK (token). One multi-tenant MCP/tool server can then serve every client with per-tenant encrypted secrets. `dehuy69` and `blackmouse572` already attempt this for KiotViet. For scale and trust, a Haravan/Sapo **public OAuth app** would replace per-store private tokens, but app-store listing requirements were not researched.
- Because no MCP server is official and all have few stars, a production deployment should fork or audit one, or write a thin in-house tool layer exposing about 5 functions (`check_stock(sku)`, `get_order(code/phone)`, `create_draft_order(...)`, `get_daily_sales(date)`, `get_shipping_status(order)`), rather than handing the model a 36–70-tool server.
- For very small shops with no POS, a Google Sheet (stock list plus orders tab) behind a service account is the cheapest option. It is free and within quotas for chat-scale traffic.

### Gaps
- Sapo and Haravan: which subscription tier is required for API or private apps, and any API surcharge. Not found.
- Nhanh.vn and Pancake POS: API pricing and plan requirements; Nhanh v3 order endpoints not verified.
- MISA eShop/AMIS: whether a partner API exists.
- Odoo: not researched in this pass. Odoo is generally known to expose XML-RPC/JSON-RPC APIs, but I did not verify that in a source here.
- Official MCP servers from KiotViet, Sapo, Haravan, Nhanh or MISA: none found as of Oct 2026.

---

## 3. Shipping carriers (order shipping status)

### Takeaway
GHN, GHTK and Ahamove publish developer docs with token-based REST APIs and status webhooks. GHN webhooks must be configured by GHN support, while GHTK and Ahamove take a partner-provided callback URL. I could not find official Viettel Post or J&T Express Vietnam developer docs. Third-party aggregators (TrackingMore, AfterShip) cover Viettel Post tracking with webhooks.

### Cited Findings
- **GHN:** send `Token` and `ShopId` headers. The token is created in the customer portal under "Bảo mật → Token API & IP tin cậy", with optional trusted IPs. Staging `dev-online-gateway.ghn.vn` and production `online-gateway.ghn.vn` are separate accounts — [GHN API overview](https://developer.ghn.vn/en/docs/getting-started/overview); [GHN Get Token](https://developer.ghn.vn/en/docs/token/get-token)
- GHN order-status webhook: JSON POST with `ShopID`, `OrderCode`, `Status` and more. Return 2xx or it is retried. "Webhooks are configured by GHN, per client" through support (Basic auth or API-key header). Config changes take up to about 15 min. Duplicates are possible, so make the endpoint idempotent — [GHN Order Status Callback](https://developer.ghn.vn/en/docs/webhook/callback-order-status)
- **GHTK:** `Token` header plus `X-Client-Source` (partner code). The token is created in the GHTK portal with configurable expiry and permissions, including "Get order status". Hosts: `services.giaohangtietkiem.vn` (prod) and `services-staging.ghtklab.com`. The webhook posts `label_id`, `partner_id`, `status_id` and `action_time` to the partner URL (with a `hash` parameter); only HTTP 200 counts, with one retry otherwise — [GHTK webhook](https://api.ghtk.vn/en/docs/submit-order/webhook); [pro-docs.ghtk.vn webhook](https://pro-docs.ghtk.vn/5_webhook/); [GHTK overview](https://docs.giaohangtietkiem.vn/en/docs/submit-order/logistic-overview)
- **Ahamove (on-demand):** the API key comes after an integration form. Webhook callbacks fire on each order update from ACCEPTED onward. Callback auth can be API key, bearer or basic. Staging and prod are separate — [Ahamove webhook](https://developers-stg.ahamove.com/en/docs/webhook); [Ahamove intro](https://developers.ahamove.com/docs/introduction)
- **Viettel Post:** TrackingMore and AfterShip offer Viettel Post tracking APIs with webhooks — [TrackingMore](https://www.trackingmore.com/viettelpost-tracking-api); [AfterShip](https://www.aftership.com/th/carriers/viettelpost/api)
- KiotViet includes "Cổng vận chuyển KiotViet" in all plans — [KiotViet Phí dịch vụ](https://www.kiotviet.vn/phi-dich-vu). SePay eShop Chuyên nghiệp (upcoming) lists "Kết nối đơn vị vận chuyển" — [SePay Bảng giá](https://sepay.vn/bang-gia.html)
- MCP/n8n: no GHN, GHTK or Viettel Post MCP server or n8n node found in GitHub searches — [GitHub search (no relevant hits)](https://github.com/search?q=ghn+mcp&type=repositories)

### Inferences
- When the shop creates shipments from its POS (KiotViet, Haravan, Sapo shipping gateway), the agent can often read the tracking code and status from the POS order record. That needs one integration instead of one per carrier. Direct carrier APIs are a fallback, or a choice for shops that book shipments on carrier portals.
- GHN and GHTK credentials are self-serve tokens that clients can paste in, so replication is easy. GHN webhooks need a support request per client, so polling the GHN tracking API on demand ("where is my order?") is simpler to replicate.

### Gaps
- Official Viettel Post and J&T Express Vietnam developer API docs and costs: not found.
- Carrier API pricing: carriers generally do not charge for API access, but **I did not verify this**.

---

## 4. Booking / reservations (book tables, hold reservations; F&B, spa, clinic)

### Takeaway
No dominant Vietnamese booking platform with an open API turned up. KiotViet's beauty/salon and hotel editions have booking-related APIs on the Cao cấp plan. iPOS Booking (Foodbook) handles F&B table bookings but has no public API that I found. For an AI assistant, the practical options are (a) Google Calendar as the booking ledger, (b) Cal.com (free if self-hosted, with API and webhooks), or (c) a simple DB or Google Sheet of slots managed by the agent's own tools.

### Cited Findings
- KiotViet beauty/salon Public API (Cao cấp plan only) returns employees with `employeeBranches` including `bookingEmployeeId`. Doc updated 16/07/2025 — [KiotViet salon API](https://www.kiotviet.vn/huong-dan-su-dung-kiotviet/salon-ket-noi-api/ket-noi-api-salon/)
- KiotViet hotel API (Cao cấp) at `api-integration-hotel.kiotviet.vn/public/...` (pricebooks, sales channels, etc.) — [KiotViet hotel API](https://www.kiotviet.vn/huong-dan-su-dung-kiotviet/hotel-ket-noi-api/ket-noi-api/)
- KiotViet Web for beauty and hotel includes "Đặt hẹn online" plus Fanpage/Zalo chat — [KiotViet Phí dịch vụ](https://www.kiotviet.vn/phi-dich-vu)
- iPOS Booking (Foodbook) puts booking forms on website, Facebook and Zalo, sends confirmation and reminder messages, and allocates tables. No developer API mentioned — [App Store: iPOS Booking](https://apps.apple.com/vn/app/id1555430267)
- Bistrochat (regional, includes Vietnam) aggregates booking channels; public API unknown — [eber.co Bistrochat](https://eber.co/partners/bistrochat)
- Cal.com: self-hosted Community Edition is free to license (VPS about $5–20/month). Cloud Team tier is $12–15/seat/month depending on source, with prices in conflict across review sites. API and webhooks exist — [schedulingkit.com Cal.com pricing](https://www.schedulingkit.com/pricing-guides/cal-com-pricing); [prospeo Cal.com](https://prospeo.io/s/calcom-pricing-reviews-pros-and-cons); [livemy.app self-host guide](https://livemy.app/blog/self-host-cal-com)

### Inferences
- **Recommended default for small F&B, spa or clinic:** a "slots" table (Sheet or DB) plus agent tools `check_availability(date, party_size/service)`, `hold_reservation(...)` (status=HELD, TTL e.g. 15 min, optionally pending a deposit via SePay/payOS VietQR), and `confirm_reservation(...)`. Optionally mirror each booking into the owner's Google Calendar. This copies easily to every client and avoids vendor APIs.
- If the business already runs KiotViet beauty or hotel on Cao cấp, use KiotViet's booking data instead of a parallel system.
- Google Calendar API scopes are generally classed as *sensitive*, not restricted, so a multi-client app needs Google verification but **not** a CASA assessment. **I could not confirm this classification on Google's Calendar scope page** (the scraped page did not show the classifications).

### Gaps
- Public APIs for iPOS, CukCuk (MISA F&B) and Vietnamese spa or clinic booking software: not found.
- Official Cal.com 2026 pricing page: not retrieved; review-site numbers conflict.

---

## 5. Owner's personal-assistant integrations (Gmail/Outlook, Google Calendar, Sheets, Lark, Base.vn, Notion, Zalo groups)

### Takeaway
Reading Gmail is the hardest integration to replicate. Every Gmail read scope (`gmail.readonly`, `gmail.metadata`, `gmail.modify`) is **restricted**. A multi-client app that touches that data on a server needs Google restricted-scope verification **plus an annual CASA security assessment** (about $540–$1,800/year for Tier 2 via TAC Security). The other legal path is for each client to authorize under Google's personal-use, internal-Workspace or similar exceptions. Calendar and Sheets are lighter (sensitive scopes). Microsoft 365 needs publisher verification for multi-tenant apps. Lark has an **official** MCP server. Notion has an **official hosted** MCP. Base.vn has an owner-generated access-token API. Personal Zalo groups have **no official API**.

### Cited Findings
**Google (Gmail / Calendar / Drive / Sheets)**
- Gmail scope classes (official): **restricted** = `https://mail.google.com/`, `gmail.readonly`, `gmail.compose`, `gmail.insert`, `gmail.modify`, `gmail.metadata`, `gmail.settings.basic`, `gmail.settings.sharing`. **Sensitive** = `gmail.send` and add-on message scopes. **Non-sensitive** = `gmail.labels` and add-on action scopes — [Choose Gmail API scopes](https://developers.google.com/workspace/gmail/api/auth/scopes)
- Restricted-scope rules (page updated 2026-08-19): "Every app that requests access to Google users' restricted data and has the ability to access data from or through a third-party server must go through a security assessment from Google-empanelled security assessors" under CASA (App Defense Alliance). Apps must be reverified "at least every 12 months". Brand verification first takes "typically 2-3 business days". The restricted-scope process "can potentially take several weeks". A YouTube demo video of the OAuth flow and each scope's use is required — [Google: Restricted scope verification](https://developers.google.com/identity/protocols/oauth2/production-readiness/restricted-scope-verification)
- Official exceptions (no verification needed): **Personal use** ("you are the only user of your app or ... only a few users, all of whom are known personally to you", who click through the unverified-app screen); Development/Testing/Staging (publishing status "Testing", listed test users only); **Service-owned data only** (service account accessing only its own data); **Internal use only** (Google Workspace/Cloud Identity org-owned project with "Internal" user type). Domain-wide installation skips brand verification, but restricted or sensitive scopes still need app verification — [same](https://developers.google.com/identity/protocols/oauth2/production-readiness/restricted-scope-verification)
- "Testing" status: refresh tokens expire after 7 days; publishing to "In Production" fixes that — [Google Health API setup docs, via search](https://developers.google.com/health/setup). A 100-test-user cap is cited by a third-party blog — [tech.sailjada.com](https://tech.sailjada.com/posts/2026-05-06-1856.html)
- CASA cost: TAC Security FAQ lists Tier 2 Basic $540/app and Premium $720/app. Another TAC page lists Tier 2 Enterprise $1,800 and Tier 3 $3,600–$4,500/app (pages conflict) — [TAC Security CASA FAQ](https://tacsecurity.com/?p=18721); [TAC ESOF](https://tacsecurity.com/esof/); [switchlabs comparison (Nov 2024)](https://www.switchlabs.dev/post/casa-tier-2-tier-3-security-review-providers-pricing-and-the-cheapest-option). A developer reports "$540 and 1 month" to verify a Gmail AI SaaS (Tier 2). Vendor pages claim $15k–$75k on a "legacy track" — [Unipile 2026 guide (vendor)](https://www.unipile.com/integrating-google-oauth-2-0-user-authentication-into-your-app/); [indiehackers](https://www.indiehackers.com/product/mailnotes). Avoid requesting `gmail.modify` when only reading — [Nylas scopes guide](https://developer.nylas.com/docs/cookbook/use-cases/build/google-oauth-scopes/)
- Google Workspace MCP: I found **no first-party Google-published MCP server**. The leading community server is `taylorwilsdon/google_workspace_mcp` (3,336 stars, updated 13 Oct 2026), covering Gmail, Calendar, Docs, Sheets, Slides, Chat, Forms, Tasks, Drive, with OAuth 2.1 multi-user support. Others: `aaronsb/google-workspace-mcp` (191 stars) and a read-only `jacob-bd/google-workspace-mcp` — [GitHub taylorwilsdon](https://github.com/taylorwilsdon/google_workspace_mcp); [aaronsb](https://github.com/aaronsb/google-workspace-mcp); [jacob-bd](https://github.com/jacob-bd/google-workspace-mcp). One listed server wraps Google's official `@googleworkspace/cli` (gws) — [sharedcontext.ai listing](https://sharedcontext.ai/skills/external/agentskillexchange/google-workspace-mcp-server-for-gmail-calendar-and-drive)

**Microsoft 365 / Outlook (Graph API)**
- Multi-tenant Entra app registrations need publisher verification (requires a Microsoft Partner Network account). Without it the consent screen shows "Unverified", and cross-tenant mailbox access may be blocked unless the other tenant's admin consents — [Microsoft Q&A](https://learn.microsoft.com/en-my/answers/questions/1803621/how-to-read-emails-from-an-external-user-who-conse); [Unipile Graph guide (vendor)](https://www.unipile.com/microsoft-graph-api-email/). Sources conflict on whether delegated `Mail.Read` needs admin consent; one connector doc says it does not — [3sixty docs](https://helpdocs.objective.com/3sixty_tech/Content/connectors/microsoft/microsoft-graph-mail.htm)

**Lark / Larksuite**
- Official MCP: `larksuite/lark-openapi-mcp` ("飞书/Lark官方 OpenAPI MCP", 836 stars, Beta), npm `@larksuiteoapi/lark-mcp`, runs with App ID + App Secret, supports user-identity login, selectable tool sets, stdio/SSE. International Lark uses `open.larksuite.com`, separate from Feishu China — [GitHub larksuite/lark-openapi-mcp](https://github.com/larksuite/lark-openapi-mcp); [Glama listing](https://glama.ai/mcp/servers/larksuite/lark-openapi-mcp)
- Lark Starter plan $0/user/month (1,000 automations/month, 2,000 rows/table) — [Lark pricing](https://www.larksuite.com/pricing). Free edition API cap is 10,000 calls/month from 31 Oct 2024 per a third-party (Alibaba Quick BI) doc, **not verified on Lark's site** — [Alibaba Quick BI help](https://help.aliyun.com/en/quick-bi/user-guide/creat-an-online-spreadsheet-data-source)

**Base.vn**
- Base has a "Base Public API" and webhooks. Access tokens are created at account.base.vn, and "chỉ tài khoản Owner (quản trị cấp cao) mới có quyền thao tác" — [Base Public API](https://help.base.vn/support/solutions/articles/63000258324-base-public-api); [Base: Cách tạo Access Token](https://help.base.vn/support/solutions/articles/63000271544-api-c%C3%A1ch-t%E1%BA%A1o-access-token-%C4%91%E1%BB%83-t%C3%ADch-h%E1%BB%A3p-v%E1%BB%9Bi-b%C3%AAn-th%E1%BB%A9-3); [Base API/Webhook folder](https://help.base.vn/support/solutions/folders/63000232895)

**Notion**
- Official hosted MCP at `https://mcp.notion.com/mcp` (Streamable HTTP; SSE fallback). OAuth 2.0 authorization code + PKCE. The old self-hosted `notion-mcp-server` is "no longer actively maintained" — [Notion: build MCP client](https://developers.notion.com/guides/mcp/build-mcp-client); [Notion: get started with MCP](https://developers.notion.com/docs/get-started-with-mcp)

**Zalo groups (staff reports in Zalo)**
- Zalo provides no API for personal accounts and does not allow automation. The OpenClaw "zalouser" plugin uses the reverse-engineered `zca-js`, which "có thể vi phạm ToS, dẫn đến khóa tài khoản". A "Passive Collector" can log group messages to JSONL — [OpenClaw docs: zalouser](https://docs.openclaw.ai/vi/channels/zalouser); [ClawHub zaloclaw](https://clawhub.ai/monasprox/plugins/zaloclaw)
- Known `zca-js` issues include `getGroupChatHistory()` returning 404 and duplicate messages from listeners — [zca-js issues](https://github.com/RFS-ADRENO/zca-js/issues)

### Inferences
- **Replication strategy for Gmail and Calendar (most important decision):**
  1. **One central verified app** (best UX, scales to many clients): budget brand verification, restricted-scope review (weeks) and **CASA Tier 2 every year (about $540–$1,800, roughly 14–47 million đ)**, plus a privacy policy and home page on your own domain.
  2. **Per-client Google Cloud project** (no CASA): the owner, or you on their behalf, creates a project in their own Google account, sets it to "In Production" to avoid 7-day token expiry, and clicks through the unverified-app warning. This fits Google's "personal use" exception only when the users are few and known. It is a grey area for a commercial service sold to many clients, so get legal or Google confirmation before relying on it.
  3. **Clients on Google Workspace**: create an "Internal" OAuth app in the client's own Workspace org. That is exempt, but each client's admin must set it up.
  4. **Avoid Gmail where possible**: forward specific report emails to a dedicated inbox you control, or use Calendar and Sheets only (sensitive scopes, verification but no CASA).
- **Sheets for staff reports:** have the owner share the report sheet with a service-account email. The agent then reads it with no per-user OAuth and no consent screen, which copies trivially to every client. Whether Google's "service-owned data only" exception strictly covers user-shared files is my inference, not stated by Google.
- **Lark** is the cleanest "staff report" source for replication: an official MCP, an app ID and secret per tenant, and free-plan API quotas that should cover daily summaries, though the 10k/month cap is unverified.
- **Zalo groups:** the only route is the unofficial `zca-js` personal-account login, with ban risk. Treat Zalo-group reading as "not recommended, opt-in at the client's own risk". Better to ask staff to submit reports through a Google Form, Lark form or Base.vn, or to a Zalo OA/bot the business owns (covered in the channel research).

### Gaps
- Official Google statement on CASA pricing (fees are set by assessors) and whether "personal use" can apply to a reseller's per-client projects.
- Calendar scope sensitivity classification: not confirmed on the official page.
- Microsoft publisher-verification cost: not found. It is generally believed free with a Partner Network ID, but **unverified**.
- Base.vn API rate limits and which Base apps (Wework, Request, Office) expose report data: not verified.
- Lark official API quota per plan: only a third-party source.

---

## 6. Electronic invoices (hóa đơn điện tử) — relevance to revenue reporting

### Takeaway
E-invoice systems give another revenue signal, but for an AI assistant's daily revenue report the POS invoice or order API (e.g. KiotViet `GET /invoices`) plus bank transactions (SePay or Casso) are more direct. KiotViet and SePay now bundle cheap or free e-invoicing. I found no public API docs for MISA meInvoice or Viettel S-Invoice.

### Cited Findings
- KiotViet plans include free e-invoices and a free digital signature: 5,000 invoices/year (Hỗ trợ), 20,000/branch/year (Chuyên nghiệp), 50,000/branch/year (Cao cấp) — [KiotViet Phí dịch vụ](https://www.kiotviet.vn/phi-dich-vu)
- SePay eInvoice: 100–600đ/invoice depending on pack (e.g. 300 invoices for 180,000đ; 10,000 for 1,610,000đ), excluding 8% VAT. HSM digital signature 1,272,000đ/year — [SePay Bảng giá](https://sepay.vn/bang-gia.html)
- MISA meInvoice syncs report data and connects with sales software (CukCuk, KiotViet, Sapo). Public lookup by tax code is web-only. **No API docs found** — [meInvoice home](https://www.meinvoice.vn/meInvoice/html/home-page-new.html); [accgroup comparison](https://accgroup.vn/so-sanh-hoa-don-dien-tu-cua-misa-va-viettel)
- VietQR.io advertises a free automatic invoicing API, "Xinvoice" (banner text) — [vietqr.io plugin page](https://vietqr.io/cong-cu/cong-thanh-toan)

### Inferences
- For "daily revenue + anomaly alerts", build a nightly job: POS invoices (KiotViet/Haravan/Sapo) + bank inflows (SePay/Casso) → compare totals, flag gaps (e.g. cash sales not matching transfers, refunds or cancellations above a threshold, revenue more than X% below the 7-day average) → send a summary to the owner on Zalo or Telegram. Casso and SePay already push transaction alerts to Telegram and Google Chat groups ([Casso Flow](https://casso.vn/flow/bang-gia/); [SePay](https://sepay.vn/bang-gia.html)).

### Gaps
- MISA meInvoice, Viettel S-Invoice and VNPT invoice API availability and costs for third-party read access.

---

## 7. MCP ecosystem and generic connectors (n8n, Zapier, Make, Pipedream) for Vietnamese services

### Takeaway
As of October 2026, **official** MCP servers exist for the global tools (Notion hosted MCP, Lark official MCP). **No Vietnamese business service (SePay, Casso/payOS, KiotViet, Sapo, Haravan, Nhanh, GHN, GHTK) has an official MCP server** that I could find. There are many young community MCP servers, especially for KiotViet, plus Haravan, Sapo, Pancake POS and a VietQR/SePay "agentpay" kit. n8n has no official Vietnamese nodes, only a few community packages. Its generic Webhook and HTTP Request nodes work with all of these REST APIs.

### Cited Findings
- KiotViet community MCPs: at least 8 GitHub repos (2025–2026), e.g. `haudnn/kiotviet-mcp`, `dehuy69/kiotviet-mcp`, `blackmouse572/kiotviet-mcp`, `vansyson1308/kiotviet-mcp` (Sept 2026) — [GitHub haudnn](https://github.com/haudnn/kiotviet-mcp); [dehuy69](https://github.com/dehuy69/kiotviet-mcp); [blackmouse572](https://github.com/blackmouse572/kiotviet-mcp); [vansyson1308](https://github.com/vansyson1308/kiotviet-mcp)
- Haravan: `pluginmd/Haravan-MCP` (70 tools) — [GitHub](https://github.com/pluginmd/Haravan-MCP). Sapo: `nguyennguyenit/sapo-mcp` — [GitHub](https://github.com/nguyennguyenit/sapo-mcp). Pancake POS MCP — [Glama](https://glama.ai/mcp/servers/dhprk2nj9u)
- SePay/VietQR: `phuocdu/agentpay-vn`, "VietQR payment rail for AI agents — Python SDK + MCP server" (topics: sepay, vietqr; 1 star; Jun–Jul 2026) — [GitHub](https://github.com/phuocdu/agentpay-vn). ClaudeKit has an `/integrate:sepay` command, which is not an MCP — [docs.claudekit.cc](https://docs.claudekit.cc/docs/commands/integrate/sepay). SePay developer docs carry "Mở bằng AI" (open-with-AI) links on pages — [SePay Webhooks](https://developer.sepay.vn/vi/sepay-webhooks)
- No GitHub repo matched "payos mcp", "casso mcp", "ghn mcp", "ghtk mcp" or "nhanh mcp" (Vietnamese-service sense) in Oct 2026 searches — [GitHub search](https://github.com/search?q=payos+mcp&type=repositories)
- Official non-Vietnamese MCPs: Notion hosted — [Notion docs](https://developers.notion.com/guides/mcp/build-mcp-client). Lark/Feishu — [GitHub larksuite](https://github.com/larksuite/lark-openapi-mcp)
- n8n: community packages for KiotViet (npm, doivjpxx, v0.4.0) and Haravan (`n8n-nodes-nqdev`). None for SePay or GHN — [npm doivjpxx](https://www.npmjs.com/~doivjpxx); [ecosyste.ms nqdev](https://science.ecosyste.ms/projects/38399)

### Inferences
- "Ready-made connector" maturity for Vietnamese services is low (hobby-grade, single maintainers). For a product sold to many clients, the safer path is an **in-house multi-tenant tool server** (MCP or plain function-calling) with a small, audited tool set per integration, using community MCPs as reference code.
- Zapier, Make and Pipedream are unlikely to have native SePay, KiotViet or GHN apps. This was not checked, so it is a gap. All of them can receive the vendors' webhooks and call their REST APIs generically.

### Gaps
- Zapier, Make and Pipedream native app availability for SePay, Casso, payOS, KiotViet, Sapo, Haravan, Nhanh, GHN, GHTK: not checked.
- Whether SePay, Casso or KiotViet plan official MCP servers: no announcements found.

---

## 8. Practical architecture: how the agent should call these securely

### Takeaway
Treat every integration as **read-mostly, narrowly scoped and mediated by your own backend**. Banks and POS systems push webhooks into your database. The AI only gets small, purpose-built read tools (plus tightly validated "create draft order" or "hold reservation" writes). It **never** gets a money-movement capability (payouts, refunds, transfers) or raw admin tokens.

### Cited Findings
- Casso's bank connection is explicitly read-only: "Dữ liệu chỉ đọc, không có quyền chuyển tiền" — [Casso Flow](https://casso.vn/flow/bang-gia/). payOS sells a separate **payout ("Kênh chi")** product — [payOS](https://payos.vn/). That should never be wired to an agent.
- SePay webhooks can be redelivered (up to 8 attempts), so handlers must be idempotent, keyed on `id` or `referenceCode` — [SePay error handling](https://developer.sepay.vn/en/sepay-webhooks/xu-ly-loi). GHN also says "the same event can be delivered more than once" — [GHN webhook](https://developer.ghn.vn/en/docs/webhook/callback-order-status)
- KiotViet webhooks accept a `Secret` for verification — [KiotViet Public API](https://www.kiotviet.vn/huong-dan-su-dung-kiotviet/retail-ket-noi-api/public-api/). GHN callbacks can carry Basic auth or an API-key header — [GHN](https://developer.ghn.vn/en/docs/webhook/callback-order-status). Ahamove supports API key, bearer or basic on callbacks — [Ahamove](https://developers-stg.ahamove.com/en/docs/webhook)
- Least privilege: Haravan tokens are limited to the scopes chosen at creation (e.g. `com.read_orders`, `com.read_inventories`) — [Haravan access scopes](https://docs.haravan.com/docs/omni-apis/access-scopes/). The community `sapo-mcp` blocks destructive categories unless they are listed in `SAPO_ALLOW_OPS` and requires a bearer token for non-loopback HTTP — [sapo-mcp](https://github.com/nguyennguyenit/sapo-mcp)
- SePay lets merchants configure transaction retention time — [SePay Bảng giá FAQ](https://sepay.vn/bang-gia.html). Google requires apps to limit restricted data use to what the privacy policy discloses (Limited Use) — [Google restricted scope verification](https://developers.google.com/identity/protocols/oauth2/production-readiness/restricted-scope-verification)
- Unofficial bank-login libraries (credentials stored by a third party) are a security and compliance risk — [mbbank-lib](https://pypi.org/project/mbbank-lib/)

### Inferences
- **Reference flow, "đã nhận tiền chưa?":** customer asks on Zalo → agent calls `check_payment(order_code)` → backend queries the `bank_transactions` table filled by SePay/Casso webhooks → returns {paid: true/false, amount, time}. The agent never sees bank credentials or balances, only the matched transaction. Check that the amount is at least the order total and that the code matches exactly.
- **Prompt-injection risk:** the transfer content (`content`) and customer messages are attacker-controlled text. Never let them trigger tool calls, and never reflect other customers' data. Scope every lookup by order code plus phone number.
- **Writes:** `create_order` should create a draft or pending order in the POS (e.g. KiotViet `POST /orders`, not `POST /invoices`) that staff confirm, and should log every agent write.
- **Per-client secrets:** store each client's KiotViet ClientId/secret, Haravan/Sapo token, GHN/GHTK token and SePay OAuth tokens encrypted per tenant (envelope encryption, as `blackmouse572/kiotviet-mcp` advertises). Rotate keys and give each client a "disconnect" button.
- **Owner-assistant data (Gmail, Calendar):** keep it in a separate, owner-only agent context. Never expose it to the customer-facing chatbot.

### Gaps
- No Vietnamese regulatory guidance specific to AI agents accessing bank data was found. Circular 64's third-party contract requirement applies to banks and their direct partners (SePay, Casso), not to the downstream SaaS, but this was not confirmed by a legal source.
