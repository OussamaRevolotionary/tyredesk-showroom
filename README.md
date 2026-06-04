# TyreDesk — Retail Tyre Showroom

A white-label demo tyre finder for tire retail shops. Single self-contained `index.html` — no build step, CDN-only.

**Live demo:** https://oussamarevolotionary.github.io/tyredesk-showroom/

## Features
- **Popular Vehicles** — 4-step finder (Make → Model → Year → Trim) across 23 global makes / 250+ fitments.
- **Search by Size** — native cascading Width → Profile → Rim explorer over the full 95-SKU inventory (no external widget).
- **Enter Size** — direct tyre-size lookup.
- Rich tyre cards: EU performance labels (plain-language), decoded load (kg) and speed (km/h), XL marking, "best for / not ideal for" profiles.
- Contextual AI **Tyre Advisor** chatbot (n8n + OpenAI).
- Low-friction **Reserve** lead capture (Name + Phone) → n8n → Google Sheets + Gmail.

## Backend (n8n)
Two import-ready workflows live in this repo:
- `n8n-workflow-A-chatbot.json`
- `n8n-workflow-B-lead-capture.json`

See `N8N_SETUP.md` for setup. Webhook URLs are configured at the top of the `<script>` block in `index.html`.

---
© Oussama Labs — Product 1 · TyreDesk Retail Showroom
