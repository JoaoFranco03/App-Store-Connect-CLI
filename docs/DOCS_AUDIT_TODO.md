# Docs Audit TODO

Audit date: 2026-04-02

This checklist captures documentation drift verified against the live CLI with
`go run . --help` and targeted subcommand `--help` output.

## Verified Scope

- Checked root command taxonomy against:
  - `docs.json`
  - `commands/overview.mdx`
  - high-churn command pages in `commands/`
  - key workflow guides in `guides/`
- Verified current command surfaces directly in help for:
  - `apps`
  - `review`
  - `reviews`
  - `screenshots`
  - `web`
  - `xcode-cloud`
  - root global flags

## Priority 1: Correctness for High-Traffic Docs

- [ ] Refresh `commands/overview.mdx` to match the real root command tree.
  - Remove outdated top-level entries that are no longer taught as root commands:
    - `app-info`
    - `pre-release-versions`
    - `offer-codes`
    - `win-back-offers`
  - Add missing root commands that now exist and are user-facing:
    - `release`
    - `status`
    - `release-notes`
    - `account`
    - `xcode`
    - `diff`
    - `schema`
    - `app-setup`
    - `app-tags`
    - `build-bundles`
    - `build-localizations`
    - `background-assets`
    - `product-pages`
    - `routing-coverage`
    - `pre-orders`
    - `accessibility`
    - `encryption`
    - `eula`
    - `agreements`
    - `app-clips`
    - `android-ios-mapping`
    - `marketplace`
    - `alternative-distribution`
    - `nominations`
    - `game-center`
    - `merchant-ids`
    - `pass-type-ids`
    - `notarization`
  - Recheck example commands so they reflect the current canonical paths.

- [ ] Rewrite `commands/apps.mdx` around the current `apps` surface.
  - Mark `asc apps create` as deprecated.
  - Teach `asc web apps create` as the canonical app-creation path when web-session app creation is needed.
  - Add the currently documented-in-help but missing subcommands:
    - `wall`
    - `public`
    - `ci-product`
    - `remove-beta-testers`
    - `subscription-grace-period`
    - `search-keywords`
    - `app-encryption-declarations`
    - `content-rights`
  - Decide whether `commands/app-info.mdx` remains a separate compatibility explainer or gets folded into `apps`.

- [ ] Rewrite `commands/review.mdx` to include the current review surface.
  - Add `asc review status`.
  - Add `asc review doctor`.
  - Add missing attachment commands:
    - `attachments-get`
    - `attachments-delete`
  - Add missing submission commands:
    - `submissions-update`
    - `submissions-items-ids`
  - Add missing item commands:
    - `items-list`
    - `items-update`
    - `items-remove`
  - Fix the opening guidance: the page currently tells users to use `asc submit status` for simple status checks even though `asc review status` now exists.

- [ ] Rewrite `commands/reviews.mdx` to match the current `reviews` help.
  - Add `summarizations`.
  - Add nested response commands:
    - `response view`
    - `response delete`
    - `response for-review`
  - Keep `respond`, but explain when to use `respond` vs `response ...`.
  - Replace outdated flag docs:
    - page currently documents `--filter`
    - live command uses `--stars`
  - Recheck examples so they reflect the current response subcommand layout.

- [ ] Update `guides/screenshots.mdx` to teach the approved-artifacts workflow that now exists.
  - Replace the “approve then upload approved screenshots directly” path with:
    - `asc screenshots plan`
    - `asc screenshots apply --confirm`
  - Keep direct `upload` docs for raw App Store screenshot management, but stop teaching it as the continuation of the local review workflow.
  - Recheck every device-type example for current naming.

- [ ] Fix `commands/screenshots.mdx` device-type drift.
  - `screenshots sizes --help` uses `APP_IPHONE_65`.
  - `screenshots upload --help` uses `IPHONE_65`.
  - The doc page currently uses `APP_IPHONE_65` in upload examples.
  - Decide whether to:
    - normalize the docs to the actual upload help values, or
    - explain the distinction explicitly if both are valid in different contexts.

- [ ] Fix `commands/xcode-cloud.mdx` workflow subcommand naming.
  - The page currently says `get` in the workflow subcommand section.
  - Live help says `view`.
  - Align examples, subcommand bullets, and flag sections with the current `view` naming.

## Priority 2: Site Navigation and Coverage

- [ ] Update `docs.json` command navigation to reflect the real command surface.
  - The current navigation is missing many live root commands.
  - Add pages for the most user-visible missing commands first:
    - `release`
    - `status`
    - `release-notes`
    - `account`
    - `xcode`
    - `diff`
    - `app-setup`
    - `build-localizations`
    - `build-bundles`
  - Then add long-tail pages for the remaining surfaced commands.

- [ ] Decide how `commands/app-info` should be represented in navigation.
  - Current `docs.json` still exposes `commands/app-info` as a primary command page.
  - The canonical CLI path is `asc apps info ...`.
  - Options:
    - keep `commands/app-info` as an alias explainer page, or
    - rename/retitle the page to make the canonical `apps info` path explicit.

- [ ] Audit redirects in `docs.json` for deprecated or no-longer-canonical destinations.
  - `"/api-reference/app-info/list" -> "/commands/app-info"` should be revisited once the `apps info` docs shape is settled.

## Priority 3: Grep Sweep for Repeated Drift

- [ ] Search for and replace stale canonical examples:
  - `asc apps create`
  - `asc reviews respond` when nested response commands are the real management surface
  - `pre-release-versions`
  - `offer-codes`
  - `win-back-offers`

- [ ] Search for screenshot device-type naming drift across docs:
  - `APP_IPHONE_65`
  - `IPHONE_65`
  - update pages so the naming is consistent or intentionally explained.

- [ ] Search guides for local screenshot workflows that still jump from review approval straight to `screenshots upload`.
  - especially `guides/screenshots.mdx`
  - also check automation examples that generate screenshot upload commands from reviewed artifacts

- [ ] Search for links to command pages that do not exist yet.
  - especially around `release`, `status`, and other newly surfaced root commands that still lack dedicated pages

## Suggested Execution Order

- [ ] 1. Fix `commands/overview.mdx`.
- [ ] 2. Fix `docs.json` navigation and decide the `app-info` page strategy.
- [ ] 3. Fix `commands/apps.mdx`.
- [ ] 4. Fix `commands/review.mdx`.
- [ ] 5. Fix `commands/reviews.mdx`.
- [ ] 6. Fix `commands/screenshots.mdx` and `guides/screenshots.mdx` together.
- [ ] 7. Fix `commands/xcode-cloud.mdx`.
- [ ] 8. Add missing high-priority command pages (`release`, `status`, `release-notes`, `account`, `xcode`, `diff`).
- [ ] 9. Run a repo-wide grep sweep for stale examples and internal links.

## Validation After Each Batch

- [ ] Run `make check-command-docs`.
- [ ] Open the affected command with `go run . <command> --help` and confirm examples/terminology still match.
- [ ] Re-read `docs.json` navigation after adding or moving command pages.
- [ ] For guides, prefer matching the current canonical flow taught by the command help, not older aliases or compatibility paths.
