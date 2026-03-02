# Simple Conditional Visibility

Block Visibility Control Using CSS Class Names.

```
Contributors: MaDrCloudDev, https://github.com/MaDrCloudDev
Tags: blocks, visibility, conditional content
Requires at least: 6.2
Tested up to: 6.8
Requires PHP: 7.4
Stable tag: 0.2.0
License: GPL-2.0-or-later
License URI: https://www.gnu.org/licenses/gpl-2.0.html
```

## Description

Simple Conditional Visibility lets you show or hide blocks by adding simple `cvis-*` class names in each block's **Advanced > Additional CSS Classes** field.

It works server-side (secure): if a block should be hidden, it is removed before HTML is sent to the browser.
For user/request-dependent rules, the plugin automatically applies strict no-cache headers.

## How to Use (No Extra UI)

1. Edit any block.
2. Open **Advanced > Additional CSS Classes**.
3. Paste one `cvis-*` class and update the page.

Start with these:

- `cvis-hide` (always hide)
- `cvis-logged-in` or `cvis-logged-out`
- `cvis-query:utm=summer`
- `cvis-secure` (force strict no-cache for sensitive conditions)

## Free vs Premium

Free plugin includes:

- all core visibility rules listed in "Free Rules" below

Premium add-on includes:

- audience label targeting (`cvis-audience:*`, `cvis-not-audience:*`)
- user profile field to manage audience labels
- custom key/value targeting rules (`cvis-if:*`, `cvis-not:*`)
- custom user attributes (for those premium key/value rules)
- WooCommerce targeting classes (`cvis-wc-*`)
- Easy Digital Downloads targeting classes (`cvis-edd-*`)
- WP Fusion tag targeting classes (`cvis-wpf-tag:*`, `cvis-not-wpf-tag:*`)

If premium is not active, premium rule classes are ignored.

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

`cvis-tablet`

- Show this block only on tablet devices (user-agent based).

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

`cvis-view:pricing`

- Show only on the page route/slug or page ID you provide.
- Examples: `cvis-view:pricing`, `cvis-view:account/billing`, `cvis-view:42`.

`cvis-date-after:2026-03-01`

- Show only after this date/time.

`cvis-date-before:2026-03-31`

- Show only before this date/time.

`cvis-query:utm=summer`

- Show only when the URL query param matches.

`cvis-preset:vip_banner`

- Apply a reusable rule preset defined by a developer via filter.

`cvis-user:15,22`

- Show only for specific logged-in user IDs.
- User IDs are numeric IDs from the WordPress Users list.

`cvis-not-user:15,22`

- Hide for specific logged-in user IDs.

`cvis-cookie:campaign=summer`

- Show only when a cookie exists (or matches key=value).

`cvis-not-cookie:staff=1`

- Hide when a cookie exists (or matches key=value).

`cvis-referrer:google.com`

- Show only when HTTP referrer contains the value.
- Referrer data is browser-provided and may not exist on every visit.

`cvis-not-referrer:affiliate.example`

- Hide when HTTP referrer contains the value.

`cvis-view:category` / `cvis-view:tag` / `cvis-view:author` / `cvis-view:date`

- Show only on those archive contexts.

`cvis-view:post-type:product`

- Show only on the post type archive or context post type.

`cvis-view:taxonomy:product_cat`

- Show only on a specific taxonomy archive.

`cvis-acf:membership=gold`

- Show only when ACF field value matches the current post context.

`cvis-not-acf:membership=gold`

- Hide when ACF field value matches the current post context.

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

`cvis-wc-product:120`

- Show only on specific WooCommerce product IDs.

`cvis-not-wc-product:120`

- Hide on specific WooCommerce product IDs.

`cvis-wc-in-cart:120`

- Show only when cart contains one of the product IDs.

`cvis-not-wc-in-cart:120`

- Hide when cart contains one of the product IDs.

`cvis-wc-purchased:120`

- Show only when current user has purchased one of the product IDs.

`cvis-not-wc-purchased:120`

- Hide when current user has purchased one of the product IDs.

`cvis-edd-download:220`

- Show only on specific EDD download IDs.

`cvis-not-edd-download:220`

- Hide on specific EDD download IDs.

`cvis-edd-in-cart:220`

- Show only when EDD cart contains one of the download IDs.

`cvis-not-edd-in-cart:220`

- Hide when EDD cart contains one of the download IDs.

`cvis-edd-purchased:220`

- Show only when current user has purchased one of the download IDs.

`cvis-not-edd-purchased:220`

- Hide when current user has purchased one of the download IDs.

`cvis-wpf-tag:vip`

- Show only for users with one of the WP Fusion tags.

`cvis-not-wpf-tag:vip`

- Hide for users with one of the WP Fusion tags.

Premium context keys available by default:

- `audience`, `audiences`
- `user_id`
- `logged_in` (`1` or `0`)

Add-ons can add more keys via `cvis_context_vars`.

## Optional Developer Controls

These are optional for advanced teams and agencies:

- `cvis_visibility_presets` filter
  - Define reusable class bundles, then use `cvis-preset:your_name`.
- `cvis_disabled_rule_keys` filter
  - Disable selected rule families globally (example: disable query rules).
- `cvis_allowed_block_types` filter
  - Limit conditional visibility to specific block types.
- `cvis_show_hidden_indicator` filter
  - Hide editor-only indicators globally.
- `cvis_device_type` filter
  - Override mobile/tablet/desktop detection when needed.
- `cvis_remove_data_on_uninstall` filter (or `CVIS_REMOVE_DATA_ON_UNINSTALL` constant)
  - Remove plugin user meta data when uninstalling.
- CSS variables for indicator color
  - `--cvis-indicator-accent`, `--cvis-indicator-bg`, `--cvis-indicator-text`

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

Specific page route only:
`cvis-view:pricing`

Premium plan targeting:
`cvis-if:plan=pro`

Specific users only:
`cvis-user:15,22`

Cookie campaign only:
`cvis-cookie:campaign=summer`

Developer preset:
`cvis-preset:vip_banner`

ACF field match:
`cvis-acf:membership=gold`

Premium WooCommerce cart rule:
`cvis-wc-in-cart:120`

## Security notes

- This plugin hides content server-side (not CSS-only hiding).
- The plugin auto-applies strict no-cache for user/request-dependent rules (logged-in, role, capability, user id, cookie, referrer, audience, `cvis-if/cvis-not`, WooCommerce cart/purchase, EDD cart/purchase, WP Fusion tags).
- Use `cvis-secure` when you want to force strict no-cache even without user/request-dependent rules.
- Caches/CDNs must still be configured correctly for personalized pages.

## Changelog

### 0.2.0

- Replaced brittle parser with deterministic condition evaluation.
- Made class-based `cvis-*` rules the primary documented workflow.
- Added support for spaced class syntax like `cvis-cap: manage_options`.
- Added freemium-ready feature flags (`cvis_feature_flags`) for extension-enabled context keys.
- Added class-first specific-user rules (`cvis-user:*`, `cvis-not-user:*`).
- Added class-first request rules (`cvis-cookie:*`, `cvis-not-cookie:*`, `cvis-referrer:*`, `cvis-not-referrer:*`).
- Expanded `cvis-view:*` tokens for archive/location targeting (`category`, `tag`, `author`, `date`, `post-type:*`, `taxonomy:*`).
- Added class preset support (`cvis-preset:*`) via `cvis_visibility_presets`.
- Added policy hooks for global rule governance (`cvis_disabled_rule_keys`, `cvis_allowed_block_types`) and indicator toggling (`cvis_show_hidden_indicator`).
- Added class-first ACF targeting rules (`cvis-acf:*`, `cvis-not-acf:*`).
- Added premium integration rule families for WooCommerce (`cvis-wc-*`), Easy Digital Downloads (`cvis-edd-*`), and WP Fusion tags (`cvis-wpf-tag:*`, `cvis-not-wpf-tag:*`).
- Added tablet device rule (`cvis-tablet`) and device override hook (`cvis_device_type`).
