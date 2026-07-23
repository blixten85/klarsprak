# klarsprak — AI Agent Guide

Statisk prototyp-webbplats (Cloudflare Worker med assets-binding) som visar en
ordbok där juridiska/myndighetstermer översätts till vardagssvenska. Komplement
till politiker.denied.se. Live på klarsprak.denied.se.

## Conventions

- All kod är en enda statisk HTML-fil (`public/index.html`), ingen
  build-process.
- Worker-namn: `klarsprak`. Config i `wrangler.jsonc`.
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
