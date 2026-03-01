# Simple Conditional Visibility

Block Visibility Control Using CSS Class Names.

This plugin uses fields WordPress already has:
- block **Additional CSS Classes**
- premium add-on user profile audience labels (admin-managed)
- query params (optional, for `cvis-query:*`)

No extra condition-builder UI is required.

## Description

Simple Conditional Visibility lets you show or hide any block using short `cvis-*` classes.

It is built to be:
- simple for non-technical users
- secure (server-side rendering decisions)
- flexible for advanced targeting

## Create Conditional Blocks in Seconds

It takes 3 steps:
1. Add or select a block in the editor.
2. Add a `cvis-*` class in **Advanced > Additional CSS Classes** (space-separated preferred; commas are tolerated).
3. Save and view the page.

## Packed With Features

- Hide a block globally: `cvis-hide`
- Cache hardening marker: `cvis-secure`
- Logged-in / logged-out targeting
- Device targeting (mobile/desktop)
- Role targeting
- Date window targeting
- Query-string targeting
- Premium add-on audience targeting (`cvis-audience:*`)
- Premium add-on K/V targeting (`cvis-if:key=value`, `cvis-not:key=value`)

## Authoring Model

Use each input for one clear purpose:

- **Additional CSS Classes**: rule definitions (`cvis-*`)
- **Premium Add-ons/Extensions**: audience labels and custom context keys

Important:
- For custom K/V access rules, use extension-provided context values.
- Do not rely on block HTML Anchor/title as condition inputs.

## Core Rule Examples

- `cvis-logged-in`
- `cvis-logged-out`
- `cvis-role:administrator`
- `cvis-date-after:2026-03-01`
- `cvis-date-before:2026-03-31`
- `cvis-query:utm=summer`

## Security Model

- Visibility is evaluated server-side during `render_block`.
- Hidden blocks are removed from output (not just hidden with CSS).
- Strict no-cache is auto-applied for user-dependent rules (logged-in, role/cap, audience, cvis-if/cvis-not).
- `cvis-secure` is an optional force marker and also suppresses editor hidden preview.

## Caching Caveat (Important)

For auth/query/device-sensitive pages, cache layers must vary correctly or bypass cache.

If not configured, a cached privileged response can be served to the wrong user.

## Installation

### WordPress install (standard)

1. Copy plugin files into `/wp-content/plugins/simple-conditional-visibility/`.
2. Activate the plugin in WordPress.
3. Add `cvis-*` classes to blocks.

### Local development from this repo

```bash
cd plugin-source-code
npm run verify
```

`npm run verify` does:
- PHP lint
- build
- sync to local `wp-content/plugins/simple-conditional-visibility`
- sync integrity verification (host + container, when Docker WordPress is running)

## FAQ

### Does this work with any block?

Yes for class-based rules, because it uses the block wrapper output in `render_block`.

### Is visibility controlled with CSS?

No. CSS is not the security boundary here.
Visibility decisions are made server-side before final HTML output.

### Can non-admin users edit audience data?

With premium enabled, audience labels are restricted to user-management capability (`promote_users` by default).

### How do I create custom conditions like plan tiers?

Use the premium add-on, then use `cvis-if:key=value` with values from:
- premium context providers
- `cvis_context_vars` filter

### Can I use HTML Anchor and title normally?

Yes. They are no longer needed for Simple Conditional Visibility rule inputs.
Use them for their normal block/HTML purpose.

## Project Docs

- Plugin user readme: [plugin-source-code/readme.txt](plugin-source-code/readme.txt)
- Architecture: [ARCHITECTURE.md](ARCHITECTURE.md)
- Engineering decisions: [CHANGELOG-ENGINEERING.md](CHANGELOG-ENGINEERING.md)
- Active plan: [PLAN.md](PLAN.md)
- Freemium release plan: [FREEMIUM-PLAN.md](FREEMIUM-PLAN.md)
- Premium add-on starter: [premium-addon-starter/README.md](premium-addon-starter/README.md)
- Cache deployment playbook: [CACHE-GUIDANCE.md](CACHE-GUIDANCE.md)
- WP.org compliance checklist (internal): [WPORG-GUIDELINES-CHECKLIST.md](WPORG-GUIDELINES-CHECKLIST.md)
- Agent handoff guide: [AGENTS.md](AGENTS.md)

## License

GPL-2.0-or-later
