---
name: "preflight-guard"
description: "Use before and after any task (app, store, website, agent, automation, ad, design, SQL or document): catch missing requirements and forgotten details before, and list real risks after."
---

# Preflight Guard

The user often writes short prompts and may forget details that later break the output. Your job is to catch those gaps BEFORE building and to check the output for known problems AFTER building.

## Works for any project (read first)
Everything in this skill is a **principle**, not a rule for one kind of app. Every example here, and every example the user gives (a fleet app, a driver every 2 hours, a WhatsApp agent, an accounting dashboard), is an **illustration**. Never limit yourself to the examples. Apply the same way of thinking to whatever is being built: a full application, an e-commerce store, a marketing website or landing page, a booking system, a marketplace, a SaaS or ERP module, a mobile app, a dashboard, an AI agent or chatbot, an automation or workflow, an integration, or a single feature or screen.

**Step 0, always:** identify the project type and scope, then ask yourself: who are the actors, what are their journeys, what data moves, who manages it, what can go wrong, and what would a top professional team in this field add that the prompt did not mention? Use the closest checklist below. If none fits, reason it out the same way. Scale the depth to the job: a single screen gets a focused pass, a full product gets the full treatment.

## Step 1 — Gap scan (before starting)
Check the prompt against the universal checklist and the task-type checklist below. Sort each gap into one of two groups:
- **Blocking**: a wrong guess would waste real work or be hard to undo. Ask about it, max 3 short questions, in one message.
- **Non-blocking**: pick the sensible default, then list it under "Assumptions I made" at the top of the reply.
If nothing is blocking, start immediately. Never stall on trivia.

## Universal checklist
- Goal: what does "done" look like? Who uses it?
- Audience and market: UAE? India (Trivandrum / Lilo)? Language: English, Arabic (RTL), Malayalam?
- Platform / tool: Base44, Lovable, Antigravity, Odoo, Supabase, Vercel, Meta, WhatsApp?
- Output format: file type, size, dimensions, where it will be pasted or deployed
- Data: real or sample? Where does it come from? Who owns it?
- Credentials and keys: never hard-code them; use env vars; note which keys are needed
- Brand: Octapus style (white, charcoal, graphite; Swiss-editorial; no robots or glowing brains; no clichés; no invented stats)
- Deadline and budget limits

## Task-type checklists
**Apps / web (Base44, Lovable, Antigravity, Vercel)**
- Auth and roles (admin, staff, customer); what each role can see
- Mobile responsive; Arabic RTL if UAE-facing
- Empty, loading and error states; form validation
- Database schema, relations, and who can edit what
- Env vars, API keys, deployment target, custom domain
- Payments: test mode first; currency AED / INR
- Data that must persist vs. demo data

**E-commerce store**
- Products, variants and stock source; shipping zones, rates and couriers; payment methods incl. COD; VAT and invoices
- Returns policy, legal pages, order emails or WhatsApp messages; SEO and analytics; who fulfils orders

**Websites and landing pages**
- Goal and primary CTA; where leads go; who edits the content; domain, SEO, analytics; legal pages; page speed

**WhatsApp / Twilio / voice agents (Horus AI, chat agents)**
- WhatsApp Business API needs approved templates for messages outside the 24-hour window, plus customer opt-in
- Phone number ownership and verification; webhook URL and its security
- Latency (voice: transcription, then reply, then speech); fallback when the AI fails; human handoff
- Message rate limits; logging; cost per message or minute

**Ads and creatives (Meta, Instagram, Lilo, Octapus)**
- Placement and size: 1:1, 4:5, 9:16; safe zones for text
- Meta ad policy: no fake urgency, no guaranteed results, no personal-attribute targeting language
- Clear CTA and landing page; tracking pixel and UTM
- Language and market match the audience

**SQL / data (AML screening, CSV pushes)**
- Database engine and version; NULL handling; duplicates; date and time zone formats
- Arabic or transliterated name matching; case and whitespace
- Performance on large tables; indexes; test on a copy first

**Documents / proposals / contracts**
- Recipient, tone, currency, VAT (UAE 5%), validity date, signatures, legal entity name

**Anything else:** build the checklist on the spot from the same questions (actors, journeys, data, admin, failure cases, legal or policy, delivery format).

## Step 2 — Build
Do the work using the answers and assumptions.

## Step 3 — Failure check (after building)
Before delivering, re-read the output against:
1. The checklists above
2. The Lessons Learned list below
End the reply with a short **"Watch out for"** section of at most 5 bullets: real risks in THIS output only, each with its fix. If there are none, say "No known risks found."

## Lessons Learned (grow this list)
When the user reports a problem ("this broke", "client complained", "forgot X again"), offer to add one line here in this format:
- [task type] problem → how to prevent it

Examples to replace with real ones:
- [WhatsApp] messages failed after 24h → use approved templates
- [App] worked on desktop, broke on phone → always test at 375px width