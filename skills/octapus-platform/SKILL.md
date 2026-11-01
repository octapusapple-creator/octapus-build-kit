---
name: octapus-platform
description: Use when building any Octapus application: build in a standard integration layer so a central Octapus platform can later connect via a scoped API key to control the payment wall (on/off) and push one-time notifications, securely and with least privilege.
---

# Octapus Platform Layer: Build Every App Ready to Plug In

Octapus will run a central platform that connects to every app it has built, to do two things: turn each app's **payment wall** on or off, and **push notifications** to the app's users. So every app must be built from the start with a small, standard integration layer that this platform can connect to later with a single **scoped API key** — no rebuild, no code change, no full access to the app. Build this layer into every app even before the central platform exists.

Core security rule: the key Octapus uses to connect reaches **only** the payment-wall and notifications segments — never the app's database, users or anything else. Least privilege, always.

## 1. The scoped API key (the "door")
Each app issues a key that opens only two doors: billing and notifications.

- On first deploy, the app generates a **platform key** with a fixed, narrow scope: `billing:read`, `billing:write` (toggle the wall), `notifications:write` (send a notification). Nothing else.
- The key is shown **once** in the app's Settings > Platform screen and in the auto-generated API docs (section 5), then stored hashed; it is never shown in full again and never in the frontend bundle.
- The key maps to a `platform_integration` record, not to a user account, so it can never log in as a person or read user data.
- Every request with the key is checked server-side against its scope; a call to anything outside the two segments is rejected and logged.
- The key can be **rotated and revoked** from Settings without touching the rest of the app. Keep a "last used" timestamp and an access log.

## 2. Payment wall (toggle on and off)
Every app ships with a payment wall that is controlled by a setting and by the platform key.

- A setting `payment_wall = on | off` (default off until billing is set up). When on, the app gates whatever the app defines as paid (the whole app, or specific features).
- The platform can flip it: `POST /platform/billing/wall {"state":"on|off"}` using the scoped key. The app applies it immediately (e.g. unpaid users see a clean "Your subscription is inactive — contact Octapus" screen, never a broken app).
- `GET /platform/billing/status` returns the current wall state and plan, read-only, for the platform dashboard.
- Build the wall behind one gate (`isPaidActive()`), read from the setting, so turning it on or off is instant and never scattered through the code.
- Grace handling: on an outage or if the platform is unreachable, fail to the **last known state**, not to locked-out, so a network blip never shuts a paying customer out.

## 3. Notifications (one-time broadcast from Octapus)
Every app can receive a notification from the Octapus platform and show it to its users.

- `POST /platform/notifications {"title","body","audience":"all|role|segment","once":true}` using the scoped key.
- The app stores it, shows it in-app (a notifications bell or a banner), and optionally pushes it (email / WhatsApp / push) through whatever providers the app already has (see mastermind's provider layer).
- `once:true` means deliver a single time per user and never repeat; dedupe by a notification id so a retried call does not double-send.
- Respect each user's opt-out and quiet hours. Keep a delivery log (sent, seen) the platform can read read-only.

## 4. Secure connection (keep the boundary, stay committed to the client)
The app serves its client; the Octapus layer is a narrow, honest control channel, not a backdoor.

- **HTTPS only**; verify a **signature** on every platform request (HMAC with a shared secret, or signed webhook), so a stolen key alone is not enough and replays are rejected (timestamp + nonce).
- Scope enforced on the server for every call; deny by default.
- All platform actions (wall toggled, notification sent, key used) are written to the app's **audit log**, visible to the app owner. Nothing the platform does is hidden from the client who owns the app.
- Secrets (key hash, signing secret) live in the platform's secret store / env, never in code or git.
- The layer is **off and inert** until a key is issued and the client has agreed to platform billing — so an app with no Octapus billing simply runs normally.

## 5. Auto-generated docs expose only the two segments
When the app's API documentation is generated after a build, it must include a **Platform Integration** section and nothing beyond the two doors:

- The two endpoint groups only: Billing (wall on/off, status) and Notifications (send, status).
- How to authenticate with the scoped key and sign requests; how to rotate or revoke.
- A copy-paste example for each call.
- It must **not** document or expose any internal, user or data endpoint under this key. Internal APIs are documented separately and are not reachable with the platform key.

## 6. Standard shape to reuse in every app
Build it the same way each time so future apps connect in minutes:
- A `platform/` module with: key issue/verify, scope check, signature check, the billing endpoints, the notification endpoints, the audit hook.
- Settings > Platform screen: show integration status, the key (once), rotate/revoke, wall state, recent platform activity.
- The two settings: `payment_wall`, and the provider choices reused from mastermind for sending notifications.
- Keep it portable across the stacks Octapus uses (Base44, Lovable, Supabase, Antigravity, custom), as a small adapter per stack with the same interface.

## 7. Reply format
After building, state briefly:
1. **Platform key:** the scope it has (billing + notifications only) and where to find/rotate it
2. **Payment wall:** how to turn it on/off, locally and from the platform
3. **Notifications:** how a one-time broadcast is sent and shown
4. **Security:** signing, scope enforcement, audit log, revocation
5. **Docs:** that only the two segments are exposed
