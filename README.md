# content-compose-web-example

A worked example of a **composed content project** built with
[throughline-compose](https://github.com/rhodium-org/throughline-compose), and the
**web member** of the medium-swap trio alongside
[content-compose-letter-example](https://github.com/timebacksolutions/content-compose-letter-example)
and
[content-compose-email-example](https://github.com/timebacksolutions/content-compose-email-example).

All three carry the **same council rent-arrears message** to the same lay tenant — the
same balance (**£318.60**), the same pay-by date (**1 August 2026**), the same rent
account (**4471-0932**). Only the **medium** changes: this project renders the message as
a **web page**, the letter renders it as a posted letter, and the email renders it as an
email. Hold the message and five axes fixed; swap only the channel.

| Namespace | Source | Axis | Same as the letter and email? |
|---|---|---|---|
| `plain` | [throughline-plain-language](https://github.com/rhodium-org/throughline-plain-language) `@v2026-07` | readability | ✅ identical |
| `conventions` | [throughline-conventions-uk](https://github.com/rhodium-org/throughline-conventions-uk) `@v2026-07` | British-English conventions | ✅ identical |
| `tone` | [throughline-tone-formal](https://github.com/rhodium-org/throughline-tone-formal) `@v2026-07` | register (formal) | ✅ identical |
| `purpose` | [throughline-purpose-instruct](https://github.com/rhodium-org/throughline-purpose-instruct) `@v2026-07` | purpose (instruct) | ✅ identical |
| `audience` | [throughline-audience-general](https://github.com/rhodium-org/throughline-audience-general) `@v2026-07` | audience (general reader) | ✅ identical |
| `medium` | [throughline-medium-web](https://github.com/rhodium-org/throughline-medium-web) `@v2026-07` | medium (**web**) | ❌ **letter/email → web** |

## The one axis that changed

The council still wants the message readable, correctly styled, in the formal register,
doing the instruct job and pitched at a lay tenant — so readability, conventions, tone,
purpose and audience are composed **unchanged** from the letter and email. The channel is
what differs: a web page is scanned on screen with links to click, where a letter is read
straight through on paper. So everything the channel imposes changes, and only that:

- it opens with a **search-friendly title** and **no address block, salutation or
  sign-off** (`medium:SR-0007`, `medium:SR-0008`) — the letter and email have those slots;
- it is broken into **short, scannable headed sections** (`medium:SR-0002`), where the
  letter is a single linear read;
- it offers **one clear online action to pay** — a live pay link — rather than writing a
  postal address out in full;
- it leads with **how much is owed and the pay-by date** (`medium:SR-0001`,
  `medium:SR-0004`).

Swapping the channel was a **one-line change** in `throughline.toml`: the `medium`
source's `url` points at `throughline-medium-web` instead of `throughline-medium-letter`.

## The authored page

The requirements graph is the *spec*; the page it governs is
[`content/rent-arrears-web-page.md`](content/rent-arrears-web-page.md). Read it against
the graph and see each composed axis bite: the web framing (title, scannable sections, a
live pay link, no sign-off), the general-reader gloss of *arrears*, the GOV.UK amount and
date style (£318.60, 1 August 2026), and the uncontracted formal register throughout.

## How it's wired

- The project's own graph lives under `intents/`, `user-requirements/` and
  `system-requirements/`. Each page requirement **grounds** through `implements` →
  `UR-0001` → `derives_from` → `INT-0001` (its own throughline), and **separately**
  `satisfies` the borrowed
  `plain:`/`conventions:`/`tone:`/`purpose:`/`audience:`/`medium:` clause it honours.
- `satisfies` is a traceability link, not a grounding link — so a page requirement still
  justifies itself through its own intent, not through a borrowed standard.

## Running it

```sh
tl-compose check --strict     # fetches all six pinned sources, merges, validates
tl-compose trace SR-0001      # show the lead requirement across the plain and medium axes
```

Drive this project with `tl-compose`, never bare `tl`: bare `tl` fails fast the moment it
meets a namespace-qualified reference (`medium:SR-0008`) it cannot resolve, because only
the composition-aware tool fetches and merges the sources.
