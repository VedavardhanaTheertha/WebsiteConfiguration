# Website Configuration

This repository supplies shared settings for
[MainWebsiteUI](https://github.com/VedavardhanaTheertha/MainWebsiteUI).
It is checked out there as the `settings/` Git submodule. Publish reviewed settings
commits before updating the website's submodule reference; a parent commit does not
include uncommitted files here.

Topic prose belongs in WebsiteLibrary Markdown and short reusable UI labels in the
website's `content/languages/*.json`. Do not add duplicate editorial catalogs here.

## Files and ownership

| File | Owns |
|---|---|
| `home.json` | Homepage copy and four curated page-reference lists |
| `navigation.json` | Menu order/routes, footer policy destinations, social `connectId` references, utility routes |
| `{dev,prod}/images.json` | Stable image aliases and media-directory base |
| `{dev,prod}/toplibrary.json` | Featured collection item IDs |
| `image_policy.json` | Framework image optimization and allowed remote patterns |
| `common.json` | Shared webpage presentation defaults (common fixed unscrollable background image, font families, and uniform font size scale) |
| `theme.json` | Shared CSS theme tokens (colors, spacing, radiuses, shadows) |
| `ui_elements.json` | Reader/viewer behavior, defaults, timing, pagination, scrolling |
| `public_actions.json` | Public action destinations or official channel references |
| `announcements.json` | Popup eligibility, schedule, delay, and dismissal |
| `search.json` | Matching behavior and collection field weights |
| `knowledge_sections.json` | Supported knowledge section order and default images |
| `dev/synthetic_events.json` | Optional development/local synthetic event copies |

Environment-specific hero groups live in `library/hero/{dev,prod}/*.hero.json`.
Preserve their selection and ordering; local uses the development group.
Learn's labels, subjects, order, and suggested paths live in `library/learn/`
Markdown. Guru ordering comes from numeric canonical Markdown filename prefixes.

## Homepage

`home.json` has localized homepage fields and these page-reference lists:

- `top_carousel`: six filenames.
- `icon_carousel`: five objects containing `page`, `icon`, and `tone`.
- `bottom_carousel`: six filenames distinct from the top list.
- `explore_carousel`: three filenames.

Filenames resolve under `library/pages/`. Card titles, descriptions, images,
destinations, page types, and footnotes belong in those multilingual Markdown
records, not duplicated in settings.

Do not configure `remidies_carousel` here. Content generation chooses three distinct
articles from `library/remedies/` and shares that selection across languages and
content variants. Keep at least four eligible remedy articles. The existing key
spelling is a data contract.

## Common presentation

`common.json` configures shared presentation properties across all webpages:

### Background

- `background.backgroundImage`: Valid `@image.*` alias, `@<key>` alias, or image URL for the common background image.
- `background.opacity`: Number between 0 and 1 for background transparency (default `0.12`).
- `background.size`: CSS background-size (e.g. `cover`, `contain`; default `cover`).
- `background.repeat`: CSS background-repeat (e.g. `no-repeat`; default `no-repeat`).
- `background.position`: CSS background-position (default `center`).

This background image renders with fixed viewport positioning behind the main content area, remaining unscrollable across all pages.

### Font Families

- `fontFamily.display`: Display serif font family stack for headings and titles.
- `fontFamily.body`: Body sans-serif font family stack for reading copy.
- `fontFamily.sanskrit`: Traditional Sanskrit font family stack.
- `fontFamily.kannada`: Kannada font family stack.
- `fontFamily.mono`: Monospace font family stack.

The root layout injects these as `--font-*` CSS custom properties.

### Font Sizes

- `fontSize.micro`: Micro elements such as badges, tags, eyebrows, tooltips (default `12px`).
- `fontSize.caption`: Secondary hints, timestamps, compact descriptions (default `13px`).
- `fontSize.ui`: Submenus, compact controls, filters, dropdown items (default `14px`).
- `fontSize.nav`: Navigation links, menu items (default `15px`).
- `fontSize.body`: Standard body text, descriptions (default `16px`).
- `fontSize.badge`: Category tags and status badges (default `12px`).
- `fontSize.formLabel`: Form field labels (default `13px`).
- `fontSize.formInput`: Form input fields (default `16px`).
- `fontSize.actionButton`: Compact action and CTA buttons (default `14px`).
- `fontSize.meta`: Metadata keys, dates, and counts (default `13px`).

Values must be valid CSS lengths (`px`, `rem`, `em`). The root layout injects these as `--text-*` CSS custom properties consumed by utility classes in `src/app/globals.css`.

## Image resolution and theme

All image-map resolution uses the website's `build/image-resolution.mjs`. Absolute
HTTPS targets pass through independently of `baseUrl`. Relative targets are
**appended** to the media-directory base, even if they begin with `/`. For example,
base `https://example.org/media/` plus `/portraits/guru.webp` resolves to
`https://example.org/media/portraits/guru.webp`, not the origin root. Unsafe schemes,
credentials, malformed paths, and traversal are rejected.

`image_policy.json` has `unoptimized` and `remotePatterns`. Each remote pattern has
`protocol`, `hostname`, and `pathname`; an empty list is valid. Keep allowed hosts
consistent with authored image URLs. The framework configuration loads this policy
instead of maintaining a second list.

`theme.json` has one `tokens` object with CSS custom properties in the `--color-*`,
`--spacing-*`, `--radius-*`, and `--shadow-*` namespaces. Values are CSS
token strings; unknown fields, invalid values, unresolved references, and cycles fail
validation. The generator writes `src/gen/settings/theme.css`; do not edit generated
CSS. Responsive/layout rules remain website code.

## UI behavior

`ui_elements.json` supports:

| Group | Fields and behavior |
|---|---|
| `collections` | Positive `initialVisible` and `loadIncrement` counts |
| `autoScroll` | `enabled`, positive `pixelsPerSecond`, positive `returnDurationMs`, non-negative `resumeDelayMs` |
| `hero` | `autoplay`, positive `intervalMs`, non-negative `fadeDurationMs` and `zoomDurationMs` |
| `upcomingEvents` | Non-negative `lookAheadDays`, positive `maxCards`; inclusive UTC window includes ongoing events |
| `readingTime` | Positive `wordsPerMinute`; supplied reading minutes take precedence |
| `bhaktiReader` | Percentage `initialSize`, `minSize`, `maxSize`, and `step` |
| `mediaViewer` | `swipeThresholdPx`, `videoControlExclusionPx`, and `wrap`; wrap boundaries apply to arrows, keyboard, and swipes |
| `browserDefaults` | `advancedFiltersOpen`, `lineageView`, upcoming/past event sort, and album sort |
| `interaction` | Dropdown-close, modal-reset, and search-preload delays in milliseconds |

Horizontal rails preserve touch scrolling, desktop dragging, pause/resume after
interaction, and reduced-motion behavior. Disabling autoplay does not disable manual
navigation. Hero behavior is shared with generated standalone hero HTML. Reset and tab
transitions use configured defaults rather than a second set of constants.

## Official actions

`public_actions.json` owns action destinations. Its `connectId` references resolve
to canonical `library/connect/*.md` records.
Destinations must be verified and valid for the intended action. The official
WhatsApp destination is a broadcast channel, not a private-message inbox; do not use
it as a prefilled enquiry destination.

Preserve the complete monetary query string. The payment QR is generated at build
time from the same configured destination. Material enquiries use the localized
catalog in `library/donations/pages/donation.md` and the configured email destination.
External services handle payment processing and official communication.
Do not imply message delivery or put credentials in public settings.

## Announcements

`announcements.json` supports:

- `enabled`: global visibility.
- `delayMs`: non-negative appearance delay.
- `pages`: exact site-root-relative paths without deployment prefix, query, or
  fragment. Empty means all pages; child paths are not implicitly included.
- `startsOn`, `endsOn`: null or valid `YYYY-MM-DD`; inclusive UTC boundaries.
- `dismissal`: `session`, `persistent`, or `none`.

Navigation restarts the appearance delay; open pages respond to schedule boundaries.
Popup prose, associated page copy, and metadata belong to the canonical announcement
Markdown. Its destination comes from `navigation.json`. For the Yajna progress
snapshot, update its count and `pages.local_copy.yajna_as_of_date` together.
The generator requires a real, shared ISO date;
it never substitutes the build date.

## Search and collections

`search.json` supports prefix matching, `fuzzyTolerance` from 0 to 1,
`fuzzyMinLength`, and `multiwordOrFallback`. The latter retries a multiword query with
OR only if the primary AND query finds nothing. Field-weight `profiles` cover
`bhakti`, `sevas`, `events`, `knowledge`, `mediaAlbums`, and `mediaGrid`; unknown
profiles/fields fail validation. Generated and runtime indexes use the same settings.

`knowledge_sections.json` must include every supported section ID exactly once with
a unique positive `order` and valid `@image.*` reference defined in both image maps.
IDs/routes remain website code; articles remain topic Markdown.

Synthetic events apply only to development/local. `enabled` controls copies,
`maxEvents` is null or a non-negative integer, and `dateShift.policy` is
`earliest-to-today` or `fixed-offset` with `offsetDays`. Originals are preserved;
production never creates synthetic copies.

## Apply and validate changes

From the website checkout, `npm run settings:build` validates shared UI settings.
Use `npm run content:build` or restart `npm run dev:local` after actions, channels,
search, image, knowledge, or hero changes so dependent generated data is refreshed.
Full builds regenerate all settings automatically. Browser timer delays cannot exceed
2147483647 milliseconds.

Before requesting integration review, run the website's `npm run ci` and read its
verification reports. Describe the settings change and any checks not performed.
