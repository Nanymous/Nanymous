# Setup Guide

Everything in this profile is already wired for the GitHub user **Nanymous**.
This guide covers placement, personalization, the contribution-snake workflow, optional services, and testing.

---

## 1. Where the files live

This repository **is** your profile repository: [`Nanymous/Nanymous`](https://github.com/Nanymous/Nanymous).

```
Nanymous/
├── README.md                     ← rendered on https://github.com/Nanymous
├── assets/
│   ├── hero.svg                  ← animated hero banner
│   ├── divider.svg               ← animated section divider
│   └── footer.svg                ← animated wave footer
├── .github/workflows/snake.yml   ← contribution snake generator
├── SETUP.md                      ← this file
└── CUSTOMIZATION.md              ← colors, icons, widgets, animations
```

GitHub automatically renders the root `README.md` of a repository named after its owner
on the profile page. No extra configuration is needed for the README itself — just push to `main`.

> The custom SVGs are referenced with absolute `raw.githubusercontent.com` URLs pointing at `main`,
> so they resolve correctly both on the profile page and inside the repository view.

---

## 2. Your identity (already configured)

The profile is fully personalized for **Nanymous** — there are no `YOUR_*` placeholders left.
These values live in `README.md` (the `identity.yml` block, the typing animation, the hero banner, and the Contact section):

| Field | Value | Where it appears |
| :--- | :--- | :--- |
| Display name | `Nanymous` | Typing animation, `identity.yml`, hero banner |
| Location | `IRAN, Shiraz` | `identity.yml` |
| GitHub | `github.com/Nanymous` | Every stats/widget URL, Follow badge |
| Telegram | `t.me/Nanymous` | Telegram badge link |
| Email | `manymuos@gmail.com` | `mailto:` link |

There is **no website and no LinkedIn** — the Contact section links GitHub, Telegram, and Email only.
To change any of these, edit the value directly in `README.md` (see CUSTOMIZATION.md §10 for the social links).

The **Featured Projects** section is already populated with your real repositories
[`veldo-docs`](https://github.com/Nanymous/veldo-docs) and [`hodhod-docs`](https://github.com/Nanymous/hodhod-docs)
(descriptions and stacks taken from their own READMEs / topics). Edit that table directly to add or change projects.

To confirm nothing is left to fill in:

```bash
grep -n "YOUR_" README.md   # expect: no output
```

---

## 3. Enable the contribution snake

The snake is **self-hosted** through GitHub Actions (no external API, no token to manage).
The workflow [`.github/workflows/snake.yml`](.github/workflows/snake.yml) uses
[`Platane/snk/svg-only@v3`](https://github.com/Platane/snk) and publishes two SVGs to an `output` branch:

- `output/github-snake.svg` (light)
- `output/github-snake-dark.svg` (dark, `palette=github-dark`)

### One-time enablement

1. Open **Settings → Actions → General** in this repository.
2. Under **Actions permissions**, allow all actions (or at least `Platane/snk/*` and `crazy-max/ghaction-github-pages@*`).
3. Under **Workflow permissions**, select **Read and write permissions** so the job can push the `output` branch.
   (The job also declares `permissions: contents: write` explicitly.)
4. Open the **Actions** tab. If prompted, click **"I understand my workflows, go ahead and enable them"**.

### When it runs

| Trigger | Behavior |
| :--- | :--- |
| `push` to `main` | Regenerates on every push to `main` (including the push that adds this workflow) |
| `schedule` | Daily at `00:00 UTC` (`cron: "0 0 * * *"`) |
| `workflow_dispatch` | Manual: **Actions → Generate Contribution Snake → Run workflow** |

### First run

The snake images return 404 **until the first successful run** creates the `output` branch.
After pushing, either wait for the push-triggered run or start it manually:

```bash
gh workflow run snake.yml --repo Nanymous/Nanymous
gh run watch --repo Nanymous/Nanymous
```

Verify the outputs exist:

```bash
curl -sI https://raw.githubusercontent.com/Nanymous/Nanymous/output/github-snake-dark.svg | head -1
# expect: HTTP/2 200
```

> Pushes made by `GITHUB_TOKEN` do not re-trigger workflows, so there is no infinite loop.

---

## 4. Optional external services

All widgets in the README use **public, token-free endpoints**. Nothing requires a secret.
Two items are worth knowing about:

### Trophies (community mirror)

The original `github-profile-trophy.vercel.app` instance is currently disabled by its host.
This README uses `https://trophy.ryglcloud.net`, a **volunteer mirror officially listed** in the
[upstream load-balancing section](https://github.com/ryo-ma/github-profile-trophy#load-balancing-endpoints).
If it ever goes down, either pick another listed mirror or self-host:

1. Fork [`ryo-ma/github-profile-trophy`](https://github.com/ryo-ma/github-profile-trophy) and deploy to Vercel.
2. Add a `PAT_1` environment variable in Vercel (a GitHub PAT with public read access). **Never commit the token.**
3. Replace `trophy.ryglcloud.net` with your instance host in `README.md`.

### github-readme-stats (optional upgrade)

The public `github-readme-stats.vercel.app` instance is paused, so this profile uses
[`github-profile-summary-cards`](https://github.com/vn7n24fzkq/github-profile-summary-cards) instead (verified working).
If you prefer the classic readme-stats cards, self-host:

1. Fork [`anuraghazra/github-readme-stats`](https://github.com/anuraghazra/github-readme-stats) and deploy to Vercel.
2. Add a `PAT_1` environment variable in Vercel (GitHub PAT, public read scope). **Never commit the token.**
3. Swap the summary-card URLs for `https://<your-instance>/api?username=Nanymous&...`
   and `https://<your-instance>/api/top-langs/?username=Nanymous&...`.

No other service needs configuration.

---

## 5. Test the final result

1. Push to `main`, then open **https://github.com/Nanymous** in a private window (avoids cache).
2. Confirm the hero banner animates (glow, scanline, particles) and the typing line cycles.
3. Confirm every stats card, the streak, the 3D contribution graph, the snake, and trophies render.
4. Toggle GitHub theme (Settings → Appearance) — the `picture` elements switch dark/light variants
   for the contribution graph, snake, and trophies.
5. Check the **Actions** tab: the snake run is green and the `output` branch exists.
6. Resize to mobile width (or use device emulation): images scale down, badge rows wrap, nothing overflows.
7. Re-run `grep -n "YOUR_" README.md` — it should return nothing (the profile is fully configured).

### Troubleshooting

| Symptom | Fix |
| :--- | :--- |
| Snake image broken | First workflow run hasn't completed yet — run it manually (section 3). |
| An image is stale after editing | GitHub's image proxy (camo) caches; wait a few minutes or append `?v=2` to the URL. |
| Trophy image broken | Mirror temporarily down — switch to another [listed mirror](https://github.com/ryo-ma/github-profile-trophy#load-balancing-endpoints) or self-host. |
| Stats card shows an error | The summary-cards instance is rate-limited momentarily; it recovers on reload. |
| Workflow can't push `output` | Enable **Read and write** workflow permissions (section 3, step 3). |