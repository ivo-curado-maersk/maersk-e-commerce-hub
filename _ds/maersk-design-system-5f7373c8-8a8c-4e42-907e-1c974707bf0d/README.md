# Maersk Design System (MDS) — how to build with these components

MDS ships **React components** that wrap Maersk's Lit web components. This bundle
covers the **complete library — all 71 exports**, including `McIcon` (icons work
offline; see Icons below):
- **Actions:** `McButton`, `McLinkButton`, `McButtonGroup`, `McSplitButton`
- **Forms:** `McInput`, `McTextarea`, `McSelect`, `McSelectNative`, `McMultiSelect`,
  `McTypeahead`, `McTypeaheadMultiSelect`, `McCheckbox`/`McCheckboxGroup`,
  `McRadio`/`McRadioGroup`, `McSwitch`, `McSwitchGroup`, `McMultiChoiceFieldset`,
  `McNumberStepper`, `McInputGroup`, `McPicker`, `McFileUpload`, `McLabel`, `McHint`,
  `McError`
- **Date & time:** `McCalendar`, `McInputDate`, `McInputTime`, `McTimePicker`,
  `McMonthYearPicker`, `McDateRange`
- **Content & status:** `McCard`, `McBadge`, `McTag`, `McAvatar`, `McNotification`,
  `McTooltip`, `McToast`, `McLoadingIndicator`, `McProgressIndicator`, `McTextAndIcon`
- **Navigation & layout:** `McAccordion`, `McTabBar`, `McSegmentedControl`,
  `McPagination`, `McTimeline`, `McTopBar`, `McSideBar`, `McToolbar`, `McList`,
  `McTable`, `McStepIndicator`, `McThemeSwitch`
- **Overlays:** `McModal`, `McDialog`, `McDrawer`, `McPopover`, `McMenu`
- **Chat / code:** `McChat`, `McChatTextarea`, `McCodePreview`
- **Icon:** `McIcon`

Import them from the bundle global (`window.Mds`); each renders a self-styled custom
element. No theme provider or context wrapper is required — just make sure the
bundle's `styles.css` is loaded on the page (it defines the `--mds_*` design tokens
and ships the Maersk brand fonts). Without `styles.css`, components fall back to
system fonts and default colors.

Brand/theme in this bundle: **Maersk, light theme.**

**Compound components** nest their children (e.g. `McSelect` > `McOption`,
`McRadioGroup` > `McRadio`, `McList` > `McListItem`, `McTabBar` > `McTab`,
`McAccordion` > `McAccordionItem`, `McSegmentedControl` > `McSegmentedControlItem`,
`McTimeline` > `McTimelineItem`, `McStepIndicator` > `McStepIndicatorItem`,
`McSwitchGroup` > `McSwitch`, `McButtonGroup` > `McButtonGroupItem`). Named slots are
passed as a `slot="…"` prop on the child (e.g. `<McButton slot="trigger">` inside
`McTooltip`/`McPopover`/`McMenu`, `<McList slot="menu">` inside `McSplitButton`,
`<McButton slot="footer">` inside `McModal`/`McDialog`/`McDrawer`).

**Overlays** (`McModal`, `McDialog`, `McDrawer`, `McPopover`, `McMenu`, `McToast`)
render nothing until opened — set the boolean `open` prop to show them. Declarative
data components take arrays rather than children: `McPagination` (page props),
`McSelectNative` (`options`), `McTable` (`columns` + `data`), `McTypeahead`/
`McTypeaheadMultiSelect` (`data`), `McCodePreview` (`code`), `McChat`
(`historymessages` + `suggestions`).

**Date/time value formats:** `McCalendar`/`McInputDate` take an ISO date string
(`value="2026-07-14"`); `McInputTime`/`McTimePicker` take `"HH:mm"`;
`McMonthYearPicker` takes `{ month, year }` where `month` is **0-based**
(`{ month: 6, year: 2026 }` = July 2026); `McDateRange` takes `{ from, to }` ISO
strings and labels via `legend`/`fromlabel`/`tolabel` (it has no `label` prop).

## Style through PROPS, not CSS classes

MDS has **no utility-class system**. You style components by setting their props;
their appearance is driven entirely by the design tokens. Do not invent class names.

- **McButton** — label is the child. Key props:
  - `appearance`: `primary` | `secondary` | `neutral` | `error` | `inverse`
  - `variant`: `filled` | `outlined` | `plain`
  - `fit`: `small` | `medium` | `large`
  - `disabled`, `loading`, `hiddenlabel`, `icon`, `trailingicon`, `width` (`full-width`)
- **McCard** — content via props (or slots):
  - `variant`: `bordered` | `borderless`; `orientation`: `vertical` | `horizontal` | `horizontal-reverse`
  - `heading`, `subheading`, `body`, `footer`, `image`, `fit`, `contentalignment`
- **McInput** — `label` is REQUIRED (used as the accessible name). Also:
  - `type` (`text`|`email`|`password`|`search`|`number`|…), `placeholder`, `value`
  - `fit`, `disabled`, `required`, `labelposition`, `trailingicon`

> **Icons work offline.** The full Maersk icon set (both 20px and 24px) is bundled and
> preloaded, so `<McIcon icon="anchor" />` and the `icon`/`trailingicon` props on other
> components render with no network call. Icon names are the MDS glyph name **without
> the `mi-` prefix** (e.g. `icon="anchor"`, `icon="box"`, `icon="container"`,
> `icon="calendar"`, `icon="bell"`, `icon="house"`, `icon="magnifying-glass"`,
> `icon="arrow-right"`, `icon="check-circle"`, `icon="exclamation-triangle"`). `McIcon`
> also takes `size` (`"16"|"20"|"24"`) and `color` (any CSS color). An unknown icon name
> renders empty, so use real MDS glyph names.

## For your own layout/glue, use MDS design tokens

When you write wrapper markup (grids, spacing, custom color), reference the shipped
CSS custom properties rather than hard-coded values. They live in
`tokens/design-tokens-rem.css` (all `:root`). Examples of real token families:
`var(--mds_brand_appearance_primary_default_background-color)`,
`var(--mds_brand_appearance_neutral_weak_background-color)`,
`var(--mds_global_border_width)`, and the type tokens
`var(--mds_brand_typography_text_font-family)` / `…_headline_font-family`
(Maersk Text / Maersk Headline).

## Where the truth lives

Read these before composing:

- `components/general/<Name>/<Name>.d.ts` — the exact prop contract.
- `components/general/<Name>/<Name>.prompt.md` — per-component usage + examples.
- `styles.css` and its imports (`tokens/design-tokens-rem.css`, `_ds_bundle.css`) —
  the full token vocabulary and component styling.

## Idiomatic example

```jsx
import { McCard, McButton, McInput } from window.Mds; // provided by the bundle

function BookingCard() {
  return (
    <McCard variant="bordered" heading="New booking" subheading="Ocean freight"
            style={{ maxWidth: 360 }}>
      <div style={{ display: 'flex', flexDirection: 'column', gap: 16 }}>
        <McInput label="Booking reference" placeholder="e.g. 240512345" />
        <McButton appearance="primary" width="full-width">Confirm booking</McButton>
      </div>
    </McCard>
  );
}
```

# Mds (@maersk-global/mds-react-wrapper@0.0.0)

This design system is the published @maersk-global/mds-react-wrapper React library, bundled as a single
browser global. All 71 components are the real upstream code.

## Where things are

- `_ds_bundle.js` — the whole-DS bundle at the project root; loads every component to `window.Mds`. First line is a `/* @ds-bundle: … */` metadata header.
- `styles.css` — the single stylesheet entry: it `@import`s the tokens, fonts, and component styles (`_ds_bundle.css`). Link this one file.
- `components/<group>/<Name>/<Name>.prompt.md` (example JSX + variants), `<Name>.d.ts` (types), `<Name>.html` (variant grid).
- `tokens/*.css` — CSS custom properties, names verbatim from upstream.
- `fonts/` — `@font-face` files + `fonts.css` (when the package ships fonts).

For a specific component, `read_file("components/<group>/<Name>/<Name>.prompt.md")`.

## Loading

Add these two lines to your page once (React must be on the page first):

```html
<link rel="stylesheet" href="styles.css">
<script src="_ds_bundle.js"></script>
```

Components are then available at `window.Mds.*`. Mount into a dedicated child node (e.g. `<div id="ds-root">`), not the host page's own React root, so the two trees don't collide:

```jsx
const { McAccordion } = window.Mds;
ReactDOM.createRoot(document.getElementById('ds-root')).render(<McAccordion />);
```

## Tokens

438 CSS custom properties from @maersk-global/mds-design-tokens. Names are
preserved verbatim from upstream. See `tokens/` for the full list.

- **color** (149): `--mds_global_link_inline_text-decoration`, `--mds_global_link_inline_hover_text-decoration`, `--mds_global_link_stand-alone_text-decoration`, …
- **spacing** (83): `--mds_foundations_breadcrumb_item_padding`, `--mds_foundations_breadcrumb_collapsed_padding`, `--mds_foundations_breadcrumb_truncated_item_padding`, …
- **typography** (100): `--mds_brand_typography_headline_font-family`, `--mds_brand_typography_headline_x-small_font-style`, `--mds_brand_typography_headline_x-small_font-weight`, …
- **radius** (30): `--mds_brand_border_x-small_radius`, `--mds_brand_border_small_radius`, `--mds_brand_border_medium_radius`, …
- **shadow** (22): `--mds_brand_appearance_shadow_low_first-layer_offset-x`, `--mds_brand_appearance_shadow_low_first-layer_offset-y`, `--mds_brand_appearance_shadow_low_second-layer_offset-x`, …
- **other** (54): `--mds_global_border_width`, `--mds_global_border_style`, `--mds_global_breakpoint_xs_min-width`, …

## Components

### general
- `McAccordion`
- `McAccordionItem`
- `McAvatar`
- `McBadge`
- `McButton`
- `McButtonGroup`
- `McButtonGroupItem`
- `McCalendar`
- `McCard`
- `McChat`
- `McChatTextarea`
- `McCheckbox`
- `McCheckboxGroup`
- `McCodePreview`
- `McDateRange`
- `McDialog`
- `McDrawer`
- `McError`
- `McFileUpload`
- `McHint`
- `McIcon`
- `McInput`
- `McInputDate`
- `McInputGroup`
- `McInputTime`
- `McLabel`
- `McLinkButton`
- `McList`
- `McListItem`
- `McLoadingIndicator`
- `McMenu`
- `McModal`
- `McMonthYearPicker`
- `McMultiChoiceFieldset`
- `McMultiSelect`
- `McNotification`
- `McNumberStepper`
- `McOption`
- `McPagination`
- `McPicker`
- `McPickerItem`
- `McPopover`
- `McProgressIndicator`
- `McRadio`
- `McRadioGroup`
- `McSegmentedControl`
- `McSegmentedControlItem`
- `McSelect`
- `McSelectNative`
- `McSideBar`
- `McSplitButton`
- `McStepIndicator`
- `McStepIndicatorItem`
- `McSwitch`
- `McSwitchGroup`
- `McTab`
- `McTabBar`
- `McTable`
- `McTag`
- `McTextAndIcon`
- `McTextarea`
- `McThemeSwitch`
- `McTimeline`
- `McTimelineItem`
- `McTimePicker`
- `McToast`
- `McToolbar`
- `McTooltip`
- `McTopBar`
- `McTypeahead`
- `McTypeaheadMultiSelect`
