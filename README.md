# Deploy Lasersite on Render

This repository is a deploy blueprint. It contains no source code — just the
recipe Render reads to build your install: a database, the app, and the one
secret that matters, generated for you.

You need two things: the licence details from your purchase email, and a Render
account.

## 1. Add your licence to Render, once

Your licence key is the password to the image registry. Render stores it in your
workspace, not in this file.

**Render → Workspace Settings → Registry Credentials → Add Credential**

| Field | Value |
| --- | --- |
| Name | `lasersite` — exactly this, the blueprint refers to it by name |
| Registry | `registry.lasersite.ai` |
| Username | the email you bought with |
| Password | your licence key |

## 2. Deploy the blueprint

**Render → New → Blueprint** → point it at this repository → **Apply**.

Render creates the database, generates `AUTH_SECRET`, pulls the image and starts
the app. Nothing to type.

## 3. Open your site

It lands on **`/setup`**, which asks for your business name, an optional
one-line description, your name, your email and a password. That creates your
admin account and your site in one step, and the page disappears afterwards.

Everything after that — payments, email, your own domain — is optional and done
from the admin.

---

## If the deploy fails

**`unauthorized` or `403` while pulling the image.** The registry credential is
missing, misnamed, or the licence is wrong. It must be named exactly
`lasersite`, and the username is the email you bought with — not your Render
login.

**The app starts, then the health check fails.** Open the service logs and search
for `[migrate]`. The database sets itself up on first boot; if that could not
finish, the line says why. `/setup` also names the reason rather than showing a
blank page.

**`connection timeout`, and the database looks healthy.** That is what an
out-of-memory Postgres looks like from the app side. This blueprint asks for the
1 GB plan for exactly that reason — if you changed it down, change it back.

## Updating later

Your install checks for new versions itself and shows them under
**Admin → Version**. Applying one is a decision you make, not something that
happens to you: the release is signed, and the screen tells you what changed
before you apply it.

The blueprint pins an exact version, so redeploying this repository will not
silently move you to a different one.

## Not using Render?

Any container host works — Fly.io, Railway, your own Docker host. The full
instructions ship with the software.
