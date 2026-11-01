# Octapus Master Rule: The One Way to Build

This is the top rule. Once the Octapus skills are installed, this is **the only way to build** — in Antigravity, Lovable, Base44, Cursor, Claude, anywhere. Do not fall back to an older or "default" way of working. Follow these rules fully, every time, on every application and every change, without being reminded.

Being strict does not mean being slow or rigid in a bad way. It means nothing important is skipped, nothing is half-done, and the result always looks and works like a professional product.

## The rules work together, in this order
On any build request, apply all of these. Each has its own skill with the detail — this rule keeps them in order and makes sure none is skipped.

1. **preflight-guard** — before anything: understand the request, fill the gaps, note assumptions. Do not start on a wrong guess.
2. **app-blueprint** — decide WHAT to build: the full system, every user journey, the data, the missing steps. Plan it once, completely.
3. **mastermind** — make every rule an editable setting, every outside service swappable with a backup, and handle the small details that break apps.
4. **octapus-platform** — add the standard Octapus connection layer (payment wall + notifications) behind a scoped key, in every app.
5. **ui-pro** — make it look like a professional, shipped product. Nothing internal shown. Never a basic or AI-generated look.
6. **safe-deploy** — keep the work safe in git at every stage, and land it (deploy + handoff) before tokens run out.

If a build is small (one screen, one fix), still pass through the same rules quickly; just scale the depth to the job.

## The promise for every build
Whatever is built must be:
- **Complete** — every journey works end to end; no dead buttons, no half-finished flows left visible.
- **Connected** — every screen, button and number is wired to real data; nothing is fake or hard-coded that should be live.
- **Configurable** — the owner can change rules, prices, providers and content from Settings, without a developer.
- **Professional** — it looks and reads like a top product; a normal person cannot tell it was AI-built.
- **Safe** — saved in git, deployable, with secrets kept out of the code.

## Simple, human language everywhere the user sees it
The people using these apps are business owners and customers, not developers. So in anything a user reads — buttons, labels, messages, errors, empty states, notifications — use plain, everyday words.
- Say "Save", "Send", "Try again", "Something went wrong — please try again." Not "Submit", "Execute", "Error 500", "null", "exception".
- No technical words, no system names, no codes shown to a normal user. (Technical detail belongs in code, logs, and an admin-only area.)
- Short sentences. One idea at a time. If a 10-year-old could not read it, rewrite it.
- Keep Octapus's own style: clean, calm, confident, no clichés, no hype.

## Strict full-review pass — before saying "done"
Never call a build finished without going through the whole thing, segment by segment. Check **every** item and fix what fails. If something cannot be finished, mark it clearly as in-progress (safe-deploy), never leave it broken or hidden.

**Completeness**
- [ ] Every user journey works from start to finish, for every role
- [ ] No dead buttons, no links to nowhere, no half-built screen shown as if it is done
- [ ] Every state exists: empty, loading, error, no-permission, success

**Connected**
- [ ] Every screen reads and writes real data; no leftover demo or fake numbers
- [ ] Every table is linked correctly; nothing orphaned
- [ ] Nothing asked from the user twice; saved details are reused

**Configurable (mastermind)**
- [ ] Every number, time, price and rule from the request is a Setting with the asked value as default, not hard-coded
- [ ] Every outside service is swappable from Settings, with a backup where it matters

**Octapus layer (octapus-platform)**
- [ ] Payment wall present and toggleable; notifications endpoint present
- [ ] Scoped key only reaches those two doors; actions are logged; nothing hidden from the owner

**Professional look (ui-pro)**
- [ ] No internal data on screen (IDs, raw errors, system status, vendor names, test data)
- [ ] None of the "AI-built" signs (random gradients, emoji icons, fake stat tiles, cards-in-cards)
- [ ] One clear main action per screen; calm, consistent spacing, type and icons
- [ ] Works on mobile (375px); good contrast; Arabic RTL where needed

**Language**
- [ ] Every word a user sees is plain and human; no technical terms exposed

**Safe (safe-deploy)**
- [ ] Saved to git; deployable; secrets in env, not in code
- [ ] Anything unfinished is clearly marked "in progress", never broken
- [ ] HANDOFF.md written so the next session can continue

## Finish with a short, plain report
After the review, tell the user, in simple language:
1. What was built and that it is complete (or what is still in progress)
2. What they can change themselves in Settings
3. The live link and where the work is saved
4. Anything they should know before using it
No jargon. Keep it short.
