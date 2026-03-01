# Simple Conditional Visibility
Contributors:      Simple Conditional Visibility Contributors
Tags:              blocks, visibility, conditional content
Requires at least: 6.2
Tested up to:      6.8
Requires PHP:      7.4
Stable tag:        0.2.0
License:           GPL-2.0-or-later
License URI:       https://www.gnu.org/licenses/gpl-2.0.html

Block Visibility Control Using CSS Class Names.

## Description

Simple Conditional Visibility lets you show or hide blocks by adding simple `cvis-*` class names in each block's **Advanced > Additional CSS Classes** field.

It works server-side (secure): if a block should be hidden, it is removed before HTML is sent to the browser.
For user-dependent rules, the plugin automatically applies strict no-cache headers.

## Free vs Premium

Free plugin includes:
- all core visibility rules listed in "Free Rules" below

Premium add-on includes:
- audience label targeting (`cvis-audience:*`, `cvis-not-audience:*`)
- user profile field to manage audience labels
- custom key/value targeting rules (`cvis-if:*`, `cvis-not:*`)
- custom user attributes (for those premium key/value rules)

If premium is not active, `cvis-audience:*`, `cvis-not-audience:*`, `cvis-if:*`, and `cvis-not:*` are ignored.

## Free Rules (Built-In)

Add one or more of these class names to a block:

`cvis-hide`
- Always hide this block from site visitors.

`cvis-secure`
- Optional force marker for strict no-cache headers.
- Also hides editor preview for blocked content.

`cvis-logged-in`
- Show this block only to logged-in users.

`cvis-logged-out`
- Show this block only to logged-out visitors.

`cvis-mobile`
- Show this block only on mobile devices.

`cvis-desktop`
- Show this block only on desktop devices.

`cvis-role:administrator`
- Show only to users with one of the listed WordPress roles.
- You can list multiple roles with commas: `cvis-role:administrator,editor`.

`cvis-cap:manage_options`
- Show only to users who have a specific WordPress capability.
- `manage_options` is usually admins only.

`cvis-post-type:post,page`
- Show only on specific content types (post/page/custom type).

`cvis-post-status:publish`
- Show only when current content status matches (publish, draft, etc.).

`cvis-view:front-page,archive`
- Show only on specific request/page views.

`cvis-front-page` / `cvis-home` / `cvis-singular` / `cvis-archive` / `cvis-search` / `cvis-404`
- Shortcut class names for common page views.

`cvis-date-after:2026-03-01`
- Show only after this date/time.

`cvis-date-before:2026-03-31`
- Show only before this date/time.

`cvis-query:utm=summer`
- Show only when the URL query param matches.

## Premium Rules

These require the premium add-on to be active:

`cvis-if:key=value`
- Show only when a premium context value matches.

`cvis-not:key=value`
- Hide when a premium context value matches.

`cvis-audience:group-a`
- Show only for users whose profile has audience label `group-a`.

`cvis-not-audience:group-a`
- Hide for users whose profile has audience label `group-a`.

Premium context keys available by default:
- `audience`, `audiences`
- `user_id`
- `logged_in` (`1` or `0`)

Add-ons can add more keys via `cvis_context_vars`.

## Installation

1. Upload plugin files to `/wp-content/plugins/simple-conditional-visibility`, or install from Plugins screen.
2. Activate the plugin.
3. Add `cvis-*` classes in block **Advanced > Additional CSS Classes**.
4. Optional: open **Tools > Simple Conditional Visibility Rules** for copy/paste examples.

## Quick Examples

Logged-in only:
`cvis-logged-in`

Admin only:
`cvis-role:administrator`

Premium audience group only:
`cvis-audience:group-a`

Campaign URL only:
`cvis-query:utm=summer`

Premium plan targeting:
`cvis-if:plan=pro`

## Security notes
- This plugin hides content server-side (not CSS-only hiding).
- The plugin auto-applies strict no-cache for user-dependent rules.
- Use `cvis-secure` when you want to force strict no-cache even without user-dependent rules.
- Caches/CDNs must still be configured correctly for personalized pages.

## Changelog

### 0.2.0
* Replaced brittle parser with deterministic condition evaluation.
* Made class-based `cvis-*` rules the primary documented workflow.
* Added support for spaced class syntax like `cvis-cap: manage_options`.
* Added freemium-ready feature flags (`cvis_feature_flags`) for extension-enabled context keys.
