---
name: safe-deploy
description: Use on any build done inside a token-limited AI tool (Antigravity, Lovable, Base44, Cursor): keep the work safe in git at every stable stage, and before tokens run out automatically commit, push, and deploy a hosted "under construction" version that can be edited later.
---

# Safe Deploy: Never Lose a Build to Running Out of Tokens

When building inside an AI tool, the session can run out of tokens or credits mid-build. The work must never be left only in the tool's memory. It must always be recoverable from git, and whatever is finished must be live on a URL, clearly marked as still in progress. Build so the next session can pick it up and keep editing.

## 1. Reserve tokens for landing the work (do this from the start)
Shipping is not free: committing, pushing and deploying cost tokens too. So keep a reserve and never spend the whole budget on building.

- Early in the build, estimate a **landing reserve**: roughly the tokens needed to git add + commit + push + trigger deploy + write a short handoff note. On most stacks this is small (a few hundred to a couple of thousand tokens); a first-time deploy with config and secrets is larger.
- Treat the budget as: **build until used ≈ (total − landing reserve), then stop and land.** Do not start a new large feature once near that line.
- If the tool shows remaining tokens or credits, watch them. If it does not, watch your own output: long files, many edits and big installs burn fast, so land earlier.

## 2. Save to git at every stable stage (not just at the end)
A "stable stage" is any point where the app builds and runs, even if incomplete: schema done, auth working, one screen finished.

- At each stable stage: `git add -A` and commit with a clear message of what works and what does not yet.
- Push to the remote (`git push`) at every stage, so nothing lives only in the tool. First session: `git init`, create the remote repo, set `origin`, push `main`.
- Commit messages say the state plainly: `feat: checkout works; shipping + refunds not done yet`.
- Use a branch per big feature if the tool allows; merge when stable. Never leave uncommitted work at the end of a session.

## 3. When tokens are about to run out, land automatically
As soon as the budget hits the landing reserve (or the tool warns it is low), stop building and do this, in order, without waiting to be told:

1. Make the app at least **runnable**: comment out or stub any half-written code so it compiles and starts. A running incomplete app beats a broken complete-looking one.
2. `git add -A`, commit (`wip: stopping on low tokens — <what is done> / <what is left>`), and push.
3. **Deploy / host** the current build to its target (Vercel, Netlify, Lovable or Base44 publish, the platform's deploy). Deployment should come from the git repo so it is repeatable.
4. On the live app, show the **under-construction signal** (section 4).
5. Write a **handoff note** (section 5) and push it.
6. Tell the user in one short message: the live URL, the repo, what works, what is left, and how to resume.

If tokens run out before deploy, the push in step 2 is the safety net: the work is in git and can be deployed from the next session or by hand.

## 3b. Separate "building" from "live" so customers never see a broken app
- Push day-to-day work to a **staging / preview** deploy (most platforms give every branch or commit a preview URL). Promote to the **production** URL only from a stable stage.
- A half-finished feature behind a flag (see mastermind, if used): default off in production, on in staging.

## 4. Under-construction signal on the app side
When a deploy is incomplete, the live app must say so, politely and professionally (follow ui-pro styling, not a loud banner):
- A small, dismissible top strip or a labelled badge: **"In progress — some features are still being built."** Neutral colours, not alarming red.
- For a feature that is not ready yet, the menu item or button is visible but shows a quiet **"Coming soon"** state rather than erroring or vanishing.
- Never show raw errors, stack traces or blank screens to a visitor. A missing page shows a calm "This part is still being built" with a link back.
- Control it with one setting, e.g. `build_status = live | in_progress` (and optional per-feature flags), so the strip disappears the moment the build is finished — no code change.
- Keep a real "under construction" holding page for a brand-new project that has no usable screens yet: logo, one line, and a contact, on-brand.

## 5. Build an editable, resumable structure (so the next session continues cleanly)
Whatever is shipped incomplete must be easy to pick up and edit later, by a new AI session or a developer:
- Clear folder structure, small focused files, meaningful names; no giant single file.
- Secrets in env vars / the platform's secret store, never in code, so deploy config carries over.
- A short **`HANDOFF.md`** at the repo root, updated every time you stop, containing:
  - Live URL(s): production and staging
  - What works, what is unfinished, and the next 3 steps to do
  - Where the unfinished code is (files, `TODO:` markers) and any stubs left in
  - How to run locally, env vars needed, how to deploy
  - Known issues and decisions made
- Leave `TODO:` comments at each unfinished spot so the next session can find them by search.
- A `README.md` with the stack, setup and deploy steps.

## 6. Reply format when landing
Give the user a short, plain summary:
1. **Live:** production and staging URLs (with the in-progress note if incomplete)
2. **Saved:** repo and the last commit
3. **Done / Not done:** two short lists
4. **Resume:** "Open HANDOFF.md and continue from the next-steps list" — so any future session carries on without losing context
