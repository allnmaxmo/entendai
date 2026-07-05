# Cloudflare Pages

STOP. Your knowledge of Cloudflare Pages, Pages Functions, Workers AI, and Wrangler may be outdated. Retrieve current Cloudflare documentation before changing Cloudflare runtime, bindings, limits, or deploy configuration.

## Docs

- https://developers.cloudflare.com/pages/
- https://developers.cloudflare.com/pages/functions/
- https://developers.cloudflare.com/workers-ai/
- MCP: `https://docs.mcp.cloudflare.com/mcp`

For all limits and quotas, retrieve the relevant product's `/platform/limits/` page.

## Commands

| Command | Purpose |
|---------|---------|
| `npm run dev` | Local Pages development |
| `npm run deploy` | Deploy to Cloudflare Pages |
| `npx wrangler types` | Generate TypeScript types |

Run `wrangler types` after changing bindings in `wrangler.jsonc`.

## Project Shape

- Static UI lives in `public/index.html`.
- Pages Functions live in `functions/`.
- The explanation API route is `functions/api/explicar.js`, available at `/api/explicar`.

## Errors

- Pages Functions errors: https://developers.cloudflare.com/pages/functions/debugging-and-logging/
- Workers AI errors: https://developers.cloudflare.com/workers-ai/platform/errors/
