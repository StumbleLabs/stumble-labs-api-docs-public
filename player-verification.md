# Player Verification API (Account Ownership Proof)

> **TL;DR** - If you are using `POST https://api.stumblelabs.net/api/live/users/search` to verify that a Discord
> user actually owns a Stumble Guys account (the classic "change your skin" flow),
> **you no longer need it**. Stumble Labs now ships dedicated verification routes
> built specifically for this. They are lighter, faster, and purpose-built.

---

## Why a dedicated flow?

A lot of integrations (Discord bots, account-linking pages, etc.) verify ownership
by asking the player to switch their in-game skin and then polling
`https://api.stumblelabs.net/api/live/users/search` to detect the change.

That works, but `https://api.stumblelabs.net/api/live/users/search` is a **full player lookup**: on every call it
also fetches action emotes, username history, creation date, inventory, XP
progression, and feeds several background pipelines. For a simple "did the skin
change?" check, that is a lot of overhead - and it can be **slow**.

**We do not recommend using `https://api.stumblelabs.net/api/live/users/search` for Discord/ownership
verification anymore.** Use the routes below instead - they only do the live
profile fetch needed to read the current skin.

---

## How verification works

The principle is the same one you already use: the player proves they control the
account by changing their skin **on demand**.

1. **Start** a verification challenge for a player. The API reads their current
   skin and gives you a **target skin** to ask them to equip. We toggle between the
   two default skins everyone owns:
   - If the player is currently on `SKIN1` (*Mr. Stumble*) → target is `SKIN2` (*Mrs. Stumble*)
   - Otherwise → target is `SKIN1` (*Mr. Stumble*)
2. **The player changes** their skin in-game to the target skin.
3. **You confirm** - the API re-reads the live skin and tells you whether it now
   matches the target.

There are two ways to consume it:

- **Stateful** (`/start` + `/status`): we generate and store a token for you. Best
  if you don't want to manage state yourself.
- **Stateless** (`/check`): you manage your own token/session (e.g. in your own
  database) and just ask us "does this player currently have skin X?".

> All routes require your API key in the `x-api-key` header.
> Base URL: `https://api.stumblelabs.net/api`

---

## Option A - Stateful flow (recommended for most bots)

### 1. Start a challenge

```
POST https://api.stumblelabs.net/api/live/users/verify/start
Header: x-api-key: <YOUR_API_KEY>
Body:   { "username": "PlayerName" }     // or { "userid": 193589871 }
```

Response:

```json
{
  "success": true,
  "message": "Verification started.",
  "data": {
    "token": "a1b2c3d4-5e6f-4a7b-8c9d-0e1f2a3b4c5d",
    "reused": false,
    "userId": 193589871,
    "userName": "PlayerName",
    "currentSkin": "SKIN1",
    "currentSkinName": "Mr. Stumble",
    "targetSkin": "SKIN2",
    "targetSkinName": "Mrs. Stumble",
    "targetSkinIcon": "https://cdn.stumblelabs.net/skins/skin2/preview.png",
    "instruction": "Change your skin to Mrs. Stumble and confirm.",
    "expiresAt": "2026-06-09T18:38:00.000Z"
  }
}
```

Show the player the `instruction` / `targetSkinName` and keep the `token`.
The challenge is valid until `expiresAt` (8 minutes).

**Idempotency:** `/start` is idempotent per player while a challenge is active.
If you call it again for the same player before the current challenge expires (or is
verified), you get the **same token and target skin** back, with `"reused": true`
in the response - no duplicate challenge is created. This keeps the target stable
even if you retry, and avoids redundant lookups.

### 2. Poll the status

After the player tells you they changed their skin, poll:

```
GET https://api.stumblelabs.net/api/live/users/verify/status/<token>
Header: x-api-key: <YOUR_API_KEY>
```

Still pending:

```json
{
  "success": true,
  "message": "Still pending.",
  "data": {
    "status": "pending",
    "userId": 193589871,
    "currentSkin": "SKIN1",
    "targetSkin": "SKIN2",
    "expiresAt": "2026-06-09T18:38:00.000Z"
  }
}
```

Verified:

```json
{
  "success": true,
  "message": "Verified.",
  "data": {
    "status": "verified",
    "userId": 193589871,
    "userName": "PlayerName",
    "currentSkin": "SKIN2",
    "targetSkin": "SKIN2",
    "expiresAt": "2026-06-09T18:38:00.000Z"
  }
}
```

Token not found / expired:

```json
{
  "success": false,
  "message": "Verification token not found or expired.",
  "data": { "status": "expired" },
  "errors": ["Token not found or expired"],
  "status": 404
}
```

**Polling tips:** poll every ~3–5 seconds. The status endpoint is throttled
server-side (it caches the live skin for a few seconds), so polling faster will
not give you fresher data - it just wastes requests. Stop polling once you get
`verified` or `expired`.

---

## Option B - Stateless check (manage your own token)

If your bot already stores its own verification tokens (with reCAPTCHA, Discord
session, expiry, etc.) and only needs the skin comparison, use this. It keeps no
server-side state - you pass the expected skin and we tell you if it matches right
now.

```
POST https://api.stumblelabs.net/api/live/users/verify/check
Header: x-api-key: <YOUR_API_KEY>
Body:   { "username": "PlayerName", "targetSkin": "SKIN2" }
```

Response:

```json
{
  "success": true,
  "message": "Skin matches.",
  "data": {
    "matches": true,
    "userId": 193589871,
    "userName": "PlayerName",
    "currentSkin": "SKIN2",
    "currentSkinName": "Mrs. Stumble",
    "targetSkin": "SKIN2",
    "targetSkinName": "Mrs. Stumble"
  }
}
```

When the skin does not match, you get `200` with `"matches": false`. A `200`
response means the request succeeded - check the `matches` field for the result.

---

## Notes & behavior

- **Always live.** Verification always reads the player's skin live (cache
  bypassed), so it reflects the real, current in-game skin.
- **Identifier.** All routes accept either `username` or `userid`/`userId`. If both
  are sent, the user id takes precedence.
- **Suggested target.** The `/start` route picks the target skin for you (the
  default-skin toggle). With `/check` you decide the expected skin yourself.
- **Errors.** Player not found returns `404`. Validation problems (missing
  identifier, malformed token, missing `targetSkin`) return `400`. A failed live
  lookup on `/status` returns `503` with `"status": "pending"` so you can retry.

---

## Migration summary

| Old way | New way |
| --- | --- |
| `POST https://api.stumblelabs.net/api/live/users/search?disableCache=true` + compare `skin` manually | `POST https://api.stumblelabs.net/api/live/users/verify/start` then `GET https://api.stumblelabs.net/api/live/users/verify/status/:token` |
| Manage your own token + compare skin via `/search` | `POST https://api.stumblelabs.net/api/live/users/verify/check` (you keep the token, we do the live skin check) |

Please migrate your ownership/Discord verification logic off `https://api.stumblelabs.net/api/live/users/search`.
Keep `https://api.stumblelabs.net/api/live/users/search` for what it is meant for - full player profiles - and use
the verification routes for ownership checks.
