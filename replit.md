# ENERGUM Store

French dietary supplement e-commerce store selling SOMMEIL sleep gummies and FOCUS energy gummies.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 8080, proxied at `/api`)
- `pnpm --filter @workspace/energum-store run dev` — run the frontend (port 22651, proxied at `/`)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string, `SESSION_SECRET`

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Frontend: React + Vite + Tailwind CSS (dark navy/blue/orange brand theme)
- API: Express 5
- DB: PostgreSQL + Drizzle ORM (`lib/db`)
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec in `lib/api-spec`)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/energum-store/` — React/Vite frontend
- `artifacts/api-server/` — Express 5 API server
- `lib/db/src/schema/index.ts` — DB schema (products, reviews, orders, coupons, promotions)
- `lib/api-spec/openapi.yaml` — OpenAPI spec (source of truth for API contracts)
- `lib/api-zod/src/generated/api/api.ts` — Generated Zod schemas + React Query hooks
- `lib/api-zod/src/index.ts` — Single export: `export * from "./generated/api/api"` (must stay this way after codegen)

## Architecture decisions

- Contract-first API: OpenAPI spec → Orval codegen → Zod schemas + React Query hooks
- Dark-only brand theme: deep navy background, electric blue primary, orange accent
- SOMMEIL (blue) and FOCUS (orange) color differentiation throughout the UI
- Coupon system validates server-side before order creation
- Promotions drive a live countdown timer in the banner and hero section

## Product

- **Homepage**: Hero with product image, live promo countdown, stats, featured products, testimonials, 30-day guarantee section
- **Products page**: Grid with search + category filters (SOMMEIL/FOCUS)
- **Product detail**: Images, benefits, reviews with star ratings, add-to-cart with quantity, leave-a-review form
- **Checkout**: Customer info form, coupon code validation, order summary
- **Order confirmation**: Full order details with guarantee reminder

## User preferences

- French language throughout the UI
- Dark deep-navy theme (same in light/dark modes — brand is always dark)

## Gotchas

- After running codegen, `lib/api-zod/src/index.ts` is overwritten to two lines. Restore it to `export * from "./generated/api/api"` (single export).
- API server routes under `/api` — all paths served from the shared proxy at `localhost:80`.
- Product images for seeded data use Unsplash; replace with real product photos as needed.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
