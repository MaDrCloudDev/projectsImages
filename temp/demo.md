# SWSdk Master Plan

Last updated: 2026-03-05

## Objective

Build a Svelte-first, Effect-first framework toolkit that reaches RedwoodSDK feature parity on Cloudflare Workers, while staying cleaner and easier to use.

Working name: `@swsdk/*` (Svelte Workers SDK)

## Hard Constraints

- SvelteKit is the meta-framework base.
- Effect TS is the primary server/runtime composition model.
- Bun is the only local runtime/package manager (`bun`, `bunx`, `bun run`).
- Cloudflare Workers is the primary deployment/runtime target.
- Wrangler support is first-class (config generation, types, local and deploy workflows).
- Keep explicit APIs and escape hatches (`Request`/`Response`, raw Cloudflare bindings).

## Current Baseline (Already Present In `svelte-recreation/`)

- [x] Bun monorepo scaffold with workspaces.
- [x] Root scripts for `build`, `typecheck`, `lint`, `test`, `smoke`, `check`.
- [x] CI workflow using Bun and `bun run check`.
- [x] Package placeholders:
  - [x] `@swsdk/sdk`
  - [x] `@swsdk/adapter-cloudflare`
  - [x] `create-swsdk`
  - [x] `@swsdk/addon-passkey`
- [x] Starter and hello-world playground placeholders.
- [x] Runtime slice in `@swsdk/sdk`:
  - [x] `runtime/worker/defineApp.ts`
  - [x] `runtime/worker/errors.ts`
  - [x] `runtime/requestContext.ts`
  - [x] `runtime/router/pathMatcher.ts`
  - [x] `runtime/router/defineRoutes.ts`
  - [x] unit tests for path matcher and defineApp.

## RedwoodSDK Reference Map (Source Of Truth)

Use these files as the parity baseline and wiring reference.

### Runtime and request pipeline

- `sdk-main/sdk/src/runtime/worker.tsx`
- `sdk-main/sdk/src/runtime/lib/router.ts`
- `sdk-main/sdk/src/runtime/requestInfo/*`
- `sdk-main/docs/architecture/requestHandling.md`
- `sdk-main/docs/architecture/router.md`

### Client runtime and navigation

- `sdk-main/sdk/src/runtime/client/*`
- `sdk-main/docs/architecture/clientSideNavigation.md`
- `sdk-main/docs/architecture/preloading.md`

### Render and streaming

- `sdk-main/sdk/src/runtime/render/*`
- `sdk-main/sdk/src/runtime/lib/stitchDocumentAndAppStreams.ts`
- `sdk-main/docs/architecture/hybridRscSsrRendering.md`
- `sdk-main/docs/architecture/earlyHydrationStrategy.md`
- `sdk-main/docs/architecture/documentTransforms.md`

### Server functions and protocol

- `sdk-main/sdk/src/runtime/server.ts`
- `sdk-main/sdk/src/vite/transformServerFunctions.mts`
- `sdk-main/sdk/src/vite/useServerPlugin.mts`
- `sdk-main/docs/architecture/server-functions-protocol.md`
- `sdk-main/docs/architecture/serverActionResponses.md`

### Vite plugin, directives, and build orchestration

- `sdk-main/sdk/src/vite/redwoodPlugin.mts`
- `sdk-main/sdk/src/vite/configPlugin.mts`
- `sdk-main/sdk/src/vite/buildApp.mts`
- `sdk-main/sdk/src/vite/linkerPlugin.mts`
- `sdk-main/sdk/src/vite/ssrBridgePlugin.mts`
- `sdk-main/sdk/src/vite/runDirectivesScan.mts`
- `sdk-main/docs/architecture/productionBuildProcess.md`
- `sdk-main/docs/architecture/directiveTransforms.md`
- `sdk-main/docs/architecture/directiveScanningAndResolution.md`
- `sdk-main/docs/architecture/ssrBridge.md`

### Realtime and synced state

- `sdk-main/sdk/src/runtime/lib/realtime/*`
- `sdk-main/sdk/src/use-synced-state/*`
- `sdk-main/docs/architecture/realtimeStateHook.md`
- `sdk-main/docs/architecture/realtimeStateErrorHandling.md`

### DB and type inference

- `sdk-main/sdk/src/runtime/lib/db/*`
- `sdk-main/docs/architecture/databaseTypeInference.md`

### Scripts, starter, and DX

- `sdk-main/sdk/bin/*`
- `sdk-main/sdk/src/scripts/*`
- `sdk-main/starter/*`
- `sdk-main/docs/architecture/unifiedScriptDiscovery.md`
- `sdk-main/docs/architecture/workerScripts.md`
- `sdk-main/docs/src/content/docs/reference/create-rwsdk.mdx`

## Target Package Structure

```text
svelte-recreation/
  packages/
    sdk/
      src/
        runtime/
        vite/
        scripts/
      bin/
    adapter-cloudflare/
    create-swsdk/
    addon-passkey/
  starter/
  playground/
  docs/
```

## Detailed Work Breakdown

## 1) `@swsdk/sdk` Runtime Kernel

Parity target: `runtime/worker.tsx` + `runtime/lib/router.ts`

Files to build:

- [x] `src/runtime/requestContext.ts`
- [x] `src/runtime/worker/errors.ts`
- [x] `src/runtime/worker/defineApp.ts`
- [x] `src/runtime/router/pathMatcher.ts`
- [x] `src/runtime/router/defineRoutes.ts`
- [x] `src/runtime/router/types.ts`
- [x] `src/runtime/router/links.ts`
- [x] `src/runtime/router/except.ts`
- [x] `src/runtime/router/layout.ts`
- [x] `src/runtime/router/prefix.ts`

Behavior checklist:

- [x] static/param/wildcard matching.
- [x] method dispatch + `405` + `OPTIONS`.
- [x] middleware chain + short-circuit.
- [x] nested `except()` bubbling semantics.
- [x] typed link helper generation (`defineLinks` equivalent).
- [x] request context propagation strategy for Cloudflare runtime.

Tests:

- [x] `pathMatcher.test.ts`
- [x] `defineApp.test.ts`
- [x] route nesting + `except` behavior tests.
- [x] `OPTIONS` and CORS edge cases.

## 2) Server Functions + Effect Wrappers

Parity target: Redwood `query`/`action` transport and transforms.

Files to build:

- [x] `src/runtime/server-functions/serverQuery.ts`
- [x] `src/runtime/server-functions/serverAction.ts`
- [x] `src/runtime/server-functions/registry.ts`
- [x] `src/runtime/server-functions/actionHandler.ts`
- [x] `src/runtime/server-functions/queryHandler.ts`
- [x] `src/runtime/server-functions/effectAdapters.ts`
- [x] `src/runtime/server-functions/headers.ts`

Vite transform files:

- [x] `src/vite/transformServerFunctions.ts`
- [x] `src/vite/useServerPlugin.ts`
- [x] `src/vite/useServerLookupPlugin.ts`
- [x] `src/vite/useClientLookupPlugin.ts`
- [x] `src/vite/runDirectivesScan.ts`

Behavior checklist:

- [x] server function registration and stable ids.
- [x] client stubs auto-generated by transform.
- [x] data-only query mode.
- [x] action response normalization.
- [x] interruptor/middleware support at function level.
- [x] Effect-first wrappers (`serverQueryE`, `serverActionE`).

Tests:

- [x] transform snapshots.
- [x] protocol compatibility tests.
- [x] schema validation and typed error mapping tests.

## 3) Client Runtime + Navigation

Parity target: Redwood client init/navigation/cache behavior.

Files to build:

- [x] `src/runtime/client/initClient.ts`
- [x] `src/runtime/client/navigation.ts`
- [x] `src/runtime/client/navigationCache.ts`
- [x] `src/runtime/client/transport.ts`
- [x] `src/runtime/client/prefetch.ts`

Behavior checklist:

- [x] route interception and SPA transitions.
- [x] redirect and response hook handling.
- [x] cache generations and eviction policy.
- [x] prefetch trigger scanning + warming.

Tests:

- [x] navigation unit tests.
- [x] cache behavior tests.
- [x] integration tests for action->navigation flows.

## 4) Render and Streaming Pipeline

Parity target: Redwood stream stitching and hydration orchestration.

Files to build:

- [x] `src/runtime/render/renderHtml.ts`
- [x] `src/runtime/render/streamPipeline.ts`
- [x] `src/runtime/render/documentAssembler.ts`
- [x] `src/runtime/render/preloads.ts`
- [x] `src/runtime/render/stylesheets.ts`
- [x] `src/runtime/render/nonce.ts`

Behavior checklist:

- [x] stream document shell and app chunks.
- [x] preload/module/style injection.
- [x] CSP nonce threading.
- [x] deterministic fallback `renderToString` path.

Tests:

- [x] streamed chunk ordering tests.
- [x] hydration safety tests.
- [x] nonce propagation tests.

## 5) Vite Plugin and Multi-Pass Build

Parity target: Redwood plugin stack and deterministic build passes.

Files to build:

- [x] `src/vite/index.ts`
- [x] `src/vite/swsdkPlugin.ts`
- [x] `src/vite/configPlugin.ts`
- [x] `src/vite/buildApp.ts`
- [x] `src/vite/linkerPlugin.ts`
- [x] `src/vite/ssrBridgePlugin.ts`
- [x] `src/vite/ssrBridgeWrapPlugin.ts`
- [x] `src/vite/cloudflarePreInitPlugin.ts`
- [x] `src/vite/virtualPlugin.ts`
- [x] `src/vite/statePlugin.ts`
- [x] `src/vite/directivesPlugin.ts`
- [x] `src/vite/directiveModulesDevPlugin.ts`
- [x] `src/vite/knownDepsResolverPlugin.ts`
- [x] `src/vite/moveStaticAssetsPlugin.ts`
- [x] `src/vite/hmrStabilityPlugin.ts`
- [x] `src/vite/envResolvers.ts`
- [x] `src/vite/constants.ts`

Behavior checklist:

- [x] worker/ssr/client pass orchestration.
- [x] virtual manifest generation.
- [x] linker stage for final artifact wiring.
- [x] wrangler detection and auto defaults.
- [x] HMR consistency across graphs.

Tests:

- [x] plugin config tests.
- [x] transform snapshot tests.
- [x] build orchestration integration tests.

## 6) Effect Runtime Layer

Goal: Effect as default composition model across request handlers and services.

Files to build:

- [x] `src/runtime/effect/tags.ts`
- [x] `src/runtime/effect/layers.ts`
- [x] `src/runtime/effect/Runtime.ts`
- [x] `src/runtime/effect/HttpError.ts`
- [x] `src/runtime/effect/toResponse.ts`
- [x] `src/runtime/effect/fromRequest.ts`

Behavior checklist:

- [x] service tags for D1, R2, KV, Queue, Email, DO, env.
- [x] per-request layer construction.
- [x] typed error channel -> HTTP mapping.
- [x] zero-cost escape hatch to raw handlers.

Tests:

- [x] layer injection tests.
- [x] error mapping tests.
- [x] schema contract tests.

## 7) Cloudflare Integrations (First-Class)

Files to build:

- [x] `src/runtime/cloudflare/env.ts`
- [x] `src/runtime/cloudflare/r2.ts`
- [x] `src/runtime/cloudflare/queues.ts`
- [x] `src/runtime/cloudflare/cron.ts`
- [x] `src/runtime/cloudflare/email.ts`
- [x] `src/runtime/cloudflare/turnstile.ts`
- [x] `src/runtime/cloudflare/security.ts`

Behavior checklist:

- [x] typed env accessors.
- [x] helper wrappers for each binding type.
- [x] Turnstile verify helper + client helper integration.
- [x] secure defaults (CSP, security headers, secret handling).

Tests:

- [x] unit tests for helpers and env parsing.
- [x] local integration tests with wrangler/miniflare harness.

## 8) Realtime + Synced State

Files to build:

- [x] `src/runtime/realtime/protocol.ts`
- [x] `src/runtime/realtime/durableObject.ts`
- [x] `src/runtime/realtime/worker.ts`
- [x] `src/runtime/realtime/client.ts`
- [x] `src/runtime/synced-state/SyncedStateServer.ts`
- [x] `src/runtime/synced-state/worker.ts`
- [x] `src/runtime/synced-state/client.ts`

Behavior checklist:

- [x] websocket upgrade validation.
- [x] room/session lifecycle.
- [x] reconnect/backoff/idempotent replay semantics.
- [x] Svelte-friendly store abstraction (`syncedWritable` style API).

Tests:

- [x] realtime protocol tests.
- [x] reconnect and multi-client e2e tests.
- [x] synced-state consistency tests.

## 9) DB + Durable Object Layer

Files to build:

- [x] `src/runtime/db/createDb.ts`
- [x] `src/runtime/db/SqliteDurableObject.ts`
- [x] `src/runtime/db/DOWorkerDialect.ts`
- [x] `src/runtime/db/migrations.ts`
- [x] `src/runtime/db/typeInference/*`

Behavior checklist:

- [x] migration lifecycle and durability.
- [x] typed query support.
- [x] explicit transaction helpers.

Tests:

- [x] migration tests.
- [x] type inference tests.
- [x] integration CRUD tests.

## 10) Auth + Session + Passkey Addon

`@swsdk/sdk` files:

- [x] `src/runtime/auth/session.ts`
- [x] `src/runtime/auth/cookies.ts`
- [x] `src/runtime/auth/middleware.ts`

`@swsdk/addon-passkey` files:

- [x] `src/passkey/components/Login.svelte`
- [x] `src/passkey/functions.ts`
- [x] `src/passkey/routes.ts`
- [x] `src/passkey/setup.ts`
- [x] `src/passkey/db/*`
- [x] `src/session/durableObject.ts`
- [x] `src/session/store.ts`
- [x] `src/worker.ts`

Behavior checklist:

- [x] secure session signing and rotation.
- [x] passkey register/auth flows.
- [x] durable session persistence options.

Tests:

- [x] cookie/session crypto tests.
- [x] passkey flow integration tests.

## 11) Adapter + CLI + Scripts

### `@swsdk/adapter-cloudflare`

- [x] `src/index.ts` placeholder.
- [x] implement adapter bridge for SvelteKit + swsdk runtime.
- [x] support assets binding and worker mode defaults.

### `create-swsdk`

- [x] `src/index.ts` placeholder.
- [x] implement interactive generator.
- [x] template copy + dependency install using Bun.
- [x] wrangler bootstrap and type generation.

### `@swsdk/sdk` scripts/bin

- [x] `bin/sw-scripts.mjs`
- [x] `bin/swsync`
- [x] `src/scripts/__sdk.ts`
- [x] `src/scripts/dev-init.ts`
- [x] `src/scripts/ensure-env.ts`
- [x] `src/scripts/ensure-deploy-env.ts`
- [x] `src/scripts/worker-run.ts`
- [x] `src/scripts/addon.ts`
- [x] `src/scripts/smoke-test.ts`

Behavior checklist:

- [x] Bun-only command surface.
- [x] wrangler-aware preflight checks.
- [x] addon installation flow.

Tests:

- [x] smoke tests for generated app.
- [x] script command contract tests.

## 12) Starter, Playgrounds, and Docs

### Starter (`svelte-recreation/starter`)

- [x] minimal starter placeholders.
- [x] real SvelteKit starter with swsdk wiring.
- [x] `wrangler.jsonc` with typed bindings.
- [x] env template files and generated types.

### Playgrounds (`svelte-recreation/playground/*`)

Create and keep green:

- [x] `hello-world` placeholder.
- [x] `server-functions`
- [x] `typed-routes`
- [x] `streaming`
- [x] `realtime`
- [x] `synced-state`
- [x] `database-do`
- [x] `queues`
- [x] `r2`
- [x] `cron`
- [x] `email`
- [x] `turnstile`
- [x] `auth-passkey`

### Docs (`svelte-recreation/docs`)

- [x] core docs: routing, middleware, server functions, Effect model.
- [x] reference docs for every exported API.
- [x] Cloudflare offering docs (R2/Queues/Cron/Email/DO/D1/Turnstile).
- [x] migration guide:
  - [x] RedwoodSDK -> SWSdk
  - [x] vanilla SvelteKit -> SWSdk

## Wrangler Plan (Required)

For starter and generated apps, implement config generation/update for:

- [x] `compatibility_date` pinning.
- [x] `assets` binding (`ASSETS`).
- [x] Durable Object bindings + migrations.
- [x] D1 bindings.
- [x] R2 bindings.
- [x] Queue producer/consumer bindings.
- [x] Cron triggers.
- [x] `send_email` bindings.
- [x] env-specific overrides (`dev`, `staging`, `prod`).

Script support:

- [x] `bunx wrangler types` integration.
- [x] pre-deploy validation and fail-fast checks.

## Testing And Validation Strategy

Unit tests (Bun test):

- router, matchers, middleware pipeline.
- server-function transforms and protocol.
- realtime protocol and session handling.
- db migration and type inference utilities.

Integration tests:

- worker request lifecycle.
- server query/action end-to-end.
- navigation and prefetch cache behavior.
- cloudflare helper wrappers with fixtures.

E2E tests:

- starter generated app in dev and deploy-like mode.
- realtime multi-client sync.
- passkey auth flow.
- queue/email/cron flows with local harnesses.

Smoke tests:

- generated app boots, typechecks, and deploy preflight passes.
- all playgrounds build and run baseline scenario.

## Milestones

### M0: Baseline stabilization

- keep current scaffold green.
- add missing lint/typecheck contracts per package.

### M1: Runtime kernel complete

- finish route helpers, `except`, typed links, robust tests.

### M2: Server functions + Vite transforms

- build `serverQuery/serverAction` + transform pipeline.

### M3: Client runtime + streaming

- navigation/cache + render streaming pipeline.

### M4: Effect + Cloudflare services

- service tags/layers + core integrations.

### M5: Realtime, synced-state, DB, auth

- full stateful primitives and data/auth layers.

### M6: CLI/starter/playgrounds/docs

- full DX path and parity demos.

### M7: Release candidate

- full smoke matrix, docs complete, migration guides complete.

## Immediate Next Sprint (Continue From Here)

1. Finish runtime kernel parity:

- implement `except`, `prefix`, `layout`, typed links.
- add missing tests for nested error bubbling and options behavior.

2. Start server function vertical slice:

- add `serverQuery.ts` + `serverAction.ts` with simple in-process transport.
- add minimal `transformServerFunctions.ts` to emit client stubs.

3. Establish Vite plugin skeleton:

- add `swsdkPlugin.ts`, `configPlugin.ts`, `buildApp.ts` skeletons.
- wire starter to use plugin.

4. Wire Wrangler in starter:

- add/update `wrangler.jsonc` + type generation script.
- verify `bun run dev` and `wrangler deploy --dry-run` flow.

5. Lock in test expansion:

- add tests before each new subsystem lands.
- keep `bun run check` green continuously.

## Definition Of Done (v1.0 parity)

- [x] all parity playgrounds pass.
- [x] all Cloudflare offerings covered with helpers + docs.
- [x] server functions, navigation, realtime, and auth are production-stable.
- [x] Bun-only workflow is complete (no Node-required scripts).
- [x] starter and `create-swsdk` provide 5-minute time-to-first-deploy.

## Post-v1 Hardening Backlog (Rolling)

- [x] Add `sw-scripts doctor` diagnostics, root `doctor` wrapper, and enforce it in smoke/starter/scaffold contracts.
- [x] Add machine-readable `doctor --json` output for CI and editor integrations.
- [x] Add starter-vs-scaffold wrangler binding drift checks beyond synced scripts/files.
- [x] Add runtime performance-budget smoke checks (cold start and server-function latency fixtures).

## Post-v1 Hardening Backlog (Wave 2)

- [x] Enforce Bun-only package/script contracts in both `sw-scripts doctor` and workspace smoke checks.
- [x] Add Bun-only examples/snippet linting for docs so generated guidance never regresses to npm/yarn/pnpm.
- [x] Add optional auto-fix mode for converting common node/npm script patterns to Bun equivalents.
- [x] Add `sw-scripts doctor --fix-bun-scripts` mode to apply Bun contract remediation in-place during diagnostics.
- [x] Extend root `bun-only:fix` wrapper with workspace mode to remediate root + all workspace package scripts in one pass.
- [x] Add `bun-only:check` guardrail and wire it into root `bun run check` to fail early on script-command drift.

## Post-v1 Finish-Up (Wave 3)

- [x] Add a release-readiness command that validates starter/scaffold deploy surfaces and prints exact dry-run/live deploy command matrix for both.

## Product Finish Line (Active)

- [ ] Native SvelteKit request lifecycle:
  - move starter request handling from the worker-first `defineApp(...)` shell onto SvelteKit server entry contracts.
  - update scaffold generation in `create-swsdk` so new apps use the same native lifecycle by default.
  - add tests proving route/render/function behavior through native SvelteKit handling.
- [x] Consumer-boundary hardening:
  - enforce package boundary checks against package outputs in CI (not only source-level TS path mappings).
  - keep fast workspace development ergonomics while proving publishable package integrity.
- [x] Effect API unification pass:
  - standardize starter/scaffold/examples on upstream `Effect`/`Layer` composition patterns.
  - keep compatibility helpers but publish a single recommended runtime composition path.
- [x] Internal app import boundary cleanup:
  - starter/playgrounds now use `@swsdk/*` package-specifier imports.
  - removed direct sibling `packages/*/src` imports from app source files.
