# klarsprak — AI Agent Guide

Statisk prototyp-webbplats (Cloudflare Worker med assets-binding) som visar en
ordbok där juridiska/myndighetstermer översätts till vardagssvenska. Komplement
till politiker.denied.se. Live på klarsprak.denied.se.

## Conventions

- Frontend är statiska HTML-filer (`public/index.html`, `public/admin.html`,
  inline CSS+JS), ingen build-process.
- Backend är en riktig Worker (`src/worker.js`) som hanterar `/api/submit`,
  `/api/admin/queue`, `/api/admin/review/:id` och annars vidarebefordrar till
  assets-bindingen.
- Worker-namn: `klarsprak`. Config i `wrangler.jsonc` (assets-binding +
  D1-binding `DB` mot databasen `klarsprak-db`).
- Admin-endpoints skyddas i två lager: Cloudflare Access (Zero Trust-app
  "klarsprak admin (UI + API)", e-postpolicy) blockerar vid edgen, och
  Worker-koden kräver därutöver bearer-token mot secreten `ADMIN_TOKEN`.
- Hemligheter (nycklar, lösenfraser, tokens) lämnar aldrig enheten okrypterade

## Allowed
- Create branches
- Modify code
- Run tests
- Open PRs

## Forbidden
- Push directly to main/master
- Merge PRs
- Delete branches
- Disable workflows
- Modify secrets
- Change GitHub org settings

## Requirements
- All tests must pass
- Keep PRs focused
- Never include unrelated changes
- Never commit credentials
- Never force push
