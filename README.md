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

- Frontend är statiska HTML-filer (`public/index.html`, `public/admin.html`,
  inline CSS+JS) — ingen build-process.
- Deployas som en Cloudflare Worker (`src/worker.js`) med både en
  assets-binding (`public/`) och en D1-binding (`klarsprak-db`) i
  `wrangler.jsonc`.
- Auto-deploy till Cloudflare vid push till `main` via
  `.github/workflows/deploy.yml`.

## Inlämnings- och granskningsflöde

Vem som helst kan föreslå en term via knappen "Föreslå term" på startsidan.
Förslaget postas till `POST /api/submit` och sparas i D1-databasen
`klarsprak-db` (tabell `submissions`) med status `pending`. Inget publiceras
automatiskt.

`/api/submit` är IP-baserat rate-begränsat: max 5 inlägg per `CF-Connecting-IP`
per rullande timme (räknas mot `submissions.submitter_ip`, se
`migrations/0002_add_submitter_ip.sql`). Förfrågningar utan identifierbar IP
spärras inte (kan inte räknas).

En människa (kontoägaren) granskar kön på `/admin.html`:

- Sidan frågar efter admin-token (sparas endast i `sessionStorage`, aldrig
  i kod eller README).
- `GET /api/admin/queue` listar väntande förslag.
- `POST /api/admin/review/:id` med body `{"action": "approve"|"reject",
  "note"?: string}` uppdaterar status.

Admin-endpoints skyddas av en bearer-token-check mot secreten `ADMIN_TOKEN`:

```sh
openssl rand -hex 32 | bunx wrangler secret put ADMIN_TOKEN
```

Token delas manuellt med kontoägaren — spara den inte i git.

**Framtida uppgradering:** ersätt token-skyddet med Cloudflare Access på
`/admin.html` + `/api/admin/*`, samma mönster som politiker-webapp använder
för sin `/admin`-yta. Inte gjort ännu.

## Databas

D1-migrationer ligger i `migrations/`. Kör mot den riktiga databasen med:

```sh
bunx wrangler d1 migrations apply klarsprak-db --remote
```

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
Deploy-token behöver även behörighet att uppdatera D1-bindingen (D1:Edit)
utöver Workers Scripts:Edit.
