# klarsprak

Prototyp-webbplats: en ordbok som översätter juridiska och myndighetstermer
till vardagssvenska, och visar gapet mellan hur en vanlig medborgare rimligen
tolkar ett ord och hur domstolar/myndigheter faktiskt tillämpar det.

Byggd som komplement till [politiker.denied.se](https://politiker.denied.se) —
underlag för diskussionen om institutioners språkliga transparens gentemot
medborgare.

Live: **klarsprak.denied.se**

## Status

**Opublicerat utkast.** Innehållet är AI-genererat och väntar på granskning:
juridisk sakkontroll och avstämning av vardagsbetydelsen mot seriösa
uppslagsverk (SO/SAOL, NE, Wiktionary). Ingen av definitionerna ska
betraktas som auktoritativa ännu.

## Teknik

- En enda statisk HTML-fil (`public/index.html`, inline CSS+JS) — ingen
  build-process.
- Deployas som en Cloudflare Worker med assets-binding (`wrangler.jsonc`).
- Auto-deploy till Cloudflare vid push till `main` via
  `.github/workflows/deploy.yml`.

## Utveckling

```sh
bunx wrangler dev
```

## Deploy

```sh
bunx wrangler deploy
```

Kräver `CLOUDFLARE_API_TOKEN` och `CLOUDFLARE_ACCOUNT_ID` (secrets i
GitHub Actions för auto-deploy, eller lokalt i miljön för manuell deploy).
