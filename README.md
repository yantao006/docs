# AdCue docs site (Mintlify)

User-facing documentation for AdCue, hosted by **Mintlify**.

This folder is **not** the internal engineering `docs/` tree (research, design references, deploy notes). Keep product docs here only.

## Local preview

```bash
# From repo root (or install mint globally once)
npm i -g mint

# Run the docs site from this directory
cd docs-site
mint dev
```

Open the URL printed by the CLI (usually `http://localhost:3000`).

If `mint` is not installed, see [Mintlify CLI](https://www.mintlify.com/docs/installation).

## Monorepo deploy (Mintlify GitHub App)

Docs live in a **subdirectory** of the adcue monorepo.

| Setting | Value |
|---------|--------|
| Repository | `yantao006/adcue` (or the production docs remote) |
| docs.json location | subdirectory mode **on** |
| Path | **`/docs-site`** (no trailing slash) |
| Deploy branch | `main` (default) |

Dashboard steps:

1. Install the [Mintlify GitHub App](https://www.mintlify.com/docs/deploy/github) on the adcue repo.
2. Open [Git Settings](https://app.mintlify.com/settings/deployment/git-settings).
3. Enable **docs.json is in a subdirectory**.
4. Set path to `/docs-site`.
5. Save — Mintlify redeploys from this folder.

Preview URL: `https://adcue.mintlify.app` (subdomain `adcue`). Production custom domain: **`https://docs.adcue.app`** (S3 / OAS-20).

DNS + remaining dashboard steps: **`docs/docs-s3-domain.md`**.

## Layout

```text
docs-site/
├── docs.json              # Nav, theme, logo (AdCue navy #070B24)
├── introduction.mdx
├── quickstart.mdx
├── create-account.mdx
├── connect/
│   ├── google-ads.mdx
│   └── mcp-setup.mdx
├── product/
│   └── usage-billing.mdx  # Outline until Creem UX freezes (Doc-D4)
├── mcp/
│   ├── overview.mdx
│   ├── tools.mdx          # Generated from tools.json
│   └── safety.mdx
├── knowledge/
│   ├── faq.mdx
│   ├── security.mdx
│   └── pricing.mdx        # Outline until plan matrix freezes
├── logo/                  # Brand SVG copies for Mintlify
├── images/                # Screenshots (later)
├── generated/
│   └── tools.json         # Registry export (S1)
└── README.md
```

Regenerate the catalog from a local `google-ads-mcp` checkout:

```bash
npm run docs:export-tools -- --mcp-root /path/to/google-ads-mcp
npm run docs:generate-tools-mdx
npm run docs:check-tools
```

See `scripts/docs-catalog/README.md`.

## Theme

- Primary / dark: AdCue navy `#070B24` (matches admin shell)
- Light accent: `#2F6F5E` (brand green from product favicon/mark family)
- Logos: copies of `public/brand/adcue/` SVGs under `logo/`

## Ship sequence (context)

| Ship | Scope |
|------|--------|
| **S0** | Scaffold only |
| **S1** | Catalog export → `generated/tools.json` + tools page + CI gate |
| **S2** | MVP EN narrative MDX (~12 pages per Mintlify scheme §5.1) |
| **S3** (this) | `docs.adcue.app` + Next `/docs` 301 hard-cut |

S3 is live in Next config: `adcue.app/docs*` permanently 301s to `https://docs.adcue.app`. Attach the Mintlify custom domain and Vercel DNS per `docs/docs-s3-domain.md`. Do not invent a second docs IA; keep authoring in this folder.

## Authoring notes

- Canonical product paths use **`/app/*`** (e.g. `/app/connections`), not bare `/connections`.
- Billing vendor is **Creem** (never Stripe in new docs).
- Tool names must match the V2 registry after S1 — never invent `meta_*` or legacy 109-tool count claims.
