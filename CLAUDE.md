# klarsprak — Claude Code Guide

Statisk prototyp-webbplats (Cloudflare Worker med assets-binding) som visar en
ordbok där juridiska/myndighetstermer översätts till vardagssvenska, med gapet
mellan vad allmänheten tror och vad termen faktiskt betyder. Komplement till
politiker.denied.se. Live på klarsprak.denied.se.

## Conventions

- All kod är en enda statisk HTML-fil (`public/index.html`, inline CSS+JS),
  ingen build-process.
- Worker-namn: `klarsprak`. Config i `wrangler.jsonc` (assets-binding).
- Deploy sker via `.github/workflows/deploy.yml` vid push till main.
- Innehållet i ordboken är AI-genererat och opublicerat — inte juridiskt
  sakgranskat. Ändra gärna presentation/kod, men flagga tydligt om
  sakinnehållet (termer/definitioner) ändras utan mänsklig juridisk granskning.
