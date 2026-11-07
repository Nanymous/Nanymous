# Customization Guide

How to change every visual and dynamic part of the profile. All examples use the username **Nanymous**.

---

## 1. Color system

The whole profile commits to one neon-on-deep-space palette. Change a color here, then update the matching spots listed beside it.

| Role | Hex | Used in |
| :--- | :--- | :--- |
| Background (deep space) | `#050810` | hero/footer SVG, streak `background` |
| Background (raised) | `#0B1226` | hero/footer gradient, badge `labelColor`, 3D `strokeColor` |
| Primary neon (cyan) | `#00E5FF` | hero text gradient, typing `color`, divider, streak `sideNums`, 3D `colors`, profile-views badge |
| Electric blue | `#38BDF8` | hero gradient mid-stop, 3D `colors` |
| Accent purple | `#A855F7` | hero gradient end, divider, streak `ring`/`currStreakLabel`, 3D `colors` |
| Indigo bridge | `#818CF8` | 3D `colors` |
| Body text | `#C9D1D9` | hero subtitle, streak `sideLabels` |
| Muted text | `#8B949E` / `#6E7681` | hero eyebrow, footer tagline, streak `dates` |
| Success green | `#22C55E` | hero "SYSTEM ONLINE" dot |
| Amber (fire) | `#F59E0B` | streak `fire` |

**Where to edit each:**

- **Custom SVGs** — open `assets/hero.svg`, `assets/divider.svg`, `assets/footer.svg` and change the
  `stop-color` / `fill` / `stroke` values in the `<defs>` gradients (`#neon`, `#bg`, `#lineFade`, etc.).
- **Typing animation** — the `color=00E5FF` query parameter in the `readme-typing-svg.demolab.com` URL.
- **Streak** — `background`, `ring`, `fire`, `currStreakNum`, `sideNums`, `currStreakLabel`, `sideLabels`, `dates` params.
- **3D contribution graph** — the `colors=00E5FF,38BDF8,818CF8,A855F7` param (comma-separated hex, no `#`).
- **Summary cards** — the `theme=tokyonight` param (see section 5 for other themes).
- **Badges** — the hex color segment in each `img.shields.io/badge/Label-Message-HEX` URL.
- **Skill icons** — `theme=dark` (or `theme=light`) on the `skillicons.dev` URLs.

---

## 2. Hero banner

The banner is a hand-built animated SVG at [`assets/hero.svg`](assets/hero.svg) (1200×400).

- **Text** — edit the `<text>` elements: the eyebrow (`HELLO WORLD, I AM`), the name (`NANYMOUS`),
  and the stack subtitle. The name appears twice (a blurred bloom copy + the sharp copy) — change both.
- **Glow / gradients** — the `#neon` gradient and the `#glow` / `#softGlow` filters in `<defs>`.
- **Motion** — every `<animate>` / `<animateTransform>` has a `dur` (speed) and `values` (range).
  Increase `dur` to slow things down; the scanline, particles, circuit dashes, and underline shimmer are all independent.
- **Size** — keep the `viewBox="0 0 1200 400"` ratio; the README sets `width="1200"` and GitHub scales it responsively.

**Prefer a hosted banner instead?** [Capsule Render](https://github.com/kyechan99/capsule-render) is verified working.
Example footer wave you can drop in place of `assets/footer.svg`:

```
https://capsule-render.vercel.app/api?type=waving&color=0:00E5FF,100:A855F7&height=150&section=footer&text=Thanks%20for%20visiting&fontColor=FFFFFF&fontSize=28&fontAlignY=70&animation=fadeIn
```

For a header banner use `section=header`, `type=waving`, and add `text=Nanymous&fontSize=60&fontAlignY=40`.

---

## 3. Typing animation

Service: [readme-typing-svg](https://github.com/DenverCoder1/readme-typing-svg) (`readme-typing-svg.demolab.com`).

| Param | Effect |
| :--- | :--- |
| `lines` | Semicolon-separated phrases. URL-encode: space `+`, comma `%2C`, `&` `%26`, apostrophe `%27`, semicolon between lines `;` |
| `font` / `weight` / `size` | Typography (any Google Font, e.g. `JetBrains+Mono`, `Fira+Code`) |
| `color` | Text color (hex, no `#`) |
| `duration` | ms per line typing |
| `pause` | ms pause after each line |
| `width` / `height` | Canvas size |
| `center` / `vCenter` | Center the text horizontally / vertically |

Add a line by appending `;Your+new+phrase` to `lines`.

---

## 4. Technology icons

### Skill rows [skillicons.dev](https://skillicons.dev)

```
https://skillicons.dev/icons?i=dart,py,php,lua,js,cs&theme=dark&perline=6
```

- `i` — comma-separated icon slugs. Browse the full list at <https://skillicons.dev> (e.g. `ts`, `react`, `docker`, `postgres`, `nodejs`).
- `perline` — icons per row (controls wrapping).
- `theme` — `dark` or `light`.
- Current slugs in use: `dart, py, php, lua, js, cs, html, css, threejs, flutter, laravel, flask, git, github, linux, windows, vscode`.

### Badges [shields.io](https://shields.io)

Format: `https://img.shields.io/badge/<Label>-<Message>-<HEX>?style=for-the-badge&logo=<slug>&logoColor=white`
Spaces in Label/Message become `%20`. Logos come from [Simple Icons](https://simpleicons.org).

### Custom logos (data URI)

Simple Icons no longer ships **C#** or **Microsoft SQL Server**, so those badges embed a
white logo as a base64 data URI (`logo=data:image/svg+xml;base64,...`). To regenerate one from
[Devicon](https://github.com/devicons/devicon):

```bash
# Example: C# (replace the original fill with white so it reads on the colored badge)
curl -s https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/csharp/csharp-plain.svg \
  | sed 's/fill="#68217A"/fill="white"/' \
  | base64 -w0
# Paste the output into: logo=data:image/svg+xml;base64,<OUTPUT>
```

The SQL Server badge uses a simple white database-cylinder mark (the official multicolor logo is too heavy for a badge).
Any Devicon icon works the same way: `https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/<name>/<name>-plain.svg`.

---

## 5. GitHub statistics

Service: [github-profile-summary-cards](https://github.com/vn7n24fzkq/github-profile-summary-cards) (verified working;
the classic `github-readme-stats` public instance is currently paused — see SETUP.md §4 to self-host it).

Available cards (swap the path segment):

```
api/cards/stats                 api/cards/repos-per-language
api/cards/productive-time       api/cards/most-commit-language
api/cards/profile-details
```

- `theme` — try `tokyonight` (current), `2077`, `synthwave`, `radical`, `dracula`, `github_dark`, `nord_dark`, `moonlight`.
  Full theme list: <https://github.com/vn7n24fzkq/github-profile-summary-cards/blob/master/docs/themes.md>.
- `utcOffset` — shift productive-time hours to your timezone, e.g. `&utcOffset=3.5` (omit to keep UTC).
- Each card is 340×200; the README pairs them two-per-row with `width="340"`.

**Streak** ([streak-stats](https://github.com/DenverCoder1/github-readme-streak-stats), `streak-stats.demolab.com`):
adjust `ring`, `fire`, `background`, `border_radius`, `hide_border`, and the `*Num` / `*Label` / `dates` color params.
Add `&exclude_days=Sun,Sat` to ignore weekends, or `&mode=weekly` for a weekly view.

---

## 6. Contribution graph (animated 3D)

Service: [CatsJuice/ssr-contributions-img](https://github.com/CatsJuice/ssr-contributions-img)
(`ssr-contributions-svg.vercel.app`). Playground: <https://ssr-contributions-svg.vercel.app/>.

| Param | Effect |
| :--- | :--- |
| `chart` | `3dbar` (current) or `calendar` |
| `format` | **must be `svg`** for embedding (default `html` won't render as an image) |
| `colors` | Comma-separated hex palette, overrides `theme` (e.g. `00E5FF,38BDF8,818CF8,A855F7`) |
| `animation` | `wave` (current), `fall`, or `raise` |
| `weeks` | Number of weeks (1–52) |
| `flatten` | `1` = empty days become flat floor tiles, `2` = empty days omitted, `0` = off |
| `gradient` | `true` adds a vertical gradient to each cube |
| `gap` / `scale` / `light` | Cube spacing / vertical exaggeration / light intensity |
| `strokeWidth` / `strokeColor` | Cube outline |
| `legend` / `legendPosition` | Show a Less→More legend (`bottomLeft`, `right`, `top`, …) |
| `dark` | `true` for dark-mode legend text; the README uses a `<picture>` to switch automatically |

Prefer a flat calendar? [ghchart](https://github.com/2016rshah/githubchart-api) is verified working with a custom color:
`https://ghchart.rshah.org/00E5FF/Nanymous`.

---

## 7. Contribution snake

Workflow: [`.github/workflows/snake.yml`](.github/workflows/snake.yml) · Action: [Platane/snk](https://github.com/Platane/snk).

- **Palette** — change the `outputs` query string, e.g. `dist/github-snake-dark.svg?palette=github-dark`
  (other palettes: `github`, `github-dark`, or custom `color_snake`/`color_dots` params — see the action README).
- **Schedule** — edit `cron: "0 0 * * *"` (daily UTC). `0 */12 * * *` = twice daily.
- **Outputs** — add more lines under `outputs:` for extra variants (e.g. a GIF via `Platane/snk@v3` instead of `svg-only`).
- The README reads the SVGs from the `output` branch; keep the filenames in sync if you rename them.

---

## 8. Trophies

Service: [ryo-ma/github-profile-trophy](https://github.com/ryo-ma/github-profile-trophy) via the officially listed mirror
`trophy.ryglcloud.net` (the original Vercel instance is disabled — see SETUP.md §4).

| Param | Effect |
| :--- | :--- |
| `theme` | `discord` (dark, current), `flat` (light, current), `onedark`, `dracula`, `gruvbox`, `nord`, `algolia`, `radical`, … |
| `column` | `-1` = single adaptive row (current); a number fixes columns |
| `no-bg` / `no-frame` | `true` removes background / frame for a clean look |
| `margin-w` / `margin-h` | Spacing between trophies |
| `rank` | Filter ranks, e.g. `rank=SSS,SS,S,AAA,AA,A` or hide low ranks `rank=-C,-B` |
| `title` | Comma-list to show only certain trophies (e.g. `Commits,Stars,Followers`) |

The README uses a `<picture>` (dark → `discord`, light → `flat`) so text stays readable in both GitHub themes.

---

## 9. Dividers & footer

- **Divider** — [`assets/divider.svg`](assets/divider.svg): a gradient hairline with two traveling pulses and a center diamond.
  Reuse it anywhere with `<img src=".../assets/divider.svg" width="1200" alt=""/>`. Edit pulse speed via the `dur` values.
- **Footer** — [`assets/footer.svg`](assets/footer.svg): three seamless looping waves (`animateTransform` translate by one
  600px period), twinkling stars, and a gradient headline. Change the headline `<text>`, wave colors (`fill`), and speeds (`dur`).

---

## 10. Social links & projects

- **Social** — the Contact section links **GitHub** (`github.com/Nanymous`), **Telegram** (`t.me/Nanymous`), and
  **Email** (`mailto:manymuos@gmail.com`). To change a destination, edit the `<a href>`; the badge text is the
  `Label-Message` part of each shields URL (e.g. `Telegram-Contact`). Add a new network by copying an `<a><img></a>`
  pair and choosing a [Simple Icons slug](https://simpleicons.org) (or a data-URI logo per section 4).
- **Projects** — the Featured Projects table currently lists [`veldo-docs`](https://github.com/Nanymous/veldo-docs)
  and [`hodhod-docs`](https://github.com/Nanymous/hodhod-docs); edit the rows to add or change projects. For live repo cards instead, self-host
  github-readme-stats (SETUP.md §4) and use:

  ```html
  <p align="center">
    <a href="https://github.com/Nanymous/YOUR_REPO">
      <img src="https://<your-instance>/api/pin/?username=Nanymous&repo=YOUR_REPO&theme=tokyonight&hide_border=true" width="340" alt="YOUR_REPO"/>
    </a>
  </p>
  ```

---

## 11. Section headings & layout

- Headings use a terminal-style `## // Title` prefix — rename freely; keep the `//` for consistency or drop it.
- Major groups are separated by `assets/divider.svg`. Add/remove divider `<img>` lines to change rhythm.
- Side-by-side cards use centered `<p>` blocks with fixed `width` values; GitHub's `max-width: 100%` makes them wrap
  and scale on mobile. Avoid HTML `<table>` for card grids (tables don't wrap gracefully on narrow screens).
- GitHub strips `<script>`, `<style>`, and `style=` attributes — all styling here lives inside the SVG files or in
  widget URL parameters, which is fully supported.