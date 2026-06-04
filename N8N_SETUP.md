# TyreDesk — N8N Setup (Product 1)

Two import-ready workflows live in this folder. **You only set credentials — nothing else needs editing.**

- `n8n-workflow-A-chatbot.json` → the floating Tyre Advisor chatbot
- `n8n-workflow-B-lead-capture.json` → the "Reserve This Tyre" lead form

---

## 1. Import both workflows
In n8n: **Workflows → ⋮ (top-right) → Import from File** → pick each JSON file. Do this for both.

## 2. Attach credentials (the only manual step)

### Workflow A — Chatbot
- Open node **"OpenAI Chat (gpt-4o-mini)"** → Credential field → select (or create) your **OpenAI** credential (`openAiApi`). Paste your OpenAI API key once.
- Nothing else to change. Model `gpt-4o-mini`, temperature `0.45`, max tokens `400` are already set per spec.

### Workflow B — Lead Capture
- **"Google Sheets — Append"** node:
  - Select your **Google Sheets OAuth2** credential.
  - Set **Document** = your leads spreadsheet (replace `REPLACE_WITH_YOUR_GOOGLE_SHEET_ID`).
  - **Sheet** is set to `Sheet1` — change if your tab has a different name.
  - The sheet should have these header columns (row 1): `timestamp | name | phone | tyre | brand | vehicle | source`.
- **"Gmail — Notify"** node:
  - Select your **Gmail OAuth2** credential.
  - Set **To** = the email that should receive lead alerts (replace `REPLACE_WITH_YOUR_NOTIFY_EMAIL@gmail.com`).

## 3. Activate + copy the production URLs
- Toggle each workflow **Active**.
- Open each **Webhook** node and copy its **Production URL**:
  - Workflow A path: `…/webhook/tyredesk-chatbot`
  - Workflow B path: `…/webhook/tyredesk-lead`

## 4. Paste the URLs into the website
Open `tyredesk-showroom.html` (or the deployed `index.html`) and replace the two placeholders near the top of the `<script>` block:

```js
const N8N_CHATBOT_URL = "PLACEHOLDER_CHATBOT_WEBHOOK";   // ← Workflow A production URL
const N8N_LEAD_URL     = "PLACEHOLDER_LEAD_WEBHOOK";      // ← Workflow B production URL
```

## 5. Test
- **Chatbot:** open the site, search a vehicle, ask the advisor a question → you should get a reply.
- **Lead:** click **Reserve This Tyre**, submit Name + Phone → a new row appears in your Google Sheet and a Gmail notification arrives.

---

### Payloads (for reference)
**Chatbot** POSTs: `{ message, context, product: "retail-showroom" }` → expects `{ reply }`.
**Lead** POSTs: `{ name, phone, tyre_size, tyre_brand, vehicle, timestamp, source: "TyreDesk Demo" }` → expects `{ success, message }`.

> Note on OpenAI: Workflow A calls the OpenAI API via an HTTP Request node using the built-in `openAiApi` credential type (same pattern as the Engineering Suite workflow). This avoids version drift in the LangChain OpenAI node and keeps the import clean across n8n versions.
