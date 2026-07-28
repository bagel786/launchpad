# Design

<!-- impeccable:design -->

The visual world is **the multi-part carbonless work order** — the pre-printed job ticket a service business fills out, stamps, tears along the perforation, and hands over. The site is not styled *like* a form. It *is* one document, read top to bottom.

This is the durable system. It governs any future surface (a case-study page, a services detail page) as additional parts of the same form set.

## Color

Business forms are printed in flat process inks on tinted carbonless stock. That is the entire palette rationale — no gradients, no shadows-as-depth, no glass.

### Inks

| Token | Value | Role |
|---|---|---|
| `--ink-blue` | `#0770cc` | The press blue, sampled from the logo. Rule lines, box borders, field labels, and — at full strength — whole field regions. |
| `--ink-blue-deep` | `#064a87` | Blue on blue: knocked-back detail inside a blue field, and hover state on blue. |
| `--ink-black` | `#14171a` | Data ink. Everything "typed into" the form. |
| `--ink-red` | `#c8322b` | Stamp ink and required-field marks. Reserved. Never decorative. |

### Stocks

| Token | Value | Role |
|---|---|---|
| `--stock-bond` | `#f4f4f0` | The white original. Default page ground. Cool bond white, **not cream** — this is a printed business form, not a letterpress keepsake. |
| `--stock-canary` | `#f2d64b` | The file copy. Owns the clients section. |
| `--stock-pink` | `#f0b8bd` | The customer copy. Owns the remarks/testimonials section. |
| `--stock-manila` | `#d9cfae` | Envelope and tab stock. Sparing — folder tabs, footer. |

### Strategy

**Committed.** `--ink-blue` carries 40–50% of the surface as full-bleed field regions, not as accents scattered on neutral. Canary and pink each own one complete section at full bleed. A section is one stock; stocks never mix within a part.

Verified contrast, computed not eyeballed:

| Pair | Ratio |
|---|---|
| bond on `--ink-blue` | 4.54 |
| bond on `--ink-blue-deep` | 8.15 |
| black on bond / canary / pink / manila | 16.3 / 12.4 / 10.5 / 11.6 |
| `--ink-blue` on bond | 4.54 |
| `--ink-red` on bond | 4.83 |
| `--ink-red-deep` on canary / pink | 5.40 / 4.58 |
| `--ink-blue-deep` on canary | 6.20 |

`--ink-blue` at 4.54 on bond is a working but thin margin: it carries labels, rules, and headings, never long body copy. Two extra inks exist purely to clear AA on the tinted stocks, because the bright red and press blue both fail there:

| Token | Value | Role |
|---|---|---|
| `--ink-red-deep` | `#9e2119` | Stamp ink on canary and pink. `--ink-red` drops to 3.7 and 3.1 there. |
| `--ink-blue-onstock` | `#064a87` | Blue stamp ink on canary. `--ink-blue` drops to 3.5 there. |

## Type

Faces chosen as objects from the form's own world: the pre-printed apparatus is set in type, the data is typed in.

| Token | Face | Role |
|---|---|---|
| `--type-display` | Archivo Black | Form titles and section headings. Caps, tight tracking. The heavy gothic of a printed form header. |
| `--type-label` | Archivo Narrow (600) | Pre-printed field labels, box captions, form numbers, routing legend. Caps, wide tracking, small — 10–12px. |
| `--type-data` | Courier Prime | Everything entered into the form: headlines, body copy, names, client names. The typewriter face, used because it is literally the face that fills these forms. |

Self-hosted woff2 in `assets/fonts/`. No external font requests, ever.

The inversion is the point: the **content** is typewriter, the **chrome** is set type. Most sites do the opposite.

## Structure

- **The box.** Every content region is a ruled box with a 1px `--ink-blue` border and a label plate notched into its top-left, sitting on the rule. Boxes butt against each other sharing borders — a form grid, not floating cards. **No border-radius above 0 anywhere. No box-shadow used as elevation.**
- **Numbering.** Sections carry circled sequence numbers (① through ⑥) in the label plate. This is the nav's vocabulary too.
- **Perforation.** Parts are separated by a horizontal tear line: a dashed rule with the small round perf holes rendered as a repeating radial-gradient. It spans full bleed.
- **Grid.** 12 columns, 1px blue gutter rules visible at desktop as the form's own ruling. Collapses to a single column below 720px; the ruling stays.
- **Carbon misregistration.** The display line carries two flat hard offsets, canary at 3px and pink at 6px, no blur: the second and third copies printed a hair off the first. This is the device that makes the page read as *multi-part* rather than merely as a form, so it belongs on the primary display line of any surface. It is a press artefact and is the one permitted exception to the no-shadow rule — a blur on it would make it elevation and break it.
- **The instruction strip.** A black band under the nav carrying `PRESS FIRMLY · YOU ARE MAKING THREE COPIES` and the three stock chips. Every carbonless pad prints this; it also teaches the routing legend before the visitor meets the colored parts.
- **Signature and date rules.** Any field a client is meant to return carries a `SIGNED` / `DATE` rule pair. This is what distinguishes an unreturned form part from an empty section.
- **The stamp.** Status is a rotated (-4° to -7°) outlined block-caps mark in `--ink-red` or `--ink-blue`, at ~0.85 opacity with a slight ink-bleed via a subtle blur on a duplicate layer. Vocabulary is fixed: `COMPLETED`, `IN PROGRESS`, `AWAITING CLIENT SIGNATURE`, `QUOTE ON REQUEST`, `RECEIVED`. Stamps are never invented for decoration; each states a true fact.

## Imagery

Team photographs are inconsistent in source (studio portraits and phone selfies). They are unified by a single mandatory treatment, applied identically to all four:

- Square, face-centered crop.
- `filter: grayscale(1) contrast(1.15)` with a `--ink-blue` `multiply` overlay — a one-color halftone-style duotone, as a form photo would be reproduced.
- Set inside a ruled blue box with a label plate carrying the name and role.

Any future portrait receives the same treatment. This is a system rule, not a workaround.

Client identities are set as typeset wordmarks in `--type-label`, inside ruled job-ticket boxes. Third-party logo files are not hotlinked.

## Motion

The form's native motion is **the stamp thumping down** — nothing slides, nothing fades in from below.

- Section status stamps land once on first scroll into view: scale from 1.15 to 1 over 180ms on a sharp ease-out, with a 2px settle. Once only, never repeated.
- Links and buttons change ink, not position — no transforms on hover.
- Everything above the fold is visible on load. No scroll-triggered content reveals; only stamps animate.
- `prefers-reduced-motion: reduce` removes the stamp animation entirely; stamps are simply present.

## Accessibility floor

- WCAG AA on every text pair, verified by computation, not by eye.
- Focus is a 2px `--ink-red` outline with 2px offset — visible on all four stocks and on blue.
- Holds at 375px with no horizontal scroll. The ruled grid collapses; the ruling does not disappear.
- Decorative form ornament (perf holes, gutter rules, circled numbers) is `aria-hidden`. Stamps that carry real status are readable text, not images.
- The skip link works and is visible on focus.

## Prohibitions

Each of these bans a device the form world does not itself use:

- No rounded corners. No gradients other than the flat perf-hole repeat. No shadow used as elevation — the only shadow in the system is the zero-blur carbon offset above, which is a printing artefact.
- No icon library. Any mark is drawn in the form's grammar (rules, boxes, checkmarks, circled numbers).
- No stock photography. No illustration.
- No color outside the four inks and four stocks.
- No sans-serif body copy. Body copy is typed data; typed data is Courier Prime.
