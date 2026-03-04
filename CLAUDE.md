# react-toko-kelontong

A grocery store (toko kelontong) web app built with TanStack Start.

## Stack

- **Framework**: TanStack Start (SSR/SSG on top of TanStack Router)
- **Runtime**: Nitro (via `nitro-nightly`)
- **UI**: React 19 + Tailwind CSS v4
- **Routing**: TanStack Router (file-based, `src/routes/`)
- **Data fetching**: TanStack Query v5
- **Icons**: Lucide React
- **Package manager**: pnpm
- **Build tool**: Vite 7
- **Language**: TypeScript (strict mode)
- **Testing**: Vitest + Testing Library

## Path Aliases

- `#/*` → `./src/*` (Node.js subpath imports)
- `@/*` → `./src/*` (TypeScript paths)

## Project Structure

`routes/` is the pages directory — no separate `pages/` folder needed. Keep route files thin (loader + `createFileRoute`); extract large UI into `components/`.

```
src/
├── routes/
│   ├── __root.tsx                   # Root layout (Header, Footer, devtools)
│   ├── index.tsx                    # Home / featured products
│   ├── products/
│   │   ├── index.tsx                # Product listing + filters
│   │   └── $productId.tsx           # Product detail
│   ├── categories/
│   │   └── $categorySlug.tsx        # Products by category
│   ├── cart.tsx                     # Shopping cart
│   ├── checkout/
│   │   ├── index.tsx                # Checkout form
│   │   └── success.tsx              # Order confirmation
│   ├── orders/
│   │   ├── index.tsx                # Order history
│   │   └── $orderId.tsx             # Order detail
│   ├── auth/
│   │   ├── login.tsx
│   │   └── register.tsx
│   └── _admin/                      # Pathless layout (admin auth guard)
│       ├── admin.tsx                # Admin dashboard
│       ├── admin.products.tsx
│       └── admin.orders.tsx
│
├── components/
│   ├── ui/                          # Generic primitives (Button, Input, Badge…)
│   ├── product/
│   │   ├── ProductCard.tsx
│   │   ├── ProductGrid.tsx
│   │   └── ProductFilter.tsx
│   ├── cart/
│   │   ├── CartItem.tsx
│   │   └── CartDrawer.tsx
│   ├── layout/
│   │   ├── Header.tsx
│   │   ├── Footer.tsx
│   │   └── ThemeToggle.tsx
│   └── checkout/
│       └── CheckoutForm.tsx
│
├── server/                          # Server functions (createServerFn)
│   ├── products.ts
│   ├── cart.ts
│   ├── orders.ts
│   └── auth.ts
│
├── queries/                         # TanStack Query hooks (client-side)
│   ├── useProducts.ts
│   ├── useCart.ts
│   └── useOrders.ts
│
├── store/                           # Client state (cart, UI)
│   └── cart.ts
│
├── lib/
│   ├── utils.ts                     # cn(), formatCurrency(), etc.
│   ├── constants.ts                 # Categories, config values
│   └── validators.ts                # Zod schemas
│
├── types/
│   ├── product.ts
│   ├── order.ts
│   └── user.ts
│
├── integrations/
│   └── tanstack-query/              # QueryClient provider + devtools
│
├── styles.css                       # Global styles + Tailwind + CSS tokens
└── router.tsx
```

## Commands

```bash
pnpm dev          # Start dev server on port 3000
pnpm build        # Production build
pnpm preview      # Preview production build
pnpm test         # Run tests (vitest)
pnpm lint         # ESLint
pnpm format       # Prettier check
pnpm check        # Prettier write + ESLint fix
```

## Code Style

- **Prettier**: no semicolons, single quotes, trailing commas
- **ESLint**: `@tanstack/eslint-config` base; import order and cycle rules disabled
- **TypeScript**: strict mode, `noUnusedLocals`, `noUnusedParameters`
- Use `verbatimModuleSyntax` — always use `import type` for type-only imports

## Routing Conventions

- Add routes by creating files under `src/routes/`
- TanStack Router auto-generates route types — do not manually edit generated files
- Root layout lives in `src/routes/__root.tsx`
- Use `createFileRoute` for page routes, `createRootRouteWithContext` for root
- Router context carries `queryClient: QueryClient`

## Theme

- Dark/light/auto theme with `localStorage` persistence (`key: 'theme'`)
- Theme is initialized via an inline script in `<head>` to prevent flash
- Toggle component: `src/components/ThemeToggle.tsx`
- CSS custom properties defined in `src/styles.css`

## Server Functions & API Routes

- Use `createServerFn` from `@tanstack/react-start` for server-side logic
- API routes via `server.handlers` in route definitions (GET, POST, etc.)
- Data loading via route `loader` functions or TanStack Query

## Notes

- Files/routes prefixed with `demo` can be deleted (starter scaffolding)
- Sentry packages are externalized in the Nitro Rollup config
