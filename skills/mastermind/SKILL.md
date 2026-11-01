---
name: mastermind
description: "Use for any build (app, e-commerce store, website, booking system, agent, automation, feature): turn every business rule into an editable setting with a default, put every outside service behind a swappable provider with fallback, handle the minor details, and find the easiest way."
---

# Mastermind: Build It Configurable, Swappable and Complete

The user's prompt gives specific values and specific services ("every 2 hours", "use OpenAI", "send by Twilio"). Build exactly those as the **defaults**. But a business changes its rules and its vendors constantly, so never lock them into code. The owner must be able to change them from a Settings screen, without a developer and without a rebuild.

## Works for any project (read first)
Everything in this skill is a **principle**, not a rule for one kind of app. Every example here, and every example the user gives (a fleet app, a driver every 2 hours, a WhatsApp agent, an accounting dashboard), is an **illustration**. Never limit yourself to the examples. Apply the same way of thinking to whatever is being built: a full application, an e-commerce store, a marketing website or landing page, a booking system, a marketplace, a SaaS or ERP module, a mobile app, a dashboard, an AI agent or chatbot, an automation or workflow, an integration, or a single feature or screen.

**Step 0, always:** identify the project type and scope, then ask yourself: who are the actors, what are their journeys, what data moves, who manages it, what can go wrong, and what would a top professional team in this field add that the prompt did not mention? Then apply the four parts below to that project. If none fits, reason it out the same way. Scale the depth to the job: a single screen gets a focused pass, a full product gets the full treatment.

## 1. Every business rule is a setting, never hard-coded
Whenever the prompt contains a number, time, limit, rule or choice, build it with the stated value as the **default**, and expose it in **Settings**.

Detect these in every prompt:
- Times and intervals: "every 2 hours", "after 30 minutes", "open 9 to 6", "reminder 1 day before"
- Quantities and limits: max bookings, max drivers per area, file size, retries
- Money: prices, fees, commission %, VAT 5%, discounts, minimum order, currency
- Areas and lists: zones, cities, categories, vehicle types, statuses, reasons for cancelling
- Rules: who approves, auto-assign or manual, cancellation window, OTP length or expiry
- Messages: notification texts, WhatsApp and email templates, AI agent instructions or persona
- Branding: logo, colors, company name, language

Examples across project types (illustrations only):
- Store: shipping fee, free-shipping threshold, delivery areas, COD on or off, low-stock alert level, return window, tax rate, coupon rules
- Website: CTA text, contact number, WhatsApp number, lead email recipients, banner or announcement, form fields shown
- Booking: slot length, buffer time, working hours, cancellation window, deposit amount, reminder timing
- AI agent: instructions, tone, opening message, handoff keywords, working hours, max messages per conversation

Worked example: "the driver picks up every 2 hours" becomes
- a setting `pickup_interval` with default **2 hours**, editable in Settings > Scheduling
- input with clear units (minutes or hours) and validation (min, max)
- optionally overridable per company, branch, zone or driver, if that makes sense
- the change takes effect for new schedules; tell the admin how existing ones are affected

**How to store it:**
- A `settings` table (key, value, type, scope: global, company, branch or user, updated_by, updated_at), or a typed config table
- Code reads settings through one helper, `getSetting('pickup_interval')`, with the default as fallback, never a literal `2` inside the logic
- Group settings in the UI by topic (General, Scheduling, Pricing, Notifications, Integrations, AI), with plain-language labels and helper text, defaults shown, and a "Reset to default"
- Settings changes are permission-protected (owner or admin only) and logged in the audit log

Do NOT turn everything into a setting. Keep it as a setting only if a business owner could reasonably want to change it. Technical constants stay in code.

## 2. Every outside service is a swappable provider
Never hard-wire a vendor into the business logic. Every external service goes behind a small **adapter** (provider interface), so a different provider can be connected from Settings.

Applies to:
- **AI / LLM**: OpenAI, Anthropic, Gemini, Groq, OpenRouter, Mistral, a local model
- **WhatsApp / messaging**: Meta WhatsApp Cloud API, Twilio, 360dialog, WATI, Gupshup
- **SMS / OTP**: Twilio, Vonage, MessageBird, local UAE or India gateways
- **Voice / speech**: speech-to-text and text-to-speech providers
- **Email**: Resend, SendGrid, SMTP
- **Payments**: Stripe, Telr, Network International, PayTabs, Razorpay
- **Maps / location**: Google Maps, Mapbox, OpenStreetMap
- **Storage**: Supabase storage, S3, Cloudinary

Any outside service the project uses follows this, including ones not listed (shipping or courier APIs, accounting sync, CRM, analytics, translation, image generation).

**Pattern:**
- One interface per capability, e.g. `ai.generate(messages, options)`, `messaging.send(to, template, vars)`
- One adapter file per provider that implements it
- Business logic calls the interface only, never a vendor SDK directly
- The active provider, model name, API key and options come from Settings > Integrations, never from code

**Settings > Integrations screen (owner or admin only):**
- Choose the provider from a dropdown; fields change based on the provider
- API key field: masked, write-only after saving (never shown back in full), stored encrypted or in secrets, never in the frontend bundle, never logged
- Model or sender selection, base URL (for OpenAI-compatible APIs and OpenRouter)
- "Test connection" button with a human result ("Connected", or "The key was rejected. Check it and try again.")
- Usage view: requests or tokens this month, estimated cost, optional monthly limit

**Fallback and running out of credits (critical for AI agents):**
- Allow a **primary** and a **backup** provider for AI and messaging
- If the primary fails because credits or tokens ran out, the key is invalid, it hits a rate limit or times out, automatically retry once, then switch to the backup
- If all providers fail: the agent sends a polite holding reply ("Thanks, our team will get back to you shortly."), creates a task for a human, and alerts the admin (email, WhatsApp or in-app)
- Admin alerts when usage reaches 80% of a limit and when a provider fails repeatedly
- Never show the end customer a technical error

Store example: switch the payment gateway or courier from Settings, keep the old one as backup. Agent example: a WhatsApp AI agent built on Provider A runs out of tokens. The admin opens Settings > Integrations > AI, picks OpenRouter or Gemini, pastes a key, clicks Test connection, saves. The agent continues on the new provider with the same instructions and conversation history. No code change, no redeploy.

## 3. The minor details a senior team never misses
Check these on every build and include whatever applies:
- **Time**: store in UTC, show in the company time zone (default Asia/Dubai or Asia/Kolkata); handle "every X hours" across midnight, weekends and holidays; working hours as a setting
- **Scheduling and recurring jobs**: what happens if a run is missed, overlaps or the server restarts; idempotent jobs (never send twice); a log of each run
- **Concurrency**: two users booking the same slot; two drivers accepting the same job; lock or check before confirming
- **Money**: use integer minor units or decimals, never floats; rounding rules; VAT; refunds and partial refunds
- **Phone and data formats**: E.164 phone numbers with country code (+971, +91); trim and normalize names and emails; duplicate detection
- **Notifications**: who gets notified on every status change; quiet hours; opt-out; WhatsApp 24-hour window and approved templates
- **Retries and failures**: network failure, payment failure, webhook received twice, provider down
- **Limits**: rate limiting, pagination, large files, very long text
- **Data lifecycle**: soft delete and restore, archive, export (CSV or Excel), backup
- **Edits after the fact**: what happens when an order, price or rule changes after creation
- **Permissions**: every action checked on the backend, not only hidden in the UI
- **Language**: English and Arabic or Malayalam where the audience needs it; RTL
- **Commerce**: stock never goes below zero, prices shown with tax rules applied consistently, order number sequence, invoice per order, partial shipments and partial refunds
- **Content**: editable without code, image sizes optimised, broken-link and empty-content handling
- **Observability**: error logging and an admin activity log, hidden from normal users

## 4. Always look for the easy way
Before building anything complex, ask: is there a simpler way that gives the same result?
- Prefer a setting over a code change, a template over a custom screen, a ready integration over custom code, one reusable component over many similar ones
- Reduce the owner's daily work: smart defaults, auto-fill, bulk actions, duplicate a record, import from CSV
- Remove steps that do not add value; automate repetitive actions with a clear on or off switch in Settings
- Mention the simpler alternative in the reply when it saves real time or money

## 5. Reply format
After building, include a short section:
1. **Made configurable**: each setting, its default and where it lives in Settings
2. **Swappable services**: each provider interface, the current default, the backup, and how to switch
3. **Details handled**: the minor cases covered
4. **Easier way**: any simplification you applied or suggest
